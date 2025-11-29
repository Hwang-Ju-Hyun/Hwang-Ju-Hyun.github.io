---
layout: post
title: MyBomberMan
tags: [Test, Color]
color: brown
/*author: sylhare*/
categories: Example
feature-img: "assets/img/feature-img/bom.png"
thumbnail: "assets/img/thumbnails/feature-img/bom.png"
badges: 
  - label: "OpenGL"
    color: "#00BFFF" 
  - label: "GLFW"
    color: "#20B2AA" 
  - label: "IMGUI"
    color: "#00599C" 	
  - label: "JSON"
    color: "#ff3e3e"
excerpt_separator: <!--more-->
---

<div style="display: flex; gap: 20px; flex-wrap: wrap; margin-bottom: 20px;">
  <div style="flex: 1; min-width: 250px; border: 1px solid #444; border-radius: 8px; padding: 15px; background-color: #1c1c1c; color: #fff;">
    <h3>About</h3>
    <p>
      <strong>MyBomberMan</strong>은 솔로 프로젝트로 진행된 2D TopDown 게임입니다.<br>
      본 프로젝트는 <strong>OpenGL</strong> 그래픽스 API만을 활용하여 게임을 구현했습니다.<br>
      처음으로 그래픽스 API를 통해 만든 게임이였습니다.기본적으로 커스텀 게임 프레임워크를 구축하였고,
      OpenGL을 활용한 <strong>2D 렌더링</strong>과 <strong>입출력 처리</strong>와 <strong>Serializer</strong> 활용 등등 그래픽스와 게임 로직 전반을 직접 구현했습니다.	  
	  <br>게임 시스템을 직접 구축하며 <strong>문제해결 능력</strong>과 <strong>성능 관리 경험</strong>을 쌓을 수 있는 프로젝트였습니다.	  
	  <br>      
    </p>
  </div>

  <div style="flex: 1; min-width: 250px; border: 1px solid #444; border-radius: 8px; padding: 15px; background-color: #1c1c1c; color: #fff;">
    <h3>Project Info</h3>
    <ul>
      <li><strong>Role:</strong> Programmer</li><br>
      <li><strong>Team Size:</strong> 1명 </li><br>
      <li><strong>Time Frame:</strong> 1개월</li><br>
      <li><strong>Engine:</strong> Custom Engine</li><br>
      <li><strong>Tools & Libraries:</strong><br>
	  - ImGUI<br>
	  - OpenGL (렌더링)</li>
    </ul>
  </div>
</div>

---

<div style="text-align: center; margin: 30px 0;">
  <img src="/assets/BOMB.gif" alt="Pathfinding" 
       style="width: 80%; max-width: 1200px; height: auto; border-radius: 8px;" />
  <p style="color: #aaa; font-size: 0.9em;"></p>
</div>

## 주요 구현 내용
### 1. 컴포넌트 기반 게임 엔진 구조 설계 및 구축
- **GameObject–Component** 아키텍처를 직접 설계하여 Transform,Collider등 핵심 컴포넌트를 구현

### 2. 렌더링 파이프라인 구현
- 직접 Shader(정점/프래그먼트) 작성 및 VAO/VBO 기반의 **2D 렌더링 파이프라인 구축**
OpenGL을 활용한 2D 이미지 출력

### 3. 입력 및 게임 로직 처리
- 키보드 입력, 캐릭터 이동, 폭탄 설치 및 폭발 로직 구현
**폭탄 설치 → 타이머 → 범위 판정 → 블록 파괴/피격** 처리까지 일련의 게임플레이 로직 구현

### 4. 맵 에디터 구현
- **Serializer**를 활용하여 **맵 데이터를 저장/로드**할 수 있는 시스템을 구축하고<br>
-**ImGui**를 통해 직접 맵을 편집하며 게임 레벨을 제작
- 오브젝트 타입, 스폰 포인트, 충돌 영역 등을 시각적으로 편집하고 JSON 포맷으로 바로 저장/로드
---


<div style="text-align: center; margin: 30px 0;">
  <img src="/assets/edit.gif" alt="Pathfinding" 
       style="width: 80%; max-width: 1200px; height: auto; border-radius: 8px;" />
  <p style="color: #aaa; font-size: 0.9em;">▲ 실시간 맵 편집</p>
</div>


# 어려웠던 점과 해결 방안
###  실시간 입력 처리와 동시 입력 문제

#### 문제 
- 초기 입력 시스템은 각 키와 마우스 버튼의 상태를 개별 **bool 변수**로 관리하고, Press/Release 이벤트만 처리.

- **동시 입력** 시 일부 키가 **무시**되거나 Hold 상태가 정확히 판단되지 않아 캐릭터 이동이 비정상적으로 동작.

- 키별 상태를 반복되는 **switch/case 구조**로 처리하여 **코드 중복**이 많고 **유지보수**가 어려움.

- 새로운 키를 추가할 때마다 수많은 코드를 수정해야 했음.

#### 원인
- 상태 관리 단순화: 각 키를 개별 bool로 관리 → 동시에 여러 키가 눌릴 경우 **마지막 처리 키만 상태 반영**

- **Hold 상태 미지원**: Press/Release만 판단 → 지속 입력 상태 판단 불가

- **코드 구조 문제**: **switch/case** 반복문과 개별 배열 병행 → 키 간 **동기화 불가**, **유지보수 어려움**

#### 문제 코드
````cpp
if (_action == GLFW_PRESS)
{     
    switch (_key)
    {
    case GLFW_KEY_W:
        m_bWKeyPressed = true;
        m_vKeyArr[(int)KEY::W] = true;
        break;
    case GLFW_KEY_D:
        m_bDKeyPressed = true;
        m_vKeyArr[(int)KEY::D] = true;
        break;
    //키가 늘어갈때마다 늘어가는 case 코드들
    ...
    }
}
else if(_action==GLFW_RELEASE) //키가 눌려있는 상태에서 키를 땟을 때
{
    //키가눌렸다면 false로 변환
    if (m_bWKeyPressed)
    {        
        m_bWKeyPressed = false;
        m_bWKeyReleased = true;
        m_vKeyArr[(int)KEY::W] = false;
    }
    if (m_bDKeyPressed)
    {
        m_bDKeyPressed = false;
        m_bDKeyReleased = true;
        m_vKeyArr[(int)KEY::D] = false;
    }
    //키가 늘어갈때마다 늘어가는 if 코드들
    ...
}

````

#### 해결
- 모든 키와 마우스 입력을 **`unordered_map`으로 관리** → 키 추가/삭제 용이, O(1) 접근 속도 확보
- **Update()** 단계(매 프레임)에서 현재/이전 상태를 비교하여 **Press/Hold/Release**를 계산
- **반복문으로 상태를 갱신**하여 **동시 입력** 시에도 모든 키 상태가 정확히 처리


#### 개선된 코드

````cpp

struct KeyInfo
{
    bool bHeld = false;                //현재 눌려 있는 상태 
    bool bPrevHeld = false;            // 이전 프레임 눌림 여부 
    KEY_STATE eState = KEY_STATE::NONE;// 최종 Key 상태
};

class InputManager
{
public:
    SINGLETON(InputManager);
private:
    static std::unordered_map<int, KeyInfo> m_mKeyState;       
public:
    void Init();
    void Update();        
    inline KEY_STATE GetKeyCode(int _key){ return m_mKeyState[_key].eState; }    
};

// Update()에서 상태 자동 계산
void Update()
{
    // m_mKeyState: 모든 키에 대한 상태를 저장한 unordered_map<int, KeyInfo>
    // key = 키 코드, info = 해당 키의 상태 정보
    for (auto iter = m_mKeyState.begin(); iter != m_mKeyState.end(); iter++)
    {
        int key = iter->first;        // 현재 순회 중인 키 코드
        KeyInfo& info = iter->second; // 현재 키의 상태 정보 참조

        info.eState = NONE;            // 매 프레임 초기화: 기본 상태를 NONE으로 설정

        if (!info.bPrevHeld && info.bHeld)
        {
            info.eState = PRESS;
        }
        else if (info.bPrevHeld && info.bHeld)
        {
            info.eState = HOLD;
        }
        else if (info.bPrevHeld && !info.bHeld)
        {
            info.eState = RELEASE;
        }
        // 현재 상태를 이전 상태로 저장: 다음 프레임 비교용
        info.bPrevHeld = info.bHeld;
    }
}
````

---

# 배운점
- **Prefab** 시스템을 학습하고 이해하였고 그것을 활용한 **동적 오브젝트** 생성(폭탄, 폭탄 Fragmenet)<br>
- **Serializer 기법**을 학습하여 **JSON Serializer**를 이용해 **외부 데이터**에서 **오브젝트 정보**를 로드하고, ImGui를 통해 맵을 실시간으로 구성 및 편집
- OpenGL과 같은 낮은 수준의 그래픽스 API를 사용해 게임을 구현하면서<br>
조금만 개념을 잘못 이해하거나 흐름을 놓쳐도 쉽게 버그가 발생한다는 사실을 체감했습니다. 이 과정에서 각 시스템과 데이터 흐름을 꼼꼼하게 관리하고, 세밀한 디버깅과 검증이 얼마나 중요한지 깨달았고, 
이를 통해 **설계**와 **문제해결** 또는 **구현**에서의 신중함과 정확성의 가치를 크게 배웠습니다.

---

# 느낀 점
- 하나의 클래스나 모듈에 **너무 많은 책임**을 부여하면 **유지보수**가 어려워지고 **버그 발생** 확률이 높아진다는 것을 경험하며
**SPR 원칙**의 중요성을 깨달았습니다.<br>

- 직접 설계한 게임 시스템을 구현하면서, 코드 한 줄 한 줄에도 **이유와 근거**가 필요하다는 것을 절실히 느꼈습니다.<br>
제가 작성한 코드 중에는 주석이나 기록이 부족하여, 며칠 지나 다시 보면 왜 이렇게 작성했는지, 어떤 부작용을 고려했는지 기억이 잘 나지 않는 경우가 많았습니다.<br>

- 그래서 프로그래밍은 단순히 기능을 구현하는 것이 아니라, **설계 의도와 구조**를 꼼꼼히 **기록**하며 관리해야 하는 작업임을 몸소 체험했습니다.

---