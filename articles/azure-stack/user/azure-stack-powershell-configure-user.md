---
title: 사용자로 PowerShell 사용 하 여 Azure Stack에 연결 | Microsoft Docs
description: 사용자의 Azure Stack 인스턴스에 연결 하는 단계입니다.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2018
ms.author: sethm
ms.reviewer: Balsu.G
ms.openlocfilehash: 75b942ea99dace60b3c086b84e3b3e2157f8b821
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093729"
---
# <a name="connect-to-azure-stack-with-powershell-as-a-user"></a>사용자로 PowerShell 사용 하 여 Azure Stack에 연결

*적용 대상: Azure Stack 통합 시스템 및 Azure Stack 개발 키트*

이 문서에서는 Azure Stack에 연결 하는 단계를 제공 합니다. PowerShell 사용 하 여 Azure Stack 리소스를 관리에 연결 해야 합니다. 예를 들어, 제품을 구독할 virtual machines 만들기 및 Azure Resource Manager 템플릿을 배포 하려면 PowerShell을 사용할 수 있습니다. PowerShell cmdlet을 실행 합니다.

시작 하기:
  - 요구 사항이 있는지 확인 합니다.
  - Azure와 연결 되어 Active Directory (Azure AD) 또는 Active Directory Federation Services (AD FS). 
  - 리소스 공급자를 등록 합니다.
  - 연결을 테스트합니다.

## <a name="prerequisites"></a>필수 조건

이러한 필수 구성이 요소를 구성할 수 있습니다 합니다 [개발 키트](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), 또는 Windows 기반 외부 클라이언트에서 라면 [VPN을 통해 연결](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* 설치할 [Azure Stack 호환 Azure PowerShell 모듈](azure-stack-powershell-install.md)합니다.
* 일관 된 [azure storage: 차이점 및 고려 사항](azure-stack-powershell-download.md).

Azure Stack 구성에서 값을 사용 하 여 다음 스크립트 변수를 대체 해야 합니다.

- **Azure AD 테 넌 트 이름**  
  Azure Stack, 예를 들어 yourdirectory.onmicrosoft.com를 관리 하는 데 Azure AD 테 넌 트의 이름입니다.
- **Azure Resource Manager 끝점**  
  Azure Stack 개발 키트에 대 한이 값으로 설정 됩니다 https://management.local.azurestack.external합니다. 이 값을 얻으려면 Azure Stack 통합 시스템, 서비스 공급자에 게 문의 합니다.

## <a name="connect-with-azure-ad"></a>Azure AD를 사용 하 여 연결

```PowerShell  
    # Set your tenant name
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackUser" -TenantId $TenantId
```

## <a name="connect-with-ad-fs"></a>AD FS를 사용 하 여 연결

  ```PowerShell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance
  Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"

  $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
  $tenantId = (invoke-restmethod "$($AuthEndpoint)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

  # Sign in to your environment

  $cred = get-credential

  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $tenantId `
    -Credential $cred
  ```

## <a name="register-resource-providers"></a>리소스 공급자 등록

리소스 공급자는 포털을 통해 배포 된 리소스가 없는 새 사용자 구독에 대 한 자동으로 등록 되지 않습니다. 다음 스크립트를 실행 하 여 명시적으로 리소스 공급자를 등록할 수 있습니다.

```PowerShell  
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider -Force
    }
```

## <a name="test-the-connectivity"></a>연결 테스트

설정 모두 감안 하면, Azure Stack에서 리소스를 만들려면 PowerShell을 사용 하 여 연결을 테스트 합니다. 테스트로 응용 프로그램에 대 한 리소스 그룹을 만들고 가상 컴퓨터를 추가 합니다. "Myresourcegroup 이라는" 이름의 리소스 그룹을 만들려면 다음 명령을 실행 합니다.

```PowerShell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>다음 단계

- [Azure Stack용 템플릿 개발](azure-stack-develop-templates.md)
- [PowerShell을 사용하여 템플릿 배포](azure-stack-deploy-template-powershell.md)
- 클라우드 운영자 환경에 대 한 PowerShell을 설정 하려는 경우 참조를 [Azure Stack 운영자의 PowerShell 환경을 구성](../azure-stack-powershell-configure-admin.md) 문서.