---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 14feb7ad09a24904034f9ae90cf4a54cf786a44c
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44369535"
---
B 시리즈 VM 제품군을 사용하면 워크로드에 필요한 기준 수준 성능을 제공하는 VM 크기를 선택할 수 있으며, Intel® Broadwell E5-2673 v4 2.3GHz 또는 Intel® Haswell 2.4GHz E5-2673 v3 프로세서 vCPU의 CPU 성능을 최대 100%까지 버스트할 수 있습니다.

B 시리즈 VM은 웹 서버, 개념 증명, 소규모 데이터베이스 및 개발 및 빌드 환경과 같이 CPU의 전체 성능이 지속적으로 필요하지 않은 작업에 적합합니다. 이러한 작업에는 일반적으로 버스트 가능한 성능 요구 사항이 있습니다. B 시리즈는 기준 성능을 갖춘 VM 크기를 구입할 수 있는 기능을 제공하며, 기준 성능보다 적게 사용할 경우 VM 인스턴스에서 크레딧을 적립합니다. VM에 대한 크레딧이 적립되면 응용 프로그램에 더 높은 CPU 성능이 필요할 때 VM이 vCPU의 최대 100%까지 사용하여 기준 이상으로 버스트할 수 있습니다.

B 시리즈는 다음 6가지 VM 크기로 제공됩니다.

| 크기          | vCPU 수 | 메모리: GiB | 임시 저장소(SSD) GiB | VM의 CPU 기준 성능 | VM의 CPU 최대 성능 | 적립 크레딧/시간 | 최대 적립 크레딧 |
|---------------|--------|-------------|----------------|--------------------------------|---------------------------|-----------------------|--------------------|
| Standard_B1s  | 1      | 1           | 4              | 10%                            | 100%                      | 6                     | 144                |
| Standard_B1ms | 1      | 2           | 4              | 20%                            | 100%                      | 12                    | 288                |
| Standard_B2s  | 2      | 4           | 8              | 40%                            | 200%                      | 24                    | 576                |
| Standard_B2ms | 2      | 8           | 16             | 60%                            | 200%                      | 36                    | 864                |
| Standard_B4ms | 4      | 16          | 32             | 90%                            | 400%                      | 54                    | 1296               |
| Standard_B8ms | 8      | 32          | 64             | 135%                           | 800%                      | 81                    | 1944               |




## <a name="q--a"></a>질문과 대답 

### <a name="q-how-do-you-get-135-baseline-performance-from-a-vm"></a>Q: VM에서 135% 기준 성능을 얻으려면 어떻게 할까요?
**A**: 135%는 VM 크기를 구성하는 8개 vCPU에서 공유됩니다. 예를 들어 애플리케이션에서 일괄 처리를 수행하는 8개 코어 중 4개를 사용하고 해당 4개 vCPU에서 30% 사용률에서 실행하는 경우 총 VM CPU 성능은 120%가 됩니다.  즉 VM이 기준 성능에서 15% 델타에 따라 크레딧 시간을 적립한다는 것을 의미합니다.  그러나 동일한 VM에서 8개 vCPU를 모두 100% 사용하여 해당 VM에 최대 800% CPU 성능을 제공하는 크레딧을 사용할 수 있음도 의미합니다.


### <a name="q-how-can-i-monitor-my-credit-balance-and-consumption"></a>Q: 크레딧 잔액과 소비량을 모니터링하려면 어떻게 할까요?
**A**: 몇 주 후에 새로운 2개의 메트릭을 소개할 예정입니다. **Credit** 메트릭을 통해 VM에서 적립한 크레딧 수를 확인할 수 있고, **ConsumedCredit** 메트릭을 통해 VM에서 적립한 크레딧으로부터 소비한 CPU 크레딧 수를 확인할 수 있습니다.    포털의 메트릭 창에서 또는 Azure Monitor API를 통한 프로그래밍 방식으로 이러한 메트릭을 볼 수 있습니다.

Azure에 대한 메트릭 데이터에 액세스하는 방법에 대한 자세한 내용은 [Microsoft Azure의 메트릭 개요](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md)를 참조하세요.

### <a name="q-how-are-credits-accumulated"></a>Q: 크레딧은 어떻게 적립될까요?
**A**: 정확히 기준 성능 수준에서 실행되는 VM이 버스팅 크레딧의 순 적립 또는 소비가 적용되지 않도록 VM 적립 및 소비 속도가 설정됩니다.  VM이 기준 성능 수준 이하로 실행될 때마다 크레딧이 순 증가하고, VM이 기준 성능 수준보다 더 많은 CPU를 활용할 때마다 크레딧이 순 감소합니다.

**예제**: 내가 사용하는 적은 시간과 출석 데이터베이스 응용 프로그램에 대해 B1ms 크기를 사용하여 VM을 배포합니다. 이 크기에 따라 응용 프로그램에서 vCPU의 최대 20%를 기준으로 사용할 수 있으며, 사용하거나 적립할 수 있는 분당 크레딧은 0.2입니다. 

내 응용 프로그램은 직원들의 작업일 중 업무 시작과 종료에 해당하는 오전 7-9시와 오후 4-6시 사이에 바쁘게 사용됩니다. 다른 20시간 동안에는 일반적으로 유휴 상태이며 vCPU의 10%만 사용합니다. 최대 사용 시간이 아닌 경우 분당 0.2 크레딧을 적립하지만 분당 0.1 크레딧만 사용하므로 VM은 시간당 0.1 x 60 = 6 크레딧을 적립합니다.  최대 사용 시간이 아닌 20시간 동안 나는 120 크레딧을 적립하게 됩니다.  

최대 사용 시간 동안에는 내 응용 프로그램에서 평균 60%의 vCPU 사용률을 유지하지만 분당 0.2 크레딧을 적립하는 한편 분당 0.6 크레딧을 소비하므로 분당 0.4 크레딧 또는 시간당 0.4 x 60 = 24 크레딧의 순 비용이 발생합니다. 1일 최대 사용 시간이 4시간이므로 최대 사용량에 대해 4 x 24 = 96 크레딧이 필요합니다.

최대 사용 시간이 아닌 동안에 적립된 120 크레딧에서 최대 사용 시간 동안에 소비된 96 크레딧을 빼면, 하루 24 크레딧이 추가로 적립되어 다른 작업을 버스트하는 데 사용할 수 있습니다.


### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>Q: B 시리즈에서 Premium Storage 데이터 디스크를 지원하나요?
**A**: 예, B 시리즈 크기는 모두 Premium Storage 데이터 디스크를 지원합니다.   
    
### <a name="q-why-is-my-remaining-credit-set-to-0-after-a-redeploy-or-a-stopstart"></a>Q: redepoy 또는 중지/시작 후에 남은 크레딧이 0으로 설정되는 이유는 무엇인가요?
**A** : VM이 "REDPLOYED" 상태이고 VM이 다른 노드로 이동한 경우 누적된 크레딧은 손실됩니다. VM이 중지/시작했지만 동일한 노드에 남아 있는 경우 VM은 누적된 크레딧을 유지합니다. VM이 노드에서 새로 시작할 때마다 초기 크레딧을 가져오는데 Standard_B8ms의 경우 240분입니다.

    

    
