---
title: '빠른 시작: Azure Data Explorer Node 라이브러리를 사용하여 데이터 수집'
description: 이 빠른 시작 문서에서는 Node.js를 사용하여 Azure 데이터 탐색기로 데이터를 수집(로드)하는 방법에 대해 알아봅니다.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 10/25/2018
ms.openlocfilehash: c638369efc89ca4442b69c9337827fe3872fd197
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085963"
---
# <a name="quickstart-ingest-data-using-the-azure-data-explorer-node-library"></a>빠른 시작: Azure Data Explorer Node 라이브러리를 사용하여 데이터 수집

Azure 데이터 탐색기는 로그 및 원격 분석 데이터에 사용 가능한 빠르고 확장성이 우수한 데이터 탐색 서비스입니다. Azure 데이터 탐색기는 2개의 Node용 클라이언트 라이브러리, [수집 라이브러리](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-ingest) 및 [데이터 라이브러리](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-data)를 제공합니다. 이러한 라이브러리를 사용하여 데이터를 클러스터로 수집(로드)하고 코드에서 데이터를 쿼리할 수 있습니다. 이 빠른 시작에서는 먼저 테스트 클러스터에서 테이블 및 데이터 매핑을 만듭니다. 그런 다음, 클러스터 큐에 수집을 넣고 결과의 유효성을 검사합니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [Azure 체험 계정](https://azure.microsoft.com/free/)을 만듭니다.

## <a name="prerequisites"></a>필수 조건

이 빠른 시작을 완료하려면 Azure 구독 외에 다음 항목이 필요합니다.

* [테스트 클러스터 및 데이터베이스](create-cluster-database-portal.md)

* 개발 컴퓨터에 설치된 [Node.js](https://nodejs.org/en/download/)

## <a name="install-the-data-and-ingest-libraries"></a>데이터 및 수집 라이브러리 설치

*azure-kusto-ingest* 및 *azure-kusto-data* 설치

```bash
npm i --save azure-kusto-ingest azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>import 문 및 상수 추가

라이브러리에서 클래스 가져오기

```javascript
const KustoClient = require('azure-kusto-data').KustoClient;
const KustoIngestClient = require('azure-kusto-ingest').KustoIngestClient;
const KustoConnectionStringBuilder = require('azure-kusto-ingest').KustoConnectionStringBuilder;
```

응용 프로그램을 인증하기 위해 Azure 데이터 탐색기는 Azure Active Directory 테넌트 ID를 사용합니다. 테넌트 ID를 찾으려면 [Office 365 테넌트 ID 찾기](https://docs.microsoft.com/onedrive/find-your-office-365-tenant-id)를 따르세요.

이 코드를 실행하기 전에 `authorityId`, `kustoUri`, `kustoIngestUri` 및 `kustoDatabase`의 값을 설정합니다.

```javascript
const authorityId = "<TenantId>";
const kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443/";
const kustoIngestUri = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443/";
const kustoDatabase  = "<DatabaseName>";
```

이제 연결 문자열을 구성합니다. 이 예제에서는 디바이스 인증을 사용하여 클러스터에 액세스합니다. 또한 Azure Active Directory 응용 프로그램 인증서, 응용 프로그램 키, 사용자와 암호를 사용할 수 있습니다.

이후 단계에서 대상 테이블 및 매핑을 만듭니다.

```javascript
const kcsbIngest = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoIngestUri, authorityId);
const kcsbData = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoUri, authorityId);
const destTable = "StormEvents";
const destTableMapping = "StormEvents_CSV_Mapping";
```

## <a name="set-source-file-information"></a>소스 파일 정보 설정

추가 클래스를 가져오고 데이터 원본 파일에 대한 상수를 설정합니다. 이 예제에서는 Azure Blob Storage에 호스트된 예제 파일을 사용합니다. **StormEvents** 샘플 데이터 집합에는 [National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/)에서 제공하는 날씨 관련 데이터가 포함되어 있습니다.

```javascript
from azure.storage.blob import BlockBlobService
from azure.kusto.ingest import KustoIngestClient, IngestionProperties, FileDescriptor, BlobDescriptor, DataFormat, ReportLevel, ReportMethod

const container = "samplefiles";
const account = "kustosamplefiles";
const sas = "?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
const filePath = "StormEvents.csv";
const fileSize = 64158321; // in bytes
const blobPath = `https://${account}.blob.core.windows.net/${container}/${filePath}${sas}`;
```

## <a name="create-a-table-on-your-test-cluster"></a>테스트 클러스터에 테이블 만들기

`StormEvents.csv` 파일에 있는 데이터 스키마와 일치하는 테이블을 만듭니다. 이 코드를 실행하면 다음과 같은 메시지가 반환됩니다. ‘로그인하려면 웹 브라우저를 사용하여 https://microsoft.com/devicelogin 페이지를 열고 XXXXXXXXX 코드를 입력하여 인증하세요.’ 단계에 따라 로그인한 후 돌아가서 다음 코드 블록을 실행합니다. 연결을 만드는 후속 코드 블록을 위해 다시 로그인해야 합니다.

```javascript
const kustoClient = new KustoClient(kcsbData);
const createTableCommand = ".create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)";

kustoClient.executeMgmt(kustoDatabase, createTableCommand, (err, results) => {
    console.log(result.primaryResults[0]);
});
```

## <a name="define-ingestion-mapping"></a>수집 매핑 정의

테이블을 만들 때 사용되는 데이터 형식과 열 이름에 들어오는 CSV 데이터를 매핑합니다.

```javascript
const createMappingCommand = `.create table StormEvents ingestion csv mapping ${destTableMapping} '[{\"Name\":\"StartTime\",\"datatype\":\"datetime\",\"Ordinal\":0}, {\"Name\":\"EndTime\",\"datatype\":\"datetime\",\"Ordinal\":1},{\"Name\":\"EpisodeId\",\"datatype\":\"int\",\"Ordinal\":2},{\"Name\":\"EventId\",\"datatype\":\"int\",\"Ordinal\":3},{\"Name\":\"State\",\"datatype\":\"string\",\"Ordinal\":4},{\"Name\":\"EventType\",\"datatype\":\"string\",\"Ordinal\":5},{\"Name\":\"InjuriesDirect\",\"datatype\":\"int\",\"Ordinal\":6},{\"Name\":\"InjuriesIndirect\",\"datatype\":\"int\",\"Ordinal\":7},{\"Name\":\"DeathsDirect\",\"datatype\":\"int\",\"Ordinal\":8},{\"Name\":\"DeathsIndirect\",\"datatype\":\"int\",\"Ordinal\":9},{\"Name\":\"DamageProperty\",\"datatype\":\"int\",\"Ordinal\":10},{\"Name\":\"DamageCrops\",\"datatype\":\"int\",\"Ordinal\":11},{\"Name\":\"Source\",\"datatype\":\"string\",\"Ordinal\":12},{\"Name\":\"BeginLocation\",\"datatype\":\"string\",\"Ordinal\":13},{\"Name\":\"EndLocation\",\"datatype\":\"string\",\"Ordinal\":14},{\"Name\":\"BeginLat\",\"datatype\":\"real\",\"Ordinal\":16},{\"Name\":\"BeginLon\",\"datatype\":\"real\",\"Ordinal\":17},{\"Name\":\"EndLat\",\"datatype\":\"real\",\"Ordinal\":18},{\"Name\":\"EndLon\",\"datatype\":\"real\",\"Ordinal\":19},{\"Name\":\"EpisodeNarrative\",\"datatype\":\"string\",\"Ordinal\":20},{\"Name\":\"EventNarrative\",\"datatype\":\"string\",\"Ordinal\":21},{\"Name\":\"StormSummary\",\"datatype\":\"dynamic\",\"Ordinal\":22}]'`;

kustoClient.executeMgmt(kustoDatabase, createMappingCommand, (err, results) => {
    console.log(result.primaryResults[0]);
});
```

## <a name="queue-a-message-for-ingestion"></a>수집을 위해 메시지를 큐에 넣음

BLOB 저장소에서 데이터를 끌어온 후 Azure 데이터 탐색기에 수집하기 위해 메시지를 큐에 넣습니다.

```javascript
const { DataFormat, JsonColumnMapping } =  require("azure-kusto-ingest").IngestionPropertiesEnums;
const { BlobDescriptor } = require("azure-kusto-ingest").Descriptors;
const ingestClient = new KustoIngestClient(kcsbIngest);


// All ingestion properties are documented here: https://docs.microsoft.com/azure/kusto/management/data-ingest#ingestion-properties
const ingestionProps  = new IngestionProperties(kustoDatabase, destTable, DataFormat.csv, null,destTableMapping, {'ignoreFirstRecord': 'true'});
const blobDesc = new BlobDescriptor(blobPath, 10);
ingestClient.ingestFromBlob(blobDesc,ingestionProps, (err) => {
    if (err) throw new Error(err);
});
```

## <a name="validate-that-table-contains-data"></a>테이블에 데이터가 포함되어 있는지 검증

데이터가 테이블에 수집되었는지 검증합니다. 큐에 넣은 수집이 예약되고 데이터가 Azure 데이터 탐색기에 로드될 때까지 5~10분 정도 기다립니다. 그런 후, 다음 코드를 실행하여 `StormEvents` 테이블의 레코드 수를 가져옵니다.

```javascript
const query = "StormEvents | count";

kustoClient.execute(kustoDatabse, query, (err, results) => {
    if (err) throw new Error(err);  
    console.log(results.primaryResults[0].toString());
});
```

## <a name="run-troubleshooting-queries"></a>쿼리 문제 해결 실행

[https://dataexplorer.azure.com](https://dataexplorer.azure.com)에 로그인하고 클러스터에 연결합니다. 데이터베이스에서 다음 명령을 실행하여 지난 4시간 동안 수집 실패가 있었는지 확인합니다. 실행하기 전에 데이터베이스 이름을 바꿉니다.
    
```Kusto
.show ingestion failures
| where FailedOn > ago(4h) and Database == "<DatabaseName>"
```

다음 명령을 실행하여 지난 4시간 동안 진행된 모든 수집 작업의 상태를 확인합니다. 실행하기 전에 데이터베이스 이름을 바꿉니다.

```Kusto
.show operations
| where StartedOn > ago(4h) and Database == "<DatabaseName>" and Operation == "DataIngestPull"
| summarize arg_max(LastUpdatedOn, *) by OperationId
```

## <a name="clean-up-resources"></a>리소스 정리

다른 빠른 시작 및 자습서를 진행하려는 경우 만든 리소스를 그대로 둡니다. 그렇지 않으면 데이터베이스에서 다음 명령을 실행하여 `StormEvents` 테이블을 정리합니다.

```Kusto
.drop table StormEvents
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [쿼리 작성](write-queries.md)
