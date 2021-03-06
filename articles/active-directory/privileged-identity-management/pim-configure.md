---
title: Azure AD Privileged Identity Management란? | Microsoft Docs
description: Azure Active Directory PIM(Privileged Identity Management)에 대해 간략히 설명합니다.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: pim
ms.topic: overview
ms.date: 03/07/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 6720294fe9a3e166d0d6ef8f141e53212ef4b194
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496796"
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management란?

Azure AD(Azure Active Directory) Privileged Identity Management를 사용하면 조직 내에서 액세스를 관리, 제어 및 모니터링할 수 있습니다. 여기에는 Azure AD, Azure 리소스 및 기타 Microsoft Online Services(예: Office 365 또는 Microsoft Intune)의 리소스에 대한 액세스가 포함됩니다.

> [!NOTE]
> 테넌트에 대해 Privileged Identity Management를 사용하도록 설정하면, 서비스와 상호 작용하거나 서비스에서 혜택을 얻는 각 사용자에게 유효한 Azure AD Premium P2 또는 Enterprise Mobility + Security E5 유료 또는 평가판 라이선스가 필요합니다. 다음에 해당하는 그룹의 사용자를 예로 들 수 있습니다.
>
>- 권한 있는 역할 관리자 역할에 할당된 경우 
>- PIM을 통해 관리할 수 있는 다른 디렉터리 역할에 적격으로 지정된 경우 
>- PIM에서 요청을 승인/거부할 수 있는 경우 
>- JIT(Just-in-time) 또는 직접(시간 기반) 할당을 통해 Azure 리소스 역할에 할당된 경우  
>- 액세스 검토에 할당된 경우
>
>자세한 내용은 [Azure Active Directory 버전](../fundamentals/active-directory-whatis.md)을 참조하세요.

악의적인 사용자가 해당 액세스 권한을 얻거나 권한이 있는 사용자가 실수로 중요한 리소스에 영향을 줄 수 있으므로 조직에서는 보호되는 정보 또는 리소스에 액세스할 수 있는 사용자의 수를 최소화하려고 합니다.  그러나 사용자는 여전히 Azure AD, Azure, Office 365 또는 SaaS 응용 프로그램에서 권한 있는 작업을 수행해야 합니다. 조직은 사용자에게 구독 및 Azure AD와 같은 Azure 리소스에 대한 권한 있는 액세스를 제공할 수 있습니다. 이러한 사용자가 해당 관리자 권한으로 수행하는 작업에 대한 감시 요구 사항이 있습니다. Azure AD Privileged Identity Management는 과도하거나 필요하지 않거나 잘못 사용되는 액세스 권한의 위험을 완화하는 데 도움이 됩니다.

조직에 도움이 되는 Azure AD Privileged Identity Management 작업은 다음과 같습니다.

- Azure 리소스를 관리할 수 있는 권한 있는 역할이 할당된 사용자와 Azure AD에서 관리 역할이 할당된 사용자를 확인합니다.
- Microsoft Online Services(예: Office 365 및 Intune) 및 구독, 리소스 그룹 및 개별 리소스(예: Virtual Machines)에 대한 Azure 리소스에 주문형 “Just-In-Time” 관리 액세스를 사용합니다. 
- 관리자가 Azure 리소스를 변경한 내용(미리 보기)을 포함하여 관리자 활성화 기록을 확인합니다.
- 관리자 할당의 변경 내용에 대한 경고를 받습니다.
- Azure AD 권한 있는 관리자 역할을 활성화하기 위한 승인 필요합니다.
- 관리 역할의 구성원 자격을 검토하고, 지속적인 구성원 자격에 대한 근거를 제공하도록 사용자에게 요구합니다.

Azure AD에서 Azure AD Privileged Identity Management는 전역 관리자와 같은 기본 제공 Azure AD 조직 역할에 할당된 사용자를 관리할 수 있습니다. Azure에서 Azure AD Privileged Identity Management는 소유자 또는 참가자를 포함하여 Azure RBAC 역할을 통해 할당된 사용자 및 그룹을 관리할 수 있습니다.

## <a name="just-in-time-administrator-access"></a>시간에 맞추는 관리자 액세스

지금까지 Azure Portal, 다른 Microsoft Online Services 포털 또는 Windows PowerShell의 Azure AD cmdlet을 통해 사용자를 관리자 역할에 할당할 수 있었습니다. 그 결과, 해당 사용자는 **영구 관리자**가 되어 할당된 역할에 항상 활성 상태가 됩니다. Azure AD Privileged Identity Management는 **적격인 관리자**개념을 소개합니다. 적격 관리자는 때때로 권한 있는 액세스 권한이 필요하지만 매일 하루 종일 액세스할 필요가 없는 사용자여야 합니다. 역할은 사용자가 액세스가 필요할 때까지 비활성으로 있다가, 활성화 프로세스를 완료하고 미리 정해진 시간 동안 활성 관리자가 됩니다. 점점 더 많은 조직에서 이 방법을 사용하여 권한 있는 역할에 대한 "지속적인 관리자 액세스"를 줄이거나 제거합니다.


## <a name="terminology"></a>용어

*적격 역할 사용자* – 적법한 Azure AD 역할(역할에 대한 활성화 필요)로 할당된 조직 내 사용자입니다.

*위임된 승인자* – Azure AD 내에서 역할을 활성화하도록 요청을 승인하는 작업을 담당하는 하나 이상의 개인 또는 그룹입니다.

## <a name="scenarios"></a>시나리오

Privileged Identity Management에서 지원하는 시나리오는 다음과 같습니다.

**권한 있는 역할 관리자는 다음을 수행할 수 있습니다.**

- 특정 역할에 대한 승인을 사용하도록 설정
- 요청을 승인할 승인자 사용자 및/또는 그룹 지정
- 모든 권한 있는 역할에 대한 요청 및 승인 기록 보기

**지정된 승인자는 다음을 수행할 수 있습니다.**

- 보류 중인 승인(요청) 보기
- 역할 상승 요청(단일 및/또는 대량) 승인 또는 거부
- 승인/거부에 대한 근거 제공 

**적격 역할 사용자는 다음을 수행할 수 있습니다.**

- 승인이 필요한 역할의 활성화 요청
- 활성화 요청 상태 보기
- 활성화가 승인되면 Azure AD에서 작업 수행

## <a name="who-can-do-what-in-pim"></a>PIM에서 수행할 수 있는 작업

PIM을 처음 사용하는 경우 해당 사용자에게 디렉터리에 대한 [보안 관리자](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) 및 [권한 있는 역할 관리자](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) 역할이 자동으로 할당됩니다.

Azure AD 역할의 경우 권한 있는 역할 관리자 역할에 있는 사용자만 PIM의 다른 관리자에 대한 할당을 관리할 수 있습니다. [PIM 관리를 위해 다른 관리자에게 액세스 권한을 부여](pim-how-to-give-access-to-pim.md)할 수 있습니다. 전역 관리자, 보안 관리자 및 보안 읽기 권한자는 PIM에서 Azure AD 역할에 대한 할당을 볼 수 있습니다.

Azure 리소스 역할의 경우 구독 관리자, 리소스 소유자 또는 리소스 사용자 액세스 관리자만 PIM에서 다른 관리자에 대한 할당을 관리할 수 있습니다. 권한 있는 역할 관리자, 보안 관리자 또는 보안 읽기 권한자인 사용자는 기본적으로 PIM에서 Azure 리소스 역할에 대한 할당을 볼 수 있는 액세스 권한을 가지고 있지 않습니다.

## <a name="privileged-identity-management-overview-entry-point"></a>Privileged Identity Management 개요(진입점)

Azure AD Privileged Identity Management는 Azure AD 디렉터리 역할 및 Azure 리소스에 대한 역할을 관리할 수 있도록 지원합니다. Azure 리소스에 대한 역할 기능은 Azure AD의 관리 역할과 다릅니다. Azure 리소스 역할은 할당된 리소스 및 리소스 계층 구조의 모든 하위 리소스(상속이라고도 함)에 대한 세부적인 권한을 제공합니다. [RBAC, 리소스 계층 구조 및 상속에 대해 자세히 알아보세요](../../role-based-access-control/role-assignments-portal.md). Azure AD 디렉터리 역할과 Azure 리소스에 대한 PIM은 모두 [PIM 개요] 진입점 왼쪽 탐색 메뉴의 [관리] 섹션 아래에 있는 해당 링크에 액세스하여 관리할 수 있습니다.

PIM은 왼쪽 탐색 메뉴의 [작업] 섹션에서 역할을 활성화하고 보류 중인 활성화/요청 및 Azure AD 디렉터리 역할에 대해 보류 중인 승인을 보는 데 편리한 액세스를 제공하고, 보류 중인 응답을 검토합니다.

개요 진입점에서 작업 메뉴 항목 중 하나에 액세스하면 Azure AD 디렉터리 역할과 Azure 리소스 역할 둘 다에 대한 결과가 결과 보기에 포함되어 있습니다.

![빠른 시작](./media/pim-configure/quick-start.png)

[내 역할]에는 Azure AD 디렉터리 역할 및 Azure 리소스 역할에 대한 활성 및 적격 역할 할당 목록이 포함되어 있습니다. [적격 역할 할당 활성화에 대해 자세히 알아보세요](pim-how-to-activate-role.md).

Azure 리소스에 대한 역할 활성화에서는 적격 역할 멤버가 미래의 날짜/시간으로 활성화를 예약하고 관리자가 허용한 최대 기간 내에서 특정 활성화를 선택할 수 있는 새로운 환경을 소개하고 있습니다.

![](./media/pim-configure/activations.png)

예약된 활성화가 더 이상 필요하지 않은 경우 사용자는 왼쪽 탐색 메뉴에서 보류 중인 요청으로 이동하고 해당 요청과 동일한 줄에 있는 [취소] 단추를 클릭하여 보류 중인 요청을 취소할 수 있습니다.

![보류 중인 요청](./media/pim-configure/pending-requests.png)

## <a name="privileged-identity-management-admin-dashboard"></a>Privileged Identity Management 관리 대시보드

Azure AD 권한 있는 ID 관리자는 다음과 같은 중요한 정보를 전달하는 관리 대시보드를 제공합니다.

* 보안을 향상시킬 기회를 알려주는 경고
* 각 권한 있는 역할에 할당된 사용자 수  
* 적격 및 영구 관리자 수
* 디렉터리의 권한 있는 역할 활성화 그래프
* Azure 리소스 역할에 대한 Just-In-Time, 시간 제한 및 영구 할당의 수
* 지난 30일 동안 새 역할 할당이 있는 사용자 및 그룹(Azure 리소스 역할)


![PIM 대시보드 - 스크린샷](./media/pim-configure/PIM_Admin_Overview.png)

## <a name="privileged-role-management"></a>권한 있는 역할 관리

Azure AD Privileged Identity Management를 사용하면 Azure AD 디렉터리 역할에 대한 각 역할에 영구 또는 적격 관리자를 추가하거나 제거하여 관리자를 관리할 수 있습니다. Azure 리소스에 대한 PIM을 사용하면, 테넌트의 구독 관리를 사용할 수 있는 소유자, 사용자 액세스 관리자 및 전역 관리자가 사용자 또는 그룹을 Azure 리소스 역할에 적격(Just-In-Time 액세스), 시작 및 종료 날짜/시간이 있는 시간 제한(활성화 필요 없음) 액세스 또는 영구(역할 설정에서 사용하도록 설정된 경우)로 할당할 수 있습니다.

![PIM 추가/제거 관리자 - 스크린샷](./media/pim-configure/PIM_AddRemove.png)

## <a name="configure-the-role-activation-settings"></a>역할 활성화 설정 구성

[역할 설정](pim-how-to-change-default-settings.md)을 사용하여 Azure AD 디렉터리 역할에 대해 다음을 포함한 적격 역할 활성화 속성을 구성할 수 있습니다.

* 역할 활성화 기간
* 역할 활성화 알림
* 역할 활성화 프로세스 중 사용자가 제공해야 하는 정보
* 서비스 티켓 또는 인시던트 번호
* [승인 워크플로 요구 사항](./azure-ad-pim-approval-workflow.md)

![PIM 설정 - 관리자 활성화 - 스크린샷](./media/pim-configure/PIM_Settings_w_Approval_Disabled.png)

이미지에서 **Multi-Factor Authentication** 에 대한 단추가 비활성입니다. 높은 권한이 필요한 특정 역할에 대해 우리는 강화된 보호를 위해 MFA가 필요합니다.

Azure 리소스 역할에 대한 역할 설정을 사용하면 관리자가 다음을 포함한 Just-In-Time 및 직접 할당 설정을 구성할 수 있습니다.

- 종료 날짜/시간 없이 역할에 사용자 또는 그룹을 할당(영구 할당)하는 기능
- 할당의 기본 기간(영구적이지 않은 경우)
- 최대 활성화 기간(적격 역할 구성원이 활성화된 경우)
- 역할 활성화(Just-In-Time 할당) 또는 할당 프로세스(직접 할당) 중에 사용자가 제공해야 하는 정보

![](./media/pim-configure/role-settings-details.png)

## <a name="role-activation"></a>역할 활성화

[역할을 활성화](pim-how-to-activate-role.md)하기 위해 적격 관리자는 역할에 대해 시간 제한이 있는 "활성화"를 요청합니다. Azure AD Privileged Identity Management에서 **내 역할 활성화** 옵션을 사용하여 활성화를 요청할 수 있습니다.

역할을 활성화하려는 관리자는 Azure 포털에서 Azure AD Privileged Identity Management를 초기화해야 합니다.

역할 활성화는 사용자 지정할 수 있습니다. PIM 설정에서 활성화 길이 및 역할을 활성화하기 위해 관리자가 제공해야 하는 정보를 결정할 수 있습니다.

![PIM 관리자 요청 역할 활성화 - 스크린샷](./media/pim-configure/PIM_RequestActivation.png)

## <a name="review-role-activity"></a>역할 작업 검토

직원 및 관리자가 권한 있는 역할을 사용하는 방법을 추적하는 방법은 두 가지가 있습니다. 첫 번째 옵션은 [디렉터리 역할 감사 기록](pim-how-to-use-audit-log.md)을 사용하는 것입니다. 감사 기록 로그는 권한 있는 역할 할당, 역할 활성화 기록 및 Azure 리소스 역할에 대한 설정 변경 내용을 추적합니다. 

![PIM 활성화 기록 - 스크린샷](./media/pim-configure/PIM_ActivationHistory.png)

두 번째 옵션은 정기적인 [액세스 검토](pim-how-to-start-security-review.md)를 설정하는 것입니다. 이러한 액세스 검토는 할당된 검토자(예: 팀 관리자) 또는 자체적으로 검토할 수 있는 직원에 의해 수행될 수 있습니다. 사용자의 액세스 필요 여부를 모니터링하는 가장 좋은 방법입니다.

## <a name="azure-ad-pim-at-subscription-expiration"></a>구독 만료 시 Azure AD PIM

Azure AD PIM을 사용하기 전에 테넌트에 Azure AD Premium P2(또는 EMS E5) 평가판 또는 유료 구독이 있어야 합니다.  또한 테넌트 관리자에게 라이선스를 할당해야 합니다.  특히 Azure AD PIM을 통해 관리되는 Azure AD 역할의 관리자, Azure AD PIM을 통해 관리되는 Azure RBAC 역할의 관리자 및 액세스 검토를 수행하는 관리자가 아닌 사용자에게 라이선스를 할당해야 합니다.
조직에서 Azure AD Premium P2를 갱신하지 않거나 평가판이 만료되면, 테넌트에서 Azure AD PIM 기능을 더 이상 사용할 수 없고 적격 역할 할당이 제거되고 사용자가 더 이상 역할을 활성화할 수 없습니다. [Azure AD PIM 구독 요구 사항](./subscription-requirements.md)에서 자세히 살펴볼 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [PIM 사용 시작](pim-getting-started.md)
- [PIM을 사용하기 위한 구독 요구 사항](subscription-requirements.md)
- [Azure AD에서 하이브리드 및 클라우드 배포를 위한 권한 있는 액세스 보안](../users-groups-roles/directory-admin-roles-secure.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
