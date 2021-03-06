---
title: ADAL을 사용하여 Android에서 앱 간 SSO를 사용하도록 설정하는 방법 | Microsoft Docs
description: 'ADAL SDK의 기능을 사용하여 애플리케이션에서 Single Sign On을 활성화하는 방법입니다. '
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/13/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 4abf6bd2d82753e22d4fde92e219109274ce36be
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39580581"
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>ADAL을 사용하여 Android에서 앱 간 SSO를 사용하도록 설정하는 방법
사용자가 자격 증명을 한 번만 입력하고 이러한 자격 증명이 애플리케이션에서 자동으로 작동되도록 SSO(Single Sign-on)를 제공하는 것은 이제 업계 표준입니다. 종종 전화 통화 또는 문자로 전송하는 코드와 같은 추가 요소(2FA)로 결합된 작은 화면에서 자신의 사용자 이름 및 암호를 입력하는 어려움은 사용자가 한 번을 초과하여 로그인해야 하는 경우 불만족을 가져옵니다.

또한 다른 애플리케이션이 Microsoft365에서 Microsoft 계정 또는 회사 계정을 사용할 수 있는 ID 플랫폼을 적용하는 경우 고객은 게시자에 관계 없이 모든 애플리케이션에서 자격 증명을 사용할 수 있기를 기대합니다.

Microsoft ID SDK와 함께 Microsoft ID 플랫폼은 자체 응용 프로그램 제품 내의 SSO 또는 전체 디바이스에서 브로커 기능 및 Authenticator 응용 프로그램으로 고객을 만족시키는 기능을 제공합니다.

이 연습에서는 고객에게 SSO를 제공하도록 애플리케이션 내에서 SDK를 구성하는 방법을 알려줍니다.

이전 문서는 애플리케이션을 [Microsoft ID Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android)와 통합 하는 방법을 알고 있다고 가정합니다.

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Microsoft ID 플랫폼의 SSO 개념
### <a name="microsoft-identity-brokers"></a>Microsoft ID Broker
Microsoft는 다른 공급 업체의 응용 프로그램에서 자격 증명의 브리징을 허용하고 자격 증명을 확인하는 위치에서 단일 보안 위치가 필요한 특수한 향상된 기능을 허용하는 모든 모바일 플랫폼에 대한 응용 프로그램을 제공합니다. SDK는 이러한 **브로커**를 호출합니다. iOS 및 Android에서 브로커는 고객이 독립적으로 설치하거나 해당 직원에 대한 일부 또는 모든 디바이스를 관리하는 회사에서 디바이스에 푸시할 수 있는 다운로드 가능한 응용 프로그램을 통해 제공됩니다. 이러한 broker는 일부 애플리케이션 또는 IT 관리자가 원하는 기능에 따라 전체 디바이스에 대해서만 보안 관리를 지원합니다. Windows에서 이 기능은 기술적으로 웹 인증 브로커로 알려진 운영 체제에 기본적으로 제공된 계정 선택기로 제공됩니다.

#### <a name="broker-assisted-logins"></a>브로커 지원 로그인
브로커 지원 로그인은 브로커 응용 프로그램 내에서 발생하고 Microsoft ID 플랫폼을 적용하는 디바이스의 모든 응용 프로그램에서 자격 증명을 공유하도록 브로커의 저장소와 보안을 사용하는 로그인 환경입니다. 애플리케이션은 사용자가 로그인할 수 있도록 브로커를 사용합니다. iOS 및 Android에서 해당 브로커는 고객이 독립적으로 설치하거나 사용자에 대한 디바이스를 관리하는 회사에서 디바이스에 푸시할 수 있는 다운로드 가능한 응용 프로그램을 통해 제공됩니다. 이 응용 프로그램 유형의 예는 iOS에서 Microsoft Authenticator 응용 프로그램입니다. Windows에서 이 기능은 기술적으로 웹 인증 브로커로 알려진 운영 체제에 기본적으로 제공된 계정 선택기로 제공됩니다.
환경은 플랫폼별로 다르며 올바르게 관리되지 않는 경우 사용자에게 작업 중단이 발생할 수 있습니다. Facebook 애플리케이션을 설치하고 다른 애플리케이션에서 Facebook Connect를 사용하는 경우 아마도 이 패턴과 가장 친숙할 것입니다. Microsoft ID 플랫폼은 동일한 패턴을 사용합니다.

Android에서 계정 선택기는 사용자에게 덜 방해가 되는 애플리케이션 맨 위에 표시됩니다.

#### <a name="how-the-broker-gets-invoked"></a>브로커를 호출하는 방법
호환되는 브로커가 Microsoft Authenticator 응용 프로그램과 마찬가지로 디바이스에 설치된 경우 Microsoft ID SDK는 사용자가 Microsoft ID 플랫폼에서 계정을 사용하여 로그인하려는 것을 나타내는 경우 브로커 호출 작업을 자동으로 수행합니다. 
 
 #### <a name="how-microsoft-ensures-the-application-is-valid"></a>Microsoft가 애플리케이션이 유효한지 확인하는 방식
 
 브로커를 호출하는 애플리케이션의 ID를 확인하는 것은 브로커 지원 로그인에서 제공된 보안에 중요합니다. iOS와 Android는 제공된 애플리케이션에만 유효한 고유한 식별자를 적용하지 않으므로 악의적 애플리케이션은 합법적인 애플리케이션의 ID를 “스푸핑”하고 합법적인 애플리케이션을 의미하는 토큰을 받을 수 있습니다. 런타임 시 Microsoft가 적절한 애플리케이션과 항상 통신하고 있음을 확인하려면 Microsoft에 해당 애플리케이션을 등록할 때 개발자에게 사용자 지정 redirectURI를 요청합니다. **개발자가 이 리디렉션 URI를 만드는 방법은 아래에 자세히 설명되어 있습니다.** 이 사용자 지정 redirectURI는 애플리케이션의 인증서 지문을 포함하고 Google Play 스토어에서 애플리케이션에 고유하도록 보장됩니다. 응용 프로그램이 브로커를 호출하면 브로커는 Android 운영 체제에 브로커를 호출한 인증서 지문을 제공하도록 요청합니다. 브로커는 ID 시스템에 대한 호출에서 Microsoft에 이 인증서 지문을 제공합니다. 애플리케이션의 인증서 지문이 등록하는 동안 개발자가 제공한 인증서 지문과 일치하지 않는 경우 애플리케이션이 요청하는 리소스를 위한 토큰에 대한 액세스가 거부됩니다. 이러한 확인을 통해 개발자가 등록한 응용 프로그램만 토큰을 받습니다.

조정된 SSO 로그인은 다음과 같은 이점이 있습니다.

* 사용자는 공급 업체에 관계 없이 모든 응용 프로그램에서 SSO를 경험합니다.
* 애플리케이션은 조건부 액세스와 같은 고급 비즈니스 기능을 사용하고 Intune 시나리오를 지원합니다.
* 응용 프로그램은 비즈니스 사용자에 대한 인증서 기반 인증을 지원할 수 있습니다.
* 애플리케이션 및 사용자의 ID로서 더 안전한 로그인 환경은 추가 보안 알고리즘 및 암호화로 브로커 애플리케이션에 의해 확인됩니다.

Microsoft ID SDK가 브로커 응용 프로그램과 작업하여 SSO를 활성화하는 방법에 대한 표현은 다음과 같습니다.

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

이 배경 정보를 사용하여 Microsoft ID 플랫폼 및 SDK를 사용하여 응용 프로그램 내에서 SSO를 더 잘 이해하고 구현할 수 있어야 합니다.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>브로커 지원 SSO에 대한 SSO 설정
디바이스에 설치되어 있는 모든 브로커를 사용하는 애플리케이션에 대한 기능은 **기본적으로 해제되어**있습니다. 브로커와 함께 애플리케이션을 사용하려면 몇 가지 추가 구성을 수행하고 애플리케이션에 코드를 추가해야 합니다.

수행할 단계는 다음과 같습니다.

1. MS SDK에 대한 애플리케이션 코드의 호출에서 브로커 모드를 활성화합니다.
2. 새 리디렉션 URI 설정 및 앱과 앱 등록에 이를 제공
3. Android 매니페스트에서 올바른 사용 권한 설정

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1단계: 응용 프로그램에서 브로커 모드 활성화
"설정" 또는 인증 인스턴스의 초기 설정을 만들 때 브로커를 사용하는 응용 프로그램에 대한 기능은 설정되어 있습니다. 앱에서 이를 수행하려면

```
AuthenticationSettings.Instance.setUseBroker(true);
```

#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>2단계: URL 구성표와 함께 새 리디렉션 URI 설정
올바른 애플리케이션이 반환된 자격 증명 토큰을 받는지 확인하려면 Android 운영 체제에서 확인할 수 있는 방식으로 애플리케이션에 다시 호출하는지 확인해야 합니다. Android 운영 체제는 Google Play 스토어에서 인증서의 해시를 사용합니다. 불량 애플리케이션에서 인증서의 이 해시를 스푸핑할 수 없습니다. 브로커 애플리케이션의 URI와 함께 Microsoft는 토큰이 올바른 애플리케이션에 반환되는지 확인합니다. 고유한 리디렉션 URI는 애플리케이션에 등록돼야 합니다.

리디렉션 URI는 다음의 적절한 형식이어야 합니다.

`msauth://packagename/Base64UrlencodedSignature`

예: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

[Azure Portal](https://portal.azure.com/)을 사용하여 앱 등록에 이 리디렉션 URI를 지정할 수 있습니다. Azure AD 앱 등록에 대한 자세한 내용은 [Azure Active Directory와 통합](active-directory-how-to-integrate.md)을 참조하세요.

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>3단계: 응용 프로그램에 올바른 사용 권한 설정
Android에서 브로커 애플리케이션은 Android OS의 계정 관리자 기능을 사용하여 애플리케이션 간에 자격 증명을 관리합니다. Android에서 브로커를 사용하려면 앱 매니페스트에 AccountManager 계정을 사용할 수 있는 권한이 있어야 합니다. 이러한 사용 권한은 [여기의 계정 관리자에 대한 Google 설명서](http://developer.android.com/reference/android/accounts/AccountManager.html)에 자세히 설명돼 있습니다.

특히 이러한 사용 권한은 다음과 같습니다.

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>SSO를 구성했습니다.
이제 Microsoft ID SDK는 자동으로 응용 프로그램에서 자격 증명을 공유하고 디바이스에 있는 경우 브로커를 호출합니다.

