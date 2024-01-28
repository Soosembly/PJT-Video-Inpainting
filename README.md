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

**1. SAM** Segmentation & Masking
  </br>동영상에서 인페인팅할 객체를 선택하고 정확하게 판별(분리)하기 위해 Segmentation 기법을 사용
  </br>해당 부분이 누락된 것처럼 분할된 객체를 Masking하여 인페인팅 알고리즘이 수행할 수 있게 한다.
  
**2. DeAOT** Tracking, use Long-term Memory
  </br>Long-term Memory으로 Masking된 객체가 특정 프레임 내에서 따라 움직이는 것을 연속적으로 Tracking & Masking을 수행하여
  </br>동영상 내에 모든 마스킹 이미지를 추출한다.

**3. E2FGVI** Inpainting
  </br>Input 값으로 Masking된 영상을 넣으면 복원해야하는 누락된 지점으로 인식한다.
  </br>이 과정에서 알고리즘은 주변의 픽셀 정보로 누락된 부분의 색생과 텍스처 등을 추정하고 채운다.

**4. Gradio** 라이브러리를 이용해 인페인팅 GUI를 구성함

<br><br>
## 04 Project Details Course
### SAM
<details>
<summary> Segment Anything Model </summary> 
	
 📑 [**Paper**](https://ai.meta.com/research/publications/segment-anything/) 


대규모 데이터셋이 구축되지 않았던 기존의 Segmentation 작업은 매번 학습에 소모되는 시간과 비용이 너무 크다는 문제가 있었습니다. NLP 분야의 LLM처럼, **Zero-shot**이 가능한 모델을 만들수 없을까 했고, 2023년 4월 Meta에서는 Image Segmentation계의 **Foundation** 모델을 만드는 것을 목표로 이 모델을 발표했습니다.

Meta는 다음 세 가지를 새롭게 선보였습니다. **Task**, **Model**, **Data**.
1. **Task** ( Promptable Segmentation Task )\
	Segment Anything Task의 핵심은 **프롬프팅이 가능**하다는 것.\
	원하는 영역의 **Point**나 **Box** 또는 **자연어**, (+ **Mask**)로 구성된 프롬프트를 입력하면, 아무리 모호한 정보일지라도 유효한 Segmentation Mask를 출력한다.
	<p align="center"><img src="assets/readme01.png" width="360"></p>
 
2. **Model** ( Segment Anything Model, SAM )\
	이를 위한 모델인 SAM은 **두 개의 인코더**와 **하나의 디코더**로 구성.
	Image Encoder와 Prompt Encoder로부터 온 임베딩 정보를 매핑해 Mask Decoder가 예측된 Segmentation Mask를 출력하는 구조.\
	Mask Decoder는 Transformer의 Decoder를 조금 수정한 것으로, 이미지 임베딩과 프롬프트 임베딩을 모두 업데이트 하기 위해 **Self-Attention**과 **Cross-Attention**을 양방향으로 활용.\
	SAM의 Prompt Encoder와 Mask Decoder는 **가볍고 빠름**.\
	같은 이미지 임베딩이 여러 개의 프롬프트와 함께 재사용되기 때문에, CPU 환경의 웹 상에서 50ms 이하의 속도로 Mask를 예측할 수 있음.
	<p align="center"><img src="assets/readme02.gif" width="360"></p>

3. **Data** ( Segment Anythin Data Engine, SA-1B Dataset )\
	Foundation 모델 개발에 있어 가장 중요한 것은 대규모 데이터셋.\
	Segment Anything은 자체적인 **Data Engine**을 개발했고, 그 결과 10억 개의 Mask를 가진 **SA-1B** 데이터셋이 탄생했다.

	<p align="center"><img src="assets/readme03.gif" width="520"></p>

</details>

</br>

### DeAOT
<details>
<summary> Decoupling Features in Associating Objects with Transformers </summary> 

📑 [**Paper**](https://arxiv.org/abs/2210.09782)

비디오 내의 객체들을 세밀하게 구분하는 'semi-supervised 비디오 객체 세분화(VOS, Video Object Segmentation)'에 관한 모델입니다. 특히, 비전트랜스포머를 사용, 'AOT(Associating Objects with Transformers)'라는 방법을 통해 VOS 문제를 해결하는 데 집중하고 있습니다. 

이전 프레임에서 현재 프레임으로 정보를 차례대로 전달하는 '계층적 전파 hierarchical propagation' 방식을 사용하며, 이 방식은 각 객체의 정보를 점진적으로 전달하지만, 깊은 층에서는 일부 시각적 정보가 손실될 수 있는 단점이 있습니다. 

이 문제를 해결하기 위해, 연구자들은 'DeAOT'라는 새로운 접근 방식을 제안합니다. DeAOT는 객체별 정보와 무관한 정보를 분리하여 처리함으로써 보다 효율적인 정보 전달을 가능하게 합니다. 또한, 이 방법은 추가적인 계산 부담을 줄이기 위해 특별히 설계된 '게이트 전파 모듈 Gated Propagation Module(GPM)'을 사용합니다. 

결과적으로, DeAOT는 기존 AOT 및 다른 방식의 모델인 XMem보다 뛰어난 정확도 및 효율성을 보여줍니다.

<p align="center"><img src="assets/readme05.png" width="360"></p>

다시 정리하면 DeAOT는 두 개의 독립된 branch를 통해서 객체의 visual features와 mask features의 정보를 계층적 전파를 하는 방식입니다.\
Visual branch는 패치별 시각적 임베딩에 대한 attention map을 계산하여 객체를 일치시키는 역할을 하며 ID Branch는 객체별 정보를 과거 프레임에서 현재 프레임으로 전파하기 위한 역할을 합니다. 

<p align="center"><img src="assets/readme05.gif" width="420"></p>

</details>
</br>

### E2FGVI
<details>
<summary> End-to-End Framework for Flow-Guided Video Inpainting </summary> 
	
📑 [**Paper**](https://arxiv.org/abs/2204.02663)

마스킹 된 영역(Target object e.g. 특정 로고 etc.)을 영상의 flow와 사전 학습된 feature들을 이용해 Feature propagation과 Hallucination으로 Inpainting 역할을 하는 모델입니다.

1. **기존방법: Flow-based methods**
- 이런 일반적인 흐름기반 방법(flow-based method)는 인페인팅을 **pixel propagation** 문제로 생각하여 시간적 일관성을 자연스럽게 보존

	1. flow completion : 
	   손상된 영역에 flow field가 없으면 후자의 프로세스에 영향을 미치므로 먼저 추정된 optical flow가 먼저 완료 되어야 한다
	2. pixel propagation : 
	   앞서 완성된 optical flow의 가이드에 따라 가시영역의 픽셀을 양방향으로 전파, 손상된 비디오 영역을 채움 
	3. content hallucination : 
	   픽셀 전파 후, 나머지 누락된 영역은 사전 학습된 이미지 인페인팅 네트워크로 환각으로 채움
	
	-  인페인팅의 방법은 전체 인페인팅 파이프라인을 구성하기 위해 개별적으로 적용, 인상적인 결과를 얻을 수 있지만, 처음 두 단계에서는 많은 수작업이 필요해서, **각 프로세스는 별도로 수행**해야 하는 **단점**이 있다.
	
	- 따라서, 두 가지 주요한 문제를 야기한다.
	    a. **이전 단계에서 발생한 오류가** 누적 후속 단계에서 증폭, **최종 성능에 큰 영향을 미침**
	    b. **복잡한 연산**을 해야하지만, GPU acceleration 처리불가, **많은 시간이 소요**

	<p align="center"><img src="assets/readme04.png" width="360"></p>

2. **개선모델: E2FGVI** (**Fig. Ours**) 
- 이전 모델을 보완, 이전 방법과 다르게 **End-to-End**로 최적화 할 수 있어 보다 효율, 효과적인 인페인팅 프로세스 구현.
	
	1. **Flow-Completion** 모듈: 	
	   복잡한 단계 대신 원-스텝 완성을 위해 마스킹 된 비디오에 직접 적용
	2. **Feature Propagation** 모듈: 
	   pixel-level propagation과 달리, flow-guided propagation 프로세스는 (변형이 가능한 convolution의 도움을 받아서) feature space 수행됨. 
	   → 학습 가능한 sampling offset과 feature-level 연산 통해 **정확하지 않은 flow추정 부담을 덜어줌**
	3. **Content Hallucination** 모듈: 
	   공간과 시간적 차원에서 장거리 종속성을 효과적으로 모델링하기 위해 temporal focal transformer를 제안.
	   →이 모듈에서 local and non-local temporal neighbors을 모두 고려, **보다 시간적으로 일관된 인페인팅 결과** 도출

- 70개의 프레임 기준으로 이 크기의 비디오 하나를 완성하는 데에 약 4분 소요. E2FGVI는 프레임당 0.12초로 약 8.4초 소요.
 	<p align="center"><img src="assets/readme06.png" width="360"></p>

</details>

<br>

### GUI
<details>
<summary> Gradio </summary>
	
**gradio** 라이브러리를 활용하여 SAM, DeAOT, E2FGVI 세가지 모델을 통합해서 **한 화면구성 안에서** 3가지 단계가 **모두 동작** 할 수 있도록 설계했다.

<p align="center"> <b>전체 GUI</b> </p>

<p align="center"><img src="assets/gui1.png" width="540"></p>

<p align="center"> <b>하이퍼파라미터</b> 조절 블록 </p>

<p align="center"><img src="assets/gui2.png" width="360"></p>

<br>

### Step 1. SAM 
비디오를 로드하고 영상의 첫 프레임에서 인페인팅 하고자하는 로고를 포인트 프롬프트로 선택하여 마스킹을 한다. 아래 영상에서는 2개의 포인트 프롬프트로 전체 로고 선택이 가능했다.

<p align="center"><img src="assets/gui3.gif" width="720"></p>

### Step 2. DeAOT

첫 프레임에서 마스킹된 로고를 나머지 프레임에서 자동 추적하여 마스킹하기위해 Tracking을 실행한다.
<p align="center"><img src="assets/gui4.gif" width="720"></p>

### Step 3. E2FGVI

영상 내 모든 프레임에서 마스킹된 로고를 인페인팅하여 결과물을 다운로드한다. 
<p align="center"><img src="assets/gui5.gif" width="720"></p>

</details>

<br><br>
## 05 Project Results

<p align="center">
<img src="assets/r_baloon.gif" height="100">
<img src="assets/r_baloon_seg.gif" height="200">
<img src="assets/r_baloon_inpainted.gif" height="200">
</p>



<p align="center">
<img src="assets/r_MarineDebris.gif" height="100">
<img src="assets/r_MarineDebris_seg.gif" height="200">
<img src="assets/r_MarineDebris_inpainted.gif" height="200">
</p>

<p align="center">
<img src="assets/r_logo.gif" height="100">
<img src="assets/r_logo_seg.gif" height="200">
<img src="assets/r_logo_inpainted.gif" height="200">
</p>

<br>



<br><br>
## 06 Project Retrospective
-
-
