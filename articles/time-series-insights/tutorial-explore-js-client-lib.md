---
title: Azure Time Series Insights JavaScript 클라이언트 라이브러리 살펴보기 | Microsoft Docs
description: Azure Time Series Insights JavaScript 클라이언트 라이브러리 및 관련 프로그래밍 모델에 대해 알아봅니다.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 06/05/2018
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 33dcf6f69d1287b4e040b3cccf4164667db2b75f
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269977"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>자습서: Azure Time Series Insights JavaScript 클라이언트 라이브러리

웹 개발자가 TSI(Time Series Insights)에 저장된 데이터를 쿼리하고 시각화하도록 돕기 위해 JavaScript D3 기반 TSI 클라이언트 라이브러리가 개발되었습니다.  이 자습서에서는 샘플 웹 애플리케이션을 사용하여 TSI 클라이언트 라이브러리 및 관련 프로그래밍 모델을 탐구하도록 안내합니다.

이 자습서의 주제는 라이브러리로 실험하고, TSI 데이터에 액세스하고 차트 컨트롤을 사용하여 데이터를 렌더링 및 시각화하는 방법을 이해할 수 있는 기회를 제공하는 것입니다. 목표는 자기 자신의 웹 애플리케이션에 라이브러리를 사용할 수 있을 만큼 충분한 세부 정보를 제공하는 것입니다.

이 자습서에서는 다음에 대해 알아봅니다.

> [!div class="checklist"]
> * TSI 샘플 애플리케이션.
> * TSI JavaScript 클라이언트 라이브러리.
> * 샘플 애플리케이션이 라이브러리를 사용하여 TSI 데이터를 시각화하는 방법.

## <a name="video"></a>비디오: 

### <a name="in-this-video-we-introduce-the-open-source-time-series-insights-javascript-sdkbr"></a>이 비디오에서는 오픈 소스 Time Series Insights JavaScript SDK를 소개합니다.</br>

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="prerequisites"></a>필수 조건

이 자습서에서는 [Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [FireFox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/) 등 대부분의 최신 웹 브라우저에서 볼 수 있는 "개발자 도구" 기능(일명 DevTools 또는 F12)을 사용합니다. 아직 이 기능에 익숙하지 않은 경우 계속 진행하기 전에 브라우저에서 이 기능을 탐구하는 것이 좋습니다.

## <a name="time-series-insights-sample-application"></a>Time Series Insights 샘플 애플리케이션

이 자습서 전체에서 Time Series Insights 샘플 애플리케이션을 사용하여 TSI JavaScript 클라이언트 라이브러리의 사용을 포함하여 애플리케이션 이면의 소스 코드를 탐구합니다. 이 샘플은 라이브러리를 사용하는 방법을 보여주는 SPA(단일 페이지 웹 애플리케이션)입니다. 이 샘플은 샘플 TSI 환경에서 데이터를 쿼리하고 시각화하는 방법을 보여줍니다.

1. [Time Series Insights 샘플 응용 프로그램](https://insights.timeseries.azure.com/clientsample)으로 이동합니다. 로그인하라는 메시지가 표시된 다음과 같은 이미지와 유사한 페이지가 표시됩니다.

   ![TSI 클라이언트 샘플 로그인 메시지](media/tutorial-explore-js-client-lib/tcs-sign-in.png)

2. **로그인**을 선택하고 자격 증명을 입력하거나 선택합니다. 엔터프라이즈/조직 계정(Azure Active Directory) 또는 개인 계정(Microsoft 계정 또는 MSA)을 사용할 수 있습니다.

   ![TSI 클라이언트 샘플 자격 증명 프롬프트](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)

3. 로그인에 성공하면 다음과 같은 이미지와 유사한 페이지가 표시됩니다. 이 페이지에는 TSI 데이터로 채워진 예제 차트의 여러 스타일이 표시됩니다. 사용자 계정 및 **로그아웃** 옵션이 오른쪽 위 모서리에 표시됩니다.

   ![로그인 후 TSI 클라이언트 샘플 기본 페이지](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)

### <a name="page-source-and-structure"></a>페이지 원본 및 구조

먼저 브라우저에 렌더링한 페이지 이면의 HTML 및 JavaScript 소스 코드를 보겠습니다. 모든 요소를 연습하지는 않고 주요 섹션에 대해 알아보면서 페이지가 작동하는 방법에 대한 감각을 제공합니다.

1. 브라우저에서 **개발자 도구**를 엽니다. 현재 페이지를 구성하는 HTML 요소, 일명 HTML 또는 DOM 트리를 검사합니다.

2. `<head>` 및 `<body>` 요소를 확장하고 다음 섹션을 살펴봅니다.

   - `<head>` 요소 아래에서 페이지 기능에 도움이 되는 추가 파일을 끌어오는 요소가 있습니다.
     - Azure Active Directory 인증 라이브러리 **adal.min.js**(일명 ADAL)를 참조하는 데 사용되는 `<script>` 요소. ADAL은 OAuth 2.0 인증(로그인)과 API 액세스를 위한 토큰 획득을 지원하는 JavaScript 라이브러리입니다.
     - **sampleStyles.css** 및 **tsiclient.css** 같은 스타일 시트(일명 CSS)를 위한 여러 `<link>` 요소. 스타일 시트는 색, 글꼴, 간격 등의 시각적 페이지 스타일링 세부 정보를 제어하는 데 사용됩니다.
     - TSI Client JavaScript 라이브러리 **tsiclient.js**를 참조하는 데 사용되는 `<script>` 요소입니다. 이 라이브러리는 페이지에서 TSI 서비스 API를 호출하고 페이지에 차트 컨트롤을 렌더링하는 데 사용합니다.

     >[!NOTE]
     > ADAL JavaScript 라이브러리에 대한 소스 코드는 [azure-activedirectory-library-for-js repository](https://github.com/AzureAD/azure-activedirectory-library-for-js)에서 사용할 수 있습니다.
     > TSI 클라이언트 JavaScript 라이브러리에 대한 소스 코드는 [tsiclient 리포지토리](https://github.com/Microsoft/tsiclient)에서 사용할 수 있습니다.

   - `<body>` 요소 아래에 페이지 항목 및 다른 `<script>` 요소의 레이아웃을 정의하는 컨테이너 역할을 하는 `<div>` 요소가 있습니다.
     - 첫 번째 `<div>` 요소는 **로그인** 대화 상자(`id="loginModal"`)를 지정합니다.
     - 두 번째 `<div>` 요소는 다음에 대한 상위 요소 역할을 합니다.
       - 페이지 맨 위 부근의 상태 메시지 및 로그인 정보(`class="header"`)를 위해 사용되는 헤더 `<div>` 요소.
       - 모든 차트(`class="chartsWrapper"`)를 포함하여 페이지 본문 요소의 나머지 부분에 대한 `<div>` 요소.
       - 페이지를 제어하는 데 사용되는 모든 JavaScript를 포함하는 `<script>` 섹션.

   [![개발자 도구를 포함한 TSI 클라이언트 샘플](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

3. `<div class="chartsWrapper">` 요소를 확장하면 더 많은 하위 `<div>` 요소를 찾을 수 있습니다. 이러한 요소는 각 차트 컨트롤 예제의 위치를 지정하는 데 사용됩니다. 각 차트 예제마다 하나씩 `<div>` 요소 여러 쌍이 있는 것을 확인할 수 있습니다.

   - 첫 번째(`class="rowOfCardsTitle"`) 요소는 차트가 보여 주는 내용을 요약하는 설명 텍스트를 포함합니다. 예:  "전체 크기 범례가 있는 고정 꺾은선형 차트"
   - 두 번째(`class="rowOfCards"`) 요소는 실제 차트 컨트롤을 한 줄 안에 배치하는 추가 하위 `<div>` 요소를 포함한 상위 요소입니다.

   ![본문 div 요소](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)

4. 이제 `<div class="chartsWrapper">` 요소 바로 아래의 `<script type="text/javascript">` 요소를 확장합니다. 인증, TSI 서비스 API 호출, 차트 컨트롤 렌더링 등과 같은 모든 페이지 로직을 처리하는 데 사용하는 페이지 수준 JavaScript 섹션의 시작 부분을 확인하세요.

   ![본문 스크립트](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)

## <a name="tsi-javascript-client-library-concepts"></a>TSI JavaScript 클라이언트 라이브러리 개념

여기서는 자세히 검토하지 않지만, 기본적으로 TSI 라이브러리**tsclient.js**는 두 가지 중요한 범주에 대한 추상화를 제공합니다.

- **TSI 쿼리 API를 호출하는 래퍼 메서드**: 집계 식을 사용하여 TSI 데이터를 쿼리할 수 있게 하는 REST API입니다. 메서드는 라이브러리의 `TsiClient.Server` 네임스페이스 아래에 구성됩니다.
- **여러 유형의 차트 작성 컨트롤을 만들고 채우는 메서드**: 웹 페이지에서 TSI 집계 데이터를 렌더링하는 데 사용되는 메서드입니다. 메서드는 라이브러리의 `TsiClient.UX` 네임스페이스 아래에 구성됩니다.

다음 개념은 다목적이며 TSI 클라이언트 라이브러리 API에 일반적으로 적용할 수 있습니다.

### <a name="authentication"></a>인증

앞에서 언급했듯이 이 샘플은 사용자 인증을 위해 ADAL의 OAuth 2.0 지원을 사용하는 SPA입니다. 다음은 스크립트의 이 섹션에서 몇 가지 관심 지점입니다.

1. 인증에 ADAL을 사용할 때는 클라이언트 애플리케이션이 Azure AD(Azure Active Directory) 애플리케이션 레지스트리에 자체를 등록해야 합니다. SPA와 마찬가지로 이 애플리케이션은 "묵시적" OAuth 2.0 권한 부여 흐름을 사용하도록 등록됩니다. 따라서 애플리케이션은 이 흐름에 참여하기 위해 클라이언트 ID GUID(`clientId`) 같은 몇몇 등록 속성을 런타임에 지정하고 URI(`postLogoutRedirectUri`)를 리디렉션합니다.

2. 나중에 애플리케이션은 Azure AD에서 "액세스 토큰"을 요청합니다. 액세스 토큰은 특정 서비스/API 식별자( https://api.timeseries.azure.com )에 대해 유한한 권한 집합을 할당하기 위해 발급됩니다. 서비스/API 식별자를 토큰 "대상"이라고도 합니다. 토큰 권한은 로그인한 사용자를 대신하여 발급됩니다. 서비스/API의 식별자는 역시 애플리케이션의 Azure AD 등록에 포함된 또 다른 속성입니다. ADAL은 애플리케이션에 대한 액세스 토큰을 반환한 후 TSI 서비스 API에 액세스할 때 "전달자 토큰"으로 전달됩니다.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=145-204&highlight=4-9,36-39)]

### <a name="control-identification"></a>컨트롤 식별

앞에서 설명했듯이, `<body>` 요소 내의 `<div>` 요소는 페이지에 표시된 모든 차트 컨트롤에 대한 레이아웃을 제공합니다. 각 `<div>` 요소는 배치 속성 및 `id` 속성을 포함한 차트 컨트롤의 시각적 특성을 지정합니다. `id` 속성은 렌더링 및 업데이트할 각 컨트롤을 식별하고 바인딩하기 위해 JavaScript 코드에 사용하는 고유한 식별자를 제공합니다.

### <a name="aggregate-expressions"></a>집계 식

TSI 클라이언트 라이브러리 API는 집계 식을 많이 사용합니다. 집계 식은 "검색 용어"를 하나 이상 생성하는 기능을 제공합니다. API는 [Time Series Insights 탐색기](https://insights.timeseries.azure.com/demo)가 검색 범위, where 조건자, 측정값 및 분할 값을 사용하는 방법과 유사하게 설계되었습니다. 대부분의 라이브러리 API는 서비스가 TSI 데이터 쿼리를 만들기 위해 사용하는 집계 식의 배열을 사용합니다.

### <a name="call-pattern"></a>호출 패턴

차트 컨트롤 채우기 및 렌더링은 일반적인 패턴을 따릅니다. 이 패턴을 TSI 샘플 애플리케이션 컨트롤을 인스턴스화하고 로드하는 페이지 JavaScript 전체에 사용하는 것을 확인할 수 있을 것입니다.

1. TSI 집계 식을 하나 이상 저장하기 위한 `array`를 선언합니다.

   ```javascript
   var aes =  [];
   ```

2. 집계 식 개체를 1개에서 n개까지 만든 다음, 집계 식 배열에 추가합니다.

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```

   **집계 식 매개 변수**

   | 매개 변수 | 설명 | 예 |
   | --------- | ----------- | ------- |
   | `predicateObject` | 데이터 필터링 식입니다. |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | 사용하는 측정값의 속성 이름입니다. | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | 측정값 속성의 원하는 집계입니다. | `['avg', 'min']` |
   | `searchSpan`      | 집계 식의 지속 시간 및 간격 크기입니다. | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | 분할하려는 문자열 속성(선택 사항 - null일 수 있음)입니다. | `{property: 'Station', type: 'String'}` |
   | `color`         | 렌더링하려는 개체의 색입니다. | `'pink'` |
   | `alias`           | 집계 식의 식별 이름입니다. | `'Factory3Temperature'` |
   | `contextMenuActions` | 시각화에서 시간 시리즈 개체에 바인딩할 작업의 배열입니다(선택 사항). | 자세한 내용은 [고급 기능 섹션의 팝업 바로 가기 메뉴](#popup-context-menus)를 참조하세요. |

3. `TsiClient.Server` API를 사용하여 집계 데이터를 요청하는 TSI 쿼리를 호출합니다.

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates 매개 변수**

   | 매개 변수 | 설명 | 예 |
   | --------- | ----------- | ------- |
   | `token`     | TSI API에 대한 액세스 토큰입니다. |  `authContext.getTsiToken()`자세한 내용은 [인증 섹션](#authentication)을 참조하세요. |
   | `envFQDN`   | TSI 환경의 FQDN(정규화된 도메인 이름). | Azure Portal에서, 예: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | TSI 쿼리 식의 배열. | 앞에서 설명한 대로 `aes` 변수를 사용합니다. `aes.map(function(ae){return ae.toTsx()}`. |

4. TSI 쿼리에서 반환된 압축된 결과를 시각화하기 위해 JSON으로 변환합니다.

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

5. `TsiClient.UX` API를 사용하여 차트 컨트롤을 만들고 이 컨트롤을 페이지의 `<div>` 요소 중 하나에 바인딩합니다.

   ```javascript
   var lineChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

6. 차트 컨트롤을 변환된 JSON 개체로 채우고 페이지에 컨트롤을 렌더링합니다.

   ```javascript
   lineChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>컨트롤 렌더링

TSI 클라이언트 라이브러리는 현재 꺾은선형 차트, 원형 차트, 가로 막대형 차트, 열 지도, 계층 구조 컨트롤, 액세스 가능 그리드, 불연속 이벤트 시간 표시줄 및 상태 전환 시간 표시줄과 같은 8개의 고유한 분석 컨트롤을 표시합니다.

### <a name="line-bar-pie-chart-examples"></a>꺽은선형, 가로 막대형, 원형 차트 예제

애플리케이션에서 보여 준 몇몇 표준 차트 컨트롤 이면의 코드 및 해당 컨트롤을 만들기 위한 프로그래밍 모델/패턴을 살펴보겠습니다. 구체적으로, ID 값 `chart3`, `chart4` 및 `chart5`를 사용하여 컨트롤을 렌더링하는 `// Example 3/4/5` 주석 아래의 HTML 섹션을 검사합니다.

[페이지 원본 및 구조 섹션](#page-source-and-structure)의 단계 #3부터 재현하여 차트 컨트롤을 각각 설명 제목 행이 있는 페이지의 행에 배열합니다. 이 예제에서는 "동일한 데이터의 여러 차트 형식" 제목 `<div>` 요소 아래에 3개 차트가 채워지고, 제목 아래의 `<div>` 요소 3개에 바인딩됩니다.

[!code-javascript[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

다음과 같은 JavaScript 코드 섹션은 앞에서 요약한 패턴을 사용하여 TSI 집계 식을 만들고 이 식을 사용하여 TSI 데이터를 쿼리하고 차트 3개를 렌더링합니다. `tsiClient.ux` 네임스페이스 `LineChart`, `BarChart`, `PieChart`에서 각 차트를 만들고 렌더링하기 위해 사용한 형식 3가지에 주목하세요. 또한 차트 3개 모두 동일한 집계 식 데이터 `transformedResult`를 사용할 수 있다는 점에도 주목하세요.

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

차트 3개는 렌더링할 때 다음과 같이 나타납니다.

[![동일한 데이터의 여러 차트 형식](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>고급 기능

또한 TSI 클라이언트 라이브러리는 이용할 만한 선택적 고급 기능 몇 가지도 표시합니다.

### <a name="states-and-events"></a>상태 및 이벤트

제공된 고급 기능의 한 예는 상태 전환 및 불연속 이벤트를 차트에 추가하는 기능입니다. 이 기능은 인시던트, 경고 및 켜기/끄기 같은 상태 스위치를 강조 표시하는 데 유용합니다.

`// Example 10` 주석 아래에 있는 HTML 섹션 이면의 코드를 살펴보세요. 이 코드는 선 컨트롤을 "여러 계열 형식을 가진 꺾은선형 차트" 제목 아래에 렌더링하고 ID 값이 `chart10`인 `<div>` 요소에 바인딩합니다.

1. 먼저 `events4`라는 구조체를 정의하여 추적할 상태 변경 요소를 저장합니다. 구조에는 다음과 같은 요소가 포함되어 있습니다.

   - `Component States`라는 문자열 키.
   - 상태를 나타내는 값 개체의 배열. 각 개체에는 다음이 포함됩니다.
     - JavaScript ISO 타임스탬프를 포함한 문자열 키.
     - 상태의 특성, 즉 색상 및 설명을 포함하는 배열.

2. 그런 다음, "Incidents"에 대해 추적할 이벤트 요소의 배열을 저장하는 `events5` 구조체를 정의합니다. 배열 구조체는 `events4`에 대해 요약한 구조체와 같은 도형입니다.

3. 끝으로 다음 차트 옵션 매개 변수와 함께 두 구조체를 전달하여 꺾은선형 차트를 렌더링합니다. `events:` 및 `states:`. `tooltip:`, `theme:` 또는 `grid:`를 지정하기 위한 다른 옵션 매개 변수에 주목하세요.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

시각적으로, 다이아몬드 마커/팝업을 사용하여 인시던트를 나타내며, 시간 단위를 따라 색으로 표시한 막대/팝업이 상태 변화를 나타냅니다.

[![여러 계열 형식을 가진 꺾은선형 차트](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>팝업 바로 가기 메뉴

고급 기능의 또 다른 예로 사용자 지정 바로 가기 메뉴(오른쪽 클릭 팝업 메뉴)가 있습니다. 사용자 지정 바로 가기 메뉴는 애플리케이션의 범위 내에서 작업 및 논리적 다음 단계가 논리를 활성화하는 데 유용합니다.

`// Example 13/14/15` 주석 아래에 있는 HTML 섹션 이면의 코드를 살펴보세요. 이 코드는 처음에 "원형/가로 막대형 차트를 만들기 위한 바로 가기 메뉴를 포함한 꺾은선형 차트" 제목 아래에 꺾은선형 차트를 렌더링하고 ID 값이 `chart13`인 `<div>` 요소에 바인딩합니다. 바로 가기 메뉴를 사용할 경우, 꺾은선형 차트는 ID가 `chart14` 및 `chart15`인 `<div>` 요소에 바인딩된 원형 및 가로 막대형 차트를 동적으로 만드는 기능을 제공합니다. 또한 원형 차트와 가로 막대형 차트 모두 바로 가기 메뉴를 사용하여 자체의 기능, 즉 각각 원형 차트의 데이터를 가로 막대형 차트에 복사하는 기능과 가로 막대형 차트 데이터를 브라우저 콘솔 창에 인쇄하는 기능을 활성화합니다.

1. 먼저 일련의 사용자 지정 작업을 정의합니다. 하나 이상의 요소가 있는 배열을 포함하는 각 작업. 각 요소는 단일 바로 가기 메뉴 항목을 정의합니다.

   - `barChartActions`: 이 작업은 원형 차트에 대한 바로 가기 메뉴를 정의하며, 이 메뉴에는 단일 항목을 정의하는 하나의 요소가 포함됩니다.
     - `name`: 메뉴 항목에 사용되는 텍스트, 즉 "콘솔에 매개 변수 출력"입니다.
     - `action`: 메뉴 항목과 연결된 작업입니다. 이 작업은 언제나 차트를 만드는 데 사용한 집계 식을 기반으로 인수 3개를 취하는 익명 함수입니다. 이 경우 인수를 브라우저 콘솔 창에 씁니다.
       - `ae`: 집계 식 배열입니다.
       - `splitBy`: splitBy 값입니다.
       - `timestamp`: 타임스탬프입니다.

   - `pieChartActions`: 이 작업은 가로 막대형 차트에 대한 바로 가기 메뉴를 정의하며, 이 메뉴에는 단일 항목을 정의하는 하나의 요소가 포함됩니다. 도형 및 스키마는 이전의 `barChartActions` 요소와 같지만 `action` 함수는 가로 막대형 차트를 인스턴스화하고 렌더링하므로 크게 다르다는 데 주목하세요. 또한 `ae` 인수를 사용하여 런타임에 메뉴 항목이 열릴 때 전달된 집계 식 배열을 지정한다는 데 주목하세요. 또한 이 함수는 `barChartActions` 바로 가기 메뉴를 사용하여 `ae.contextMenu` 속성을 설정합니다.
   - `contextMenuActions`: 이 작업은 꺾은선형 차트에 대한 바로 가기 메뉴를 정의하며, 이 메뉴에는 세 개의 메뉴 항목을 정의하는 세 개의 요소가 포함됩니다. 각 요소에 대한 도형과 스키마는 이전에 설명한 요소와 같습니다. `barChartActions` 요소와 마찬가지로, 첫 번째 항목은 함수 인수 3개를 브라우저 콘솔 창에 씁니다. `pieChartActions` 요소와 유사하게, 두 번째 항목은 각각 원형 및 가로 막대형 차트를 인스턴스화 및 렌더링합니다. 두 번째 항목 두 개도 각각 `pieChartActions` 및 `barChartActions` 바로 가기 메뉴를 사용하여 해당 `ae.contextMenu` 속성을 설정합니다.

2. 그런 다음, 집계 식 두 개를 `aes` 집계 식 배열에 푸시하여 각 항목에 대한 `contextMenuActions` 배열을 지정합니다. 이 식은 꺾은선형 차트 컨트롤과 함께 사용됩니다.

3. 끝으로 꺾은선형 차트만 처음에 렌더링되며, 그 때부터 런타임에 원형 차트와 가로 막대형 차트를 모두 렌더링할 수 있습니다.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

스크린샷은 차트를 해당 팝업 바로 가기 메뉴와 함께 표시합니다. 원형 및 가로 막대형 차트는 꺾은선형 차트 바로 가기 메뉴 옵션을 사용하여 동적으로 만들어졌습니다.

[![원형/가로 막대형 차트를 만드는 바로 가기 메뉴를 포함한 꺾은선형 차트](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>브러시

브러시를 사용하여 확대/축소 및 탐색 같은 작업을 정의하는 시간 범위를 설정할 수 있습니다.

브러시를 보여 주기 위해 사용하는 코드는 [팝업 바로 가기 메뉴](#popup-context-menus-section)를 다루는 이전의 "원형/가로 막대형 차트를 만드는 바로 가기 메뉴를 설명하는 꺾은선형 차트"에 표시됩니다.

1. 브러시 작업은 바로 가기 메뉴와 유사하게 브러시에 대한 일련의 사용자 지정 작업을 정의합니다. 하나 이상의 요소가 있는 배열을 포함하는 각 작업. 각 요소는 단일 바로 가기 메뉴 항목을 정의합니다.
   - `name`: 메뉴 항목에 사용되는 텍스트, 즉 "콘솔에 매개 변수 출력"입니다.
   - `action`: 메뉴 항목과 연결된 작업이며, 항상 두 개의 인수를 사용하는 익명 함수입니다. 이 경우 인수를 브라우저 콘솔 창에 씁니다.
      - `fromTime`: 브러시 선택 영역의 "from"(시작) 타임스탬프입니다.
      - `toTime`: 브러시 선택 영역의 "to"(종료) 타임스탬프입니다.

2. 브러시 작업은 또 다른 차트 옵션 속성으로 추가됩니다. `brushContextMenuActions: brushActions` 속성이 `linechart.Render` 호출에 전달된다는 데 주목하세요.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

![브러시를 사용하여 원형/가로 막대형 차트를 만드는 바로 가기 메뉴를 포함한 꺾은선형 차트](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * TSI 샘플 애플리케이션에 로그인하고 해당 소스 탐색.
> * TSI JavaScript 클라이언트 라이브러리에 API 사용.
> * JavaScript를 사용하여 차트 컨트롤을 만들고 TSI 데이터로 채우기.

앞에서 설명했듯이, TSI 샘플 애플리케이션은 데모 데이터 집합을 사용합니다. 자기 자신의 TSI 환경 및 데이터 집합을 만들 수 있는 방법을 알아보려면 다음 문서로 계속 진행하세요.

> [!div class="nextstepaction"]
> [자습서: Azure Time Series Insights 환경 만들기](tutorial-create-populate-tsi-environment.md)


