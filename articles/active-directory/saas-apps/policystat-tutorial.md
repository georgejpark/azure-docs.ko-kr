---
title: '자습서: PolicyStat와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 PolicyStat 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 571b1723c1c064415e4d8cbee3620799af14d508
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428267"
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>자습서: PolicyStat와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 PolicyStat를 통합하는 방법에 대해 알아봅니다.

PolicyStat를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- PolicyStat에 액세스할 수 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 자신의 Azure AD 계정으로 PolicyStat에 자동으로 로그온(Single Sign-On)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

PolicyStat와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- PolicyStat Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 얻을 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다.  이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 PolicyStat 추가
1. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-policystat-from-the-gallery"></a>갤러리에서 PolicyStat 추가
PolicyStat가 Azure AD에 통합되도록 구성하려면 갤러리의 PolicyStat를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 PolicyStat를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

1. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![애플리케이션][2]
    
1. 새 응용 프로그램을 추가하려면 대화 상자 맨 위 있는 **새 응용 프로그램** 단추를 클릭합니다.

    ![애플리케이션][3]

1. 검색 상자에 **PolicyStat**를 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/policystat-tutorial/tutorial_policystat_search.png)

1. 결과 패널에서 **PolicyStat**를 선택하고 **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 사용하여 PolicyStat에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 PolicyStat 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 PolicyStat의 관련 사용자 간에 연결 관계를 설정해야 합니다.

PolicyStat에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 연결 관계를 설정합니다.

PolicyStat에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
1. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On 테스트하는 데 사용합니다.
1. **[PolicyStat 테스트 사용자 만들기](#creating-a-policystat-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 PolicyStat에 만듭니다.
1. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
1. **[Single Sign-On 테스트](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 PolicyStat 응용 프로그램에서 Single Sign-On을 구성합니다.

**PolicyStat에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **PolicyStat** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Configure Single Sign-On][4]

1. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Configure Single Sign-On](./media/policystat-tutorial/tutorial_policystat_samlbase.png)

1. **PolicyStat 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/policystat-tutorial/tutorial_policystat_url.png)

    a. **로그온 URL** 텍스트 상자에서 다음 패턴으로 URL을 입력합니다. `https://<companyname>.policystat.com`

    나. **식별자** 텍스트 상자에서 `https://<companyname>.policystat.com/saml2/metadata/` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE] 
    > 이러한 값은 실제 값이 아닙니다. 실제 로그온 URL 및 식별자로 값을 업데이트합니다. 이러한 값을 얻으려면 [PolicyStat 클라이언트 지원 팀](http://www.policystat.com/support/)에 문의하세요. 
 
1. **SAML 서명 인증서** 섹션에서 **메타데이터 XML**을 클릭한 후 컴퓨터에 메타데이터 파일을 저장합니다.

    ![Configure Single Sign-On](./media/policystat-tutorial/tutorial_policystat_certificate.png) 

1. 이 섹션은 사용자가 SAML 프로토콜 기반 페더레이션을 사용하여 Azure AD의 계정으로 PolicyStat에 인증할 수 있게 하는 방법을 간략하게 설명하기 위한 것입니다.

    PolicyStat 응용 프로그램에는 특정 형식의 SAML 어설션이 필요하므로, **SAML 토큰 특성** 구성에 사용자 지정 특성 매핑을 추가해야 합니다.  

     다음 스크린샷은 이에 대한 예제를 보여 줍니다.

     ![특성](./media/policystat-tutorial/tutorial_policystat_attribute.png "특성")

1. 필요한 특성 매핑을 추가하려면 다음 단계를 수행합니다.

    | 특성 이름    |   특성 값 |
    |------------------- | -------------------- |
    | uid | ExtractMailPrefix([mail]) |
    
    a. **특성 추가**를 클릭하여 **특성 추가** 대화 상자를 엽니다.

    ![Configure Single Sign-On](./media/policystat-tutorial/tutorial_policystat_04.png)

    ![Configure Single Sign-On](./media/policystat-tutorial/tutorial_policystat_addatribute.png)
    
    나. **특성 이름** 텍스트 상자에 **uid**를 입력합니다.

    다. **특성 값** 텍스트 상자에서 **ExtractMailPrefix()** 를 선택합니다.    
   
    d. **메일** 목록에서 **User.mail**을 선택합니다.
    
    e. **확인**을 클릭합니다.

1. **저장** 단추를 클릭합니다.

    ![Configure Single Sign-On](./media/policystat-tutorial/tutorial_general_400.png)

1. 다른 웹 브라우저 창에서 PolicyStat 회사 사이트에 관리자로 로그인합니다.

1. **관리** 탭을 클릭한 다음 왼쪽 탐색 창에서 **Single Sign-On 구성**을 클릭합니다.
   
    ![관리자 메뉴](./media/policystat-tutorial/ic808633.png "관리자 메뉴")

1. **설정** 섹션에서 **Single Sign-On 통합 사용**을 선택합니다.
   
    ![Single Sign-on 구성](./media/policystat-tutorial/ic808634.png "Single Sign-on 구성")

1. **특성 구성**을 클릭한 다음 **특성 구성** 섹션에서 다음 단계를 수행합니다.
   
    ![Single Sign-on 구성](./media/policystat-tutorial/ic808635.png "Single Sign-on 구성")
   
    a. **사용자 이름 특성** 텍스트 상자에 **uid**를 입력합니다.

    나. **First Name Attribute**(이름 특성) 텍스트 상자에 사용자의 **이름**을 **Britta**로 입력합니다.

    다. **Last Name Attribute**(성 특성) 텍스트 상자에 사용자의 **성**을 **Simon**으로 입력합니다.

    d. **Email Attribute**(메일 특성) 텍스트 상자에 사용자의 **메일 주소**를 **BrittaSimon@contoso.com**으로 입력합니다.

    e. **변경 내용 저장**을 클릭합니다.

1. **IDP 메타데이터**를 클릭한 다음, **IDP 메타데이터** 섹션에서 다음 단계를 수행합니다.
   
    ![Single Sign-on 구성](./media/policystat-tutorial/ic808636.png "Single Sign-on 구성")
   
    a. 다운로드한 메타데이터 파일을 열고 내용을 복사한 다음 **Your Identity Provider Metadata**(ID 공급자 메타데이터) 텍스트 상자에 붙여넣습니다.

    나. **변경 내용 저장**을 클릭합니다.

> [!TIP]
> 이제 앱을 설정하는 동안 [Azure Portal](https://portal.azure.com) 내에서 이러한 지침의 간결한 버전을 읽을 수 있습니다.  **Active Directory > 엔터프라이즈 응용 프로그램** 섹션에서 이 앱을 추가한 후에는 **Single Sign-On** 탭을 클릭하고 맨 아래에 있는 **구성** 섹션을 통해 포함된 설명서에 액세스하면 됩니다. 포함된 설명서 기능에 대한 자세한 내용은 [Azure AD 포함된 설명서]( https://go.microsoft.com/fwlink/?linkid=845985)에서 확인할 수 있습니다.
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure Portal**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/policystat-tutorial/create_aaduser_01.png) 

1. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/policystat-tutorial/create_aaduser_02.png) 

1. **사용자** 대화 상자를 열려면 대화 상자 위쪽에서 **추가**를 클릭합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/policystat-tutorial/create_aaduser_03.png) 

1. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/policystat-tutorial/create_aaduser_04.png) 

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    나. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    다. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="creating-a-policystat-test-user"></a>PolicyStat 테스트 사용자 만들기

Azure AD 사용자가 PolicyStat에 로그인할 수 있도록 하려면 PolicyStat로 프로비전되어야 합니다.  

PolicyStat는 사용자 프로비전 시간에만 지원합니다. 즉, PolicyStat에 사용자를 수동으로 추가하지 않아도 됩니다. SSO를 통해 첫 번째 로그인 시 사용자가 자동으로 추가됩니다.

>[!NOTE]
>다른 PolicyStat 사용자 계정 생성 도구 또는 PolicyStat가 제공한 API를 사용하여 Azure AD 사용자 계정을 프로비전할 수 있습니다.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 PolicyStat에 대한 액세스 권한을 부여합니다.

![사용자 할당][200] 

**Britta Simon을 PolicyStat에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 애플리케이션 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 애플리케이션**으로 이동한 후 **모든 애플리케이션**을 클릭합니다.

    ![사용자 할당][201] 

1. 응용 프로그램 목록에서 **PolicyStat**를 선택합니다.

    ![Configure Single Sign-On](./media/policystat-tutorial/tutorial_policystat_app.png) 

1. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

1. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

1. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 PolicyStat 타일을 클릭하면 PolicyStat 응용 프로그램에 자동으로 로그온됩니다.
액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../user-help/active-directory-saas-access-panel-introduction.md)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/policystat-tutorial/tutorial_general_01.png
[2]: ./media/policystat-tutorial/tutorial_general_02.png
[3]: ./media/policystat-tutorial/tutorial_general_03.png
[4]: ./media/policystat-tutorial/tutorial_general_04.png

[100]: ./media/policystat-tutorial/tutorial_general_100.png

[200]: ./media/policystat-tutorial/tutorial_general_200.png
[201]: ./media/policystat-tutorial/tutorial_general_201.png
[202]: ./media/policystat-tutorial/tutorial_general_202.png
[203]: ./media/policystat-tutorial/tutorial_general_203.png

