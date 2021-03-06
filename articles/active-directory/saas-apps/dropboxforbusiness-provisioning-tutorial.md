---
title: '자습서: Azure Active Directory로 자동 사용자 프로비전을 위한 Dropbox 구성 | Microsoft Docs'
description: Azure Active Directory와 Dropbox for Business 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 8e61fe4af83ee72df74027b2e52a3e81db486798
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347310"
---
# <a name="tutorial-configure-dropbox-for-business-for-automatic-user-provisioning"></a>자습서: 자동 사용자 프로비전을 위한 Dropbox for Business 구성

이 자습서의 목적은 사용자 계정을 Azure AD에서 Dropbox for Business으로 자동 프로비전 및 프로비전 해제하도록 Dropbox for Business 및 Azure AD에서 수행해야 하는 단계를 설명하는 것입니다.

## <a name="prerequisites"></a>필수 조건

이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 항목이 있다고 가정합니다.

*   Azure Active Directory 테넌트.
*   Dropbox for Business Single Sign-On이 설정된 구독
*   팀 관리자 권한이 있는 Dropbox for Business의 사용자 계정.

## <a name="assigning-users-to-dropbox-for-business"></a>Dropbox for Business에 사용자 할당

Azure Active Directory는 "할당"이라는 개념을 사용하여 어떤 사용자가 선택한 앱에 대한 액세스를 받아야 하는지를 판단합니다. 자동 사용자 계정 프로비전의 컨텍스트에서는 Azure AD의 애플리케이션에 "할당된" 사용자 및 그룹만 동기화됩니다.

프로비전 서비스를 구성하고 사용하도록 설정하기 전에 Dropbox for Business 앱에 액세스해야 하는 사용자를 나타내는 Azure AD의 사용자 및/또는 그룹을 결정해야 합니다. 결정했으면 다음 지침에 따라 이러한 사용자를 Dropbox for Business 앱에 할당할 수 있습니다.

[엔터프라이즈 앱에 사용자 또는 그룹 할당](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a>Dropbox for Business에 사용자를 할당하기 위한 주요 팁

*   단일 Azure AD 사용자를 Dropbox for Business에 할당하여 프로비전 구성을 테스트하는 것이 좋습니다. 추가 사용자 및/또는 그룹은 나중에 할당할 수도 있습니다.

*   Dropbox for Business에 사용자를 할당할 때 유효한 사용자 역할을 선택해야 합니다. "기본 액세스" 역할은 프로비전에 적합하지 않습니다.

## <a name="enable-automated-user-provisioning"></a>자동 사용자 프로비전 사용

이 섹션에서는 사용자의 Azure AD를 Dropbox for Business의 사용자 계정 프로비전 API에 연결하고, Azure AD의 사용자 및 그룹 할당을 기반으로 Dropbox for Business에서 할당된 사용자 계정을 만들고 업데이트하고 비활성화하도록 프로비전 서비스를 구성하는 방법을 안내합니다.

>[!Tip]
>[Azure Portal](https://portal.azure.com)에 제공된 지침에 따라 Dropbox for Business에 SAML 기반 Single Sign-On을 사용하도록 설정할 수도 있습니다. Single Sign-On은 자동 프로비전과 별개로 구성할 수 있습니다. 하지만 이 두 가지 기능은 서로 보완적입니다.

### <a name="to-configure-automatic-user-account-provisioning"></a>자동 사용자 계정 프로비전을 구성하려면

1. [Azure Portal](https://portal.azure.com)에서 **Azure Active Directory > 엔터프라이즈 앱 > 모든 응용 프로그램** 섹션으로 이동합니다.

2. 이미 Dropbox for Business에 Single Sign-On을 구성한 경우 검색 필드를 사용하여 Dropbox for Business의 인스턴스를 검색합니다. 그러지 않은 경우 **추가**를 선택하고 응용 프로그램 갤러리에서 **Dropbox for Business**를 검색합니다. 검색 결과에서 Dropbox for Business를 선택하고 응용 프로그램 목록에 추가합니다.

3. Dropbox for Business의 인스턴스를 선택한 다음 **프로비전** 탭을 선택합니다.

4. **프로비전 모드**를 **자동**으로 설정합니다. 

    ![프로비전](./media/dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. **관리자 자격 증명** 섹션에서 **권한 부여**를 클릭합니다. 새 브라우저 창에 Dropbox for Business 로그인 대화 상자가 열립니다.

6. **Sign-in to Dropbox to link with Azure AD**(Azure AD와 연결하기 위해 Dropbox에 로그인) 대화 상자에서 Dropbox for Business 테넌트에 로그인합니다.

     ![사용자 프로비전](./media/dropboxforbusiness-provisioning-tutorial/ic769518.png "사용자 프로비전")

7. Azure Active Directory에 Dropbox for Business 테넌트를 변경할 권한을 부여할지 확인합니다. **허용**을 클릭합니다.
    
      ![사용자 프로비전](./media/dropboxforbusiness-provisioning-tutorial/ic769519.png "사용자 프로비전")

8. Azure Portal에서 **연결 테스트**를 클릭하여 Azure AD가 Dropbox for Business 앱에 연결할 수 있는지 확인합니다. 연결에 실패하면 Dropbox for Business 계정에 팀 관리자 권한이 있는지 확인하고 **"권한 부여"** 단계를 다시 시도합니다.

9. 프로비전 오류 알림을 받을 개인 또는 그룹의 이메일 주소를 **알림 메일** 필드에 입력하고 확인란을 선택합니다.

10. **저장**을 클릭합니다.

11. [매핑] 섹션에서 **Synchronize Azure Active Directory Users to Dropbox for Business**(Azure Active Directory 사용자를 Dropbox for Business에 동기화)를 선택합니다.

12. **특성 매핑** 섹션에서 Azure AD에서 Dropbox for Business로 동기화되는 사용자 특성을 검토합니다. **일치** 속성으로 선택한 특성은 업데이트 작업 시 Dropbox for Business의 사용자 계정을 일치시키는 데 사용됩니다. 저장 단추를 선택하여 변경 내용을 커밋합니다.

13. Dropbox for Business에 대한 Azure AD 프로비전 서비스를 사용하도록 설정하려면 [설정] 섹션에서 **프로비전 상태**를 **켜기**로 변경합니다.

14. **저장**을 클릭합니다.

[사용자 및 그룹] 섹션에서 Dropbox for Business에 할당된 모든 사용자 및/또는 그룹의 초기 동기화가 시작됩니다. 초기 동기화는 서비스가 실행되는 동안 약 40분마다 발생하는 후속 동기화보다 더 많은 시간이 걸립니다. **동기화 세부 정보** 섹션을 사용하여 진행 상태를 모니터링하고 링크를 클릭하여 Dropbox for Business 앱의 프로비전 서비스에서 수행한 모든 작업을 설명하는 프로비전 작업 로그를 확인할 수 있습니다.

Azure AD 프로비저닝 로그를 읽는 방법에 대한 자세한 내용은 [자동 사용자 계정 프로비저닝에 대한 보고](../manage-apps/check-status-user-account-provisioning.md)를 참조하세요.


## <a name="additional-resources"></a>추가 리소스

* [엔터프라이즈 앱에 대한 사용자 계정 프로비전 관리](tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)
* [Single Sign-On 구성](dropboxforbusiness-tutorial.md)