# 📌 PJT-Video-Inpainting

<p align='center'><img src="assets/A01.png" width="1000"></p>

10개월간 진행되는 [이어드림스쿨 AI 교육 과정(DS트랙 3기, 2023)](https://yeardream.ninehire.site/)의 피날레라고 할 수 있는, 인턴 수준의 기업 연계 프로젝트에 참여했다. 이 프로젝트를 수행하기 위한 인페인팅 기술의 3단계는 객체 마스킹, 마스킹 추적, 인페인팅 순서이며, 기술 구현을 위해 특정한 객체를 감지해서 분할하는 Meta의 SAM(Segment Anything Models)과 앞서 특정한 객체를 끊김이 없이 추적하는 DeAOT(Decoupling features in Associating Objects with Transformers) 그리고 동영상 내에 모든 마스킹된 대상을 인페인팅 하는 E2FGVI(End-to-End Framework for Flow-Guided Video Inpaintihg)의 알고리즘과 Gradio 라이브러리를 활용한 GUI로 최종 결과물을 도출했다. 

<br><br>
## 01 Project Overview
[기업연계프로젝트] 동영상 내에 특정 로고를 인페인팅 함으로써, 광고 산업 뿐만 아니라 영상 제작 시 방송 규정을 준수해야 하는 부분 또는 제작 기간 한계에 따른 문제 등 비즈니스 최적화에 목표로 두었다.

<img src="assets/A02.gif" width="460">
<인페인팅기술을 활용한 VFX 영화 'Everything everywhere all at once, 2022'>
<br><br>

- 📅 진행기간: 2023/11/06 ~ 2023/12/15 (5주)
- 🤝 연계기업: [(주)커넥트브릭](https://connectbrick.com/)
- 🚣‍♀️ Team Inpainterz(5명): [강도성](https://github.com/kang952175), [경소현](https://github.com/SohyeonGyeong), [변웅진](https://github.com/1ncarnati0n), [지경호](https://github.com/zkhshub) 그리고 [**손수진**](https://github.com/Soosembly)

|팀 원|역 할|
|:--:|--|
|강도성|논문 리뷰, 오픈소스 리서치, 서버 관리, 코드 및 프로그래밍, Gradio GUI, 중간발표|
|경소현|논문 리뷰, 오픈소스 리서치, Gradio GUI|
|변웅진|논문 리뷰, 오픈소스 리서치, Gradio GUI, 키노트 프레젠테이션준비, 현장발표|
|**손수진**|논문 리뷰, 오픈소스 리서치, Gradio GUI, 키노트 프레젠테이션준비|
|지경호|논문 리뷰, 오픈소스 리서치, 서버 관리, 코드 및 프로그래밍, Gradio GUi, 최종발표|


<br><br>
## 02 Tech Stack and Dataset Description
Meta에서 개발된 객체 분할 및 세그멘테이션 모델로, 제로샷 러닝을 활용하여 이미지와 비디오에서 다양한 객체를 정확하게 식별하고 분리하는 [**SAM**(Segment Anything Models)](https://github.com/facebookresearch/segment-anything)과 객체 추적 및 연관성 부여 작업을 위해 트랜스포머 아키텍처를 활용하는 모델로, 다중 객체 추적과 연관성 부여를 개선하는 [**DeAOT**(Decoupling features in Associating Objects with Transformers)](https://github.com/yoxu515/aot-benchmark) 그리고 비디오 인페인팅 작업을 위한 종합 프레임워크로, 영상에서 누락된 부분을 자연스럽게 보정하여 누락된 부분을 채우는 [**E2FGVI** (End-to-End Framework for Flow-Guided Video Inpainting)](https://github.com/MCG-NKU/E2FGVI)등을 선별하여 적용했다.

본 프로젝트에서는 인페인팅 기술 구현을 위해 [SAM], [DeAOT], [E2FGVI] 등의 모델 알고리즘을 활용하였다. 이러한 접근 방식은 특정한 데이터셋에 의존하지 않고, 각 모델의 기존 학습된 능력을 바탕으로 이미지와 비디오에서의 객체 식별, 추적, 및 인페인팅 작업을 수행한다. 따라서, 별도의 데이터셋을 구축하거나 사용하지 않고, 모델들이 제공하는 기능을 최대한 활용하여 프로젝트 목표를 달성하였다.

<br><br>
## 03 Stages of project progress
사이클 브랜드 라파 광고 장면 중에서 선수의 상의 측면에 있는 로고(특정한 객체)를 인페인팅 하는 과정
<p align="center"> <img src="assets/readme00.png" width="1080"> </p>

- **SAM** Segmentation & Masking
  </br>동영상에서 인페인팅할 객체를 선택하고 정확하게 판별(분리)하기 위해 Segmentation 기법을 사용
  </br>해당 부분이 누락된 것처럼 분할된 객체를 Masking하여 인페인팅 알고리즘이 수행할 수 있게 한다.
  
- **DeAOT** Tracking, use Long-term Memory
  </br>Long-term Memory으로 Masking된 객체가 특정 프레임 내에서 따라 움직이는 것을 연속적으로 Tracking & Masking을 수행하여
  </br>동영상 내에 모든 마스킹 이미지를 추출한다.
  
- **E2FGVI** Inpainting
  </br>Input 값으로 Masking된 영상을 넣으면 복원해야하는 누락된 지점으로 인식한다.
  </br>이 과정에서 알고리즘은 주변의 픽셀 정보로 누락된 부분의 색생과 텍스처 등을 추정하고 채운다.
  
- **Gradio** 라이브러리를 이용해 인페인팅 GUI를 구성함

<br><br>
## 04 Project Details Course
-
-

<br><br>
## 05 Project Results
-
-

<br><br>
## 06 Project Retrospective
-
-
