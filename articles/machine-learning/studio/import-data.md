---
title: Machine Learning 스튜디오로 데이터 가져오기 - Azure | Microsoft Docs
description: 다양한 데이터 원본에서 Azure Machine Learning Studio로 데이터를 가져오는 방법. 지원되는 데이터 형식에 대해 알아봅니다.
keywords: 데이터 가져오기, 데이터 형식, 데이터 유형, 데이터 원본, 학습 데이터
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: 2f8c1eb43fddb21a59d4f00fd86b08d3fb3608f4
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269960"
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>다양한 데이터 원본에서 Azure Machine Learning Studio로 학습 데이터를 가져오기

다음 데이터를 사용하면 Machine Learning Studio에 사용자 고유의 데이터를 사용하여 예측 분석 솔루션을 개발 및 학습할 수 있습니다. 

* [**로컬 파일**](import-data-from-local-file.md) - 하드 드라이브에서 로컬 데이터를 미리 로드하여 사용자의 작업 영역에 데이터 세트 모듈을 만듭니다.
* [**온라인 데이터 원본**](import-data-from-online-sources.md) - [데이터 가져오기][import-data] 모듈을 사용하여 실험을 실행하는 동안 여러 온라인 원본 중 하나에서 데이터에 액세스합니다.
* [**Machine Learning Studio 실험**](import-data-from-an-experiment.md) - Machine Learning Studio에서 데이터 세트로 저장된 데이터를 사용합니다.
* [**온-프레미스 SQL Server 데이터베이스**](use-data-from-an-on-premises-sql-server.md) - 수동으로 데이터를 복사하지 않고도 온-프레미스 SQL Server 데이터베이스의 데이터를 사용합니다.

> [!NOTE]
> Machine Learning Studio에는 학습 데이터로 사용할 수 있는 샘플 데이터 세트가 많이 있습니다. 이에 대한 자세한 내용은 [Azure Machine Learning Studio의 샘플 데이터 세트 사용](use-sample-datasets.md)을 참조하세요.
> 
> 

또한 이 소개 문서에서는 Machine Learning Studio에 사용할 준비가 완료된 데이터를 가져오는 방법을 보여 주고 지원되는 데이터 형식과 데이터 유형을 다룹니다.

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio에서 사용할 준비가 완료된 데이터 가져오기
Machine Learning Studio는 데이터베이스에서 구분되었거나 구조화된 데이터인 텍스트 데이터 등의 직사각형 또는 테이블 형식 데이터에 대해 작업하도록 설계되어 있습니다. 단, 경우에 따라 직사각형이 아닌 데이터도 사용할 수 있습니다.

데이터가 비교적 정리되어 있는 것이 좋습니다. 즉, 실험에 데이터를 업로드하기 전에 따옴표가 없는 문자열 등의 문제를 처리합니다.

그러나 Machine Learning Studio에는 실험에서 데이터를 조작할 수 있는 모듈도 있습니다. 사용할 기계 학습 알고리즘에 따라 누락된 값 및 스파스 데이터와 같은 데이터 구조 문제 해결 방법을 결정해야 하며, 이때 도움을 줄 수 있는 모듈이 있습니다. 모듈 팔레트의 **데이터 변환** 섹션에서 이러한 함수를 수행하는 모듈을 찾습니다.

실험을 수행하는 동안 언제든지 출력 포트를 클릭하여 모듈에서 생성한 데이터를 보거나 다운로드할 수 있습니다. 모듈에 따라 서로 다른 다운로드 옵션을 사용할 수 있습니다. 또는 Machine Learning Studio의 웹 브라우저에서 데이터를 시각화할 수도 있습니다.

## <a name="data-formats-and-data-types-supported"></a>지원되는 데이터 형식 및 데이터 유형
데이터를 가져오는 데 사용하는 메커니즘과 데이터의 출처에 따라 실험에 여러 데이터 형식을 가져올 수 있습니다.

* 일반 텍스트(.txt)
* 헤더가 있거나(.csv) 없는(.nh.csv) 쉼표로 구분된 값(CSV)
* 헤더가 있거나(.tsv) 없는(.nh.tsv) 탭으로 구분된 값(TSV)
* Excel 파일
* Azure 테이블
* Hive 테이블
* SQL 데이터베이스 테이블
* OData 값
* SVMLight 데이터(.svmlight)(형식 정보는 [SVMLight 정의](http://svmlight.joachims.org/) 참조)
* 특성 관계 파일 형식(ARFF) 데이터(.arff)(형식 정보는 [ARFF 정의](http://weka.wikispaces.com/ARFF) 참조)
* Zip 파일(.zip)
* R 개체 또는 작업 영역 파일(. RData)

메타 데이터를 포함하는 ARFF와 같은 형식으로 데이터를 가져오는 경우, Machine Learning Studio에서는 이 메타 데이터를 사용하여 각 열의 머리글과 데이터 형식을 정의합니다.

이 메타 데이터를 포함하지 않는 TSV 또는 CSV 형식과 같은 데이터를 가져오는 경우, Machine Learning Studio에서는 데이터를 샘플링하여 각 열의 데이터 형식을 유추합니다. 데이터에 열 머리글이 없는 경우 Machine Learning Studio에서 기본 이름을 제공합니다.

[메타데이터 편집][edit-metadata]을 사용하여 열의 머리글과 데이터 유형을 명시적으로 지정하거나 변경할 수 있습니다.

Machine Learning Studio에서 인식하는 **데이터 유형**은 다음과 같습니다.

* 문자열
* 정수 
* double
* BOOLEAN
* Datetime
* timespan

Machine Learning 스튜디오에서는 ***데이터 테이블***이라는 내부 데이터 형식을 사용하여 모듈 간에 데이터를 전달합니다. [데이터 세트로 변환][convert-to-dataset] 모듈을 사용하여 명시적으로 데이터를 데이터 테이블 형식으로 변환할 수 있습니다.

데이터 테이블 이외의 형식을 허용하는 모든 모듈에서는 다음 모듈에 데이터를 전달하기 전에 데이터를 데이터 테이블로 자동 변환합니다.

필요한 경우 다른 변환 모듈을 사용하여 데이터 테이블 형식을 다시 CSV, TSV, ARFF 또는 SVMLight 형식으로 변환할 수 있습니다.
모듈 팔레트의 **데이터 형식 변환** 섹션에서 이러한 함수를 수행하는 모듈을 찾습니다.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
