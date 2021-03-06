---
title: Azure 가용성 영역이란? | Microsoft Docs
description: Azure에서 가용성과 복원력이 높은 응용 프로그램을 만들려면 가용성 영역에서 리소스를 실행하는 데 사용할 수 있는 별도의 실제 위치를 제공합니다.
services: ''
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2018
ms.author: cynthn
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: f1fd272bc25486e3abd068be5ddc01f5ac5f5f76
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566281"
---
# <a name="what-are-availability-zones-in-azure"></a>Azure에서 가용성 영역이란?
가용성 영역은 데이터 센터 오류에서 응용 프로그램 및 데이터를 보호하는 고가용성 기능입니다. 가용성 영역은 Azure 지역 내의 고유한 물리적 위치입니다. 각 영역은 독립된 전원, 냉각 및 네트워킹을 갖춘 하나 이상의 데이터 센터로 구성됩니다. 복원력을 보장하려면 활성화된 모든 지역에서 최소한 세 개의 별도 영역이 필요합니다. 지역 내에서 가용성 영역의 물리적 구분은 응용 프로그램 및 데이터를 데이터 센터 오류로부터 보호할 수 있습니다. 영역 중복 서비스는 단일 지점 오류에서 보호하기 위해 가용성 영역에서 응용 프로그램 및 데이터를 복제합니다. Azure는 가용성 영역을 통해 업계 최고의 99.99% VM 작동 시간 SLA를 제공합니다. 전체 [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/)는 Azure의 보장된 가용성에 대해 전반적으로 설명합니다.

Azure 지역의 가용성 영역은 장애 도메인과 업데이트 도메인의 조합입니다. 예를 들어 Azure 지역의 3개 영역에 VM을 3개 이상 만들면 장애 도메인 3개와 업데이트 도메인 3개에 VM이 효과적으로 분산됩니다. Azure 플랫폼은 업데이트 도메인에 분산된 VM을 인식하여 다른 영역에 있는 VM이 동시에 업데이트되지 않게 합니다.

영역 내에서 계산, 스토리지, 네트워킹 및 데이터 리소스를 공동 배치하고 다른 영역에 복제하여 애플리케이션 아키텍처에 고가용성을 빌드하십시오. 가용성 영역을 지원하는 Azure 서비스는 다음의 두 범주로 나뉩니다.

- **영역 서비스** - 특정 영역에 리소스를 고정(예를 들어 가상 머신, 관리 디스크, IP 주소)하거나
- **영역 중복 서비스** – 플랫폼이 영역에서 자동으로 복제됩니다(예를 들어, 영역 중복 저장소, SQL Database).

Azure에서 포괄적인 비즈니스 연속성을 구현하려면 Azure 지역 쌍과 가용성 영역의 조합을 사용하여 응용 프로그램 아키텍처를 빌드하십시오. 고가용성에 대한 Azure 지역 내의 가용성 영역을 사용하여 애플리케이션 및 데이터를 동기적으로 복제하고 재해 복구 보호에 대한 Azure 지역에서 비동기적으로 복제할 수 있습니다.
 
![지역에서 차례로 한 영역의 개념 보기](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>가용성 영역을 지원하는 지역

- 미국 중부
- 미국 동부 2(미리 보기)
- 프랑스 중부
- 북유럽
- 동남아시아 
- 서유럽
- 미국 서부 2



## <a name="services-that-support-availability-zones"></a>가용성 영역을 지원하는 서비스
가용성 영역을 지원하는 Azure 서비스는 다음과 같습니다.

- Linux 가상 머신
- Windows 가상 머신
- Virtual Machine Scale Sets
- Managed Disks
- Load Balancer
- 공용 IP 주소
- 영역 중복 저장소
- SQL Database
- Event Hubs
- Service Bus
- VPN Gateway
- ExpressRoute
- Application Gateway(미리 보기)


## <a name="pricing"></a>가격
가용성 영역에 배포된 가상 머신에 대한 추가 비용은 없습니다. Azure 지역 내에서 두 개 이상의 가용성 영역에 두 개 이상의 VM을 배포하면 VM 작동 시간 SLA 99.99%가 제공됩니다. 내부 가용성 영역 VM 대 VM 데이터 전송 요금이 추가됩니다. 자세한 내용은 [대역폭 가격 책정](https://azure.microsoft.com/pricing/details/bandwidth/) 페이지를 검토하세요.


## <a name="get-started-with-availability-zones"></a>가용성 영역 시작하기
- [가상 머신 만들기](../virtual-machines/windows/create-portal-availability-zone.md)
- [PowerShell을 사용하여 Managed Disk 추가](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [영역 중복 가상 머신 확장 집합 만들기](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [영역 중복 프런트엔드를 통해 표준 Load Balancer를 사용하여 영역에서 VM 부하 분산](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [영역 중복 프런트엔드를 통해 표준 Load Balancer를 사용하여 영역 내에 VM 부하 분산](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [영역 중복 저장소](../storage/common/storage-redundancy-zrs.md)
- [SQL Database](../sql-database/sql-database-high-availability.md#zone-redundant-configuration)
- [Event Hubs 지리적 재해 복구](../event-hubs/event-hubs-geo-dr.md#availability-zones-preview)
- [Service Bus 지리적 재해 복구](../service-bus-messaging/service-bus-geo-dr.md#availability-zones-preview)
- [영역 중복 가상 네트워크 게이트웨이 만들기](../vpn-gateway/create-zone-redundant-vnet-gateway.md)


## <a name="next-steps"></a>다음 단계
- [빠른 시작 템플릿](https://aka.ms/azqs)
