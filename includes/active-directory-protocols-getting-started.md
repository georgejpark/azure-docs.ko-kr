---
title: Azure AD .NET 프로토콜 개요 | Microsoft Docs
description: Azure AD를 사용하여 테넌트에서 웹 응용 프로그램 및 Web API에 대한 액세스 권한을 부여하기 위해 HTTP 메시지를 사용하는 방법입니다.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2018
ms.author: priyamo
ms.openlocfilehash: 44db736d5312b6850bb4fd8f47af5cd6b22535a7
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49960217"
---
## <a name="register-your-application-with-your-ad-tenant"></a>AD 테넌트에 응용 프로그램 등록
먼저, 응용 프로그램을 Azure AD(Azure Active Directory) 테넌트에 등록해야 합니다. 그러면 응용 프로그램에 대한 응용 프로그램 ID가 제공되며 토큰을 수신하는 데 사용할 수 있습니다.

* [Azure Portal](https://portal.azure.com)에 로그인합니다.
* 페이지 오른쪽 위 모서리에서 계정을 클릭하고, **디렉터리 전환** 탐색을 클릭한 다음, 적절한 테넌트를 선택하여 Azure AD 테넌트를 선택합니다. 
  * 계정에 하나의 Azure AD 테넌트만 있거나 이미 적절한 Azure AD 테넌트를 선택한 경우 이 단계를 건너뛰세요.
* 왼쪽 탐색 창에서 **Azure Active Directory**를 클릭합니다.
* **앱 등록**을 클릭하고 **새 응용 프로그램 등록**을 클릭합니다.
* 프롬프트에 따라 새 응용 프로그램을 만듭니다. 이 자습서에서는 웹 응용 프로그램이든지 네이티브 응용 프로그램이든지 상관 없지만 웹 응용 프로그램 또는 네이티브 응용 프로그램에 대한 특정 예제가 필요한 경우 [빠른 시작](../articles/active-directory/develop/v1-overview.md)을 확인하세요.
  * 웹 응용 프로그램의 경우 사용자가 로그인할 수 있는 앱의 기본 URL인 **로그인 URL**을 제공합니다(예: `http://localhost:12345`).
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * 네이티브 응용 프로그램의 경우 Azure AD에서 토큰 응답을 반환하는 데 사용할 **리디렉션 URI**를 제공합니다. 애플리케이션에 고유하게 해당되는 값을 입력합니다.(예:`http://MyFirstAADApp`)
* 등록이 끝나면 Azure AD는 응용 프로그램에 고유한 클라이언트 식별자인 **응용 프로그램 ID**를 할당합니다. 이 값은 다음 섹션에서 필요하므로 응용 프로그램 페이지에서 이 값을 복사해 둡니다.
* Azure Portal에서 응용 프로그램을 찾으려면 **앱 등록**을 클릭하고 **모든 응용 프로그램 보기**를 클릭합니다.
