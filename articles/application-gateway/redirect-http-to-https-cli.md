---
title: 인증서로 응용 프로그램 게이트웨이 만들기 - Azure CLI | Microsoft Docs
description: Azure CLI를 사용하여 애플리케이션 게이트웨이를 만들고 SSL 종료를 위한 인증서를 추가하는 방법을 알아봅니다.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 7/14/2018
ms.author: victorh
ms.openlocfilehash: 2a93034500250ae3c7a74c01a8ccbb7661c5ff96
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958767"
---
# <a name="create-an-application-gateway-with-http-to-https-redirection-using-the-azure-cli"></a>Azure CLI를 사용하여 HTTP 및 HTTPS 간의 리디렉션으로 응용 프로그램 게이트웨이 만들기

Azure CLI를 사용하여 SSL 종료를 위한 인증서로 [응용 프로그램 게이트웨이](overview.md)를 만들 수 있습니다. 라우팅 규칙은 응용 프로그램 게이트웨이에서 HTTP 트래픽을 HTTPS 포트로 리디렉션하는 데 사용됩니다. 이 예제에서는 두 개의 가상 머신 인스턴스를 포함하는 응용 프로그램 게이트웨이의 백 엔드 풀에 대한 [가상 머신 확장 집합](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)도 만듭니다.

이 문서에서는 다음 방법을 설명합니다.

> [!div class="checklist"]
> * 자체 서명된 인증서 만들기
> * 네트워크 설정
> * 인증서가 있는 애플리케이션 게이트웨이 만들기
> * 수신기 및 리디렉션 규칙 추가
> * 기본 백 엔드 풀을 사용하여 가상 머신 확장 집합 만들기

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI를 로컬로 설치하여 사용하도록 선택한 경우 이 빠른 시작에서 Azure CLI 버전 2.0.4 이상을 실행해야 합니다. 버전을 확인하려면 `az --version`을 실행합니다. 설치 또는 업그레이드해야 하는 경우 [Azure CLI 설치](/cli/azure/install-azure-cli)를 참조하세요.

## <a name="create-a-self-signed-certificate"></a>자체 서명된 인증서 만들기

프로덕션에 사용하려면 신뢰할 수 있는 공급자가 서명한 유효한 인증서를 가져와야 합니다. 이 자습서에서는 openssl 명령을 사용하여 자체 서명된 인증서 및 pfx 파일을 만듭니다.

```azurecli-interactive
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out appgwcert.crt
```

인증서에 의미 있는 값을 입력합니다. 기본 값을 적용할 수 있습니다.

```azurecli-interactive
openssl pkcs12 -export -out appgwcert.pfx -inkey privateKey.key -in appgwcert.crt
```

인증서에 대한 암호를 입력합니다. 이 예제에서는 *Azure123456!* 을 사용하고 있습니다.

## <a name="create-a-resource-group"></a>리소스 그룹 만들기

리소스 그룹은 Azure 리소스가 배포 및 관리되는 논리적 컨테이너입니다. [az group create](/cli/azure/group#create)를 사용하여 리소스 그룹을 만듭니다.

다음 예제에서는 *eastus* 위치에 *myResourceGroupAG*라는 리소스 그룹을 만듭니다.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>네트워크 리소스 만들기

[az network vnet create](/cli/azure/network/vnet#az-net)를 사용하여 *myVNet*이라는 가상 네트워크와 *myAGSubnet*이라는 서브넷을 만듭니다. 그런 후 [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network_vnet_subnet_create)를 사용하여 백 엔드 서버에 필요한 *myBackendSubnet*이라는 서브넷을 추가할 수 있습니다. [az network public-ip create](/cli/azure/network/public-ip#az-network_public_ip_create)를 사용하여 *myAGPublicIPAddress*라는 IP 주소를 만듭니다.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-the-application-gateway"></a>Application Gateway 만들기

[az network application-gateway create](/cli/azure/network/application-gateway#az-network_application_gateway_create)를 사용하여 *myAppGateway*라는 응용 프로그램 게이트웨이를 만들 수 있습니다. Azure CLI를 사용하여 애플리케이션 게이트웨이를 만들 때 용량, sku, HTTP 설정 등의 구성 정보를 지정합니다. 

응용 프로그램 게이트웨이는 앞에서 만든 *myAGSubnet* 및 *myAGPublicIPAddress*에 할당됩니다. 이 예제에서는 응용 프로그램 게이트웨이 만들 때 사용자가 만든 인증서 및 해당 암호를 연결합니다. 

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 443 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress \
  --cert-file appgwcert.pfx \
  --cert-password "Azure123456!"

```

 애플리케이션 게이트웨이가 생성될 때까지 몇 분 정도 걸릴 수 있습니다. 응용 프로그램 게이트웨이가 생성되면 다음과 같은 새 기능을 볼 수 있습니다.

- *appGatewayBackendPool* - 응용 프로그램 게이트웨이에 백 엔드 주소 풀이 하나 이상 있어야 합니다.
- *appGatewayBackendHttpSettings* - 포트 80 및 HTTP 프로토콜을 통신에 사용하도록 지정합니다.
- *appGatewayHttpListener* - *appGatewayBackendPool*에 연결되는 기본 수신기입니다.
- *appGatewayFrontendIP* - *myAGPublicIPAddress*를 *appGatewayHttpListener*에 할당합니다.
- *rule1* - *appGatewayHttpListener*에 연결되는 기본 라우팅 규칙입니다.

## <a name="add-a-listener-and-redirection-rule"></a>수신기 및 리디렉션 규칙 추가

### <a name="add-the-http-port"></a>HTTP 포트 추가

[az network application-gateway frontend-port create](/cli/azure/network/application-gateway/frontend-port#az-network_application_gateway_frontend_port_create)를 사용하여 응용 프로그램 게이트웨이에 HTTP 포트를 추가할 수 있습니다.

```azurecli-interactive
az network application-gateway frontend-port create \
  --port 80 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name httpPort
```

### <a name="add-the-http-listener"></a>HTTP 수신기 추가

[az network application-gateway http-listener create](/cli/azure/network/application-gateway/http-listener#az-network_application_gateway_http_listener_create)를 사용하여 응용 프로그램 게이트웨이에 *myListener*라는 수신기를 추가할 수 있습니다.

```azurecli-interactive
az network application-gateway http-listener create \
  --name myListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port httpPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-the-redirection-configuration"></a>리디렉션 구성 추가

[az network application-gateway redirect-config create](/cli/azure/network/application-gateway/redirect-config#az-network_application_gateway_redirect_config_create)를 사용하여 응용 프로그램 게이트웨이에 HTTP 및 HTTPS 간의 리디렉션 구성을 추가합니다.

```azurecli-interactive
az network application-gateway redirect-config create \
  --name httpToHttps \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Permanent \
  --target-listener appGatewayHttpListener \
  --include-path true \
  --include-query-string true
```

### <a name="add-the-routing-rule"></a>라우팅 규칙 추가

[az network application-gateway rule create](/cli/azure/network/application-gateway/rule#az-network_application_gateway_rule_create)를 사용하여 응용 프로그램 게이트웨이에 리디렉션 구성을 포함하는 *rule2*라는 라우팅 규칙을 추가합니다.

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name rule2 \
  --resource-group myResourceGroupAG \
  --http-listener myListener \
  --rule-type Basic \
  --redirect-config httpToHttps
```

## <a name="create-a-virtual-machine-scale-set"></a>가상 머신 확장 집합 만들기

이 예제에서는 응용 프로그램 게이트웨이의 백 엔드 풀에 서버를 제공하는 *myvmss*라는 가상 머신 확장 집합을 만듭니다. 확장 집합의 가상 머신은 *myBackendSubnet* 및 *appGatewayBackendPool*에 연결됩니다. 확장 집합을 만들려면 [az vmss create](/cli/azure/vmss#az-vmss-create)를 사용합니다.

```azurecli-interactive
az vmss create \
  --name myvmss \
  --resource-group myResourceGroupAG \
  --image UbuntuLTS \
  --admin-username azureuser \
  --admin-password Azure123456! \
  --instance-count 2 \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --vm-sku Standard_DS2 \
  --upgrade-policy-mode Automatic \
  --app-gateway myAppGateway \
  --backend-pool-name appGatewayBackendPool
```

### <a name="install-nginx"></a>NGINX 설치

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh" }'
```

## <a name="test-the-application-gateway"></a>애플리케이션 게이트웨이 테스트

애플리케이션 게이트웨이의 공용 IP 주소를 가져오려면 [az network public-ip show](/cli/azure/network/public-ip#az-network_public_ip_show)를 사용합니다. 공용 IP 주소를 복사하여 브라우저의 주소 표시줄에 붙여넣습니다.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![보안 경고](./media/redirect-http-to-https-cli/application-gateway-secure.png)

자체 서명된 인증서를 사용하는 경우 보안 경고를 받으려면 **세부 정보**, **웹 페이지로 이동**을 차례로 선택합니다. 그러면 보안 NGINX 사이트가 다음 예제와 같이 표시됩니다.

![애플리케이션 게이트웨이의 기준 URL 테스트](./media/redirect-http-to-https-cli/application-gateway-nginxtest.png)

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * 자체 서명된 인증서 만들기
> * 네트워크 설정
> * 인증서가 있는 애플리케이션 게이트웨이 만들기
> * 수신기 및 리디렉션 규칙 추가
> * 기본 백 엔드 풀을 사용하여 가상 머신 확장 집합 만들기


