---
title: Conversation Learner 모델에서 대화를 기록하는 방법 - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Conversation Learner 모델에서 대화를 기록하는 방법을 알아봅니다.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 5369e16e0f1adc48eb019f3790dc900577c144af
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243662"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-model"></a>Conversation Learner 모델에서 대화를 기록하는 방법

이 자습서에서는 Conversation Learner 인터페이스 내에서 최종 사용자 테스트를 수행하는 방법, 대화 상자를 기록하는 방법 및 모델을 개선하기 위해 기록된 대화 상자를 수정하는 방법을 보여줍니다.

## <a name="video"></a>비디오

[![자습서 9 미리 보기](https://aka.ms/cl-tutorial-09-preview)](https://aka.ms/blis-tutorial-09)

## <a name="requirements"></a>요구 사항
이 자습서를 수행하려면 일반 자습서 봇이 실행 중이어야 합니다.

    npm run tutorial-general

## <a name="details"></a>세부 정보
로그 대화 상자를 사용하여 최종 사용자와 관련된 대화 상자를 검토하고 수정할 수 있습니다.  특히 학습된 모델 및 전반적인 시스템의 성능을 향상시키기 위해 엔터티 레이블 및 작업 선택 영역을 수정할 수 있습니다. 

## <a name="steps"></a>단계

### <a name="create-the-model"></a>모델 만들기

1. Web UI에서 새 모델을 클릭합니다.
2. 이름에 LogDialogs를 입력합니다. 그런 다음, 만들기를 클릭합니다.

### <a name="create-an-entity"></a>엔터티 만들기

1. 엔터티, 새 엔터티를 차례로 클릭합니다.
2. 엔터티 이름에 도시를 입력합니다.
3. 만들기를 클릭합니다.

### <a name="create-two-actions"></a>두 가지 작업 만들기

1. 작업, 새 작업을 차례로 클릭합니다.
2. 응답에 '어느 도시인가요?'를 입력합니다.
3. 실격 엔터티에 $city를 입력합니다.
3. 만들기 클릭 

그런 다음, 두 번째 작업을 만듭니다.

1. 작업, 새 작업을 차례로 클릭합니다.
3. 응답에 '$city의 날씨는 아마도 맑습니다'를 입력합니다.
4. 필수 엔터티, $city를 입력합니다.
4. 만들기를 클릭합니다.

이제 두 가지 작업이 있습니다.

### <a name="train-the-bot"></a>봇 학습

1. 학습 대화 상자, 새 학습 대화 상자를 차례로 클릭합니다.
2. '날씨는 어떤가요'를 입력합니다.
3. 작업에 점수 지정을 클릭하고 '어느 도시인가요?'를 선택합니다.
2. '시애틀'을 입력합니다.
3. '시애틀'을 두 번 클릭하고 도시를 선택합니다.
    - 그러면 도시 엔터티로 표시됩니다.
5. 작업에 점수 지정을 클릭합니다.
6. '$city의 날씨는 아마도 맑습니다'를 선택합니다.
7. 학습 완료를 클릭합니다.

또 다른 예제 대화 상자를 추가합니다.

1. 학습 대화 상자, 새 학습 대화 상자를 차례로 클릭합니다.
2. '시애틀의 날씨는 어떤가요?'를 입력합니다. 시애틀이 엔터티로 태그가 지정됩니다.
5. 작업에 점수 지정을 클릭합니다. 
6. '$city의 날씨는 아마도 맑습니다'를 선택합니다.
7. 학습 완료를 클릭합니다.

### <a name="try-the-bot-as-the-user"></a>봇을 사용자로 사용해봅니다.
사용자에게 이 봇을 배포했다고 가정하습니다.

1. 로그 대화 상자를 클릭합니다.
2. 새 로그 대화 상자를 클릭합니다.
    - 그러면 사용자가 UI의 왼쪽의 웹 채팅 컨트롤에서 경험한 대로 봇을 나타냅니다. 오른쪽의 공백 영역을 무시할 수 있습니다.
3. '안녕하세요'를 입력합니다.
4. 봇 응답: '어느 도시인가요?'
4. '보스턴'을 입력합니다.
5. 봇 응답: '어느 도시인가요?'
    - 맞지 않은 것 같습니다. 따라서 이 대화 상자를 저장합니다.
2. 테스트 완료를 클릭합니다.

새 세션을 시작하겠습니다.

2. 새 로그 대화 상자를 클릭합니다.
3. '보스턴의 날씨 예보'를 입력합니다.
4. 봇 응답: '어느 도시인가요?'
2. 테스트 완료를 클릭합니다.

이제 두 번째 대화 상자를 수정하겠습니다.

1. 로그 대화 상자에서 '보스턴의 날씨 예보'를 클릭합니다.
    - 그러면 대화가 열립니다.
    - 대화의 사용자 쪽을 클릭하는 경우(여기서는 '보스턴의 날씨 예보') 엔터티 레이블을 변경할 수 있습니다.
    - 시스템 쪽을 클릭하는 경우(여기서는 '어느 도시인가요') 선택한 작업을 변경할 수 있습니다.
5. '보스턴의 날씨 예보'를 클릭합니다. 
    - 여기에서 근본 원인은 보스턴을 엔터티로 태그 지정하지 않은 것입니다. 해당 사항을 변경해야 합니다.
    - '보스턴'을 두 번 클릭한 다음, 도시를 선택합니다.
    - 변경 내용 제출을 클릭하고 저장을 클릭합니다. 변경에 따라 학습 대화 상자를 만들고, 변경한 시점의 학습 대화 상자에 배치합니다.
6. '$city의 날씨는 아마도 맑습니다'를 선택합니다.
7. 학습 완료를 클릭합니다. 이제 학습 대화 상자로 이동하는 경우 새 작업이 추가되어 나타납니다.

![](../media/tutorial9_logdiag1.PNG)

이제 다른 대화 상자를 수정하겠습니다.

1. 로그 대화 상자에서 '안녕하세요'를 클릭합니다.
    - 그러면 대화가 열립니다.
3. '안녕하세요'에 대한 응답은 '어느 도시인가요'입니다. 하지만 더 논리적인 응답으로 변경하려고 합니다. 더 나은 대답은 '안녕하세요, 저는 날씨 봇입니다.'입니다. 하지만 이를 수행하는 작업이 없으므로 새로 만들어야 합니다.
4. 작업을 클릭합니다.
    - 응답에 '저는 날씨 봇입니다. 날씨 예보를 도와드립니다.'를 입력합니다.
6. 응답에 대기 확인란 선택을 취소하여 비대기 작업으로 만듭니다.
7. 만들기를 클릭합니다.
8. 그런 다음, 이 새 작업을 선택하도록 클릭합니다. 그런 다음 저장을 클릭합니다.
    - 그러면 학습 세션의 해당 지점으로 돌아갑니다.
6. '어느 도시인가요?'를 선택하도록 클릭합니다.
7. '보스턴'을 입력합니다. 수행하지 않은 경우 보스턴을 엔터티로 태그 지정하도록 두 번 클릭합니다.
8. 작업에 점수 지정을 클릭합니다.
9. '$city의 날씨는 아마도 맑습니다'를 선택하도록 클릭합니다.
10. 학습 완료를 클릭합니다.

![](../media/tutorial9_addnewaction.PNG)

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [엔터티 검색 콜백](./10-entity-detection-callback.md)
