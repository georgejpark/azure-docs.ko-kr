---
title: Azure 계정으로 Office 365 등록 | Microsoft Docs
description: Azure 계정을 사용하여 Office 365 구독을 만드는 방법 알아보기
services: ''
documentationcenter: ''
author: JiangChen79
manager: adpick
editor: ''
tags: billing,top-support-issue
ms.assetid: ''
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: cwatson
ms.openlocfilehash: 682ffb5659cd776117fd3eec8fdb0fa5e0427ab8
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582306"
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a>Azure 계정으로 Office 365 구독에 등록
Azure 구독자인 경우 Azure 계정을 사용하여 Office 365 구독에 등록할 수 있습니다. Azure 구독이 있는 조직의 일부인 경우 기존 Azure AD(Azure Active Directory)에서 사용자를 위한 Office 365 구독을 만들 수 있습니다. Azure Active Directory 테넌트에서 전역 관리자 또는 대금 청구 관리자 권한이 있는 계정을 사용하여 Office 365에 등록합니다. 자세한 내용은 [Azure AD에서 계정 권한 확인](#RoleInAzureAD) 및 [Azure Active Directory에서 관리자 역할 할당](../active-directory/users-groups-roles/directory-assign-admin-roles.md)을 참조하세요.

Office 365 계정과 Azure 구독이 둘 다 이미 있는 경우 [Azure 구독에 Office 365 테넌트 연결](billing-add-office-365-tenant-to-azure-subscription.md)할 수 있습니다.

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a>Azure 계정을 사용하여 Office 365 구독 가져오기

1. [Office 365 제품 페이지](https://products.office.com/business)로 이동한 후 계획을 선택합니다.
2. 페이지의 오른쪽 위에서 **로그인**을 클릭합니다.

    ![Office 365 평가판 페이지 스크린샷](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. Azure 계정 자격 증명으로 로그인합니다. 조직에 대한 구독을 만들려고 하는 경우 Azure Active Directory 테넌트 내 전역 관리자 또는 대금 청구 관리자 디렉터리 역할의 구성원인 Azure 계정을 사용합니다.

    ![Office 365 로그인 스크린샷](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. **지금 시도**를 클릭합니다.

    ![Office 365에 대한 주문을 확인하는 스크린샷](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. 주문 접수 페이지에서 **계속**을 클릭합니다.

    ![Office 365 주문 접수 스크린샷](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

이제 모든 설정을 완료했습니다. 조직에 대한 Office 365 구독을 만든 경우 다음 단계에 따라 Azure AD 사용자가 현재 Office 365에 있는지 확인합니다.

1. Office 365 관리 센터를 엽니다.
2. **사용자**를 확장한 다음 **활성 사용자**를 클릭합니다.

    ![Office 365 관리 센터 사용자의 스크린샷](./media/billing-use-existing-azure-account-office-365-subscription/16-office-365-admin-center-users.png)

등록 후 Azure 구독이 속해 있는 Azure Active Directory 인스턴스에 Office 365 구독이 추가됩니다. 자세한 내용은 [Azure 및 Office 365 구독에 대한 추가 정보](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) 및 [Azure 구독이 Azure Active Directory와 연결되는 방법](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)을 참조하세요.

## <a id="RoleInAzureAD"></a>Azure AD에서 내 계정 사용 권한 확인
1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.
2. **모든 서비스**를 클릭하고 **Active Directory**를 검색합니다.

    ![Azure Portal의 Active Directory 스크린샷](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. **사용자 및 그룹** > **모든 사용자**를 클릭합니다.
4. 사용자 이름을 선택합니다. 

    ![Azure Active Directory 사용자를 보여 주는 스크린샷](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. **디렉터리 역할**을 클릭합니다.
  
    ![Azure Portal 디렉터리 역할을 보여 주는 스크린샷](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  기존 Azure Active Directory에서 사용자를 위한 Office 365 구독을 만들려면 **전역 관리자** 또는 **제한된 관리자** > **대금 청구 관리자** 역할이 필요합니다.

    ![Azure Portal 디렉터리 역할 대금 청구 관리자를 보여 주는 스크린샷](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-us"></a>도움 필요 시 문의하세요.

질문이 있거나 도움이 필요한 경우 [지원 요청을 만드세요](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). 
