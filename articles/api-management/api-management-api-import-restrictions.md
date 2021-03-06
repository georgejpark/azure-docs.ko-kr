---
title: Azure API Management 가져오기에서 제한 사항 및 알려진 문제 | Microsoft Docs
description: Open API, WSDL 또는 WADL 형식을 사용하여 Azure API Management로 가져오기 시 알려진 문제 및 제한 사항입니다.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: a9f4a4ed4a8771f32a4d66aed2457a43abb92a63
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39295332"
---
# <a name="api-import-restrictions-and-known-issues"></a>API 가져오기 제한 사항 및 알려진 문제
## <a name="about-this-list"></a>다음 목록 정보
API를 가져올 때 성공하기 위해 먼저 해결해야 하는 몇 가지 제한 사항이나 문제가 있을 수 있습니다. 이 문서에서는 이러한 내용을 API의 가져오기 형식별로 구성하여 설명합니다.

## <a name="open-api"> </a>OpenAPI/Swagger
OpenAPI 문서를 가져오는 중 오류를 수신하면 Azure Portal에서 디자이너를 사용하거나(Design - Front End - OpenAPI Specification Editor) <a href="http://editor.swagger.io">Swagger Editor</a>와 같은 타사 도구를 사용하여 유효성을 검사해야 합니다.

* OpenAPI에 대해 JSON 형식만 지원됩니다.
* 경로와 쿼리 간에 필수 매개 변수 이름은 고유해야 합니다. OpenAPI에서는 매개 변수 이름이 위치(예: 경로), 쿼리, 헤더 안에서만 고유하면 됩니다.  그러나 API Management에서는 경로와 쿼리 매개 변수 모두를 사용하여 작업을 구분할 수 있습니다(OpenAPI에서 지원하지 않는 기능). 따라서 전체 URL 템플릿 안에서 매개 변수 이름이 고유해야 합니다.
* **$ref** 속성을 사용하여 참조된 스키마는 다른 **$ref** 속성을 포함할 수 없습니다.
* **$ref** 포인터는 외부 파일을 참조할 수 없습니다.
* **x-ms-paths** 및 **x-servers**는 지원되는 유일한 확장명입니다.
* 사용자 지정 확장명은 가져오기 시 무시되고 내보내기에 대해 저장되거나 보존되지 않습니다.

> [!IMPORTANT]
> OpenAPI 가져오기와 관련된 중요한 정보 및 팁은 이 [문서](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/)를 참조하세요.

## <a name="wsdl"> </a>WSDL
SOAP 통과 API를 생성하는 데 WSDL 파일이 사용되며 SOAP-to-REST API의 백 엔드로 사용됩니다.
* **SOAP 바인딩** - 스타일 "document" 및 "literal" 인코딩의 SOAP 바인딩만 사용할 수 있습니다. "rpc" 스타일 또는 SOAP 인코딩은 지원되지 않습니다.
* **WSDL:Import** - 이 특성은 지원되지 않습니다. 고객은 가져오기를 문서 하나로 병합해야 합니다.
* **여러 부분으로 된 메시지** - 이러한 유형의 메시지는 지원되지 않습니다.
* **WCF wsHttpBinding** - Windows Communication Foundation으로 생성된 SOAP 서비스는 basicHttpBinding을 사용해야 합니다. wsHttpBinding은 지원되지 않습니다.
* **MTOM** - MTOM을 사용한 서비스는 <em>작동할 수 있습니다</em>. 현재는 공식적으로 지원되지 않습니다.
* **재귀** - 재귀적으로 정의된(예: 자체의 배열을 참조) 형식은 APIM에서 지원되지 않습니다.

## <a name="wadl"> </a>WADL
현재, 알려진 WADL 가져오기 문제가 없습니다.
