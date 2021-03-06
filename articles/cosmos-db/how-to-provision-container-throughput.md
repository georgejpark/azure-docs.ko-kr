---
title: Azure Cosmos DB에서 컨테이너 처리량 프로비전
description: Azure Cosmos DB의 컨테이너 수준에서 처리량을 프로비전하는 방법 알아보기
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: dd47976bca75569142f1912eee06c66061e92fa6
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097668"
---
# <a name="provision-throughput-for-an-azure-cosmos-db-container"></a>Azure Cosmos DB 컨테이너의 처리량 프로비전

이 문서에서는 Azure Cosmos DB에서 컨테이너(컬렉션, 그래프, 테이블)의 처리량을 프로비전하는 방법을 설명합니다. 단일 컨테이너 또는 [데이터베이스의 처리량을 프로비전](how-to-provision-database-throughput.md)하고 데이터베이스 안에 있는 컨테이너 간에 처리량을 공유할 수 있습니다. Azure Portal, Azure CLI 또는 CosmosDB SDK를 사용하여 컨테이너의 처리량을 프로비전할 수 있습니다.

## <a name="provision-throughput-using-azure-portal"></a>Azure Portal을 사용하여 처리량 프로비전

1. [Azure 포털](https://portal.azure.com/)에 로그인합니다.

1. [새 Cosmos DB 계정을 만들거나](create-sql-api-dotnet.md#create-a-database-account) 기존 계정을 선택합니다.

1. **데이터 탐색기** 창을 열고 **새 컬렉션**을 선택합니다. 다음 세부 정보로 양식을 채웁니다.

   * 새 데이터베이스를 만들거나 기존 데이터베이스를 사용합니다.
   * 컬렉션 ID(또는 테이블, 그래프)를 입력합니다.
   * 파티션 키 값을 입력합니다(예: `/userid`).
   * 처리량을 입력합니다(예: 1000RU).
   * **확인**을 선택합니다.

![SQL API 컨테이너 처리량 프로비전](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli"></a>Azure CLI를 사용하여 처리량 프로비전

```azurecli-interactive
# Create a container with a partition key and provision throughput of 1000 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

MongoDB API 계정의 처리량을 프로비전하는 경우 파티션 키 경로로 '/myShardKey'를 사용하고, Cassandra API 계정의 처리량을 프로비전할 때에는 파티션 키 경로로 '/myPrimaryKey'를 사용합니다.

## <a name="provision-throughput-using-net-sdk"></a>.NET SDK를 사용하여 처리량 프로비전

> [!Note]
> Cassandra API를 제외한 모든 API의 처리량을 프로비전하려면 SQL API를 사용합니다.

### <a id="dotnet-most"></a>SQL, MongoDB, Gremlin 및 Table API

```csharp
// Create a container with a partition key and provision throughput of 1000 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 1000 RU/s
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>다음 단계

다음 문서를 참조하여 Cosmos DB에서 처리량을 프로비전하는 방법을 알아보세요.

* [데이터베이스의 처리량을 프로비전하는 방법](how-to-provision-database-throughput.md)
* [Azure Cosmos DB의 요청 단위 및 처리량](request-units.md)
