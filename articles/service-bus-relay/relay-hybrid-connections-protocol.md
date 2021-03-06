---
title: Azure 릴레이 하이브리드 연결 프로토콜 가이드 | Microsoft Docs
description: Azure 릴레이 하이브리드 연결 프로토콜 가이드입니다.
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: ''
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2018
ms.author: clemensv
ms.openlocfilehash: 913e702cc72472e81937bfe3b0939695daadc011
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543530"
---
# <a name="azure-relay-hybrid-connections-protocol"></a>Azure 릴레이 하이브리드 연결 프로토콜

Azure 릴레이는 Azure Service Bus 플랫폼의 주요 기능 요소 중 하나입니다. 릴레이의 새로운 _하이브리드 연결_ 기능은 HTTP 및 WebSockets를 기반으로 하는 안전한 개방형 프로토콜의 진화된 형태입니다. 이는 소유 프로토콜을 기반으로 빌드된, 동일한 이름의 이전 _BizTalk Services_ 기능을 대체합니다. 하이브리드 연결을 Azure App Services에 통합하여 그 기능을 계속 수행합니다.

하이브리드 연결을 사용하면 네트워크로 연결된 두 응용 프로그램 사이에 양방향 이진 스트림 통신 및 간단한 데이터그램 흐름이 가능합니다. NAT 또는 방화벽 뒤에 둘 중 하나 또는 두 파티 모두 상주할 수 있습니다.

이 문서에서는 수신기 또는 발신기 역할의 클라이언트와 연결하기 위해 Hybrid 연결 릴레이를 통한 클라이언트 측 상호 작용을 설명합니다. 또한 수신기가 새로운 연결과 요청을 수락하는 방식을 설명합니다.

## <a name="interaction-model"></a>상호 작용 모델

하이브리드 연결 릴레이는 두 파티가 자체 네트워크의 관점에서 검색하고 연결할 수 있는 Azure 클라우드에 랑데부 지점을 제공하여 두 파티를 연결합니다. 해당 랑데부 지점은 이 설명서와 다른 설명서에서 그리고 API 및 Azure Portal에서 "하이브리드 연결"이라고 합니다. 이 문서의 나머지 부분에서는 하이브리드 연결 서비스 엔드포인트를 "서비스"라고 부릅니다.

이 서비스에서는 웹 소켓 연결 및 HTTP(S) 요청과 응답을 릴레이할 수 있습니다.

상호 작용 모델은 다른 많은 네트워킹 API에 의해 정해진 용어 체계를 사용합니다. 먼저 준비 상태를 표시하여 들어오는 연결을 처리하고 이후 도착하는 대로 연결을 수락하는 수신기가 있습니다. 다른 쪽에는 양방향 통신 경로를 설정하기 위해 연결이 수락되기를 기다리는 수신기에 대한 클라이언트 연결이 있습니다. "연결", "수신", "수락"은 대부분의 소켓 API에서 사용되는 것과 동일한 용어입니다.

릴레이된 통신 모델에는 서비스 엔드포인트로 아웃바운드 연결을 생성하는 파티가 있습니다. 이로 인해 “수신기” 역시 통상적으로 “클라이언트”가 되며 다른 용어 오버로드가 발생할 수도 있습니다. 따라서 하이브리드 연결에 사용되는 정확한 용어는 다음과 같습니다.

연결 양쪽의 프로그램은 서비스에 대한 클라이언트이기 때문에 "클라이언트"라고 합니다. 연결을 기다렸다가 수락하는 클라이언트는 "수신기"이며 또는 "수신기 역할"에 있다고 합니다. 서비스를 통해 수신기에 대한 새로운 연결을 시작하는 클라이언트는 "발신자" 또는 "발신자 역할"에 있다고 합니다.

### <a name="listener-interactions"></a>수신기 상호 작용

수신기에는 서비스와의 다섯 가지 상호 작용이 있으며, 모든 연결에 대한 세부 정보는 이 문서의 후반부 참조 섹션에 설명되어 있습니다.

수신 대기, 수락 및 요청 메시지는 서비스에서 수신됩니다. 갱신 및 Ping 작업은 수신기가 보냅니다.

#### <a name="listen-message"></a>메시지 수신 대기

수신기가 연결을 수락할 준비가 되었음을 나타내기 위해 아웃바운드 WebSocket 연결을 만듭니다. 연결 핸드셰이크에는 릴레이 네임스페이스에서 구성된 하이브리드 연결 이름과 그 이름에 "수신" 권한을 부여하는 보안 토큰이 있습니다.

서비스가 WebSocket을 수락하면 등록이 완료되고 설정된 WebSocket은 모든 후속 상호 작용을 사용하도록 "컨트롤 채널"로서 활성 상태가 유지됩니다. 서비스는 하나의 하이브리드 연결에 최대 25개의 동시 수신기를 허용합니다. AppHook에 대한 할당량은 결정이 필요합니다.

하이브리드 연결에 대해 활성 수신기가 둘 이상 있으면 들어오는 연결은 활성 수신기에서 무작위로 균형을 맞추며 최선의 노력으로 균등 배포가 시도됩니다.

#### <a name="accept-message"></a>메시지 수락

발신자가 서비스에 대한 새로운 연결을 열면 서비스는 하이브리드 연결에 대한 활성 수신기 중 하나를 선택하여 알립니다. 이 알림은 열린 컨트롤 채널을 통해 JSON 메시지로 수신기에 보내집니다. 메시지에는 연결을 수락하기 위해 수신기가 연결해야 하는 WebSocket 엔드포인트의 URL이 포함됩니다.

URL은 추가 작업 없이 수신기에서 바로 사용할 수 있고 사용해야 합니다.
인코딩된 정보는 짧은 시간 동안만 유효하며 종단간 연결이 설정될 때가지 특히 발신자가 대기할 수 있는 시간 동안 유효합니다. 가정할 수 있는 최댓값은 30초입니다. URL은 한 번의 성공적인 연결 시도 대해서만 사용할 수 있습니다. 랑데부 URL을 통한 WebSocket 연결이 설정되자 마자 WebSocket에 대한 모든 이후 작업은 발신자와의 사이에서 릴레이됩니다. 이 때 서비스의 개입이나 해석은 발생하지 않습니다.

### <a name="request-message"></a>메시지 요청

WebSocket 연결 외에도 해당 기능이 하이브리드 연결에서 명시적으로 활성화되면 수신기는 발신자로부터 HTTP 요청 프레임을 수신할 수 있습니다.

HTTP를 지원하는 하이브리드 연결에 속하는 수신기는 `request` 제스처를 반드시 처리해야 합니다. 수신기가 `request`를 처리하지 않아서 연결되는 동안 시간 제한 오류가 반복되면 향후 서비스의 차단 목록에 포함될 수 있습니다.

HTTP 프레임 헤더 메타데이터는 수신기 프레임워크에서 간단히 처리할 수 있도록 JSON으로 변환되며, 이는 HTTP 헤더 구문 분석 라이브러리가 JSON 파서보다 드물기 때문이기도 합니다. 발신자와 릴레이 HTTP 게이트웨이(권한 부여 정보 포함) 사이의 관계에만 관련된 HTTP 메타데이터는 전달되지 않습니다. HTTP 요청 본문은 이진 WebSocket 프레임으로 투명하게 전송됩니다.

수신기는 해당하는 응답 제스처를 사용하여 HTTP 요청에 응답할 수 있습니다.

요청/응답 흐름에는 기본적으로 제어 채널이 사용되지만 언제든 필요하면 별도의 랑데부 WebSocket으로 “업그레이드”될 수 있습니다. 별도의 WebSocket 연결은 각 클라이언트 대화의 처리량을 향상시키지만 처리해야 하는 연결이 많아져서 수신기에 부담이 되기 때문에 경량 클라이언트에는 바람직하지 않을 수 있습니다.

제어 채널에서 응답 및 요청 본문의 크기는 최대 64KB로 제한됩니다. HTTP 헤더 메타데이터는 총 32KB로 제한됩니다. 요청 또는 응답 중 하나가 임계값을 초과하면 수신기는 [수락](#accept-message) 처리에 해당하는 제스처를 사용하여 랑데부 WebSocket으로 업그레이드되어야 합니다.

요청에 대해 서비스는 제어 채널을 통해 요청을 라우팅할지 결정합니다. 여기에는 요청(헤더와 본문)이 64KB를 완전히 초과하는 경우 또는 요청이 ["청크 분할" 전송 인코딩](https://tools.ietf.org/html/rfc7230#section-4.1)을 사용하여 전송되고 서비스 요청이 64KB를 초과하거나 요청 읽기가 즉각적이지 않을 것으로 예상할만한 이유가 있는 경우 등이 포함되지만 이에 국한되지는 않습니다. 서비스가 랑데부를 통해 요청을 전달하기로 선택하면 수신자에게는 랑데부 주소만 전달됩니다.
그러면 수신기는 랑데부 WebSocket을 반드시 설정해야 하고 서비스는 랑데부 WebSocket을 통해 본문을 포함한 전체 요청을 신속하게 전달합니다. 응답 역시 랑데부 WebSocket을 반드시 사용해야 합니다.

컨트롤 채널을 통해 도착하는 요청에 대해 수신기는 컨트롤 채널을 통해 응답할지 아니면 랑데부를 통해 응답할지 결정합니다. 서비스는 컨트롤 채널을 통해 라우팅되는 요청마다 랑데부 주소를 반드시 포함해야 합니다. 이 주소는 현재 요청에서 업그레이드하는 경우에만 유효합니다.

수신기가 업그레이드를 선택하면 설정된 랑데부 소켓에 연결하여 신속하게 응답을 전달합니다.

랑데부 WebSocket이 설정되면 수신기는 동일한 클라이언트의 요청 및 응답을 처리하기 위해 해당 설정을 유지해야 합니다. 이 서비스는 발신자와의 HTTPS 소켓 연결이 지속되는 한 WebSocket을 유지하며 유지된 WebSocket을 통해 발신자의 모든 후속 요청을 라우팅합니다. 수신기 쪽에서 랑데부 WebSocket을 삭제하도록 선택하면 서비스 역시 후속 요청이 이미 진행 상태에 있는지 여부와 상관없이 발신자에 대한 연결을 삭제합니다.

#### <a name="renew-operation"></a>갱신 작업

수신기를 등록하고 컨트롤 채널을 관리하는 데 사용해야 하는 보안 토큰은 수신기가 활성화된 동안 만료될 수 있습니다. 토큰 만료는 지속적인 연결에 영향을 주지 않지만 만료가 발생한 시점 또는 직후에는 서비스에 의해 컨트롤 채널이 삭제됩니다. "갱신" 작업은 컨트롤 채널과 연결된 토큰을 교체하기 위해 수신기가 보낼 수 있는 JSON 메시지이기 때문에 컨트롤 채널은 오랜 시간 동안 유지 관리할 수 있습니다.

#### <a name="ping-operation"></a>Ping 작업

컨트롤 채널이 장시간 유휴 상태로 있는 경우 부하 분산 장치 또는 NAT와 같은 중간 매개체는 TCP 연결을 끊을 수 있습니다. "Ping" 작업은 네트워크 경로의 모든 사용자에게 연결이 활성 상태임을 상기시키기 위해 채널에서 소량의 데이터를 전송함으로써 연결 끊김을 방지하며, 이것은 또한 수신기가 "라이브" 상태인지 확인하는 테스트이기도 합니다. Ping이 실패하는 경우 컨트롤 채널은 사용할 수 없는 것으로 간주되며 수신기를 다시 연결해야 합니다.

### <a name="sender-interaction"></a>발신자 상호작용

발신자는 서비스와 두 가지 상호 작용을 수행합니다. 웹 소켓에 연결하거나 HTTPS를 통해 요청을 보냅니다. 발신자 역할로는 웹 소켓을 통해 요청을 보낼 수 없습니다.

#### <a name="connect-operation"></a>연결 작업

"연결" 작업은 쿼리 문자열을 통해 "전송" 권한을 부여하는 보안 토큰(선택 사항이지만 기본적으로 필수) 및 하이브리드 연결 이름을 제공하여 서비스에 대한 WebSocket을 엽니다. 그런 다음 서비스는 앞서 설명한 방법으로 수신기와 상호 작용을 하고 수신기가 이 WebSocket과 함께 랑데부 연결을 설정합니다. WebSocket이 수락되면 해당 WebSocket의 모든 추가 상호 작용은 연결된 수신기와 이루어집니다.

#### <a name="request-operation"></a>요청 작업

기능이 활성화된 하이브리드 연결의 경우, 발신자는 HTTP 요청을 큰 제약 없이 수신기에 보낼 수 있습니다.

쿼리 문자열 또는 요청의 HTTP 헤더에 포함된 릴레이 액세스 토큰을 제외하고, 릴레이는 릴레이 주소의 모든 HTTP 작업 및 릴레이 주소 경로의 모든 접미사에 완전히 투명하기 때문에 수신기는 종단간 권한 부여 및 [CORS](https://www.w3.org/TR/cors/)와 같은 HTTP 확장 기능까지도 완전히 제어할 수 있습니다.

릴레이 엔드포인트를 통한 발신자 권한 부여는 기본적으로 켜지지만 선택 사항입니다. 하이브리드 연결의 소유자는 익명 발신자를 허용하도록 선택할 수 있습니다. 이 서비스는 다음과 같이 인증 정보를 가로채고 검사하고 제거합니다.

1. 쿼리 문자열에 `sb-hc-token` 식에 포함되어 있으면 이 식은 쿼리 문자열에서 항상 제거됩니다. 릴레이 권한 부여가 켜지면 평가됩니다.
2. 요청 헤더에 `ServiceBusAuthorization` 헤더가 있으면 헤더 표현식은 헤더 컬렉션에서 항상 제거됩니다.
   릴레이 권한 부여가 켜지면 평가됩니다.
3. 릴레이 권한 부여가 켜지고 요청 헤더에 `Authorization` 헤더가 포함되고 이전 식이 없는 경우에만 헤더가 평가되고 제거됩니다. 그렇지 않으면 `Authorization`은 항상 그대로 전달됩니다.

활성 수신기가 없으면 서비스는 502 "잘못된 게이트웨이" 오류 코드를 반환합니다. 서비스가 요청을 처리하지 못하는 경우 서비스는 60초 후에 504 "게이트웨이 시간 초과"를 반환합니다.

### <a name="interaction-summary"></a>상호 작용 요약

이 상호 작용 모델의 결과로, 수신기에 연결되어 있고 추가 프리앰블 또는 준비가 필요 없는 "정상적인" WebSocket과의 핸드셰이크에서 발신자 클라이언트가 나옵니다. 이 모델을 사용하면 기존 WebSocket 클라이언트 구현을 통해 올바르게 구성된 URL을 WebSocket 클라이언트 계층에 제공하여 하이브리드 연결 서비스를 쉽게 활용할 수 있습니다.

수락 상호 작용을 통해 수신기가 획득한 랑데부 연결 WebSocket도 정상 상태이며 프레임워크의 로컬 네트워크 수신기의 "수락" 작업과 하이브리드 연결의 원격 "수락" 작업을 구분하는 최소한의 추가 추상화로 기존 WebSocket 서버 구현에 전달될 수 있습니다.

HTTP 요청/응답 모델은 발신자에게 OPTIONAL 권한 부여 레이어가 있는 HTTP 프로토콜 노출 영역을 큰 제약 없이 제공합니다. 수신기는 HTTP 본문을 운반하는 이진 프레임을 사용하여 다운스트림 HTTP 요청으로 되돌릴 수 있거나 그대로 처리될 수 있는 미리 구문 분석된 HTTP 요청 헤더 섹션을 가져옵니다. 응답을 동일한 형식을 사용합니다. 64KB 미만의 요청 및 응답 본문과의 상호 작용은 모든 보낸 사람이 공유하는 단일 웹 소켓을 통해 처리할 수 있습니다. 이보다 큰 요청과 응답은 랑데부 모델을 사용하여 처리할 수 있습니다.

## <a name="protocol-reference"></a>프로토콜 참조

이 섹션에서는 앞에서 설명한 프로토콜 상호 작용의 세부 정보를 설명합니다.

모든 WebSocket 연결은 일반적으로 일부 WebSocket 프레임워크 또는 API에서 추상화되는 HTTPS 1.1의 업그레이드로 포트 443에서 설정됩니다. 여기의 설명은 특정 프레임워크를 제안하지 않고 중립적으로 구현하는 방법을 설명합니다.

### <a name="listener-protocol"></a>수신기 프로토콜

수신기 프로토콜은 두 개의 연결 제스처와 세 가지 메시지 작업으로 구성됩니다.

#### <a name="listener-control-channel-connection"></a>수신기 컨트롤 채널 연결

컨트롤 채널은 다음에 대한 WebSocket 연결을 만들 때 열립니다.

`wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...`

`namespace-address`는 하이브리드 연결을 호스팅하는 Azure 릴레이 네임스페이스의 정규화된 도메인 이름으로, 일반적인 형태는 `{myname}.servicebus.windows.net`입니다.

쿼리 문자열 매개 변수 옵션은 다음과 같습니다.

| 매개 변수        | 필수 | 설명
| ---------------- | -------- | -------------------------------------------
| `sb-hc-action`   | yes      | 수신기 역할에 대한 매개 변수는 **sb-hc-action=listen**이어야 합니다.
| `{path}`         | yes      | 이 수신기를 등록하기 위해 미리 구성된 하이브리드 연결의 URL 인코딩 네임스페이스 경로입니다. 이 식은 고정 `$hc/` 경로 부분에 추가됩니다.
| `sb-hc-token`    | 예\*    | 수신기는 네임스페이스 또는 **수신** 권한을 부여하는 하이브리드 연결의 유효한 URL 인코딩 Service Bus 공유 액세스 토큰을 제공해야 합니다.
| `sb-hc-id`       | 아니요       | 이 클라이언트 제공 옵션 ID를 사용하면 종단 간 진단 추적을 수행할 수 있습니다.

등록되지 않은 하이브리드 연결 경로, 잘못되었거나 누락된 토큰 또는 일부 다른 오류로 인해 WebSocket 연결이 실패할 경우, 일반적인 HTTP 1.1 상태 피드백 모델을 사용하여 오류 피드백이 제공됩니다. 상태 설명에는 다음과 같이 Azure 지원 담당자에게 전달될 수 있는 오류 추적 ID를 포함됩니다.

| 코드 | 오류          | 설명
| ---- | -------------- | -------------------------------------------------------------------
| 404  |  찾을 수 없음      | 하이브리드 연결 경로가 유효하지 않거나 기준 URL이 잘못되었습니다.
| 401  | 권한 없음   | 보안 토큰은 누락되었거나 잘못되었거나 유효하지 않습니다.
| 403  | 사용할 수 없음      | 보안 토큰이 이 작업의 이 경로에 대해 올바르지 않습니다.
| 500  | 내부 오류 | 서비스에 오류가 발생했습니다.

처음 설정한 이후 WebSocket 연결이 서비스에 의해 의도적으로 종료된 경우, 설명 및 추적 ID를 포함하는 오류 메시지와 함께 적절한 WebSocket 프로토콜 오류 코드를 사용하여 종료 이유가 전달됩니다. 서비스는 오류 조건이 발생하지 않는 한 컨트롤 채널을 종료하지 않습니다. 정상적인 종료는 클라이언트를 제어합니다.

| WS 상태 | 설명
| --------- | -------------------------------------------------------------------------------
| 1001      | 하이브리드 연결 경로가 삭제되었거나 사용되지 않았습니다.
| 1008      | 보안 토큰이 만료되어 권한 부여 정책을 위반했습니다.
| 1011      | 서비스에 오류가 발생했습니다.

#### <a name="accept-handshake"></a>핸드셰이크 수락

이전에 설정된 컨트롤 채널을 통해 서비스에서 WebSocket 텍스트 프레임의 JSON 메시지로 "수락" 알림을 수신기에 전송합니다. 이 메시지에 대한 회신이 없습니다.

이 메시지에는 "accept"라는 JSON 개체가 포함되며, 이 개체는 현재 다음 속성을 정의합니다.

* **address** – 들어오는 연결을 수락하기 위해 서비스에 대한 WebSocket을 설정하는 데 사용할 URL 문자열입니다.
* **id** - 이 연결에 대한 고유 식별자입니다. 발신자 클라이언트에 의해 id가 제공된 경우 이는 발신자 제공 값이거나 시스템 생성 값입니다.
* **connectHeaders** – 발신자가 릴레이 엔드포인트로 보낸 모든 HTTP 헤더로, Sec-WebSocket-Protocol 및 Sec-WebSocket-Extensions 헤더를 포함합니다.

```json
{
    "accept" : {
        "address" : "wss://dc-node.servicebus.windows.net:443/$hc/{path}?..."
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736",
        "connectHeaders" : {
            "Host" : "...",
            "Sec-WebSocket-Protocol" : "...",
            "Sec-WebSocket-Extensions" : "..."
        }
     }
}
```

JSON 메시지에 제공된 주소 URL은 수신기에서 발신자 소켓을 수락하거나 거부하도록 WebSocket을 설정하는 데 사용됩니다.

##### <a name="accepting-the-socket"></a>소켓 수락

수락하면 수신기가 제공된 주소로 WebSocket 연결을 설정합니다.

"수락" 메시지에 `Sec-WebSocket-Protocol` 헤더가 있는 경우, 수신기는 해당 프로토콜을 지원하는 경우에만 WebSocket을 수락해야 합니다. 또한 WebSocket이 설정된 대로 헤더를 설정합니다.

`Sec-WebSocket-Extensions` 헤더의 경우도 마찬가지입니다. 프레임워크가 확장을 지원하는 경우 확장에 필요한 `Sec-WebSocket-Extensions` 핸드셰이크의 서버 쪽 회신으로 헤더를 설정해야 합니다.

URL은 수락 소켓을 설정하는 데 현재 상태로 사용되어야 하지만, 다음과 같은 매개 변수를 포함합니다.

| 매개 변수      | 필수 | 설명
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | yes      | 소켓을 수락하기 위해 매개 변수는 `sb-hc-action=accept`여야 합니다.
| `{path}`       | yes      | (다음 단락 참조)
| `sb-hc-id`     | 아니요       | **ID**에 대한 앞의 설명을 참조하세요.

`{path}`는 이 수신기를 등록할 미리 구성된 하이브리드 연결에 대한 URL로 인코드된 네임스페이스 경로입니다. 이 식은 고정 `$hc/` 경로 부분에 추가됩니다.

`path` 식은 등록된 이름과 구분 슬래시 뒤에 오는 쿼리 문자열 식 및 접미사로 확장할 수도 있습니다.
이렇게 하면 발신자 클라이언트가 HTTP 헤더를 포함할 수 없는 경우 수락하는 수신기에 디스패치 인수를 전달할 수 있습니다. 수신기 프레임워크는 경로에서 고정 경로 부분 및 등록된 이름을 구문 분석하고, 접두사가 `sb-`인 쿼리 문자열 인수 없이 응용 프로그램이 연결을 허용할지 여부를 결정하는 데 사용할 수 있는 나머지 부분을 만들게 됩니다.

자세한 내용은 다음 "발신자 프로토콜" 섹션을 참조하세요.

오류가 없으면 서비스는 다음과 같이 회신할 수 있습니다.

| 코드 | 오류          | 설명
| ---- | -------------- | -----------------------------------
| 403  | 사용할 수 없음      | URL이 올바르지 않습니다.
| 500  | 내부 오류 | 서비스에 오류가 발생함

 연결이 설정된 후, 발신자 WebSocket이 종료될 때 또는 다음과 같은 상태일 때 서버는 WebSocket을 종료합니다.

| WS 상태 | 설명                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | 발신자 클라이언트가 연결을 종료합니다.                                    |
| 1001      | 하이브리드 연결 경로가 삭제되었거나 사용되지 않았습니다.                        |
| 1008      | 보안 토큰이 만료되어 권한 부여 정책을 위반했습니다. |
| 1011      | 서비스에 오류가 발생했습니다.                                            |

##### <a name="rejecting-the-socket"></a>소켓 거부

 `accept` 메시지를 검사한 후 소켓을 거부하려면 거부 이유를 알리는 상태 코드 및 상태 설명이 발신자에게 다시 전달될 수 있도록 유사한 핸드셰이크가 필요합니다.

 여기서 선택한 프로토콜 디자인은 정의된 오류 상태에서 종료되도록 설계된 WebSocket 핸드셰이크를 사용하는 것이므로 수신기 클라이언트 구현에서 WebSocket 클라이언트를 계속 사용할 수 있고 기본 HTTP 클라이언트를 추가로 이용할 필요가 없습니다.

 소켓을 거부하기 위해 클라이언트는 `accept` 메시지에서 주소 URI을 가져와서 여기에 두 개의 쿼리 문자열 매개 변수를 다음과 같이 추가합니다.

| 매개 변수                   | 필수 | 설명                              |
| ----------------------- | -------- | ---------------------------------------- |
| sb-hc-statusCode        | yes      | 숫자 HTTP 상태 코드                |
| sb-hc-statusDescription | yes      | 거부 이유를 읽을 수 있는 사람 |

결과 URI는 WebSocket 연결을 설정하는 데 사용됩니다.

올바르게 완료한 경우 이 핸드셰이크는 WebSocket이 설정되지 않았기 때문에 HTTP 오류 코드 410과 함께 의도적으로 실패합니다. 문제가 발생하는 경우 다음 코드를 통해 오류를 설명합니다.

| 코드 | 오류          | 설명                          |
| ---- | -------------- | ------------------------------------ |
| 403  | 사용할 수 없음      | URL이 올바르지 않습니다.                |
| 500  | 내부 오류 | 서비스에 오류가 발생했습니다. |

#### <a name="request-message"></a>메시지 요청

`request` 메시지는 서비스가 컨트롤 채널을 통해 수신기로 전송합니다. 동일한 메시지가 이미 설정된 랑데부 WebSocket을 통해 전송됩니다.

`request`는 두 가지 부분 즉, 헤더와 바이너리 본문 프레임으로 이루어집니다.
본문이 없으면 본문 프레임이 생략됩니다. 본문이 있는지 여부를 나타내는 표시기는 요청 메시지의 부울 `body` 속성입니다.

요청 본문이 있는 요청의 구조는 다음과 같습니다.

``` text
----- Web Socket text frame ----
{
    "request" :
    {
        "body" : true,
        ...
    }
}
----- Web Socket binary frame ----
FEFEFEFEFEFEFEFEFEFEF...
----- Web Socket binary frame ----
FEFEFEFEFEFEFEFEFEFEF...
----- Web Socket binary frame -FIN
FEFEFEFEFEFEFEFEFEFEF...
----------------------------------
```

수신기는 다수의 이진 프레임에 걸쳐 분할된 요청 본문의 수신을 처리해야 합니다([WebSocket 조각](https://tools.ietf.org/html/rfc6455#section-5.4) 참조).
요청은 FIN 플래그가 설정된 이진 프레임을 수신하면 종료됩니다.

본문이 없는 요청의 경우 텍스트 프레임은 하나만 있습니다.

``` text
----- Web Socket text frame ----
{
    "request" :
    {
        "body" : false,
        ...
    }
}
----------------------------------
```

`request`에 대한 JSON 콘텐츠는 다음과 같습니다.

* **주소** - URI 문자열. 이 요청에 사용할 랑데부 주소입니다. 들어오는 요청이 64kB를 초과하면 이 메시지의 나머지 부분은 공백으로 유지되고 클라이언트는 아래 설명된 `accept` 작업과 동일한 랑데부 핸드셰이크를 반드시 시작해야 합니다. 그러면 전체 `request`를 설정된 웹 소켓에 보냅니다. 응답이 64kB를 초과할 것으로 예상되면 수신기도 랑데부 핸드셰이크를 시작한 다음, 설정된 웹 소켓을 통해 응답을 반드시 전송해야 합니다.
* **id** – 문자열. 이 요청에 대한 고유 식별자입니다.
* **requestHeaders** – 이 개체는 발신자가 엔드포인트에 제공한 모든 HTTP 헤더([위에서](#request-operation) 설명한 권한 부여 정보를 제외) 및 게이트웨이 연결과 전적으로 관련된 헤더를 포함합니다. 특히 `Via`를 제외하고 [RFC7230](https://tools.ietf.org/html/rfc7230)에 정의되거나 예약된 모든 헤더는 제거되고 전달되지 않습니다.

  * `Connection`(RFC7230, 섹션 6.1)
  * `Content-Length`(RFC7230, 섹션 3.3.2)
  * `Host`(RFC7230, 섹션 5.4)
  * `TE`(RFC7230, 섹션 4.3)
  * `Trailer`(RFC7230, 섹션 4.4)
  * `Transfer-Encoding`(RFC7230, 섹션 3.3.1)
  * `Upgrade`(RFC7230, 섹션 6.7)
  * `Close`(RFC7230, 섹션 8.1)

* **requestTarget** – 문자열. 이 속성은 요청의 ["요청 대상"(RFC7230, 섹션 5.3)](https://tools.ietf.org/html/rfc7230#section-5.3)을 보관합니다. 여기에는 `sb-hc-` 접두사가 붙은 모든 매개 변수가 제거된 쿼리 문자열 부분이 포함됩니다.
* **method** - 문자열. [RFC7231, 섹션 4](https://tools.ietf.org/html/rfc7231#section-4)에 따른 요청 방법입니다. `CONNECT` 메서드는 절대로 사용하지 말아야 합니다.
* **body** – 부울. 하나 이상의 이진 본문 프레임이 뒤따르는지 여부를 나타냅니다.

``` JSON
{
    "request" : {
        "address" : "wss://dc-node.servicebus.windows.net:443/$hc/{path}?...",
        "id" : "42c34cb5-7a04-4d40-a19f-bdc66441e736",
        "requestTarget" : "/abc/def?myarg=value&otherarg=...",
        "method" : "GET",
        "requestHeaders" : {
            "Host" : "...",
            "Content-Type" : "...",
            "User-Agent" : "..."
        },
        "body" : true
     }
}
```

##### <a name="responding-to-requests"></a>요청에 대한 응답

수신기는 반드시 응답해야 합니다. 연결을 유지하면서 반복적으로 요청에 응답하지 못하면 수신기가 차단 목록에 포함될 수 있습니다.

응답은 순서에 관계없이 보낼 수 있지만 각 요청에 대해 60초 이내에 응답해야 합니다. 그렇지 않으면 전송이 실패한 것으로 보고됩니다. 60초 기한은 서비스가 `response` 프레임을 수신할 때까지 카운트됩니다. 다중 이진 프레임에 대한 지속적인 응답은 60초 이상 유휴 상태가 될 수 없습니다. 그렇지 않으면 종료됩니다.

요청이 컨트롤 채널을 통해 수신되면 응답은 반드시 요청을 수신한 컨트롤 채널을 통해 보내거나 랑데부 채널을 통해 보내야 합니다.

응답은 "response"라는 JSON 개체입니다. 본문 콘텐츠를 처리하는 규칙은 `request` 메시지와 정확히 같으며 `body` 속성을 기반으로 합니다.

* **requestId** – 문자열. 필수. 응답을 받는 `request` 메시지의 `id` 속성 값입니다.
* **statusCode** – 숫자. 필수. 알림의 결과를 나타내는 숫자로 된 HTTP 상태 코드입니다. [502 "잘못된 게이트웨이"](https://tools.ietf.org/html/rfc7231#section-6.6.3) 및 [504 "게이트웨이 시간 초과"](https://tools.ietf.org/html/rfc7231#section-6.6.5)를 제외하고 [RFC7231, 섹션 6](https://tools.ietf.org/html/rfc7231#section-6)의 모든 상태 코드가 허용됩니다.
* **statusDescription** - 문자열. 옵션. [RFC7230, 섹션 3.1.2](https://tools.ietf.org/html/rfc7230#section-3.1.2)에 따른 HTTP 상태 코드 이유 구문입니다.
* **responseHeaders** – 외부 HTTP 응답에 설정될 HTTP 헤더입니다.
  `request`와 마찬가지로 RFC7230 정의 헤더는 절대로 사용하지 말아야 합니다.
* **body** – 부울. 이진 본문 프레임이 있는지 여부를 나타냅니다.

``` text
----- Web Socket text frame ----
{
    "response" : {
        "requestId" : "42c34cb5-7a04-4d40-a19f-bdc66441e736",
        "statusCode" : "200",
        "responseHeaders" : {
            "Content-Type" : "application/json",
            "Content-Encoding" : "gzip"
        }
         "body" : true
     }
}
----- Web Socket binary frame -FIN
{ "hey" : "mydata" }
----------------------------------
```

##### <a name="responding-via-rendezvous"></a>랑데부를 통한 응답

64kB를 초과하는 응답은 반드시 랑데부 소켓을 통해 전달되어야 합니다. 또한 요청이 64KB를 초과하고 `request`에 주소 필드만 있으면 `request`을 얻기 위한 랑데부 소켓을 설정해야 합니다. 랑데부 소켓이 설정되면 이 소켓이 지속되는 동안 해당 클라이언트에 대한 응답 및 해당 클라이언트로부터의 후속 요청은 반드시 랑데부 소켓을 통해 전달되어야 합니다.

`request`의 `address` URL은 랑데부 소켓 설정을 위해 그대로 사용되어야 하지만 다음 매개 변수가 포함됩니다.

| 매개 변수      | 필수 | 설명
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | yes      | 소켓을 수락하기 위해 매개 변수는 `sb-hc-action=request`여야 합니다.

오류가 없으면 서비스는 다음과 같이 회신할 수 있습니다.

| 코드 | 오류           | 설명
| ---- | --------------- | -----------------------------------
| 400  | 잘못된 요청 | 동작을 인식할 수 없거나 URL이 유효하지 않습니다.
| 403  | 사용할 수 없음       | URL이 만료되었습니다.
| 500  | 내부 오류  | 서비스에 오류가 발생함

 연결이 설정된 후 서버는 클라이언트의 HTTP 소켓이 종료되거나 다음 상태인 경우 WebSocket을 종료합니다.

| WS 상태 | 설명                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | 발신자 클라이언트가 연결을 종료합니다.                                    |
| 1001      | 하이브리드 연결 경로가 삭제되었거나 사용되지 않았습니다.                        |
| 1008      | 보안 토큰이 만료되어 권한 부여 정책을 위반했습니다. |
| 1011      | 서비스에 오류가 발생했습니다.                                            |


#### <a name="listener-token-renewal"></a>수신기 토큰 갱신

수신기 토큰이 만료 직전인 경우 설정된 제어 채널을 통해 텍스트 프레임 메시지를 서비스에 전송하여 교체할 수 있습니다. 메시지는 `renewToken`이라는 JSON 개체를 포함하며 이때 다음과 같은 속성을 정의합니다.

* **token** – **수신** 권한을 부여하는 하이브리드 연결 또는 네임스페이스에 유효한 URL 인코드된 Service Bus 공유 액세스 토큰입니다.

```json
{
  "renewToken": {
    "token":
      "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
  }
}
```

토큰 유효성 검사에 실패하면 액세스가 거부되고, 클라우드 서비스가 오류와 함께 컨트롤 채널 WebSocket을 닫습니다. 그렇지 않으면 회신이 없습니다.

| WS 상태 | 설명                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1008      | 보안 토큰이 만료되어 권한 부여 정책을 위반했습니다. |

### <a name="web-socket-connect-protocol"></a>웹 소켓 연결 프로토콜

발신자 프로토콜은 사실상 수신기가 설정된 방식과 동일합니다.
목표는 종단 간 WebSocket에 대한 투명도를 최대화하는 것입니다. 연결할 주소는 수신기의 경우와 동일하지만 "작업"이 다르며 토큰에는 다음과 같이 다른 권한이 필요합니다.

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

_namespace-address_는 하이브리드 연결을 호스팅하는 Azure 릴레이 네임스페이스의 정규화된 도메인 이름으로, 일반적인 형태는 `{myname}.servicebus.windows.net`입니다.

요청에 애플리케이션 정의 헤더를 비롯한 임의의 추가 HTTP 헤더를 포함할 수 있습니다. 제공된 모든 헤더는 수신기로 전달되고 **수락** 제어 메시지의 `connectHeader` 개체에서 확인할 수 있습니다.

쿼리 문자열 매개 변수 옵션은 다음과 같습니다.

| 매개 변수          | Required? | 설명
| -------------- | --------- | -------------------------- |
| `sb-hc-action` | yes       | 발신자 역할의 경우 매개 변수는 `sb-hc-action=connect`여야 합니다.
| `{path}`       | yes       | (다음 단락 참조)
| `sb-hc-token`  | 예\*     | 수신기는 네임스페이스 또는 **발신** 권한을 부여하는 하이브리드 연결의 유효한 URL 인코딩 Service Bus 공유 액세스 토큰을 제공해야 합니다.
| `sb-hc-id`     | 아니요        | 종단 간 진단 추적을 허용하고 수락 핸드셰이크 중 수신기에 대해 사용할 수 있는 옵션 ID입니다.

 `{path}`은 이 수신기를 등록하기 위해 미리 구성된 하이브리드 연결의 URL 인코딩 네임스페이스 경로입니다. 추가 통신을 위해 `path` 식을 접미사 및 쿼리 문자열 식으로 확장할 수 있습니다. 하이브리드 연결이 `hyco` 경로 아래에 등록된 경우 `path` 식은 `hyco/suffix?param=value&...`와 여기서 정의한 쿼리 문자열 매개 변수일 수 있습니다. 그러면 전체 식은 다음과 같을 수 있습니다.

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

`path` 식은 "허용" 제어 메시지에 포함된 주소 URI의 수신기에 전달됩니다.

등록되지 않은 하이브리드 연결 경로, 잘못되었거나 누락된 토큰 또는 일부 다른 오류로 인해 WebSocket 연결이 실패할 경우, 일반적인 HTTP 1.1 상태 피드백 모델을 사용하여 오류 피드백이 제공됩니다. 상태 설명에는 다음과 같이 Azure 지원 담당자에게 전달될 수 있는 오류 추적 ID를 포함됩니다.

| 코드 | 오류          | 설명
| ---- | -------------- | -------------------------------------------------------------------
| 404  |  찾을 수 없음      | 하이브리드 연결 경로가 유효하지 않거나 기준 URL이 잘못되었습니다.
| 401  | 권한 없음   | 보안 토큰은 누락되었거나 잘못되었거나 유효하지 않습니다.
| 403  | 사용할 수 없음      | 보안 토큰이 이 경로와 이 작업에 유효하지 않습니다.
| 500  | 내부 오류 | 서비스에 오류가 발생했습니다.

처음 설정한 이후 WebSocket 연결이 서비스에 의해 의도적으로 종료된 경우, 설명 및 추적 ID를 포함하는 오류 메시지와 함께 적절한 WebSocket 프로토콜 오류 코드를 사용하여 종료 이유가 전달됩니다.

| WS 상태 | 설명
| --------- | ------------------------------------------------------------------------------- 
| 1000      | 수신기는 소켓을 종료합니다.
| 1001      | 하이브리드 연결 경로가 삭제되었거나 사용되지 않았습니다.
| 1008      | 보안 토큰이 만료되어 권한 부여 정책을 위반했습니다.
| 1011      | 서비스에 오류가 발생했습니다.

### <a name="http-request-protocol"></a>HTTP 요청 프로토콜

HTTP 요청 프로토콜은 프로토콜 업그레이드를 제외한 임의의 HTTP 요청을 허용합니다.
HTTP 요청은 하이브리드 연결 WebSocket 클라이언트에 사용되는 $ hc 중위 없이 엔터티의 일반 런타임 주소를 가리킵니다.

```
https://{namespace-address}/{path}?sbc-hc-token=...
```

_namespace-address_는 하이브리드 연결을 호스팅하는 Azure 릴레이 네임스페이스의 정규화된 도메인 이름으로, 일반적인 형태는 `{myname}.servicebus.windows.net`입니다.

요청에 애플리케이션 정의 헤더를 비롯한 임의의 추가 HTTP 헤더를 포함할 수 있습니다. RFC7230([request message](#Request message) 참조)에 직접 정의된 항목을 제외하고 제공된 모든 헤더는 수신기에 전달되며 **request** 메시지의 `requestHeader` 개체에서 찾을 수 있습니다.

쿼리 문자열 매개 변수 옵션은 다음과 같습니다.

| 매개 변수          | Required? | 설명
| -------------- | --------- | ---------------- |
| `sb-hc-token`  | 예\*     | 수신기는 네임스페이스 또는 **발신** 권한을 부여하는 하이브리드 연결의 유효한 URL 인코딩 Service Bus 공유 액세스 토큰을 제공해야 합니다.

토큰은 `ServiceBusAuthorization` 또는 `Authorization` HTTP 헤더로 전달될 수도 있습니다. 토큰은 하이브리드 연결이 익명 요청을 허용하도록 구성된 경우 생략할 수 있습니다.

이 서비스는 실제 HTTP 프록시는 아니지만 프록시 역할을 효과적으로 수행하기 때문에 `Via` 헤더를 추가하거나 [RFC7230, 섹션 5.7.1](https://tools.ietf.org/html/rfc7230#section-5.7.1)에 따라 기존 `Via` 헤더에 주석을 추가합니다.
이 서비스는 `Via`에 릴레이 네임스페이스 호스트 이름을 추가합니다.

| 코드 | Message  | 설명                    |
| ---- | -------- | ------------------------------ |
| 200  | 확인       | 요청이 하나 이상의 수신기에서 처리되었습니다.  |
| 202  | 수락됨 | 요청이 하나 이상의 수신기에서 수락되었습니다. |

오류가 있으면 서비스는 다음과 같이 응답할 수 있습니다. 응답이 서비스에서 발생했는지 아니면 수신기에서 발생했는지 여부는 `Via` 헤더의 존재를 통해 파악될 수 있습니다. 헤더가 있으면 응답은 수신기에서 발생한 것입니다.

| 코드 | 오류           | 설명
| ---- | --------------- |--------- |
| 404  |  찾을 수 없음       | 하이브리드 연결 경로가 유효하지 않거나 기준 URL이 잘못되었습니다.
| 401  | 권한 없음    | 보안 토큰은 누락되었거나 잘못되었거나 유효하지 않습니다.
| 403  | 사용할 수 없음       | 보안 토큰이 이 경로와 이 작업에 유효하지 않습니다.
| 500  | 내부 오류  | 서비스에 오류가 발생했습니다.
| 503  | 잘못된 게이트웨이     | 요청을 수신기로 라우팅할 수 없습니다.
| 504  | 게이트웨이 시간 초과 | 요청이 수신기로 라우팅되었지만 수신기가 필요한 시간 내에 수신을 확인하지 못했습니다.

## <a name="next-steps"></a>다음 단계

* [릴레이 FAQ](relay-faq.md)
* [네임스페이스 만들기](relay-create-namespace-portal.md)
* [.NET 시작](relay-hybrid-connections-dotnet-get-started.md)
* [노드 시작](relay-hybrid-connections-node-get-started.md)
