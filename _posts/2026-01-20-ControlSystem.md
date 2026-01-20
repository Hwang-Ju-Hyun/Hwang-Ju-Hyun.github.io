---
layout: post
title: Robot Control System
tags: [Katex, Mermaid, Markdown]
color: rgb(250, 50, 50)
categories: Demo
feature-img: "assets/ControlSystem.png"
thumbnail: "assets/ControlSystem.png"
badges:   
  - label: "Asynchronous"
    color: "#34495E"  
  - label: "WPF"
    color: "#6cad41"
  - label: "TCP/IP"
    color: "#16A085"      
---


<div style="display: flex; gap: 20px; flex-wrap: wrap; margin-bottom: 20px;">
  <div style="flex: 1; min-width: 250px; border: 1px solid #444; border-radius: 8px; padding: 15px; background-color: #1c1c1c; color: #fff;">
    <h3>프로젝트 소개</h3>
    <p>
      <strong>다중 이동 로봇(Client)을 서버 기반으로 관제하고 경로 탐색 및 상태를 실시간으로 모니터링하는 Control System이다</strong><br><br>
      본 프로젝트는 다수의 로봇 클라이언트가 서버에 접속하여 <strong>자신의 상태(IDLE, MOVING, END)와 위치 정보</strong>를 주기적으로 전송하고 서버는 이를 수신하여 로봇별 상태 관리, 경로 명령 전송, 실시간 맵 시각화 및 로그 모니터링을 수행하는 관제 시스템이다.<br><br>
      <strong>A* 기반 경로 탐색</strong> 알고리즘을 직접 구현하였으며 <strong>비동기 TCP/IP 통신</strong>을 <strong>커스텀 프로토콜</strong>을 통해 실제 분산 환경을 가정한 구조로 설계
    </p>
  </div>

  <div style="flex: 1; min-width: 250px; border: 1px solid #444; border-radius: 8px; padding: 15px; background-color: #1c1c1c; color: #fff;">
    <h3>프로젝트 정보</h3>
    <ul>
      <li><strong>역할 :</strong> Programmer</li>
      <li><strong>팀 규모 :</strong> 1명 (프로그래머 1명)</li>
      <li><strong>개발 기간 :</strong> 7일 </li>      
	  <li><strong>사용 기술 & 스택 :</strong><br>
  - C# (.NET)<br>
  - WPF (UI)<br> 
  - TCP/IP<br>
  - Custom Binary Protocol(Header + Payload 구조)<br>
  - Packet Serialization / Deserialization
</li>
    </ul>
  </div>
</div>
---


# 최종결과물
<iframe width="960" height="515" src="https://www.youtube.com/embed/teyKZmRD5fo" 
        title="PathFind Simulator" frameborder="0" 
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
        allowfullscreen></iframe>
---

## 주요 구현 내용
**다수의 로봇 클라이언트가 동시에 서버에 접속하여각각의 상태와 위치 정보를 주기적으로 전송하는 중앙 집중형 관제 구조를 설계하였다.**

### 1. A* 경로 탐색 구현
- <strong>다중 휴리스틱(Heuristic) 지원</strong> : Manhattan, Euclidean, Octile 방식의 휴리스틱 함수를 델리게이트 패턴으로 구현하여 상황에 맞는 최적의 탐색 효율을 선택할 수 있게 설계.

- <strong>8방향 탐색 및 비용 계산</strong> : 상하좌우(10)와 대각선(14) 이동 비용을 차등 적용하여 실제 물리적 거리에 가까운 경로를 산출.

### 2. 서버 중심 상태 관리 구조
- 로봇(클라이언트) 상태(IDLE / MOVING / END)를 서버에서 중앙 관리

- 각 클라이언트는 자신의 상태를 주기적으로 전송하며 서버는 이를 RobotID 기준으로 매핑하여 상태 테이블을 갱신

- 로봇은(클라이언트) 자신의 현재 Grid 좌표(Row, Col)를 포함한 상태 패킷을 전송

### 3. TCP 기반 커스텀 프로토콜 설계
**TCP 스트림 기반 통신의 특성을 고려하여 단순 Send/Receive 방식이 아닌 패킷 단위 통신 프로토콜을 직접 설계하였다.**
- 모든 메시지는 고정 길이 **Header + 가변 Payload** 구조
  - Header에는 다음 정보 포함
    - **Header (2BYTE)**
    - **Payload Length (4BYTE)**
    - **Message Type (1BYTE)**
- Payload에는 **Client 상태, 위치, 경로 데이터** 등 포함
- TCP는 메시지 **경계가 없는 스트림**이므로 패킷이 쪼개져서 수신 여러 패킷이 한 번에 수신 되는 상황을 고려. 
  - 즉 수신 버퍼를 누적한 뒤 완전한 패킷이 구성되었을 때만 파싱하도록 구현


### 4. 비동기 통신 안정성 확보
- **async / await** 기반 비동기 수신 루프

- 클라이언트별 독립 세션 처리


<div style="margin-top:80px;"></div>
---
<div style="margin-top:80px;"></div>
# 어려웠던 점과 해결 방안
### TCP 스트림에서 발생하는 패킷 경계 문제

#### 문제
- TCP 통신에서는 하나의 ReadAsync() 호출이 **항상 하나의 메시지 단위를 보장하지 않음**
  - 패킷이 잘려서 수신됨
  - 여러 패킷이 한 번에 합쳐져서 수신됨

#### 원인
- TCP는 메시지 기반이 아닌 **스트림 기반 프로토콜**
- 패킷 경계를 직접 관리하지 않으면 데이터 깨짐 발생

#### 문제 코드
````c#
int bytesRead = await stream.ReadAsync(buffer, 0, buffer.Length);
RobotMessage msg = Deserialize(buffer); //위험
````
#### 해결
  - 수신 버퍼 누적 구조 설계
  - 완전한 패킷이 구성되었을 때만 파싱

#### 개선된 코드
````c#

//SERVER
session.AppendData(received);

foreach (Packet packet in session.ExtractPackets())
{
    RobotMessage msg = ProtocolParser.PacketToObject(packet);
}

//=========================
//=========================
//=========================

public class ClientSession
{
    private readonly List<byte> buffer = new List<byte>();

    public void AppendData(byte[] data)
    {
        buffer.AddRange(data);
    }

    public IEnumerable<Packet> ExtractPackets()
    {
        List<Packet> packets = new List<Packet>();

        while (true)
        {
            if (buffer.Count < 7)
            {
                //[Header] : 2byte
                //[Length] : 4byte
                //[Type]   : 1byte                    
                break;
            }
            ushort header = BitConverter.ToUInt16(buffer.ToArray(), 0);
            if (header != Packet.HEADER)
            {
                buffer.RemoveAt(0);
                continue;
            }

            int length = BitConverter.ToInt32(buffer.ToArray(), 2);
            int packetSize = 7 + length;

            if (buffer.Count < packetSize)
                break;

            byte[] packetBytes = buffer.Take(packetSize).ToArray();

            buffer.RemoveRange(0, packetSize);

            packets.Add(Packet.Deserialize(packetBytes));
        }
        return packets;
    }
}
````

<br>
<div style="margin-top:60px;"></div>
---
<div style="margin-top:60px;"></div>

# 배운 점 및 느낀점
- 프로젝트 초반에는 ReadAsync() 한 번에 하나의 메시지가 온다고 자연스럽게 가정했지만 실제 테스트 과정에서 패킷이 잘리거나 여러 개가 합쳐지는 현상을 겪으며 TCP의 본질이 스트림 기반 통신이라는 점을 명확히 이해하게 되었다.이 과정을 평소 코드를 짤때도 예외 상황을 기본값으로 고려하는 생각을 가지는 계기가 되었다.

> <span style="font-weight:bold; color:rgb(255,200,100);"> 
- <span style="font-weight:bold; color:rgb(255,200,100);"> 비동기는 단순 성능 문제가 아니라 흐름과 상태를 설계하는 문제</span>
- <span style="font-weight:bold; color:rgb(255,200,100);">실시간 시스템에서 중요한 것은 속도보다 안정성과 관측 가능성</span>
- <span style="font-weight:bold; color:rgb(255,200,100);">구조가 정리되면 디버깅 난이도가 급격히 낮아진다는 경험</span><br>
등의 경험을 하면서 단순히 기능을 구현하는 수준을 넘어 시스템의 안정성과 견고한 구조 설계의 중요성을 깊이 체감.