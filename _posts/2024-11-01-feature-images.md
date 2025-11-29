---
layout: post
title: "Normal Mapping"
feature-img: "/assets/nm.png"
thumbnail: /assets/nm.png
badges:
  - label: OpenGL
    color: "#00BFFF"  
  - label: GLFW
    color: "#20B2AA"  
  - label: Normal Mapping
    color: "#6F42C1"
categories: coursework


---

## 소개
이 프로젝트에서는 OpenGL을 활용하여 **3D Normal Mapping**을 구현했습니다.
**VAO(Vertex Array Object)**와 **VBO(Vertex Buffer Object)**를 사용하여 3D 오브젝트를 구성하고, 정점 데이터와 텍스처 좌표, 노멀 벡터를 GPU에 전달했습니다.
셰이더를 통해 Normal Map을 적용하여 평면 텍스처만으로도 입체적인 조명 효과를 구현했고, **조명 계산(Phong shading)**과 **Tangent Space** 계산을 통해 표면의 세부 굴곡까지 사실적으로 표현할 수 있었습니다.
이 과정을 통해 GPU 기반 그래픽 파이프라인 이해와 셰이더 프로그래밍 경험을 쌓을 수 있었습니다.

---

## 주요 구현 내용
### 1. 3D 오브젝트 생성 및 버퍼 구성
**VAO(Vertex Array Object)**와 **VBO(Vertex Buffer Object)**를 사용하여 3D 모델 데이터를 GPU로 전달
정점 위치(Position), 법선(Normal), 텍스처 좌표(TexCoord), 탄젠트(Tangent) 데이터를 포함

### 2. Normal Map 적용
Normal Map 텍스처를 셰이더에 바인딩
텍스처 기반으로 표면 굴곡을 계산하여 조명에 반영

### 3. 셰이더를 이용한 조명 계산
**Phong shading** 기반 조명 계산 적용
**Tangent Space**에서 Normal Map을 변환하여 조명 방향에 맞게 처리

---

<iframe width="960" height="515" src="https://www.youtube.com/embed/CbCPShVyAdw" 
        title="Normal Mapping" frameborder="0" 
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
        allowfullscreen></iframe>

---