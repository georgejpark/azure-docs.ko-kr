---
title: Ansible을 사용하여 Azure App Service 웹앱 크기 조정
description: Ansible을 사용하여 Linux의 App Service에서 Java 8 및 Tomcat 컨테이너 런타임을 포함하는 웹앱을 만드는 방법 알아보기
ms.service: ansible
keywords: Ansible, Azure, Devops, Bash, 플레이북, Azure App Service, Web App, 크기 조정, Java
author: tomarcher
manager: jeconnoc
ms.author: kyliel
ms.topic: tutorial
ms.date: 12/08/2018
ms.openlocfilehash: 4ec5a0691ce73a2ffebe07b316ce5b1dde8c2b49
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53249382"
---
# <a name="scale-azure-app-service-web-apps-by-using-ansible"></a>Ansible을 사용하여 Azure App Service 웹앱 크기 조정
[Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview)(또는 간단히 Web Apps)는 웹 애플리케이션, REST API 및 모바일 백 엔드를 호스팅합니다. .NET, .NET Core, Java, Ruby, Node.js, PHP 또는 Python 등 원하는 언어로 개발할 수 있습니다.

Ansible을 사용하면 사용자 환경에서 리소스의 배포 및 구성을 자동화할 수 있습니다. 이 문서에서는 Ansible을 사용하여 Azure App Service에서 앱의 크기를 조정하는 방법에 대해 설명합니다.

## <a name="prerequisites"></a>필수 조건
- **Azure 구독** - Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)을 만듭니다.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- **Azure App Service Web Apps** - Azure App Service 웹앱이 아직 설치되지 않은 경우 [Ansible을 사용하여 Azure 웹앱을 만들](ansible-create-configure-azure-web-apps.md) 수 있습니다.

## <a name="scale-up-an-app-in-app-service"></a>App Service에서 앱 강화
앱이 속한 App Service 계획의 가격 책정 계층을 변경하여 강화할 수 있습니다. 이 섹션에서는 다음 작업을 정의하는 샘플 Ansible 플레이북을 제공합니다.
- 기존 App Service 계획의 팩트 가져오기
- 세 명의 작업자에서 S2로 App Service 계획 업데이트

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_name: myAppServicePlan
    location: eastus

  tasks:
  - name: Get facts of existing App serivce plan
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku

  - name: Scale up the App service plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      is_linux: true
      sku: S2
      number_of_workers: 3
      
  - name: Get facts
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku
```

이 플레이북을 *webapp_scaleup.yml*로 저장합니다.

플레이북을 실행하려면 다음과 같이 **ansible-playbook** 명령을 사용합니다.
```bash
ansible-playbook webapp_scaleup.yml
```

플레이북을 실행한 후에 다음 예제와 비슷한 출력에서는 App Service 계획이 세 개의 작업자를 포함한 S2로 성공적으로 업데이트되었음을 보여줍니다.
```Output
PLAY [localhost] **************************************************************

TASK [Gathering Facts] ********************************************************
ok: [localhost]

TASK [Get facts of existing App serivce plan] **********************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

ok: [localhost]

TASK [debug] ******************************************************************
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 1,
        "family": "S",
        "name": "S1",
        "size": "S1",
        "tier": "Standard"
    }
}

TASK [Scale up the App service plan] *******************************************
changed: [localhost]

TASK [Get facts] ***************************************************************
ok: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 3,
        "family": "S",
        "name": "S2",
        "size": "S2",
        "tier": "Standard"
    }
}

PLAY RECAP **********************************************************************
localhost                  : ok=6    changed=1    unreachable=0    failed=0 
```

## <a name="next-steps"></a>다음 단계
> [!div class="nextstepaction"] 
> [Azure의 Ansible](https://docs.microsoft.com/azure/ansible/)