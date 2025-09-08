---
layout: post
title: Wothingthing
tags: [A Tag, Katex]
feature-img: "assets/img/feature-img/wothingthing.png"
thumbnail: "assets/img/thumbnails/feature-img/wothingthing.png"
badges: 
  - label: "Alpha Engine"
    color: "#6E7BFF"   
  - label: "JSON"
    color: "#ff3e3e"
excerpt_separator: <!--more-->
---

<div style="display: flex; gap: 20px; flex-wrap: wrap; margin-bottom: 20px;">
  <div style="flex: 1; min-width: 250px; border: 1px solid #444; border-radius: 8px; padding: 15px; background-color: #1c1c1c; color: #fff;">
    <h3>About</h3>
    <p>
      <strong>Wothingthing</strong>은 팀 프로젝트로 진행된 2D 횡스크롤 게임입니다.<br>
      본 프로젝트는 <strong>Alpha Engine</strong>이라는 <strong>2D렌더링 툴</strong>을 활용하여 게임을 구현했습니다.<br>
      이 경험을 통해 처음으로 팀과 협업하며 게임을 만들어보았습니다. 
	  <br>이 경험을 통해 팀과 협업할 시 서로의 의견을 조율해가는 능력을 기룰 수 있었고
	  또한,게임 내 2D 캐릭터 애니메이션 처리 과정, 충돌 처리, 컴포넌트 시스템 기능을 구현하며 문제 해결 능력을 기를 수 있었습니다.
	  <br>      
    </p>
  </div>

  <div style="flex: 1; min-width: 250px; border: 1px solid #444; border-radius: 8px; padding: 15px; background-color: #1c1c1c; color: #fff;">
    <h3>Project Info</h3>
    <ul>
      <li><strong>Role:</strong> Boss AI, GameFrameWork 구축,<br>LevelDesign / Team Leader</li><br>
      <li><strong>Team Size:</strong> 3명 (프로그래머 3명)</li><br>
      <li><strong>Time Frame:</strong> 1개월</li><br>
      <li><strong>Engine:</strong> Alpha Engine(Rendering Tool)</li>
    </ul>
  </div>
</div>

<iframe width="960" height="515" src="https://www.youtube.com/embed/xpTpa-FO4aM" 
        title="Wothingthing Demo" frameborder="0" 
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
        allowfullscreen></iframe>

---

## 소개
이 프로젝트는 **Alpha Engine(렌더링 툴)**을 사용하여 3명의 프로그래머로 구성되어 개발한 **2D 횡스크롤**게임입니다.
제가 미리 기초적인 GameFrameWork를 구성해놓은 상태에서 나머지 2명의 프로그래머들이 각자의 역할을 담당하며 프로젝트에 임하였습니다.
처음 게임 프로그래밍에서의 현업이였어서 많은 시행착오도 있었지만 동시에 현업할때에서의 소통의 중요성과 좋은 팀워크가 프로젝트에 얼마나 큰 영향을 미치는지 알게되었습니다.


---


## 주요 구현 내용

### 1. 게임 프레임워크 구축
- 오브젝트, 컴포넌트, 리소스 매니저 등 핵심 구조 설계

### 2. Boss몬스터 AI 설계
-  최단경로탐색 알고리즘,상태 머신(FSM)을 통한 행동 제어 구현

### 3. 파일 입출력을 통한 Level Design 제작
- JSON을 활용하여 외부 오브젝트 정보를 받아와서 각 스테이지 레벨을 디자인

---


# 어려웠던 점과 해결 방안

#### 문제
- 보스 AI의 경로 탐색(Pathfinding) 문제

#### 원인
- 타일 기반 맵 구조에서는 이동 가능 경로를 명확하게 정의하고 탐색해야 함

- 기존 구현에는 노드 그래프나 경로 탐색 알고리즘이 존재하지 않아 보스가 직접적인 방향으로만 이동 → 벽에 부딪히거나, 우회 경로를 찾지 못하는 문제 발생

- 기존에는 AI가 "어디를 갈 수 있고, 어떻게 가야 하는지"에 대한 정보 자체가 없었음 
또한 이동 방식(WALK, JUMP 등)이 경로마다 다르기 때문에, 단순 비용 기반이 아닌 커스텀 이동 로직이 필요

#### 해결
- 직접 나브메시 시스템(NaveMeshManager)을 설계 및 구현하여 경로 탐색 기반 구축

- DFS(깊이 우선 탐색)를 기반으로 한 커스텀 경로 탐색 알고리즘을 직접 설계

- 보이지 않는 **가상 노드(Virtual Nodes)**를 게임 맵의 의미 있는 위치에 배치

- 맵에 장애물 유무, 노드 간 연결 가능 여부, 이미 방문한 노드 여부 등을 조건으로 하여 DFS를 구성


<div style="text-align: center; margin: 30px 0;">
  <img src="/assets/boss.gif" alt="Pathfinding" 
       style="width: 80%; max-width: 1000px; height: auto; border-radius: 8px;" />
  <p style="color: #aaa; font-size: 0.9em;">▲ 노드(노란색)를 통해 Pathfinding</p>
</div>

#### 구현 코드 
````cpp
DFS알고리즘을 통해서 최단거리 탐색
void NaveMeshManager::FindShortestPath(int startNode, int endNode,int currentCost)
{
    //만약 해당 노드까지 탐색하였다면
    if (startNode == endNode)
    {
        // 현재까지 계산된 비용이 최소 비용보다 작다면
        if (currentCost < minCost)
        {			
            // 최소 비용 갱신
            minCost = static_cast<f32>(currentCost);
            FoundedPath = path;
            FoundedPath.push_back(endNode);
        }
        return;
    }
    //방문 처리
    visited[startNode] = true;
    //방문했으니깐 path에 넣어주고
    path.push_back(startNode);
    
    // 현재 노드에 연결된 모든 이웃 노드 탐색
    for (auto link : m_vecLink[startNode])
    {
    	int nextNode = link.first;
    	int cost=-1;
    	
        // 연결된 링크 타입에 따라 비용 계산
    	Walk* costWalk = dynamic_cast<Walk*>(link.second);
        if (costWalk != nullptr)
        {
            cost = 1; // Walk 타입은 비용 1
    	}
        else
        {
            Jump* costJump = dynamic_cast<Jump*>(link.second);
            if (costJump != nullptr)
            {
                cost = 4; // Jump 타입은 비용 4
            }		
        }	
        // 아직 방문하지 않은 노드라면 재귀적으로 DFS 수행		
        if (!visited[nextNode])
        {
            FindShortestPath(nextNode, endNode,currentCost+cost);
        }
    }	
    //다 탐색하였으면 다시 하나씩 빼줘야함
    path.pop_back();
    //빼줬으면 visited=false로 가야함
    visited[startNode] = false;
}


struct Walk :public CostLink
{
    virtual void Move(GameObject* _obj,TransComponent::Node _nodeInfo,int startNode, int endNode, TransComponent::Node _nextNode, GameObject* _player) override;
    float AccTime = 0.f;
    float melee_DelayAttack_player = 0.f;
};
struct Jump :public CostLink
{
    virtual void Move(GameObject* _obj, TransComponent::Node _nodeInfo, int startNode, int endNode, TransComponent::Node _nextNode, GameObject* _player) override;
    virtual void ExtraParam(float _val) override;
    bool IsJumpDone = false;
    //Extra Params
    float Height=100.f;
};	


void NaveMeshManager::CreateLinkTable()
{	
    // Walk, Jump 링크 객체 생성
    Walk* walk = new Walk;
    Jump* jump = new Jump;
       
    std::vector<std::pair<int/*nodeID*/, CostLink*>> link0;
    link0.push_back({ 0,walk });// 자기 자신으로 walk 가능
    link0.push_back({ 1,walk });// 1번 노드로 walk 가능
       
    std::vector<std::pair<int/*nodeID*/, CostLink*>> link1;
    link1.push_back({ 0, walk });
    link1.push_back({ 12,jump });
    link1.push_back({ 2,walk });
       
    std::vector<std::pair<int/*nodeID*/, CostLink*>> link2;
    link2.push_back({ 3, walk });
    link2.push_back({ 12,jump });
    link2.push_back({ 1,walk });
    .
    .
    .
}
````

---

# 배운 점 & 느낀 점
- 이론과 기초적인 구현까지만 알고 있던 DFS 백트랙킹 알고리즘을 실제 게임 로직에 맞게 커스터마이징하면서 알고리즘 응용 능력을 키울 수 있었습니다.
- 팀원과의 협업을 통해 기능 통합 시 의견을 조율하고 문서화를 병행하면서, 효과적인 커뮤니케이션 능력도 함께 키울 수 있었습니다.
- 프로젝트 초기에 게임 프레임워크를 구축하면서, **단순히 동작하는 코드**와 **잘 설계된 구조**는 분명히 다르다는 점을 절실히 느꼈습니다.
- 프레임워크는 단순히 한 번 짜고 끝나는 게 아니라, 계속 다듬어지고 개선되는 **기반 시스템**임을 깨달았고, 덕분에 **기초 설계의 중요성**에 대해 깊이 있는 고민을 할 수 있었습니다.

--- 
