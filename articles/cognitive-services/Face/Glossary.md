---
title: 용어집 - Face API Service
titleSuffix: Azure Cognitive Services
description: Face API 서비스를 사용할 때 나타날 수 있는 용어에 대해 설명합니다.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 215b780bc403ab2df40567c3eb5a7ae86c9fe130
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127366"
---
# <a name="glossary"></a>용어

## <a name="a"></a>A

#### <a name="attributes"></a>특성

[age](#Age-Attribute), [gender](#Gender-Attribute), [머리 자세](#Head-Pose-Attribute), [수염](#Facial-Hair-Attribute), [웃는 얼굴](#Smile-Attribute)과 같은 Attributes는 [감지](#Detection-Face-Detection) 결과에서 선택 사항입니다.
이는 returnFaceAttributes 쿼리 매개 변수를 지정하여 [감지](#Detection-Face-Detection) API에서 가져올 수 있습니다. 특성은 [얼굴 ID](#Face-ID) 및 [사각형](#Face-Rectangle) 외에도 선택한 [얼굴](#Face)에 대한 추가 정보를 제공합니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

#### <a name="age-attribute"></a>Age(연령) (특성)

age는 특정 얼굴의 연령을 나타내는 [특성](#Attributes) 중 하나입니다. age 특성은 [감지](#Detection-Face-Detection) 결과에서 선택 사항이며, returnFaceAttributes 매개 변수를 지정하여 [감지](#Detection-Face-Detection) 요청으로 제어할 수 있습니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

## <a name="b"></a>b

## <a name="c"></a>C

#### <a name="candidate"></a>Candidate(후보)

Candidate는 기본적으로 [식별](#Identification) 결과(예: 감지에서 식별된 사람 및 신뢰도 수준)입니다. 후보는 [PersonID](#Person-ID) 및 [confidence](#Confidence)로 표시되어 해당 사람이 높은 수준의 신뢰도로 식별되었음을 나타냅니다.

자세한 내용은 [얼굴 - 식별](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) 가이드를 참조하세요.

#### <a name="confidence"></a>신뢰도

Confidence는 [얼굴](#Face) 또는 [Person](#Person)(사람) 사이의 유사성을 숫자 값으로 나타내는 측정값입니다. 이는 검색, 식별 및 확인된 결과의 유사성을 나타내기 위해 [식별](#Identification) 및 [확인](#Verification)에 사용됩니다.

자세한 내용은 [얼굴 - 유사 얼굴 찾기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [얼굴 - 식별](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [얼굴 - 확인](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) 가이드를 참조하세요.

## <a name="d"></a>D

#### <a name="detectionface-detection"></a>Detection(감지)/Face Detection(얼굴 감지)

얼굴 감지는 이미지에서 얼굴을 찾는 작업입니다. 사용자는 요청에 이미지를 업로드하거나 이미지 URL을 지정할 수 있습니다. 감지된 얼굴은 Face API의 고유 ID를 나타내는 [얼굴 ID](#Face-ID)와 함께 반환됩니다. 사각형은 각 얼굴에 대한 선택적 [특성](#Attributes)(예: [연령](#Age-Attribute), [성별](#Gender-Attribute), [머리 자세](#Head-Pose-Attribute), [수염](#Facial-Hair-Attribute) 및 [웃는 얼굴](#Smile-Attribute))뿐만 아니라 이미지의 얼굴 위치를 픽셀 단위로 나타냅니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

## <a name="e"></a>E

## <a name="f"></a>F

#### <a name="face"></a>Face

얼굴은 감지된 얼굴과 관련된 Face API에서 파생된 결과에 대해 통합된 용어입니다. 궁극적으로 얼굴은 통합된 ID([얼굴 ID](#Face-ID)), 이미지에서 지정된 영역([얼굴 사각형](#Face-Rectangle)), 추가 얼굴 관련 [특성](#Face-Attributes-Facial-Attributes)(예: [연령](#Age-Attribute), [성별](#Gender-Attribute), [랜드마크](#Face-Landmarks-Facial-Landmarks) 및 [머리 자세](#Head-Pose-Attribute))으로 표시됩니다. 또한 얼굴은 [감지](#Detection-Face-Detection)에서 반환할 수 있습니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

#### <a name="face-api"></a>Face API

Face API는 얼굴 감지 및 인식에 대한 고급 알고리즘을 제공하는 클라우드 기반 API입니다. Face API의 주요 기능은 얼굴 [감지](#Detection-Face-Detection)([특성](#Face-Attributes-Facial-Attributes) 포함)와 얼굴 [인식](#Recognition)의 두 가지 범주로 나눌 수 있습니다.

자세한 내용은 [Face API 개요](./Overview.md), [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [얼굴 - 유사 얼굴 찾기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [얼굴 - 그룹](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [얼굴 - 식별](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [얼굴 - 확인](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) 가이드를 참조하세요.

#### <a name="face-attributesfacial-attributes"></a>Face Attributes/Facial Attributes(얼굴 특성)

[특성](#Attributes)을 참조하세요.

#### <a name="face-id"></a>Face ID(얼굴 ID)

얼굴 ID는 [감지](#Detection-Face-Detection) 결과에서 파생되며, [Face API](#Face-API)에서 [얼굴](#Face)을 나타내는 문자열입니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

#### <a name="face-landmarksfacial-landmarks"></a>Face Landmarks/Facial Landmarks(얼굴 랜드마크)

랜드마크는 [감지](#Detection-Face-Detection) 결과에서 선택 사항이며, 눈, 코 및 입과 같이 의미 있는 얼굴 지점입니다(다음 그림 참조). 랜드마크는 returnFaceLandmarks 부울 숫자를 사용하여 [감지](#Detection-Face-Detection) 요청으로 제어할 수 있습니다. returnFaceLandmarks가 true로 설정되면 반환된 얼굴에 랜드마크 특성이 있습니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

![HowToDetectFace](./Images/landmarks.1.jpg)

#### <a name="face-rectangle"></a>Face Rectangle(얼굴 사각형)

얼굴 사각형은 [감지](#Detection-Face-Detection) 결과에서 파생되며, 픽셀로 된 이미지의 직립 사각형(왼쪽, 위쪽, 너비, 높이)입니다. 너비와 높이 외에도 [얼굴](#Face)의 왼쪽 위 모서리(왼쪽, 위쪽)는 얼굴 크기를 각각 x 축과 y 축으로 나타냅니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

#### <a name="facial-hair-attribute"></a>Facial Hair(수염) (특성)

수염은 사용 가능한 얼굴의 수염 길이를 설명하는 데 사용되는 [특성](#Attributes) 중 하나입니다. 수염 특성은 [감지](#Detection-Face-Detection) 결과에서 선택 사항이며, returnFaceAttributes를 사용하여 [감지](#Detection-Face-Detection) 요청으로 제어할 수 있습니다. returnFaceAttributes에 'facialHair'가 포함되면 반환된 얼굴에 수염 특성이 있습니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

#### <a name="facelist"></a>FaceList

FaceList는 [PersistedFace](#PersistedFace)의 컬렉션이며 [유사 얼굴 찾기](#Find-Similar)의 단위입니다. FaceList에는 [FaceList ID](#FaceList-ID)뿐만 아니라 [name](#Name) 및 [사용자 데이터](#UserData-User-Data)와 같은 다른 특성도 함께 제공됩니다.

자세한 내용은 [FaceList - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) 가이드를 참조하세요.

#### <a name="facelist-id"></a>FaceList ID

FaceList ID는 [FaceList](#FaceList)의 식별자로 사용되는 사용자 제공 문자열입니다. FaceList ID는 구독 내에서 고유해야 합니다.

자세한 내용은 [FaceList - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) 가이드를 참조하세요.

#### <a name="find-similar"></a>Find Similar(유사 얼굴 찾기)

이 API는 얼굴 컬렉션을 기반으로 하여 유사 얼굴을 검색/쿼리하는 데 사용됩니다. 쿼리 얼굴과 얼굴 컬렉션은 요청에서 [얼굴 ID](#Face-ID) 또는 [FceList ID](#FaceList-ID)/[LargeFaceList ID](#LargeFaceList-ID)로 표시됩니다. 반환된 결과는 검색된 유사 얼굴이며, [얼굴 ID](#Face-ID) 또는 [PersistedFace ID](#PersistedFace-ID)로 표시됩니다.

자세한 내용은 [Face - 유사 얼굴 찾기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [LargeFaceList - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [FaceList - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b) 가이드를 참조하세요.

## <a name="g"></a>G

#### <a name="gender-attribute"></a>Gender(성별) (특성)

gender는 사용 가능한 얼굴의 성별을 설명하는 데 사용되는 [특성](#Attributes) 중 하나입니다. gender 특성은 [감지](#Detection-Face-Detection) 결과에서 선택 사항이며, returnFaceAttributes를 사용하여 [감지](#Detection-Face-Detection) 요청으로 제어할 수 있습니다. returnfaceAttributes에 'gender'가 포함되면 반환된 얼굴에 성별 특성이 있습니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

#### <a name="grouping"></a>Grouping(그룹화)

얼굴 그룹화는 얼굴 유사성에 따라 얼굴 컬렉션을 그룹화한 것입니다. 얼굴 컬렉션은 요청에서 얼굴 ID 컬렉션으로 표시됩니다. 그룹화의 결과로 유사 얼굴이 [그룹](#Groups)으로 그룹화되고, 다른 얼굴과 유사하지 않은 얼굴은 난잡한 그룹으로 함께 병합됩니다. 그룹화 결과에는 고작 하나의 [난잡한 그룹](#Messy-Group)만 있습니다.

자세한 내용은 [얼굴 - 그룹](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) 가이드를 참조하세요.

#### <a name="groups"></a>그룹

그룹은 [그룹화](#Grouping) 결과에서 파생됩니다. 각 그룹에는 얼굴이 [얼굴 ID](#Face-ID)로 표시되는 유사 얼굴의 컬렉션이 포함됩니다.

자세한 내용은 [얼굴 - 그룹](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) 가이드를 참조하세요.

## <a name="h"></a>H

#### <a name="head-pose-attribute"></a>Head Pose(머리 자세) (특성)

머리 자세는 다음 그림과 같이 롤 각, 피치 각 및 요 각에 따라 3D 공간에서 얼굴 방향을 나타내는 [특성](#Attributes) 중 하나입니다. 롤 각 및 요 각의 값 범위는 각각 [-180, 180] 및 [-90, 90]\(도 단위)입니다. 현재 버전에서는 감지로부터 반환되는 피치 값이 항상 0입니다. 머리 자세 특성은 [감지](#Detection-Face-Detection) 결과에서 선택 사항이며, returnFaceAttributes 매개 변수를 사용하여 [감지](#Detection-Face-Detection) 요청으로 제어할 수 있습니다. returnFaceAttributes 매개 변수에 'headPose'가 포함되면 반환된 얼굴에 머리 자세 특성이 있습니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

![GlossaryHeadPose](./Images/headpose.1.jpg)

## <a name="i"></a>I

#### <a name="identification"></a>Identification(식별)

식별은 LargePersonGroup/PersonGroup에서 하나 이상의 얼굴을 식별하는 것입니다.
[PersonGroup](#PersonGroup)/[LargePersonGroup](#LargePersonGroup)은 [Person](#Person)(사람)의 컬렉션입니다.
Face 및 LargePersonGroup/PersonGroup은 요청에서 각각 [얼굴 ID](#Face-ID) 및 [LargePersonGroup ID](#LargePersonGroup-ID)/[PersonGroup ID](#PersonGroup-ID)로 표시됩니다.
식별된 결과는 [후보](#Candidate)이며 confidence(신뢰도)가 있는 [Person](#Person)으로 표시됩니다.
입력의 여러 얼굴이 별도로 간주되며, 각 얼굴에는 고유하게 식별된 결과가 있습니다.

> [!NOTE]
> 식별하기 전에 먼저 LargePersonGroup/PersonGroup이 성공적으로 학습되어야 합니다. LargePersonGroup/PersonGroup이 학습되지 않았거나 학습 [상태](#Status-Train)가 'succeeded'(즉 'running', 'failed' 또는 'timeout')로 표시되지 않는 경우 요청 응답은 400입니다.
> 

자세한 내용은 [얼굴 - 식별](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [LargePersonGroup 사람 - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - 학습](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup 사람 - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - 학습](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) 가이드를 참조하세요.

#### <a name="isidentical"></a>IsIdentical

IsIdentical은 두 얼굴이 같은 사람에 속하는지 여부를 나타내는 [확인](#Verification) 결과의 부울 필드입니다.

자세한 내용은 [얼굴 - 확인](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) 가이드를 참조하세요.

## <a name="j"></a>J

## <a name="k"></a>K

## <a name="l"></a>L

#### <a name="landmarks"></a>랜드마크

[얼굴 랜드마크](#Face-Landmarks-Facial-Landmarks)을 참조하세요.

#### <a name="largefacelist"></a>LargeFaceList

LargeFaceList는 [PersistedFace](#PersistedFace)의 컬렉션이며 [유사 얼굴 찾기](#Find-Similar)의 단위입니다. LargeFaceList에는 [LargeFaceList ID](#LargeFaceList-ID)뿐만 아니라 [name](#Name) 및 [사용자 데이터](#UserData-User-Data)와 같은 다른 특성도 함께 제공됩니다.

자세한 내용은 [LargeFaceList - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce), [LargeFaceList - 얼굴 나열](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158db4d2de3616c086f2d6) 가이드를 참조하세요.

#### <a name="largefacelist-id"></a>LargeFaceList ID

LargeFaceList ID는 [LargeFaceList](#LargeFaceList)의 식별자로 사용되는 사용자 제공 문자열입니다. LargeFaceList ID는 구독 내에서 고유해야 합니다.

자세한 내용은 [LargeFaceList - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce) 가이드를 참조하세요.

#### <a name="largepersongroup"></a>LargePersonGroup

LargePersonGroup은 [Person](#Person)의 컬렉션이며, [식별](#Identification)의 단위입니다. LargePersonGroup에는 [LargePersonGroup ID](#LargePersonGroup-ID)뿐만 아니라 [name](#Name) 및 [사용자 데이터](#UserData-User-Data)와 같은 다른 특성도 함께 제공됩니다.

자세한 내용은 [LargePersonGroup - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [LargePersonGroup 사람- 나열](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adda06ac60f11b48b5aa1) 가이드를 참조하세요.

#### <a name="largepersongroup-id"></a>LargePersonGroup ID

LargePersonGroup ID는 [LargePersonGroup](#LargePersonGroup)의 식별자로 사용되는 사용자 제공 문자열입니다. LargePersonGroup ID는 구독 내에서 고유해야 합니다.

자세한 내용은 [LargePersonGroup - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e) 가이드를 참조하세요.

## <a name="m"></a>M

#### <a name="messy-group"></a>Messy group(난잡한 그룹)

난잡한 그룹은 [그룹화](#Grouping) 결과에서 파생되며, 다른 얼굴과 유사하지 않은 얼굴을 포함합니다. 난잡한 그룹의 각 얼굴은 [얼굴 ID](#Face-ID)로 표시됩니다.

자세한 내용은 [얼굴 - 그룹](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) 가이드를 참조하세요.

## <a name="n"></a>N

#### <a name="name-person"></a>name(이름)(Person)

name은 [Person](#Person)에 대한 사용자에게 친숙한 설명 문자열입니다. 사람의 이름은 [사람 ID](#Person-ID)와 달리 그룹 내에서 중복될 수 있습니다.

자세한 내용은 [LargePersonGroup 사람 - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup 사람 - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup 사람 - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup 사람 - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f) 가이드를 참조하세요.

#### <a name="name-largepersongrouppersongroup"></a>Name(이름) (LargePersonGroup/PersonGroup)

name은 [LargePersonGroup](#LargePersonGroup)/[PersonGroup](#PersonGroup)에 대한 사용자에게 친숙한 설명 문자열입니다. LargePersonGroups/PersonGroups의 이름은 [LargePersonGroup ID](#LargePersonGroup-ID)/[PersonGroup ID](#PersonGroup-ID)와 달리 구독 내에서 중복될 수 있습니다.

자세한 내용은 [LargePersonGroup - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [PersonGroup - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246) 가이드를 참조하세요.

## <a name="o"></a>O

## <a name="p"></a>P

#### <a name="persistedface"></a>PersistedFace

PersistedFace는 Face API의 데이터 구조입니다. PersistedFace는 [PersistedFace ID](#PersistedFace-ID)뿐만 아니라 [name](#Name) 및 [사용자 데이터](#UserData-User-Data)와 같은 다른 특성도 함께 제공됩니다.

자세한 내용은 [LargeFaceList - 얼굴 추가](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList - 얼굴 추가](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup 사람 - 얼굴 추가](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [PersonGroup 사람 - 얼굴 추가](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) 가이드를 참조하세요.

#### <a name="person-id"></a>Person ID

[PersistedFace](#PersistedFace)가 성공적으로 만들어지면 사람 ID가 생성됩니다. [Face API](#Face-API)에서 이 Face를 나타내는 문자열이 만들어집니다.

자세한 내용은 [LargeFaceList - 얼굴 추가](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList - 얼굴 추가](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup 사람 - 얼굴 추가](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [PersonGroup 사람 - 얼굴 추가](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) 가이드를 참조하세요.

#### <a name="person"></a>사람

Person은 Face API에서 관리되는 데이터 구조입니다. Person에는 [사람 ID](#Person-ID)뿐만 아니라 [name](#Name), [PersistedFace](#PersistedFace)의 컬렉션 및 [사용자 데이터](#UserData-User-Data)와 같은 다른 특성도 함께 제공됩니다.

자세한 내용은 [LargePersonGroup 사람 - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup 사람 - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup 사람 - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup 사람 - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f) 가이드를 참조하세요.

#### <a name="person-id"></a>Person ID

[Person](#Person)이 성공적으로 만들어지면 사람 ID가 생성됩니다. [Face API](#Face-API)에서 이 사람을 나타내는 문자열이 만들어집니다.

자세한 내용은 [LargePersonGroup 사람 - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup 사람 - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup 사람 - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup 사람 - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f) 가이드를 참조하세요.

#### <a name="persongroup"></a>PersonGroup

PersonGroup은 [Person](#Person)의 컬렉션이며, [식별](#Identification)의 단위입니다. PersonGroup에는 [PersonGroup ID](#PersonGroup-ID)뿐만 아니라 [name](#Name) 및 [사용자 데이터](#UserData-User-Data)와 같은 다른 특성도 함께 제공됩니다.

자세한 내용은 [PersonGroup - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246), [PersonGroup 사람 - 나열](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) 가이드를 참조하세요.

#### <a name="persongroup-id"></a>PersonGroup ID

PersonGroup ID는 [PersonGroup](#PersonGroup)의 ID로 사용되는 사용자 제공 문자열입니다. 그룹 ID는 구독 내에서 고유해야 합니다.

자세한 내용은 [PersonGroup - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - 가져오기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246) 가이드를 참조하세요.

#### <a name="pose-attribute"></a>Pose(자세)(특성)

[Head Pose(머리 자세)](#Head-Pose-Attribute)를 참조하세요.

## <a name="q"></a>Q

## <a name="r"></a>R

#### <a name="recognition"></a>인식

인식은 [유사 얼굴 찾기](#Find-Similar), [그룹화](#Grouping), [식별](#Identification), [두 얼굴이 같은지 여부 확인](#Verification)과 같이 얼굴 기술에서 인기 있는 응용 분야입니다.

자세한 내용은 [얼굴 - 유사 얼굴 찾기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [얼굴 - 그룹](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [얼굴 - 식별](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [얼굴 - 확인](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) 가이드를 참조하세요.

#### <a name="rectangle-face"></a>사각형(얼굴)

[얼굴 사각형](#Face-Rectangle)을 참조하세요.

## <a name="s"></a>S

#### <a name="smile-attribute"></a>Smile(웃는 얼굴) (특성)

smile은 사용 가능한 얼굴의 미소 표현을 설명하는 데 사용되는 [특성](#Attributes) 중 하나입니다. smile 특성은 [감지](#Detection-Face-Detection) 결과에서 선택 사항이며, returnFaceAttributes를 사용하여 [감지](#Detection-Face-Detection) 요청으로 제어할 수 있습니다. returnFaceAttributes에 'smile'이 포함되면 반환된 얼굴에 웃는 얼굴 특성이 있습니다.

자세한 내용은 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) 가이드를 참조하세요.

#### <a name="similar-face-searching"></a>유사 얼굴 검색

[유사 얼굴 찾기](#Find-Similar)를 참조하세요.

#### <a name="status-train"></a>Status(상태) (학습)

status는 'notstarted', 'running', 'succeeded', 'failed'를 포함하여 [LargeFaceList/LargePersonGroups/PersonGroups 학습](#Train) 절차를 설명하는 문자열입니다.

자세한 내용은 [LargeFaceList - 학습](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup - 학습](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup - 학습](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) 가이드를 참조하세요.

#### <a name="subscription-key"></a>구독 키

구독 키는 Face API를 호출하기 위해 쿼리 문자열 매개 변수로 지정해야 하는 문자열입니다. 구독 키는 Microsoft Cognitive Services 포털에 로그인하면 [내 구독] 페이지에서 찾을 수 있습니다. 각 구독과 관련된 두 개의 키, 즉 기본 키와 보조 키가 있습니다. 두 키는 모두 동일한 방식으로 API를 호출하는 데 사용할 수 있습니다. 구독 키는 안전하게 유지해야 하며, [내 구독] 페이지에서도 언제든지 다시 생성할 수 있습니다.

## <a name="t"></a>T

#### <a name="train-largefacelistlargepersongrouppersongroup"></a>Train(학습) (LargeFaceList/LargePersonGroup/PersonGroup)

이 API는 [유사 얼굴 찾기](#Find-Similar)/[식별](#Identification) 성능을 보장하기 위해 [LargeFaceList](#LargeFaceList)/[LargePersonGroup](#LargePersonGroup)/[PersonGroup](#PersonGroup)을 미리 처리하는 데 사용됩니다. 학습이 작동되지 않거나 [학습 상태](#Status-Train)가 succeeded(성공)로 표시되지 않으면 이 PersonGroup에 대한 식별이 실패하게 됩니다.

자세한 내용은 [LargeFaceList - 학습](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup - 학습](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup - 학습](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249), [얼굴 - 식별](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) 가이드를 참조하세요.

## <a name="u"></a>U

#### <a name="userdatauser-data"></a>UserData/User Data(사용자 데이터)

사용자 데이터는 [Person](#Person) 및 [PersonGroup](#PersonGroup)/[LargePersonGroup](#LargePersonGroup)과 관련된 추가 정보입니다. 사용자는 데이터를 더 쉽게 사용하고, 파악하고, 기억할 수 있도록 사용자 데이터를 설정합니다.

자세한 내용은 [LargePersonGroup - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - 업데이트](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acfc83a7b9412a4d53f3f), [LargePersonGroup 사람 - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup 사람 - 업데이트](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ade043a7b9412a4d53f41), [PersonGroup - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - 업데이트](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524a), [PersonGroup 사람 - 만들기](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup 사람 - 업데이트](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395242) 가이드를 참조하세요.

## <a name="v"></a>V

#### <a name="verification"></a>확인

이 API는 두 얼굴이 같은지 여부를 확인하는 데 사용됩니다. 두 얼굴은 요청에서 얼굴 ID로 표시됩니다. 확인된 결과에는 true인 경우 동일한 것임을 나타내는 부울 필드([isIdentical](#Is-Identical))와 신뢰도 수준을 나타내는 숫자 필드([confidence](#Confidence))가 포함됩니다.

자세한 내용은 [얼굴 - 확인](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) 가이드를 참조하세요.

## <a name="w"></a>W

## <a name="x"></a>X

## <a name="y"></a>Y

## <a name="z"></a>Z
