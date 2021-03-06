---
title: 기술 자료 게시, REST, Python
titleSuffix: QnA Maker - Azure Cognitive Services
description: 이 Python REST 기반 빠른 시작에서는 테스트된 최신 버전의 기술 자료를 게시된 기술 자료를 나타내는 Azure Search 인덱스에 푸시하는 방법(KB 게시)을 안내합니다. 또한 응용 프로그램 또는 챗봇에서 호출할 수 있는 엔드포인트를 만듭니다.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: diberry
ms.openlocfilehash: a9ce92677e66bf4a0c38399dea1f9bab5285824e
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53162378"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-python"></a>빠른 시작: Python을 사용하여 QnA Maker 기술 자료 게시

이 REST 기반 빠른 시작에서는 KB(기술 자료)를 프로그래밍 방식으로 게시하는 방법을 안내합니다. 게시는 최신 버전의 기술 자료를 전용 Azure Search 인덱스에 푸시하고, 응용 프로그램 또는 챗봇에서 호출할 수 있는 엔드포인트를 만듭니다.

이 빠른 시작에서 호출하는 QnA Maker API는 다음과 같습니다.
* [게시](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe) - 이 API는 요청 본문에 어떤 정보도 요구하지 않습니다.

## <a name="prerequisites"></a>필수 조건

* [Python 3.7](https://www.python.org/downloads/)
* QnA Maker 서비스가 있어야 합니다. 키를 검색하려면 대시보드의 [리소스 관리] 아래에서 [키]를 선택합니다.
* QnA Maker KB(기술 자료) ID는 아래와 같이 kbid 쿼리 문자열 매개 변수의 URL에 있습니다.

    ![QnA Maker 기술 자료 ID](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    아직 기술 자료가 없는 경우 샘플을 만들어서 빠른 시작: [새 기술 자료 만들기](create-new-kb-nodejs.md)에서 사용하면 됩니다.

> [!NOTE] 
> 전체 솔루션 파일은 [**Azure-Samples/cognitive-services-qnamaker-python** GitHub 리포지토리](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/tree/master/documentation-samples/quickstarts/publish-knowledge-base)에서 사용할 수 있습니다.

## <a name="create-a-knowledge-base-python-file"></a>기술 자료 Python 파일 만들기

`publish-kb-3x.py`라는 파일을 만듭니다.

## <a name="add-the-required-dependencies"></a>필수 종속성 추가

`publish-kb-3x.py`의 맨 위에 프로젝트에 필요한 종속성을 추가하는 다음 줄을 추가합니다.

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=1-1 "Add the required dependencies")]

## <a name="add-required-constants"></a>필요한 상수 추가

앞서의 필요한 종속성 뒤에 QnA Maker에 액세스하는 데 필요한 상수를 추가합니다. 사용자 고유의 값으로 대체합니다.

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=5-15 "Add the required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>기술 자료를 게시하기 위한 POST 요청 추가

필요한 상수 뒤에 다음 코드를 추가합니다. 이 코드는 기술 자료로 질문을 보내기 위한 QnA Maker API에 대한 HTTPS 요청을 수행한 후 답변을 수신합니다.

[!code-python[Add a POST request to publish knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=17-26 "Add a POST request to publish knowledge base")]

API 호출은 성공적인 게시에 대해 응답 본문에 내용이 없는 204 상태를 반환합니다. 이 코드는 204 응답에 대한 내용을 추가합니다.

다른 응답의 경우 해당 응답은 변경되지 않은 채 반환됩니다.

## <a name="build-and-run-the-program"></a>프로그램 빌드 및 실행

프로그램을 실행하려면 명령줄에 다음 명령을 입력합니다. 이 명령은 기술 자료를 게시한 후 성공 또는 오류에 대한 204를 출력하기 위한 요청을 QnA Maker API에 보냅니다.

```bash
python publish-kb-3x.py
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>다음 단계

기술 자료가 게시된 후 [답변을 생성할 엔드포인트 URL](../Tutorials/create-publish-answer.md#generating-an-answer)이 필요합니다. 

> [!div class="nextstepaction"]
> [QnA Maker(V4) REST API 참조](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)

[QnA Maker 개요](../Overview/overview.md)
