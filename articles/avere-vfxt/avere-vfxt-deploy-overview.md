---
title: 배포 개요 - Avere vFXT for Azure
description: Avere vFXT for Azure 배포에 대해 간략히 설명합니다.
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: aa5737d67ea2c9cb8cc7c7098764ae67fc91137d
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669820"
---
# <a name="avere-vfxt-for-azure---deployment-overview"></a>Avere vFXT for Azure - 배포 개요

이 문서에서는 Avere vFXT for Azure 클러스터를 가동하고 실행하는 데 필요한 단계에 대해 간략히 설명합니다.

Avere vFXT 시스템을 처음 배포할 때 대부분의 다른 Azure 도구를 배포하는 것보다 더 많은 단계가 필요할 수 있습니다. 시작부터 완료까지의 전체 프로세스를 명확히 이해하면 필요한 작업의 범위를 지정하는 데 도움이 됩니다. 시스템을 가동하고 실행한 후에는 클라우드 기반 계산 작업을 빠르게 처리할 수 있으므로 상당한 가치가 있습니다.

## <a name="deployment-steps"></a>배포 단계

[시스템을 계획](avere-vfxt-deploy-plan.md)한 후에는 Avere vFXT 클러스터를 만들 수 있습니다. 

먼저 vFXT 클러스터를 만드는 데 사용되는 클러스터 컨트롤러 VM을 만듭니다.

vFXT 클러스터가 가동되고 실행된 후에 클라이언트를 연결하는 방법과 필요한 경우 데이터를 새 Blob 저장소 컨테이너로 이동하는 방법을 이해할 필요가 있습니다.  

이러한 모든 단계에 대한 개요는 다음과 같습니다.

1. 필수 조건 구성 

   VM을 만들기 전에 Avere vFXT 프로젝트에 대한 새 구독을 만들고, 구독 소유권을 구성하고, 할당량을 확인하고, 필요한 경우 증가를 요청하고, Avere vFXT 소프트웨어 사용 약관에 동의해야 합니다. 자세한 지침은 [Avere vFXT 만들기 준비](avere-vfxt-prereqs.md)를 참조하세요.

1. 클러스터 컨트롤러 만들기

   *클러스터 컨트롤러*는 Avere vFXT 클러스터와 동일한 가상 네트워크에 있는 간단한 VM입니다. 컨트롤러는 vFXT 노드를 만들고, 클러스터를 구성하며, 해당 수명 동안 클러스터를 관리하는 명령줄 인터페이스도 제공합니다.

   공용 IP 주소를 사용하여 컨트롤러를 구성하는 경우 해당 컨트롤러는 vnet 외부에서 클러스터에 연결하기 위한 점프 호스트 역할을 수행할 수 있습니다.

   vFXT 클러스터를 만들고 해당 노드를 관리하는 데 필요한 모든 소프트웨어는 클러스터 컨트롤러에 사전 설치됩니다.

   자세한 내용은 [클러스터 컨트롤러 VM 만들기](avere-vfxt-deploy.md#create-the-cluster-controller-vm)를 참조하세요.

1. 클러스터 노드에 대한 런타임 역할 만들기 

   Azure는 [RBAC(역할 기반 액세스 제어)](https://docs.microsoft.com/azure/role-based-access-control/)를 사용하여 클러스터 노드 VM에 특정 작업을 수행할 수 있는 권한을 부여합니다. 예를 들어 클러스터 노드에서 IP 주소를 다른 클러스터 노드에 할당하거나 다시 할당할 수 있어야 합니다. 클러스터를 만들기 전에 적절한 권한을 부여하는 역할을 정의해야 합니다.

   클러스터 컨트롤러의 사전 설치된 소프트웨어에는 사용자 지정할 수 있는 프로토타입 역할이 포함되어 있습니다. 지침은 [클러스터 노드 액세스 역할 만들기](avere-vfxt-deploy.md#create-the-cluster-node-access-role)를 참조하세요.

1. Avere vFXT 클러스터 만들기 

   컨트롤러에서 적절한 클러스터 만들기 스크립트를 편집하고, 이 스크립트를 실행하여 클러스터를 만듭니다. 자세한 지침은 [배포 스크립트 편집](avere-vfxt-deploy.md#edit-the-deployment-script)에 있습니다. 

1. 클러스터 구성 

   Avere vFXT 구성 인터페이스(Avere 제어판)에 연결하여 클러스터 설정을 사용자 지정합니다. 지원 모니터링을 선택하고, 온-프레미스 데이터 센터를 사용하는 경우 저장소 시스템을 추가합니다.

   * [vFXT 클러스터에 액세스](avere-vfxt-cluster-gui.md)
   * [지원 사용](avere-vfxt-enable-support.md)
   * [저장소 구성](avere-vfxt-add-storage.md)(필요한 경우)

1. 클라이언트 탑재

   [Avere vFXT 클러스터 탑재](avere-vfxt-mount-clients.md)의 지침에 따라 부하 분산과 클라이언트 머신에서 클러스터를 탑재하는 방법을 알아봅니다.

1. 데이터 추가(필요한 경우)

   Avere vFXT는 확장 가능한 다중 클라이언트 캐시이므로 데이터를 새 백 엔드 저장소 컨테이너로 이동하려면 다중 클라이언트, 다중 스레드 파일 복사 전략을 사용하는 것이 가장 좋은 방법입니다. 자세한 내용은 [vFXT 클러스터로 데이터 이동](avere-vfxt-data-ingest.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

[Avere vFXT 만들기 준비](avere-vfxt-prereqs.md)로 계속 진행하여 Avere vFXT for Azure를 배포하기 위한 예비 작업을 수행합니다. 