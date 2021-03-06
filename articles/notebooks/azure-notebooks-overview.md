---
title: Azure Notebooks 개요
description: 설정 또는 구성이 필요하지 않은 경우 무료 Azure Notebooks 서비스를 사용하여 클라우드에서 Jupyter Notebook을 실행합니다.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 9cea5a8e-c52d-4bdc-9e4a-cecdc1ad02c1
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 92cfdea12dc51884ea04ed894a79ded7f532d88b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085402"
---
# <a name="overview-of-azure-notebooks"></a>Azure Notebooks 개요

Azure Notebooks는 무료 호스티드 서비스이며 설치 없이 클라우드에서 Jupyter Notebook을 개발하고 실행합니다. [Jupyter](https://jupyter.org/)(이전의 IPython)는 오픈 소스 프로젝트이며 이를 통해 Markdown 텍스트, 실행 코드, 영구 데이터, 그래픽 및 시각화를 공유 가능한 단일 캔버스인 *Notebook*(jupyter.org의 이미지 허가)으로 손쉽게 결합할 수 있습니다.

[![](https://jupyter.org/assets/jupyterpreview.png "Jupyter Notebook")](https://jupyter.org/assets/jupyterpreview.png#lightbox)

이 강력한 코드, 그래픽 및 설명 텍스트의 조합으로 인해 Jupyter는 데이터 과학 교육, 데이터 정리 및 변환, 숫자 시뮬레이션, 통계 모델링 및 기계 학습 모델의 개발을 비롯한 다양한 용도에 대중화되었습니다.

## <a name="hassle-free-experience"></a>간편한 환경

Azure Notebooks를 통해 프로토타입, 데이터 과학, 학술 연구 또는 Python 프로그래밍 학습을 빠르게 시작할 수 있습니다.

- 데이터 과학자는 설치 없이 전체 Anaconda 환경에 즉시 액세스할 수 있습니다.
- 교사는 학생들에게 간편한 Python 환경을 제공할 수 있습니다.
- 프레젠터는 참석자에게 소프트웨어를 설치하는 데 45분을 소요하도록 요청하지 않고 토크 또는 웹 세미나를 제공할 수 있습니다.
- 개발자 또는 초보 개발자는 빠른 코드 스크래치패드로 Notebook을 사용할 수 있습니다.

Notebook은 사용자가 Azure Notebooks와 같은 브라우저에서 액세스할 수 있는 클라우드 서비스를 통해 공동 작업할 수 있을 때 더욱 강력해집니다. 클라우드에서 사용자는 Jupyter를 로컬로 설치하거나 환경 유지 관리에 관여할 필요가 없습니다. 또한 원본 제어 리포지토리와 같은 외부 수단을 통해 Notebook을 공유하는 복잡성을 피해 클라우드를 통해 다른 권한 있는 사용자와 간단하게 Notebook(및 연결된 데이터 파일)을 공유할 수 있습니다. Azure Notebooks를 사용하여 사용자는 수정 또는 설명을 위해 자신의 계정으로 Notebook을 복사(또는 "복제")할 수도 있습니다. 이는 설명을 위한 용도에 특히 유용합니다.

Azure Notebooks는 일반적인 코드 작성, 실행 및 공유 플랫폼이므로 다양한 시나리오에 사용할 수 있습니다.

- 새 프로그래밍 언어 알아보기 - [FrontPage 자습서](https://notebooks.azure.com/Microsoft/projects/samples/html/Introduction%20to%20Python.ipynb) 중 하나 시도하기
- 데이터 과학 알아보기 - [Jake VanderPlas 책](https://notebooks.azure.com/jakevdp/projects/PythonDataScienceHandbook) 시도하기
- 수백 명의 학생에게 [강좌 가르치기](https://notebooks.azure.com/garth-wells/projects/CUED-IA-Computing-Michaelmas)
- 설치에 시간을 소비하지 않고 온라인 또는 회의에서 웹 세미나 제공 
- GitHub 사용자가 [GitHub 시작 배지를 만들어](https://notebooks.azure.com/help/projects/sharing/create-a-github-badge) Notebook을 직접 로드하고 실행하도록 활성화
- 슬라이드의 코드를 실행할 수 있는 [슬라이드 쇼와 같은 PowerPoint](https://notebooks.azure.com/help/jupyter-notebooks/slides) 제공

즉, Azure Notebooks를 통해 작업을 보다 효율적으로 수행할 수 있으므로 더 많이 성취할 수 있습니다.

> [!Note]
> Jupyter 자체에 대한 자세한 정보는 [jupyter.org](https://jupyter.org/) 및 [Jupyter 설명서](https://jupyter-notebook.readthedocs.io/en/latest/)에서 확인할 수 있습니다.

## <a name="pricing-and-quotas"></a>가격 책정 및 할당량

Azure Notebooks는 무료 서비스이지만 각 프로젝트는 남용을 방지하기 위해 4GB 메모리 및 1GB 데이터로 제한됩니다. 이러한 한도를 초과하는 합법적인 사용자는 Captcha 챌린지를 참조하여 Notebook 실행을 계속하세요.

모든 한도를 해제하려면 프리미엄 컴퓨팅 계층을 선택할 수 있는 Azure 구독으로 계정을 연결합니다.

## <a name="available-kernels-and-environments"></a>사용할 수 있는 커널 및 환경

각 Notebook의 경우 모든 코드 셀을 실행하는 데 사용되는 커널(즉, 런타임 환경)을 선택합니다. Azure Notebooks는 다음 커널을 지원합니다.

- Python 2.7 + Anaconda2-5.3.0
- Python 3.6 + Anaconda3-5.3.0
- Python 3.5 + Anaconda3-4.2.0(지원 중단 예정)
- R 3.3 + Microsoft R Open 3.3.0
- F# 4.1.9

Azure Notebooks는 기본 배포 외에 추가 패키지도 포함합니다. Python 커널은 예를 들어 numpy, pandas, scikit-learn, matplotlib 및 bokeh 라이브러리를 포함합니다.

해당 프로젝트의 모든 노트북에 대한 환경을 만들도록 프로젝트를 사용자 지정할 수도 있습니다. 자세한 내용은 [빠른 시작: 사용자 지정 환경을 사용하여 프로젝트 만들기](quickstart-create-jupyter-notebook-project-environment.md)를 참조하세요.

기본 배포 외에도 Azure Notebooks는 데이터 과학자에게 유용한 많은 추가 패키지의 사전 설치를 함께 제공합니다. 또한 각 언어에 대한 일반적인 프로세스를 사용하여 사용자 고유의 패키지를 설치할 수도 있습니다.

## <a name="pre-configured-jupyter-extensions"></a>미리 구성된 Jupyter 확장

Azure Notebooks는 다음 Jupyter 확장을 사용하여 미리 구성됩니다.

- [RISE](https://github.com/damianavila/RISE): Jupyter 슬라이드 쇼 확장(live_reveal이라고도 함)입니다. 자세한 내용은 [Notebook 슬라이드 쇼 실행](present-jupyter-notebooks-slideshow.md)을 참조하세요.
- [JupyterLab](https://github.com/jupyterlab/jupyterlab): Jupyter Notebook과 함께 작업하기 위한 전체 계산 환경입니다.
- [Altair](https://github.com/ellisonbg/altair): Python에 대한 선언적 통계 시각화 라이브러리입니다.
- [BQPlot](https://github.com/bloomberg/bqplot): Jupyter Notebook에 대한 대화형 플로팅 프레임워크입니다.
- [IpyWidgets](https://github.com/jupyter-widgets/ipywidgets): Jupyter Notebook에 대한 대화형 HTML 위젯입니다.

## <a name="issues-and-getting-help"></a>문제 및 도움말 보기

Azure Notebooks는 여전히 미리 보기 상태에 있으므로 서비스에 다른 Azure 서비스에 비해 더 빈번하거나 오래 지속될 수 있는 임시 중단이 발생할 수 있습니다. 일부 기능은 완료되지 않았거나 버그를 포함할 수 있습니다.

현재 중요 비즈니스용 애플리케이션 또는 중요한 Notebook 및 데이터에 Azure Notebooks 미리 보기를 사용하지 않는 것이 좋습니다.

Azure Notebooks에 대한 질문을 논의하려면 [GitHub 리포지토리](https://github.com/Microsoft/AzureNotebooks/issues)에서 문제를 제출하세요.

## <a name="next-steps"></a>다음 단계  

- [샘플 Notebook 살펴보기](azure-notebooks-samples.md)

- 빠른 시작:

  - [Notebook 만들기 및 공유](quickstart-create-share-jupyter-notebook.md)
  - [Notebook 복제](quickstart-clone-jupyter-notebook.md)
  - [로컬 Jupyter Notebook 마이그레이션](quickstart-migrate-local-jupyter-notebook.md)
  - [사용자 지정 환경 사용](quickstart-create-jupyter-notebook-project-environment.md)
  - [로그인 및 사용자 ID 설정](quickstart-sign-in-azure-notebooks.md)

- 자습서:

  - [Notebook 만들기 및 실행](tutorial-create-run-jupyter-notebook.md  )

- 방법 문서:
  
  - [프로젝트 만들기 및 복제](create-clone-jupyter-notebooks.md)
  - [프로젝트 구성 및 관리](configure-manage-azure-notebooks-projects.md)
  - [Notebook 내에서 패키지 설치](install-packages-jupyter-notebook.md)
  - [슬라이드 쇼 제공](present-jupyter-notebooks-slideshow.md)
  - [데이터 파일 작업](work-with-project-data-files.md)
  - [데이터 리소스에 액세스](access-data-resources-jupyter-notebooks.md)
  - [Azure Machine Learning Services 사용](use-machine-learning-services-jupyter-notebooks.md)
