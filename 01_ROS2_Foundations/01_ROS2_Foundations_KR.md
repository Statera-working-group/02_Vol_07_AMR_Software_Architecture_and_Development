**Volume 07. AMR Software Architecture and Development**

# Chapter 01. ROS2 Foundations

## 01.1 Introduction to ROS2

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

# 01.01 ROS2 소개 (Introduction to ROS2)

**ROS2(로봇 운영체제 2, Robot Operating System 2)** 는 현대 로보틱스와 자율주행 로봇 개발에서 가장 중요한 소프트웨어 프레임워크 중 하나이다. 이름에 운영체제(Operating System)라는 단어가 포함되어 있지만 실제로는 Windows나 Linux와 같은 운영체제가 아니다. ROS2는 **분산형 미들웨어(Distributed Middleware) 기반의 로봇 소프트웨어 프레임워크**로서 통신 인프라, 하드웨어 추상화, 개발 도구, 실행 환경, 시스템 통합 기능을 제공한다. 현재 ROS2는 자율이동로봇(AMR, Autonomous Mobile Robot), 자율주행차, 휴머노이드, 산업용 로봇, 의료 로봇, 농업 로봇, 점검 로봇, 그리고 체화형 인공지능(Embodied AI) 플랫폼에 이르기까지 광범위하게 활용되고 있다.

ROS2의 탄생 배경은 기존 ROS1의 구조적 한계를 극복하기 위한 필요성에서 시작되었다. ROS1은 로봇 소프트웨어 개발의 패러다임을 변화시킨 혁신적인 플랫폼이었다. 개발자들은 ROS1을 통해 재사용 가능한 소프트웨어 모듈을 만들고, 표준화된 인터페이스를 활용하며, 분산형 로봇 시스템을 빠르게 구축할 수 있었다. 이러한 장점 덕분에 ROS1은 학계와 연구 분야에서 폭발적으로 성장하였다.

그러나 ROS1은 본래 연구용 로봇 개발을 목적으로 설계되었기 때문에 산업 현장에서 요구되는 확장성, 신뢰성, 실시간성, 보안성 측면에서는 여러 한계를 가지고 있었다. 특히 ROS 마스터(ROS Master)라는 중앙 서버에 의존하는 구조는 단일 장애점(Single Point of Failure)을 만들었으며, 실시간 제어에 필요한 결정론적 통신(Deterministic Communication)을 제공하지 못했다. 또한 보안(Security) 기능이 부족하여 클라우드 기반 로봇 서비스나 산업용 시스템에 적용하기 어려웠다. 다중 로봇 운영, 임베디드 시스템 통합, 대규모 분산 환경 지원에도 제약이 존재하였다.

이러한 문제를 해결하기 위해 ROS2는 기존 구조를 부분적으로 개선하는 방식이 아니라 처음부터 새로운 아키텍처로 설계되었다. 가장 중요한 변화는 DDS(데이터 분배 서비스, Data Distribution Service)를 기본 통신 미들웨어로 채택한 것이다. DDS는 산업 자동화와 항공우주 분야에서도 사용되는 검증된 통신 기술로서 분산형 피어투피어 통신(Peer-to-Peer Communication), 실시간 데이터 전송, 서비스 품질 관리(QoS, Quality of Service), 높은 신뢰성 및 확장성을 제공한다. 이를 통해 ROS2는 연구용 플랫폼을 넘어 실제 산업 현장에서 사용할 수 있는 수준의 프레임워크로 발전하였다.

ROS2의 가장 큰 특징은 완전한 분산형 구조이다. ROS1에서는 모든 노드(Node)가 ROS 마스터를 통해 연결되었지만, ROS2에서는 DDS 발견 기능(Discovery)을 사용하여 노드들이 직접 서로를 발견하고 통신한다. 따라서 특정 노드가 실패하더라도 전체 시스템이 중단되지 않으며, 다수의 로봇이 동시에 동작하는 환경에서도 높은 신뢰성을 유지할 수 있다. 이러한 특성은 물류창고의 수백 대 AMR, 스마트 팩토리의 협업 로봇, 클라우드 기반 로봇 서비스에서 매우 중요한 장점이 된다.

ROS2는 모듈화(Modularity)를 핵심 철학으로 삼는다. 전체 시스템은 노드(Node)라고 불리는 독립적인 기능 단위로 구성된다. 하나의 노드는 센서 처리, 위치 추정, 경로 계획, 객체 인식, 모터 제어, 동시적 위치추정 및 지도작성(SLAM, Simultaneous Localization and Mapping), 인공지능 추론, 플릿(Fleet) 관리 등 특정 기능만 담당한다. 이러한 노드들은 토픽(Topic), 서비스(Service), 액션(Action), 파라미터(Parameter)라는 표준 통신 메커니즘을 이용하여 정보를 교환한다. 이 구조는 소프트웨어 재사용성을 높이고 유지보수를 쉽게 하며, 복잡한 시스템을 효율적으로 개발할 수 있도록 지원한다.

토픽(Topic)은 ROS2에서 가장 널리 사용되는 통신 방식이다. 발행-구독(Publish-Subscribe) 모델을 기반으로 하며 비동기적으로 데이터를 주고받는다. 예를 들어 라이다(LiDAR) 노드는 센서 데이터를 계속 발행(Publish)하고, 위치 추정, 장애물 인식, 지도 생성 노드들은 동시에 이를 구독(Subscribe)하여 사용한다. 각 노드는 다른 노드의 내부 구현을 알 필요 없이 메시지(Message) 형식만 공유하면 되기 때문에 시스템 확장성이 매우 높다.

서비스(Service)는 요청(Request)과 응답(Response) 구조를 제공한다. 특정 노드가 다른 노드에게 명령을 보내고 결과를 기다리는 방식이다. 예를 들어 로봇 상태 조회, 설정 변경, 특정 기능 실행 요청 등에 사용된다. 액션(Action)은 이보다 더 발전된 개념으로 장시간 수행되는 작업에 적합하다. 자율주행, 도킹(Docking), 순찰, 물류 미션 수행과 같이 수 초에서 수 분 이상 소요되는 작업에 활용된다. 진행 상태를 지속적으로 확인할 수 있으며 중간에 취소도 가능하다.

ROS2의 또 다른 핵심 기능은 서비스 품질(QoS, Quality of Service) 관리이다. DDS 기반 통신은 데이터 전달 신뢰성, 버퍼 크기, 지연 시간, 메시지 보존 정책 등을 세밀하게 설정할 수 있다. 예를 들어 카메라 영상처럼 초당 수십 프레임(Frame)이 발생하는 데이터는 일부 손실을 허용하고 지연을 최소화하는 것이 중요하다. 반면 긴급 정지(E-Stop, Emergency Stop)와 같은 안전 관련 메시지는 반드시 전달되어야 한다. QoS 설정을 통해 ROS2는 다양한 응용 환경에 최적화된 통신을 제공할 수 있다.

실시간성(Real-Time Support)은 ROS2의 중요한 발전 요소이다. 산업용 로봇, 휴머노이드, 자율주행차는 밀리초 (Millisecond) 단위의 응답성을 요구한다. ROS2는 실시간 리눅스(Real-Time Linux) 환경과 연계하여 결정론적 스케줄링(Deterministic Scheduling), 저지연 통신, 메모리 관리 최적화 등을 지원한다. 완전한 강실시간(Hard Real-Time)을 구현하기 위해서는 추가적인 엔지니어링이 필요하지만, ROS1에 비해 훨씬 우수한 기반을 제공한다.

ROS2는 임베디드 시스템(Embedded System) 지원도 강화되었다. 현대 로봇은 젯슨(Jetson), ARM 기반 프로세서, 신경망처리장치(NPU, Neural Processing Unit), 엣지 AI 가속기(Edge AI Accelerator) 등 다양한 하드웨어를 사용한다. ROS2는 Linux, Windows, macOS뿐 아니라 임베디드 실시간 운영체제(RTOS, Real-Time Operating System) 환경까지 지원한다. 또한 마이크로 ROS(Micro-ROS)를 이용하면 마이크로 컨트롤러 (Microcontroller) 수준에서도 ROS2 통신 구조를 사용할 수 있다. 이를 통해 센서 노드부터 GPU 서버까지 하나의 통합된 소프트웨어 아키텍처를 구축할 수 있다.

보안(Security) 역시 ROS2 설계 단계에서부터 고려되었다. DDS 보안(DDS-Security)을 통해 인증(Authentication), 암호화(Encryption), 접근 제어(Access Control)를 지원한다. 클라우드와 연결된 서비스 로봇이나 산업 설비에 투입되는 자율주행 로봇에서는 이러한 기능이 필수적이다. ROS2는 ROS1 대비 훨씬 높은 수준의 사이버 보안(Cyber Security) 기반을 제공한다.

ROS2는 현대 인공지능(AI, Artificial Intelligence) 시스템과도 긴밀하게 통합된다. 텐서RT(TensorRT), 쿠다(CUDA), 파이토치(PyTorch), ONNX 기반 AI 추론 모듈이 ROS2 노드로 동작할 수 있으며, 라이다(LiDAR), 카메라(Camera), 레이더(Radar), 열화상 카메라(Thermal Camera), 위성항법시스템(GNSS, Global Navigation Satellite System), 관성측정장치(IMU, Inertial Measurement Unit) 등의 센서 융합 시스템도 ROS2 통신 구조를 기반으로 구현된다. 이러한 특성 때문에 ROS2는 체화형 인공지능(Embodied AI)과 물리형 인공지능(Physical AI) 플랫폼의 사실상 표준 소프트웨어 프레임워크로 자리 잡고 있다.

대표적인 응용 사례로는 내비게이션2(Navigation2, Nav2)가 있다. Nav2는 ROS2 기반 자율주행 스택(Stack)으로서 위치 추정(Localization), 경로 계획(Path Planning), 장애물 회피(Obstacle Avoidance), 복구 동작(Recovery Behavior), 비용 지도(Costmap), 모션 제어(Motion Control) 등을 제공한다. 현재 대부분의 AMR과 서비스 로봇이 Nav2를 활용하고 있다. 또한 최근에는 행동 트리(Behavior Tree) 기반의 의사결정 구조가 널리 사용되면서 복잡한 임무 수행과 장애 복구 기능도 더욱 쉽게 구현할 수 있게 되었다.

현대 로봇 시스템은 단일 컴퓨터에서 동작하지 않는다. 여러 중앙처리장치(CPU), 그래픽처리장치(GPU), 엣지 컴퓨터(Edge Computer), 클라우드 서버(Cloud Server)가 협력하여 동작한다. ROS2는 이러한 분산 컴퓨팅 환경을 자연스럽게 지원하며, 센서 처리, AI 추론, SLAM, 월드 모델(World Model), 플릿 관리 등을 서로 다른 장비에서 수행하면서도 하나의 통합 시스템처럼 동작하도록 만든다. 또한 가제보(Gazebo), 아이작 심(Isaac Sim), 위봇(Webots), 유니티(Unity)와 같은 시뮬레이터(Simulator)와의 연동도 뛰어나기 때문에 디지털 트윈(Digital Twin), 강화학습(Reinforcement Learning), 시뮬레이션-실환경 전이(Sim-to-Real) 개발에 매우 적합하다.

오늘날 ROS2는 제조 자동화, 물류 자동화, 농업 로봇, 의료 로봇, 점검 로봇, 휴머노이드, 자율주행 플랫폼 등 다양한 산업 분야에서 빠르게 채택되고 있다. 물론 DDS 설정, QoS 튜닝(Tuning), 실시간 최적화, 네트워크 구성 등은 상당한 전문 지식을 요구하기 때문에 학습 곡선(Learning Curve)이 존재한다. 그러나 이러한 복잡성을 극복하면 ROS2는 대규모 로봇 시스템을 구축할 수 있는 강력한 기반을 제공한다.

결론적으로 ROS2는 단순한 로봇 프로그래밍 라이브러리가 아니다. 그것은 로봇이 환경을 인식하고, 판단하고, 행동하고, 학습하고, 다른 로봇과 협력하며, 클라우드와 연결될 수 있도록 지원하는 **현대 로보틱스의 핵심 소프트웨어 인프라**이다. 미래의 휴머노이드, 물리형 인공지능(Physical AI), 체화형 인공지능(Embodied AI), 그리고 대규모 자율주행 로봇 생태계 역시 ROS2를 중심으로 발전할 가능성이 매우 높다.

## 01.2 ROS2 Architecture and Concepts

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

# 01.02 ROS2 아키텍처와 핵심 개념 (ROS2 Architecture and Concepts)

**ROS2 아키텍처와 핵심 개념(ROS2 Architecture and Concepts)** 은 ROS2의 내부 구조, 통신 원리, 분산 컴퓨팅 모델, 미들웨어 추상화 구조, 실행 메커니즘, 그리고 핵심 운영 개념을 설명한다. ROS2를 단순히 로봇용 소프트웨어 프레임워크로 이해하는 것만으로는 실제 산업용 로봇 시스템을 설계하기 어렵다. 확장성(Scalability), 신뢰성(Reliability), 실시간성(Real-Time), 그리고 산업 적용성(Industrial-Grade)을 확보하기 위해서는 ROS2가 내부적으로 어떻게 동작하는지를 이해해야 한다. ROS2는 현대의 자율이동로봇(AMR, Autonomous Mobile Robot), 휴머노이드(Humanoid), 산업 자동화 시스템, 클라우드 로보틱스(Cloud Robotics), 체화형 인공지능(Embodied AI)을 지원하기 위해 설계된 **분산형(Distributed), 모듈형(Modular), 통신 중심(Communication-Centric) 소프트웨어 플랫폼**이다.

ROS2를 가장 높은 수준에서 바라보면, 이는 로봇 하드웨어(Hardware), 인지 시스템(Perception System), 제어 시스템(Control System), 인공지능 추론 파이프라인(AI Inference Pipeline), 통신 미들웨어(Communication Middleware), 그리고 클라우드 인프라(Cloud Infrastructure)를 하나의 통합된 컴퓨팅 생태계로 연결하는 계층형 분산 소프트웨어 아키텍처(Layered Distributed Software Architecture)라고 할 수 있다. 전통적인 로봇 소프트웨어는 하나의 프로그램 안에 모든 기능이 포함된 단일 구조(Monolithic Architecture)를 사용했지만, ROS2는 기능을 독립적인 모듈로 분리하여 운영한다. 이러한 분산 철학은 ROS2를 이해하는 가장 중요한 출발점이다.

ROS2는 현대 분산 컴퓨팅(Distributed Computing)의 개념을 적극적으로 도입하였다. 로봇 내부의 각 기능은 노드(Node)라고 불리는 독립적인 실행 단위로 구성된다. 각 노드는 다른 노드와 직접 함수 호출(Function Call)을 하지 않고, 표준화된 데이터 교환 방식을 통해 통신한다. 이러한 구조는 모듈성(Modularity), 확장성(Scalability), 유지보수성(Maintainability), 장애 격리(Fault Isolation), 그리고 소프트웨어 재사용성(Reusability)을 크게 향상시킨다. 실제 자율주행 로봇에서는 수십 개에서 수백 개의 노드가 동시에 동작하며 인지, 위치 추정, 지도 작성, 경로 계획, 모터 제어, AI 추론, 안전 감시, 플릿 관리 등을 수행한다.

ROS1과 ROS2의 가장 큰 차이점 중 하나는 ROS 마스터(ROS Master)의 제거이다. ROS1은 중앙 서버 역할을 하는 ROS Master가 모든 노드의 등록과 통신을 관리하였다. 연구용 환경에서는 충분히 유용했지만, 산업 환경에서는 단일 장애점(Single Point of Failure)이 발생한다는 문제가 있었다. ROS2는 이를 해결하기 위해 DDS(데이터 분배 서비스, Data Distribution Service) 기반의 분산 발견 구조(Distributed Discovery Architecture)를 채택하였다. 따라서 노드들은 중앙 서버 없이 서로를 자동으로 발견하고 직접 통신할 수 있다. 이 구조는 시스템의 강건성(Robustness)과 확장성을 크게 향상시킨다.

DDS는 ROS2 통신의 핵심 기반 기술이다. DDS는 원래 항공우주(Aerospace), 국방(Defense), 통신(Telecommunications), 산업 자동화(Industrial Automation)와 같은 임무 핵심 시스템(Mission-Critical System)을 위해 개발된 산업용 미들웨어 표준이다. DDS는 신뢰성 있는 통신, 서비스 품질(QoS, Quality of Service) 제어, 실시간 데이터 전송, 멀티캐스트(Multicast) 통신, 장애 허용(Fault Tolerance) 기능을 제공한다. ROS2는 DDS를 직접 노출하지 않고 그 위에 추상화 계층을 두어 개발자가 보다 쉽게 사용할 수 있도록 설계하였다.

ROS2의 내부 구조는 여러 개의 계층(Layer)으로 구성된다. 최상위 계층에는 사용자가 개발하는 응용 프로그램(Application)과 로봇 알고리즘(Algorithm)이 존재한다. 그 아래에는 ROS 클라이언트 라이브러리(Client Library)가 위치하며, C++, Python 등 다양한 언어용 API(Application Programming Interface)를 제공한다. 그 다음 계층에는 RMW(ROS Middleware)라는 미들웨어 추상화 계층이 존재한다. RMW는 ROS2와 DDS 사이를 연결하는 표준 인터페이스 역할을 수행한다. 가장 아래에는 DDS와 운영체제(Operating System), 그리고 실제 하드웨어(Hardware)가 위치한다.

RMW는 ROS2에서 매우 중요한 개념이다. ROS2는 특정 DDS 제품에 종속되지 않도록 설계되었다. 따라서 다양한 DDS 구현체를 자유롭게 선택할 수 있다. 대표적으로 Fast DDS(FastDDS), 사이클론 DDS(CycloneDDS), RTI Connext DDS, 그리고 구름 DDS(GurumDDS)가 사용된다. 이러한 구조 덕분에 개발자는 성능, 라이선스, 임베디드 지원 여부, 산업 인증 요구사항에 따라 적절한 DDS를 선택할 수 있다.

노드(Node)는 ROS2의 가장 기본적인 실행 단위이다. 각 노드는 하나의 명확한 기능만 수행하도록 설계된다. 예를 들어 라이다 드라이버 노드(LiDAR Driver Node)는 포인트 클라우드(Point Cloud)를 생성하고, 위치 추정 노드(Localization Node)는 로봇의 위치를 계산하며, AI 추론 노드(AI Inference Node)는 GPU를 이용하여 객체 인식(Object Detection)을 수행한다. 이러한 구조는 기능 분리(Separation of Concerns)를 가능하게 하여 복잡한 시스템을 관리하기 쉽게 만든다.

노드 간 통신은 토픽(Topic), 서비스(Service), 액션(Action), 파라미터(Parameter)를 통해 이루어진다. 토픽은 발행-구독(Publish-Subscribe) 기반의 비동기 통신 방식이다. 여러 개의 노드가 동일한 데이터를 동시에 사용할 수 있으므로 매우 높은 확장성을 제공한다. 서비스는 요청-응답(Request-Response) 방식으로 동작하며 즉각적인 명령 수행에 적합하다. 액션은 장시간 수행되는 작업을 위해 설계된 구조로서 진행 상태(Feedback)를 제공하고 중간 취소(Cancel)도 가능하다. 자율주행(Navigation)이나 도킹(Docking) 작업에 주로 사용된다.

메시지(Message)는 ROS2 통신의 기본 데이터 단위이다. 메시지는 노드 간에 전달되는 구조화된 데이터 형식을 정의한다. ROS2는 위치 정보(Geometry), 센서 데이터(Sensor Data), 이미지(Image), 변환 정보(Transform), 진단 정보(Diagnostics) 등 다양한 표준 메시지를 제공한다. 또한 개발자는 특정 응용 분야를 위해 사용자 정의 메시지(Custom Message)를 생성할 수도 있다.

직렬화(Serialization)는 메시지를 네트워크로 전송하기 위해 이진(Binary) 데이터 형태로 변환하는 과정이다. DDS는 직렬화와 역직렬화(Deserialization)를 자동으로 수행하기 때문에 서로 다른 운영체제, 프로세서, 컴퓨터에서도 문제없이 통신할 수 있다.

ROS2에서 가장 강력한 기능 중 하나는 서비스 품질(QoS, Quality of Service)이다. 로봇 시스템마다 통신 요구사항이 다르기 때문에 ROS2는 세밀한 통신 정책을 제공한다. QoS는 신뢰성(Reliability), 지속성(Durability), 히스토리(History), 마감 시간(Deadline), 생존성(Liveliness) 등을 설정할 수 있다. 이를 통해 실시간 제어와 대용량 센서 데이터를 동일한 시스템에서 효율적으로 처리할 수 있다.

예를 들어 신뢰성 QoS(Reliability QoS)는 메시지를 반드시 전달할 것인지, 아니면 속도를 우선할 것인지를 결정한다. Reliable 모드는 데이터 손실을 방지하지만 지연이 증가할 수 있다. 반면 Best Effort 모드는 일부 데이터 손실을 허용하는 대신 낮은 지연 시간을 제공한다. 카메라 영상은 Best Effort를 사용할 수 있지만 긴급 정지(Emergency Stop) 명령은 Reliable 설정이 필요하다.

실행기(Executor)는 ROS2의 또 다른 핵심 개념이다. ROS2는 메시지 수신, 타이머(Timer), 서비스 요청, 액션 피드백과 같은 이벤트(Event)에 반응하는 구조이다. 실행기는 이러한 콜백(Callback)을 언제, 어떤 순서로 실행할지 관리한다. 단일 스레드 실행기(Single-Threaded Executor)는 순차적으로 처리하며, 다중 스레드 실행기(Multi-Threaded Executor)는 병렬 처리를 통해 성능을 향상시킨다.

현대 로봇은 인지, 센서 융합, AI 추론, 네트워크 통신을 동시에 수행해야 하므로 동시성(Concurrency) 관리가 중요하다. ROS2는 콜백 그룹(Callback Group)과 실행기 설정을 통해 복잡한 병렬 처리 구조를 지원한다.

수명주기 노드(Lifecycle Node)는 산업용 시스템에서 매우 중요한 기능이다. 일반 노드는 시작과 동시에 동작하지만, 수명주기 노드는 미설정(Unconfigured), 비활성(Inactive), 활성(Active), 종료(Finalized) 상태를 거친다. 이를 통해 시스템 시작 순서 관리, 장애 복구(Fault Recovery), 운영 자동화(Orchestration)를 보다 안정적으로 수행할 수 있다.

변환 시스템(tf2)은 ROS2의 공간 인지(Spatial Reasoning)를 담당하는 핵심 기술이다. 로봇은 지도 좌표(Map Frame), 오도메트리(Odometry Frame), 센서 좌표(Sensor Frame), 로봇 기준 좌표(Base Frame), 카메라 좌표(Camera Frame) 등 다양한 좌표계를 사용한다. tf2는 이들 사이의 좌표 변환을 시간 정보와 함께 관리하여 센서 융합, 자율주행, 위치 추정을 가능하게 한다.

ROS2는 다중 컴퓨터(Multi-Machine) 환경을 자연스럽게 지원한다. 무거운 AI 연산은 GPU 서버에서 수행하고, 저수준 제어는 임베디드 컨트롤러에서 수행할 수 있다. DDS는 이러한 분산 환경의 통신을 자동으로 처리한다. 따라서 클라우드-엣지 로보틱스(Cloud-Edge Robotics) 구조를 쉽게 구현할 수 있다.

또한 ROS2는 가제보(Gazebo), 아이작 심(Isaac Sim)과 같은 시뮬레이터와 완벽하게 통합된다. 실제 로봇과 동일한 인터페이스를 사용하기 때문에 시뮬레이션 기반 개발(Simulation-Driven Development), 디지털 트윈(Digital Twin), 강화학습(Reinforcement Learning), 시뮬레이션-실환경 전이(Sim-to-Real)에 매우 적합하다.

최근 ROS2는 컨테이너(Container)와 클라우드 네이티브(Cloud-Native) 환경까지 확장되고 있다. 도커(Docker), 쿠버네티스(Kubernetes), 클라우드 로보틱스(Cloud Robotics) 기술과 결합하여 대규모 로봇 플릿(Fleet)을 효율적으로 운영할 수 있다. 이로 인해 ROS2는 단순한 로봇 프로그래밍 프레임워크를 넘어 **소프트웨어 정의 로보틱스(Software-Defined Robotics)의 핵심 플랫폼**으로 발전하고 있다.

결론적으로 ROS2 아키텍처는 단순한 통신 프레임워크가 아니라 인지(Perception), 제어(Control), 인공지능 추론(AI Reasoning), 클라우드 인프라(Cloud Infrastructure), 시뮬레이션(Simulation), 그리고 플릿 관리(Fleet Coordination)를 하나의 통합된 지능형 로봇 생태계로 연결하는 기반 기술이다. 앞으로 휴머노이드, 체화형 인공지능(Embodied AI), 물리형 인공지능(Physical AI), 자율주행 플랫폼이 발전할수록 ROS2의 아키텍처와 핵심 개념은 더욱 중요해질 것이다.

## 01.3 ROS2 Nodes and Topics

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

# 01.03 ROS2 노드와 토픽 (ROS2 Nodes and Topics)

**ROS2 노드와 토픽(ROS2 Nodes and Topics)** 은 ROS2 아키텍처를 구성하는 가장 핵심적인 개념이다. 노드(Node)와 토픽(Topic)은 ROS2가 분산형(Distributed) 로봇 소프트웨어 프레임워크로 동작할 수 있도록 만드는 기본 구성 요소이며, 사실상 모든 ROS2 시스템은 노드와 토픽을 중심으로 구축된다. 로봇의 인지(Perception), 위치 추정(Localization), 자율주행(Navigation), 인공지능 추론(AI Inference), 모션 제어(Motion Control), 센서 융합(Sensor Fusion), 클라우드 통신(Cloud Communication), 플릿 관리(Fleet Coordination)와 같은 기능들은 모두 노드 간의 데이터 교환을 통해 구현된다. 따라서 ROS2를 제대로 이해하기 위해서는 노드와 토픽의 개념을 명확하게 이해해야 한다.

ROS2는 분산형 모듈 소프트웨어(Distributed Modular Software) 철학을 기반으로 설계되었다. 전통적인 로봇 소프트웨어는 하나의 거대한 프로그램 안에 모든 기능을 구현하는 단일 구조(Monolithic Architecture)를 사용하였다. 그러나 이러한 방식은 시스템이 커질수록 유지보수가 어려워지고 확장성이 떨어지는 문제가 발생한다. ROS2는 이러한 문제를 해결하기 위해 로봇의 지능을 여러 개의 독립적인 소프트웨어 모듈로 분리하였다. 이러한 모듈을 노드(Node)라고 하며, 노드들은 서로 토픽을 통해 데이터를 교환한다.

노드는 ROS2에서 가장 기본적인 실행 단위이다. 쉽게 말하면 하나의 기능을 수행하는 독립적인 프로그램이라고 생각할 수 있다. 예를 들어 하나의 노드는 라이다(LiDAR) 센서를 제어하고, 또 다른 노드는 카메라 영상을 처리하며, 또 다른 노드는 딥러닝(Deep Learning)을 이용한 객체 인식(Object Detection)을 수행할 수 있다. 또한 위치 추정, 지도 작성, 경로 계획, 모터 제어 등도 각각 별도의 노드로 구현될 수 있다. 현대 자율주행 로봇에서는 수십 개에서 수백 개의 노드가 동시에 실행되며 전체 시스템을 구성한다.

이러한 노드 기반 구조는 여러 가지 장점을 제공한다. 가장 큰 장점은 기능 분리(Function Separation)이다. 특정 기능을 담당하는 노드만 수정하면 되므로 전체 시스템을 변경할 필요가 없다. 예를 들어 기존 객체 인식 노드를 최신 비전 트랜스포머(Vision Transformer) 기반 모델로 교체하더라도 자율주행이나 위치 추정 노드는 영향을 받지 않는다. 이러한 느슨한 결합 구조(Loose Coupling)는 유지보수성과 재사용성을 크게 향상시킨다.

ROS2에서 노드는 일반적으로 C++용 rclcpp 라이브러리나 Python용 rclpy 라이브러리를 사용하여 개발된다. 프로그래밍 언어는 다를 수 있지만 내부 통신 구조는 동일하다. 모든 노드는 ROS2 미들웨어(Middleware)와 DDS(데이터 분배 서비스, Data Distribution Service)를 통해 서로 통신한다.

각 노드는 고유한 이름(Name)을 가진다. 또한 네임스페이스(Namespace)를 이용하여 계층적으로 관리할 수 있다. 이는 다중 로봇(Multi-Robot) 환경에서 특히 중요하다. 예를 들어 여러 대의 AMR이 동일한 소프트웨어를 사용하더라도 서로 다른 네임스페이스를 부여하면 충돌 없이 독립적으로 운영할 수 있다. 하나의 로봇은 \`/robot1\`, 다른 로봇은 \`/robot2\` 아래에서 동일한 구조의 노드를 실행할 수 있다.

ROS2의 노드는 조합 가능성(Composable Architecture)을 지원한다. 노드를 각각 별도의 프로세스(Process)로 실행할 수도 있고, 하나의 프로세스 안에 여러 노드를 함께 실행할 수도 있다. 같은 프로세스 내에서 실행하면 프로세스 간 통신 비용을 줄일 수 있어 성능이 향상된다. 반면 독립 프로세스로 실행하면 하나의 노드가 비정상 종료되더라도 다른 노드에 영향을 주지 않으므로 안정성이 향상된다.

노드의 생명주기(Lifecycle)도 중요한 개념이다. 일반 노드는 시작과 동시에 동작하지만, 수명주기 노드(Lifecycle Node)는 미설정(Unconfigured), 비활성(Inactive), 활성(Active), 종료(Finalized) 상태를 거친다. 이를 통해 복잡한 산업용 시스템에서 시작 순서 관리, 장애 복구(Fault Recovery), 운영 자동화(Orchestration)를 보다 체계적으로 수행할 수 있다.

노드들이 서로 정보를 교환하는 가장 중요한 방법이 바로 토픽(Topic)이다. 토픽은 발행-구독(Publish-Subscribe) 방식의 비동기 통신 구조를 제공한다. 데이터를 보내는 노드를 발행자(Publisher)라고 하고, 데이터를 받는 노드를 구독자(Subscriber)라고 한다. 발행자와 구독자는 서로를 직접 알 필요가 없으며, 단지 동일한 토픽을 사용하기만 하면 된다. 이 구조는 매우 높은 확장성을 제공한다.

예를 들어 라이다 센서 노드가 \`/scan\` 토픽으로 데이터를 발행한다고 가정해 보자. 위치 추정 노드, 장애물 인식 노드, SLAM 노드, 시각화 도구(RViz), AI 인지 시스템은 모두 동시에 \`/scan\` 토픽을 구독할 수 있다. 하나의 데이터가 여러 시스템에서 동시에 활용될 수 있기 때문에 매우 효율적이다.

토픽의 또 다른 중요한 특징은 비동기성(Asynchronous Communication)이다. 발행자는 구독자의 응답을 기다리지 않는다. 데이터를 계속 발행하면 각 구독자는 자신의 속도에 맞게 처리한다. 따라서 일부 노드의 처리 속도가 느려지더라도 전체 시스템이 멈추지 않는다. 이러한 구조는 시스템의 확장성과 안정성을 크게 향상시킨다.

토픽은 실행 중에도 동적으로 확장될 수 있다. 새로운 구독자를 추가하더라도 기존 발행자를 수정할 필요가 없다. 예를 들어 운영 중인 로봇에 데이터 기록 노드(Logging Node), 클라우드 전송 노드(Cloud Telemetry Node), 디버깅 노드(Debugging Node)를 추가할 수 있다. 이러한 동적 확장성(Dynamic Extensibility)은 ROS2가 연구와 산업 현장에서 모두 널리 사용되는 이유 중 하나이다.

토픽을 통해 전달되는 데이터는 메시지(Message)라는 구조화된 형식을 사용한다. ROS2는 위치 정보(Geometry), 센서 데이터(Sensor Data), 이미지(Image), 진단 정보(Diagnostics), 로봇 상태(Robot State) 등을 위한 다양한 표준 메시지를 제공한다. 예를 들어 \`sensor_msgs\`는 라이다와 카메라 데이터를 정의하며, \`geometry_msgs\`는 위치(Position), 자세(Pose), 벡터(Vector), 좌표 변환(Transform) 정보를 정의한다. 필요에 따라 사용자 정의 메시지(Custom Message)를 생성할 수도 있다.

메시지는 네트워크 전송 전에 직렬화(Serialization) 과정을 거친다. 직렬화란 구조화된 데이터를 이진(Binary) 형식으로 변환하는 과정이다. DDS는 직렬화와 역직렬화(Deserialization)를 자동으로 수행하므로 서로 다른 운영체제와 하드웨어에서도 문제없이 통신할 수 있다.

토픽 이름은 일반적으로 기능 중심으로 정의된다. 예를 들어 \`/cmd_vel\`은 속도 명령(Velocity Command)을 의미하며, \`/camera/image_raw\`는 원시 카메라 영상을 의미한다. 일관된 명명 규칙(Naming Convention)은 시스템의 가독성과 유지보수성을 높인다.

ROS2는 DDS 기반의 서비스 품질(QoS, Quality of Service)을 통해 토픽 통신을 더욱 강화하였다. QoS는 신뢰성(Reliability), 지속성(Durability), 히스토리(History), 마감 시간(Deadline), 생존성(Liveliness)을 세밀하게 제어할 수 있다. 이를 통해 다양한 응용 환경에 최적화된 통신을 구현할 수 있다.

신뢰성 QoS(Reliability QoS)는 데이터 전달 보장을 결정한다. Reliable 모드는 데이터 손실이 발생하면 재전송하여 반드시 전달한다. Best Effort 모드는 일부 데이터 손실을 허용하는 대신 지연 시간을 최소화한다. 카메라 영상이나 라이다 데이터는 Best Effort를 사용하는 경우가 많고, 긴급 정지(Emergency Stop) 명령은 Reliable 모드를 사용한다.

지속성 QoS(Durability QoS)는 새롭게 연결된 구독자가 이전 데이터를 받을 수 있는지 결정한다. Volatile 모드는 현재 데이터만 제공하며, Transient Local 모드는 과거 데이터를 저장하여 나중에 연결된 구독자도 받을 수 있게 한다. 지도(Map), 설정(Configuration), 보정 데이터(Calibration Data) 전송에 자주 사용된다.

히스토리 QoS(History QoS)는 메시지 버퍼 크기를 결정한다. Keep Last는 최근 데이터만 저장하고, Keep All은 가능한 모든 데이터를 저장한다. 적절한 설정은 메모리 사용량과 통신 효율에 큰 영향을 준다.

마감 시간 QoS(Deadline QoS)는 데이터가 정해진 시간 내에 도착하는지 감시한다. 일정 시간 동안 데이터가 도착하지 않으면 DDS가 통신 이상을 감지할 수 있다. 이는 산업용 안전 시스템에서 매우 중요하다.

생존성 QoS(Liveliness QoS)는 발행자가 정상적으로 동작 중인지 확인한다. 예를 들어 센서 노드가 중단되면 구독자는 해당 노드가 비정상 상태임을 인식하고 장애 대응 절차를 시작할 수 있다.

ROS2의 토픽 구조는 완전한 분산형 구조이다. DDS는 피어투피어(Peer-to-Peer) 방식의 발견(Discovery) 기능을 사용하기 때문에 ROS1과 같은 중앙 서버가 필요하지 않다. 노드들은 자동으로 서로를 발견하고 연결된다. 이는 다중 로봇 시스템과 대규모 산업 환경에서 매우 큰 장점을 제공한다.

또한 DDS 도메인(DDS Domain)을 사용하면 서로 다른 로봇 시스템 간의 통신을 분리할 수 있다. 대형 공장에서는 여러 개의 로봇 플릿(Fleet)이 동시에 운영될 수 있는데, 서로 다른 도메인 ID를 사용하면 독립적으로 동작할 수 있다.

ROS2는 멀티캐스트(Multicast) 통신도 지원한다. 동일한 데이터를 여러 구독자에게 효율적으로 전송할 수 있기 때문에 대용량 라이다 데이터, 카메라 스트림, 원격 모니터링 시스템에서 매우 유용하다.

고해상도 카메라가 초당 60프레임(FPS, Frames Per Second)을 생성하거나, 고밀도 라이다가 초당 수백만 개의 포인트(Point)를 생성하는 경우에는 막대한 데이터가 발생한다. 따라서 QoS 설정, 메모리 관리, 네트워크 구성, 데이터 처리 파이프라인 최적화가 매우 중요하다.

ROS2는 동일 프로세스 내 노드 간 통신(Intra-Process Communication) 최적화도 지원한다. 메시지를 반복적으로 복사하지 않고 공유 메모리(Shared Memory)를 사용할 수 있어 지연 시간과 CPU 사용량을 크게 줄일 수 있다. 이는 AI 기반 영상 처리 파이프라인에서 매우 중요한 기능이다.

실제 자율주행 시스템에서는 라이다 노드가 스캔 데이터를 발행하고, 위치 추정 시스템은 로봇 위치를 발행하며, 경로 계획기는 경로를 발행하고, 제어기는 속도 명령을 발행한다. 전체 시스템은 수많은 토픽이 서로 연결된 거대한 데이터 흐름 네트워크라고 볼 수 있다.

최근의 AI 및 딥러닝 시스템도 ROS2 토픽 구조와 자연스럽게 통합된다. 카메라 토픽은 영상을 AI 추론 노드에 전달하고, AI 노드는 객체 목록(Object List), 의미 지도(Semantic Map), 장면 이해(Scene Understanding) 결과를 다시 토픽으로 발행한다. 체화형 인공지능(Embodied AI), 월드 모델(World Model), 다중모달 인공지능(Multimodal AI) 역시 이러한 토픽 기반 통신을 적극적으로 활용한다.

결론적으로 ROS2의 노드(Node)와 토픽(Topic)은 단순한 프로그래밍 개념이 아니다. 그것들은 현대 로봇 시스템의 **분산 신경계(Distributed Nervous System)** 역할을 수행한다. 인지, 추론, 계획, 제어, 인공지능, 클라우드 연동, 플릿 협업을 하나로 연결하는 핵심 메커니즘이며, 미래의 휴머노이드(Humanoid), 자율주행 플랫폼, 물리형 인공지능(Physical AI), 체화형 인공지능(Embodied AI)의 기반 기술이라고 할 수 있다.

## 01.4 ROS2 Services and Actions

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

# 01.04 ROS2 서비스와 액션 (ROS2 Services and Actions)

**ROS2 서비스와 액션(ROS2 Services and Actions)** 은 토픽(Topic)과 함께 ROS2의 핵심 통신 구조를 구성하는 중요한 메커니즘이다. 토픽이 지속적인 데이터 스트리밍(Data Streaming)을 위한 구조라면, 서비스(Service)와 액션(Action)은 작업 중심(Task-Oriented)의 상호작용을 위한 구조이다. 현대의 자율주행 로봇은 단순히 센서 데이터를 지속적으로 송수신하는 것만으로는 동작할 수 없다. 위치 초기화, 도킹(Docking), 지도 저장(Map Saving), 경로 이동(Navigation), 로봇 팔 제어(Manipulator Control), AI 작업 수행, 다단계 임무 실행(Mission Execution)과 같은 다양한 작업을 수행해야 한다. 서비스와 액션은 이러한 작업을 효율적으로 처리하기 위해 설계되었다.

실제 로봇 시스템에서는 단순한 데이터 교환보다 명령(Command)과 요청(Request)이 훨씬 자주 사용된다. 예를 들어 로봇의 현재 상태를 확인하거나, 위치 추정(Localization)을 초기화하거나, 센서를 활성화하거나, 특정 위치로 이동하도록 명령할 필요가 있다. 어떤 작업은 매우 짧고 즉시 완료되지만, 어떤 작업은 수 초에서 수 분 이상 지속될 수 있다. ROS2는 이러한 서로 다른 작업 특성을 지원하기 위해 서비스와 액션이라는 두 가지 통신 모델을 제공한다.

ROS2의 통신 구조는 크게 세 가지 방식으로 구분할 수 있다.

첫 번째는 토픽(Topic) 기반의 비동기 데이터 스트리밍 방식이다. 센서 데이터나 상태 정보처럼 지속적으로 발생하는 데이터를 전송할 때 사용된다.

두 번째는 서비스(Service) 기반의 요청-응답(Request-Response) 방식이다. 특정 요청에 대해 즉시 결과를 반환해야 하는 경우에 사용된다.

세 번째는 액션(Action) 기반의 장기 작업(Long-Duration Task) 방식이다. 시간이 오래 걸리는 작업을 수행하면서 중간 진행 상황(Feedback)을 지속적으로 제공할 수 있다.

서비스는 개념적으로 원격 프로시저 호출(Remote Procedure Call, RPC)과 매우 유사하다. 클라이언트(Client)가 요청(Request)을 보내고 서버(Server)가 이를 처리한 후 응답(Response)을 반환한다. 서비스는 동기식(Synchronous) 통신 구조를 사용하기 때문에 요청을 보낸 쪽은 결과가 반환될 때까지 기다리게 된다. 이러한 특성 때문에 서비스는 짧고 결정적인(Deterministic) 작업에 적합하다.

대표적인 서비스 사용 사례로는 로봇 상태 조회(Status Query), 위치 추정 초기화(Localization Reset), 설정 파일 로딩(Configuration Loading), 센서 활성화(Sensor Enable), 배터리 상태 조회(Battery Query), 지도 저장(Map Saving), 보정(Calibration) 수행 등이 있다. 이러한 작업은 일반적으로 짧은 시간 안에 완료되며 명확한 결과를 반환한다.

ROS2 서비스는 서비스 클라이언트(Service Client)와 서비스 서버(Service Server)로 구성된다. 클라이언트는 요청을 생성하고 서버는 이를 처리하여 응답을 반환한다. 이 과정은 미리 정의된 서비스 인터페이스(Service Interface)를 기반으로 수행된다. 이러한 구조는 서로 다른 시스템 간에도 높은 호환성(Interoperability)과 안정성(Reliability)을 제공한다.

서비스 인터페이스는 \`.srv\` 파일을 이용하여 정의된다. \`.srv\` 파일은 요청(Request) 구조와 응답(Response) 구조로 구성된다. 개발자는 필요한 데이터 형식만 정의하면 ROS2가 자동으로 직렬화(Serialization), 역직렬화(Deserialization), 통신 코드 생성(Code Generation)을 수행한다. 따라서 개발자는 통신 프로토콜보다 실제 로봇 기능 구현에 집중할 수 있다.

서비스의 가장 중요한 특징은 동기성(Synchronous Behavior)이다. 요청을 보낸 후 결과가 나올 때까지 기다리기 때문에 짧은 작업에는 매우 효율적이다. 그러나 시간이 오래 걸리는 작업에는 적합하지 않다. 예를 들어 현재 배터리 전압(Battery Voltage)을 조회하는 것은 즉시 완료되지만, 공장 전체를 이동하는 자율주행 임무는 수 분 이상이 소요될 수 있다. 이러한 작업을 서비스로 구현하면 클라이언트가 오랫동안 대기해야 하므로 시스템 응답성이 크게 떨어진다.

이러한 문제를 해결하기 위해 ROS2는 액션(Action)을 제공한다. 액션은 서비스 개념을 확장하여 장시간 실행되는 작업을 지원하도록 설계되었다. 액션은 목표(Goal)를 전송하고, 실행 중에는 지속적으로 피드백(Feedback)을 받으며, 필요하면 작업을 취소(Cancel)할 수 있다. 작업이 완료되면 최종 결과(Result)를 수신한다. 따라서 액션은 현대 자율주행 로봇의 핵심 제어 메커니즘이라고 할 수 있다.

액션 구조에는 여러 구성 요소가 존재한다. 액션 클라이언트(Action Client)는 목표를 전송하고, 액션 서버(Action Server)는 목표를 수락하거나 거부한 후 작업을 수행한다. 실행 중에는 진행 상황을 지속적으로 전송하며, 취소 요청도 처리할 수 있다. 작업이 완료되면 최종 결과를 반환한다. 내부적으로는 토픽과 서비스를 조합하여 구현되지만, 사용자는 이를 하나의 통합 인터페이스처럼 사용할 수 있다.

액션 인터페이스는 \`.action\` 파일을 사용하여 정의된다. 액션 파일은 세 부분으로 구성된다.

첫 번째는 목표(Goal) 정의이다. 수행하고자 하는 작업의 입력 정보를 포함한다.

두 번째는 결과(Result) 정의이다. 작업이 완료된 후 반환되는 최종 결과를 정의한다.

세 번째는 피드백(Feedback) 정의이다. 작업 진행 중 전달되는 상태 정보를 정의한다.

이 구조 덕분에 로봇은 장시간 작업을 수행하면서도 사용자나 상위 시스템과 지속적으로 상호작용할 수 있다.

액션이 가장 많이 사용되는 대표적인 분야는 자율주행(Navigation)이다. 내비게이션2(Navigation2, Nav2)에서는 목표 위치(Target Pose)를 액션으로 전달한다. 내비게이션 서버는 경로를 생성하고 로봇을 이동시키며, 남은 거리(Remaining Distance), 예상 도착 시간(Estimated Time of Arrival), 현재 상태(Current State)를 지속적으로 피드백으로 제공한다. 만약 새로운 긴급 임무가 발생하면 기존 목표를 취소하고 새로운 목표를 수행할 수도 있다.

현대의 로봇은 대부분 즉시 끝나는 작업보다 장시간 작업을 수행한다. 물체 조작(Manipulation), 도킹(Docking), 순찰(Patrol), SLAM 수행, 클라우드 동기화(Cloud Synchronization), AI 추론(AI Inference), 플릿 협업(Fleet Coordination) 등은 모두 수 초에서 수 분 이상 지속될 수 있다. 따라서 액션은 현대 로봇 시스템에서 필수적인 요소가 되었다.

서비스와 액션은 모두 강한 타입 기반(Strongly Typed Interface) 구조를 사용한다. 서비스는 \`.srv\`, 액션은 \`.action\` 파일을 사용한다. ROS2는 이러한 정의를 바탕으로 통신 코드를 자동 생성하기 때문에 개발자는 저수준 네트워크 구현 없이도 복잡한 분산 시스템을 구축할 수 있다.

서비스와 액션 아래에는 DDS(데이터 분배 서비스, Data Distribution Service)가 존재한다. DDS는 발견(Discovery), 데이터 전송(Transport), 직렬화(Serialization), 동기화(Synchronization), 서비스 품질(QoS, Quality of Service)을 자동으로 처리한다. 따라서 ROS2는 높은 수준의 사용 편의성과 산업용 수준의 통신 신뢰성을 동시에 제공한다.

서비스와 토픽의 가장 큰 차이는 결합도(Coupling)에 있다. 토픽은 느슨한 결합(Loose Coupling)을 가지므로 발행자(Publisher)와 구독자(Subscriber)가 서로를 알 필요가 없다. 반면 서비스는 클라이언트와 서버가 직접적으로 연결되므로 강한 결합(Strong Coupling)을 가진다. 액션은 이 둘의 중간 형태로 볼 수 있다. 목표 지향적(Goal-Oriented)이면서도 비동기적(Asynchronous) 특성을 제공한다.

ROS2의 서비스와 액션은 DDS의 피어투피어(Peer-to-Peer) 발견 구조를 사용한다. 따라서 ROS1과 달리 ROS 마스터(ROS Master)가 필요하지 않다. 모든 서비스와 액션은 자동으로 발견되고 연결된다. 이는 다중 로봇(Multi-Robot) 환경과 대규모 분산 시스템에서 매우 큰 장점을 제공한다.

실제 산업 환경에서는 서비스가 주로 시스템 관리(System Management)에 사용된다. 파라미터 변경(Parameter Tuning), 센서 초기화(Sensor Initialization), 하드웨어 설정(Hardware Configuration), 운영 모드 변경(Mode Switching), 진단 정보 조회(Diagnostic Query) 등이 대표적인 예이다.

반면 액션은 임무 실행(Mission Execution)에 주로 사용된다. 창고 물류(Warehouse Logistics), 자율 순찰(Autonomous Patrol), 도킹(Docking), 점검(Inspection), 로봇 팔 궤적 실행(Manipulator Trajectory), 클라우드 동기화(Cloud Synchronization)와 같은 작업이 대표적이다.

액션의 가장 강력한 기능 중 하나는 취소(Cancellation) 지원이다. 실제 환경에서는 우선순위가 수시로 변경된다. 로봇이 목적지로 이동하는 도중에 긴급 임무가 발생할 수 있다. 이 경우 현재 작업을 안전하게 중단하고 새로운 작업으로 전환해야 한다. 액션은 이러한 동적 환경을 효과적으로 지원한다.

또한 피드백 채널(Feedback Channel)은 운영자, AI 감독 시스템, 클라우드 관제 시스템이 로봇 상태를 지속적으로 모니터링할 수 있도록 해준다. 이러한 정보는 디버깅(Debugging), 모니터링(Monitoring), 예측 분석(Predictive Analytics), 운영 대시보드(Operational Dashboard) 구축에 활용된다.

최근에는 행동 트리(Behavior Tree)가 ROS2 서비스와 액션을 적극 활용하고 있다. 행동 트리는 짧은 작업은 서비스로 처리하고, 장시간 작업은 액션으로 처리하면서 복잡한 임무를 계층적으로 구성한다. 이는 현대 자율주행 시스템의 사실상 표준 구조가 되고 있다.

클라우드 로보틱스(Cloud Robotics), 다중 로봇 시스템(Multi-Robot System), 체화형 인공지능(Embodied AI), 물리형 인공지능(Physical AI) 역시 서비스와 액션에 크게 의존한다. 클라우드 AI 요청, OTA 업데이트, 지도 다운로드(Map Download), 플릿 협업(Fleet Collaboration), 분산 임무 수행(Distributed Mission Execution) 등이 모두 서비스와 액션을 통해 이루어진다.

결론적으로 ROS2의 서비스(Service)와 액션(Action)은 단순한 통신 기능이 아니다. 이들은 현대 자율주행 로봇의 **명령 및 제어 체계(Command and Control Framework)**를 구성하는 핵심 요소이다. 서비스는 짧고 즉각적인 작업을 처리하며, 액션은 장시간 수행되는 임무를 관리한다. 이 둘의 조합을 통해 ROS2는 복잡한 분산 지능 시스템, 자율주행 로봇, 휴머노이드, 클라우드 기반 로봇 플랫폼을 효과적으로 구현할 수 있다.

## 01.5 ROS2 Packages and Workspaces

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

# 01.05 ROS2 패키지와 워크스페이스 (ROS2 Packages and Workspaces)

**ROS2 패키지와 워크스페이스(ROS2 Packages and Workspaces)** 는 ROS2 소프트웨어 개발의 가장 중요한 구조적 기반 중 하나이다. 현대 로봇 시스템은 매우 복잡하며, 하나의 자율이동로봇(AMR, Autonomous Mobile Robot)만 하더라도 수백 개의 노드(Node), 수천 개의 소스 파일(Source File), 인공지능 모델(AI Model), 설정 파일(Configuration File), 실행 시스템(Launch System), 시뮬레이션 환경(Simulation Environment), 그리고 다양한 통신 인터페이스를 포함한다. 이러한 대규모 소프트웨어를 체계적으로 관리하기 위해서는 표준화된 개발 구조가 필요하다. ROS2의 패키지와 워크스페이스는 이러한 문제를 해결하기 위한 조직적 프레임워크를 제공하며, 로봇 소프트웨어를 모듈화(Modularization), 재사용(Reusability), 확장성(Scalability), 유지보수성(Maintainability) 측면에서 효율적으로 관리할 수 있도록 지원한다.

ROS2에서 패키지(Package)는 특정 기능을 담당하는 독립적인 소프트웨어 단위이며, 워크스페이스(Workspace)는 여러 패키지를 함께 관리하고 빌드(Build)하는 개발 환경을 의미한다. 이 두 개념은 ROS2 프로젝트 구조의 핵심을 이루며, 교육용 소형 로봇부터 산업용 자율주행 플릿(Fleet)에 이르기까지 거의 모든 ROS2 기반 시스템에서 사용된다.

패키지는 ROS2에서 가장 기본적인 배포 단위(Distribution Unit)이자 모듈화 단위(Modularization Unit)이다. 하나의 패키지에는 특정 기능을 구현하는 데 필요한 모든 요소가 포함된다. 여기에는 소스 코드(Source Code), 실행 파일(Executable), 메시지(Message) 정의, 서비스(Service) 인터페이스, 액션(Action) 인터페이스, 실행 파일(Launch File), 설정 파일(Configuration File), 파라미터(Parameter) 파일, 로봇 모델(URDF, Unified Robot Description Format), 인공지능 모델(AI Model), 시뮬레이션 자산(Simulation Asset), 보정 데이터(Calibration Data), 테스트 코드(Test Code), 문서(Document), 그리고 의존성 정보(Dependency Metadata)가 포함될 수 있다.

예를 들어 현대 AMR 시스템에서는 위치 추정(Localization), 라이다(LiDAR) 드라이버, 카메라 인터페이스(Camera Interface), 모터 제어(Motor Control), 자율주행(Navigation), 동시적 위치추정 및 지도작성(SLAM, Simultaneous Localization and Mapping), 인공지능 인지(AI Perception), 진단(Diagnostics), 플릿 통신(Fleet Communication), 안전 모니터링(Safety Monitoring), 클라우드 동기화(Cloud Synchronization) 등을 각각 독립적인 패키지로 구성할 수 있다. 이렇게 기능을 분리하면 특정 기능을 수정하거나 개선할 때 다른 시스템에 미치는 영향을 최소화할 수 있다.

패키지 구조는 소프트웨어 재사용성도 크게 향상시킨다. 한 로봇에서 개발된 자율주행 패키지를 다른 로봇에서도 사용할 수 있으며, 센서 드라이버 패키지 역시 여러 프로젝트에서 재활용할 수 있다. 인공지능 인지 패키지(AI Perception Package) 역시 조직 내부 또는 오픈소스(Open Source)를 통해 공유할 수 있다. 이러한 재사용성은 개발 기간을 단축하고 중복 개발을 줄이는 데 큰 도움이 된다.

모든 ROS2 패키지는 특정 메타데이터(Metadata) 파일을 포함한다. 그중 가장 중요한 파일은 \`package.xml\`이다. 이 파일은 패키지 이름(Name), 버전(Version), 설명(Description), 유지관리자(Maintainer), 라이선스(License), 그리고 의존성(Dependency) 정보를 정의한다. 로봇 시스템은 수많은 소프트웨어가 서로 연결되어 있기 때문에 의존성 관리가 매우 중요하다. ROS2는 이러한 정보를 이용하여 빌드(Build)와 실행(Runtime)에 필요한 패키지를 자동으로 관리한다.

또 다른 핵심 파일은 \`CMakeLists.txt\`이다. ROS2는 CMake와 ament 빌드 시스템(Build System)을 사용한다. 이 파일은 실행 파일, 라이브러리(Library), 인터페이스(Interface), 설치 규칙(Installation Rule)을 정의한다. ament는 일반적인 CMake 기능을 확장하여 로봇 개발에 필요한 의존성 관리, 테스트 통합, 워크스페이스 수준의 빌드 관리를 제공한다.

ROS2 패키지는 다양한 프로그래밍 언어를 동시에 지원한다. C++ 기반 패키지는 주로 rclcpp 라이브러리를 사용하며, 높은 성능과 실시간성(Real-Time Performance)이 필요한 시스템에 적합하다. Python 기반 패키지는 rclpy 라이브러리를 사용하며, 빠른 개발과 AI 통합에 유리하다. 실제 로봇에서는 저수준 제어는 C++로 구현하고, AI 및 클라우드 연동은 Python으로 구현하는 혼합 구조(Mixed-Language Architecture)가 매우 흔하다.

ROS2 패키지의 내부 구조는 표준화되어 있다. 일반적으로 \`src\` 디렉터리는 소스 코드를 저장하고, \`include\` 디렉터리는 헤더 파일(Header File)을 저장한다. \`launch\` 디렉터리는 실행 파일을 관리하며, \`config\` 디렉터리는 YAML 기반 파라미터 파일(Parameter File)을 저장한다. 또한 \`msg\`, \`srv\`, \`action\` 디렉터리는 각각 메시지, 서비스, 액션 인터페이스를 정의한다. 로봇 모델, 메시(Mesh), 센서 보정 데이터, 시뮬레이션 파일도 별도의 디렉터리에 저장된다. 이러한 표준화는 협업과 유지보수를 크게 향상시킨다.

런치 파일(Launch File)은 ROS2 패키지와 매우 밀접한 관계를 가진다. 현대 로봇은 하나의 실행 파일만으로 동작하지 않는다. 일반적으로 수십 개의 노드가 동시에 실행되며 서로 연결된다. 런치 파일은 여러 노드를 한 번에 실행하고, 파라미터를 설정하고, 네임스페이스(Namespace)를 지정하며, 전체 시스템을 자동으로 초기화하는 역할을 수행한다.

ROS2는 ROS1에서 사용되던 XML 기반 실행 구조 대신 Python 기반 런치 시스템(Python-Based Launch System)을 도입하였다. Python을 사용하면 조건문, 반복문, 환경 변수(Environment Variable), 동적 설정(Dynamic Configuration) 등을 활용할 수 있기 때문에 훨씬 유연한 시스템 구성이 가능하다. 대규모 산업용 로봇에서는 복잡한 런치 구조를 통해 전체 시스템을 관리하는 경우가 많다.

패키지는 통신 인터페이스도 정의한다. \`msg\` 디렉터리에는 토픽(Topic)을 위한 메시지 정의가 저장되며, \`srv\` 디렉터리에는 서비스(Service) 인터페이스가 저장된다. 또한 \`action\` 디렉터리에는 액션(Action) 인터페이스가 정의된다. ROS2는 이러한 정의를 기반으로 자동으로 통신 코드를 생성한다.

대규모 로봇 시스템에서는 의존성 관리(Dependency Management)가 매우 중요하다. 하나의 자율주행 패키지는 좌표 정보(Geometry Message), 변환 시스템(tf2), 센서 드라이버, 위치 추정, 행동 트리(Behavior Tree) 등 수많은 패키지에 의존할 수 있다. ROS2는 패키지 메타데이터를 이용하여 자동으로 빌드 순서를 결정하고 의존성을 관리한다.

ROS2의 대표적인 빌드 도구(Build Tool)는 colcon이다. colcon은 워크스페이스 안의 모든 패키지를 자동으로 탐색하고, 의존성을 분석하고, 컴파일(Compile)과 설치(Install)를 수행한다. 또한 변경된 패키지만 다시 빌드하는 증분 빌드(Incremental Build)를 지원하여 개발 효율성을 높인다.

워크스페이스는 ROS2 개발 환경 전체를 의미한다. 일반적으로 하나의 워크스페이스 안에는 여러 개의 패키지가 존재한다. 워크스페이스를 사용하면 프로젝트를 분리하고, 빌드 결과를 관리하며, 서로 다른 로봇 프로젝트를 독립적으로 운영할 수 있다.

일반적인 ROS2 워크스페이스는 네 개의 주요 디렉터리로 구성된다.

\* \`src\` : 소스 패키지 저장

\* \`build\` : 컴파일 중간 파일 저장

\* \`install\` : 실행 파일 및 라이브러리 저장

\* \`log\` : 빌드 및 실행 로그 저장

개발자는 대부분 \`src\` 디렉터리에서 작업하며, 나머지 디렉터리는 ROS2가 자동으로 관리한다.

워크스페이스의 중요한 개념 중 하나는 환경 설정(Environment Sourcing)이다. 빌드가 완료되면 \`setup.bash\` 파일을 실행하여 환경 변수를 설정해야 한다. 이 과정은 패키지 검색(Path Resolution), 실행 파일 탐색(Executable Discovery), 라이브러리 연결(Library Resolution), DDS 통신 환경 구성을 담당한다.

ROS2는 오버레이 워크스페이스(Overlay Workspace)라는 강력한 기능도 제공한다. 여러 워크스페이스를 계층적으로 쌓아 올릴 수 있으며, 상위 워크스페이스가 하위 워크스페이스의 기능을 확장하거나 덮어쓸 수 있다. 이는 산업용 로봇 개발에서 매우 유용하다.

예를 들어 로봇 회사가 공통 자율주행 플랫폼을 개발했다고 가정해 보자. 모든 AMR 제품은 동일한 기본 워크스페이스를 공유할 수 있다. 이후 제품별 워크스페이스를 추가하여 센서 설정, AI 모델, 고객 맞춤 기능을 구현할 수 있다. 이러한 계층형 구조(Layered Architecture)는 제품군(Product Family)을 효율적으로 관리할 수 있게 해준다.

ROS2 패키지와 워크스페이스는 협업 개발(Collaborative Development)에도 매우 적합하다. 기계 설계 엔지니어, 임베디드 개발자, AI 연구원, 인지 시스템 개발자, 자율주행 엔지니어, 클라우드 개발자, DevOps 엔지니어가 동시에 작업하더라도 각자의 패키지를 독립적으로 개발할 수 있다.

또한 Git과 같은 버전 관리 시스템(Version Control System)과도 자연스럽게 통합된다. 각 패키지는 독립적으로 관리될 수 있으며, 지속적 통합 및 배포(CI/CD, Continuous Integration/Continuous Deployment) 환경에서도 효율적으로 활용된다.

테스트(Test) 역시 패키지 단위로 수행된다. 단위 테스트(Unit Test), 통합 테스트(Integration Test), 시뮬레이션 검증(Simulation Validation), 하드웨어 연동 테스트(Hardware-in-the-Loop Test), 성능 평가(Performance Benchmark) 등을 각 패키지에 포함할 수 있다. ROS2의 ament 빌드 시스템은 이러한 테스트를 자동으로 수행할 수 있도록 지원한다.

가제보(Gazebo), 아이작 심(Isaac Sim)과 같은 시뮬레이터 역시 패키지 구조를 적극적으로 활용한다. 로봇 모델(URDF), 센서 설정, 플러그인(Plugin), 디지털 트윈(Digital Twin) 환경 등이 각각 독립적인 패키지로 구성될 수 있다. 이는 시뮬레이션과 실제 로봇 간의 일관성을 유지하는 데 매우 중요하다.

최근에는 도커(Docker)와 쿠버네티스(Kubernetes)를 활용한 클라우드 네이티브 로보틱스(Cloud-Native Robotics)가 확산되고 있다. ROS2 워크스페이스는 이러한 컨테이너(Container) 기반 환경과도 잘 통합되며, OTA(Over-The-Air) 업데이트, 클라우드 동기화, 대규모 배포를 지원한다.

결론적으로 ROS2의 패키지(Package)와 워크스페이스(Workspace)는 단순한 폴더 구조나 빌드 도구가 아니다. 그것들은 현대 로봇 시스템의 **소프트웨어 조직 아키텍처(Software Organizational Architecture)** 이다. 이를 통해 자율주행, 인공지능, 시뮬레이션, 클라우드, 실시간 제어, 플릿 관리 시스템을 체계적으로 구성하고 확장할 수 있으며, 대규모 산업용 로봇 생태계를 구축하기 위한 핵심 기반 기술로 활용되고 있다.

## 01.6 ROS2 tf2 and Coordinate Frames

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

# 01.06 ROS2 tf2와 좌표계 (ROS2 tf2 and Coordinate Frames)

**ROS2 tf2와 좌표계(ROS2 tf2 and Coordinate Frames)** 는 로봇 소프트웨어 개발에서 가장 중요하고 필수적인 개념 중 하나인 공간 좌표 관리(Spatial Coordinate Management)와 좌표 변환 시스템(Coordinate Transformation System)을 다룬다. 현대의 자율주행 로봇은 수많은 센서(Sensor), 액추에이터(Actuator), 인공지능 모듈(AI Module), 위치 추정 시스템(Localization System), 자율주행 알고리즘(Navigation Algorithm), 그리고 인지 파이프라인(Perception Pipeline)으로 구성된다. 이러한 모든 구성 요소는 하나 이상의 좌표계(Coordinate Frame)를 기준으로 동작한다. ROS2의 tf2는 이러한 좌표계 간의 관계를 일관성 있게 관리하고 동기화하며 계산할 수 있도록 지원하는 핵심 인프라이다.

로봇 공학의 본질은 공간을 이해하는 것이다. 로봇은 현재 자신의 위치가 어디인지, 센서가 어디에 장착되어 있는지, 주변 물체가 어디에 존재하는지, 바퀴와 관절이 어떻게 움직이고 있는지, 그리고 이러한 관계가 시간에 따라 어떻게 변화하는지를 알아야 한다. 일반적인 소프트웨어가 추상적인 데이터 구조(Data Structure)를 처리하는 것과 달리, 로봇은 실제 물리 공간(Physical Space)에 존재하는 기하학적 정보(Geometric Information)를 지속적으로 처리해야 한다. 따라서 좌표계와 좌표 변환은 로봇 지능(Robotic Intelligence)의 핵심 요소가 된다.

좌표계(Coordinate Frame)는 공간 속 위치(Position)와 방향(Orientation)을 정의하는 기준 체계라고 이해할 수 있다. 일반적으로 원점(Origin)과 X축, Y축, Z축으로 구성된다. 로봇 시스템에는 다양한 좌표계가 동시에 존재한다. 로봇 본체를 나타내는 기준 좌표계(Base Frame), 바퀴 위치를 나타내는 휠 좌표계(Wheel Frame), 카메라 방향을 나타내는 카메라 좌표계(Camera Frame), 라이다 스캔 기준 좌표계(LiDAR Frame), 관성측정장치 좌표계(IMU Frame), 로봇 팔 관절 좌표계(Joint Frame), 지도 좌표계(Map Frame), 그리고 오도메트리 좌표계(Odometry Frame) 등이 대표적인 예이다.

문제는 이러한 좌표계가 모두 서로 다르다는 점이다. 예를 들어 로봇 상단에 장착된 라이다는 전방 카메라와 다른 위치와 방향에서 세상을 바라본다. IMU는 자신의 내부 축을 기준으로 가속도와 각속도를 측정한다. 자율주행 시스템은 지도 좌표계(Map Frame) 기준으로 경로를 계산하지만, 모터 제어기는 로봇 본체 좌표계(Base Frame)를 기준으로 동작한다. 따라서 로봇이 올바르게 동작하기 위해서는 이들 좌표계 사이를 지속적으로 변환할 수 있어야 한다.

바로 이러한 문제를 해결하기 위해 ROS2는 tf2 프레임워크를 제공한다. tf2는 분산형 실시간 좌표 변환 시스템(Distributed Real-Time Transformation System)으로서, 다양한 노드(Node)가 좌표계 정보를 발행(Publish)하고 저장(Store)하며 조회(Query)하고 계산(Compute)할 수 있도록 지원한다. 개발자가 모든 좌표 변환을 직접 계산하는 대신 tf2가 이를 중앙 인프라처럼 관리해 준다.

ROS1에서는 tf라는 시스템을 사용했지만 ROS2에서는 향상된 tf2 아키텍처를 사용한다. tf2는 시간 관리(Time Management), 버퍼(Buffer) 구조, 타입 안전성(Type Safety), 지연 시간(Latency), 실시간성(Real-Time Capability) 측면에서 크게 개선되었다. 오늘날 ROS2 기반의 거의 모든 자율주행, 위치 추정, 인지, 로봇 팔 제어 시스템은 tf2를 사용하고 있다.

tf2의 핵심 구조는 변환 트리(Transformation Tree)이다. 각 좌표계는 하나의 노드처럼 표현되며, 부모 좌표계(Parent Frame)와 자식 좌표계(Child Frame)의 관계를 통해 공간 구조를 형성한다. 예를 들어 로봇 본체 좌표계(base_link)는 카메라, 라이다, 바퀴 좌표계의 부모가 될 수 있다. 지도 좌표계(map)는 오도메트리 좌표계(odom)의 부모가 되고, 오도메트리 좌표계는 다시 base_link의 부모가 될 수 있다. 이러한 연결 구조가 하나의 공간 트리를 형성한다.

tf2의 가장 중요한 특징 중 하나는 시간 의존성(Time Dependency)이다. 로봇은 끊임없이 움직이며 주변 환경도 변화한다. 따라서 좌표 변환은 단순히 고정된 값이 아니라 특정 시점(Time Stamp)에 대한 값이어야 한다. 창고를 이동하는 AMR은 매 순간 위치가 바뀌며, 로봇 팔은 지속적으로 관절 각도를 변경하고, 팬틸트 카메라(Pan-Tilt Camera)는 회전하면서 시야를 변경한다. tf2는 이러한 시간 정보를 포함한 좌표 변환 이력(Transformation History)을 관리한다.

이러한 시간 관리 기능은 센서 융합(Sensor Fusion)에서 매우 중요하다. 예를 들어 라이다는 초당 10회(10 Hz), 카메라는 초당 30프레임(30 FPS), IMU는 초당 200회(200 Hz)의 데이터를 생성할 수 있다. 각 센서는 서로 다른 시점에 데이터를 생성하기 때문에 단순한 좌표 변환만으로는 정확한 융합이 어렵다. tf2는 시간 정보를 고려한 좌표 변환을 제공하여 다양한 센서 데이터를 정확하게 정렬할 수 있게 해준다.

모바일 로봇(Mobile Robot)에서 가장 많이 사용되는 좌표계는 지도(map), 오도메트리(odom), 그리고 로봇 본체(base_link)이다. 지도 좌표계는 전역(Global) 기준 좌표계이며, 위치 추정 시스템은 이 좌표계에서 로봇의 위치를 계산한다. 오도메트리 좌표계는 휠 엔코더(Wheel Encoder), IMU, 비전 오도메트리(Visual Odometry) 등을 이용해 계산되는 지역(Local) 이동 좌표계이다. base_link는 실제 로봇 본체를 나타내는 좌표계이다. 카메라(camera_link), 라이다(laser_frame) 등의 센서 좌표계는 일반적으로 base_link 아래에 연결된다.

특히 map과 odom의 차이를 이해하는 것은 매우 중요하다. map은 SLAM 또는 위치 추정 시스템에 의해 보정된 전역 좌표계이다. 반면 odom은 짧은 시간 동안 부드럽고 연속적인 움직임을 제공하지만 시간이 지나면 오차(Drift)가 누적된다. 따라서 map은 전역 정확성을 제공하고, odom은 제어(Control)와 경로 계획(Path Planning)에 필요한 연속성을 제공한다. 이 두 좌표계를 분리함으로써 자율주행의 안정성과 정확성을 동시에 확보할 수 있다.

tf2의 좌표 변환은 수학적으로 이동(Translation)과 회전(Rotation)으로 구성된다. 이동은 두 좌표계 사이의 위치 차이를 나타내고, 회전은 방향 차이를 나타낸다. 회전 표현에는 오일러 각(Euler Angle)보다 쿼터니언(Quaternion)이 주로 사용된다. 쿼터니언은 짐벌락(Gimbal Lock) 문제를 방지하고 수치적으로 안정적이기 때문에 로봇 시스템에서 표준적으로 사용된다.

정적 변환(Static Transform)은 시간이 지나도 변하지 않는 좌표 관계를 의미한다. 예를 들어 로봇에 고정된 라이다와 본체 간의 위치 관계는 항상 동일하다. ROS2는 이러한 관계를 효율적으로 관리하기 위해 정적 변환 발행기(Static Transform Publisher)를 제공한다.

반면 동적 변환(Dynamic Transform)은 지속적으로 변화한다. 휠 오도메트리, 로봇 팔 관절, SLAM 기반 지도 정렬 등이 대표적인 예이다. 동적 변환 발행기(Dynamic Transform Broadcaster)는 이러한 정보를 지속적으로 tf2 시스템에 전송한다.

tf2는 발행기(Broadcaster)와 수신기(Listener) 구조를 사용한다. 발행기는 좌표 변환 정보를 전송하고, 수신기는 필요한 시점에 해당 정보를 조회한다. 내부적으로는 \`/tf\` 토픽(Topic)이 동적 변환을 담당하고, \`/tf_static\` 토픽이 정적 변환을 담당한다.

버퍼(Buffer)는 tf2의 핵심 구성 요소 중 하나이다. tf2는 과거 좌표 변환 기록을 일정 시간 동안 저장한다. 노드가 특정 시점의 좌표 변환을 요청하면 버퍼에 저장된 이력을 기반으로 적절한 변환을 제공한다.

보간(Interpolation) 기능도 매우 중요하다. 센서 데이터는 정확히 같은 시점에 생성되지 않는다. 따라서 tf2는 두 시점 사이의 변환을 계산하여 부드럽고 일관된 좌표 변환을 제공한다. 이는 센서 융합과 자율주행 성능 향상에 필수적인 기능이다.

인지 시스템(Perception System)은 tf2를 가장 많이 활용하는 분야 중 하나이다. 카메라는 자신의 좌표계에서 객체를 인식하지만, 자율주행 시스템은 이를 로봇 기준 또는 지도 기준 좌표로 변환해야 한다. tf2는 이러한 좌표 변환을 자동으로 수행하여 인지 결과를 전체 시스템이 공유할 수 있도록 한다.

라이다-카메라 융합(LiDAR-Camera Fusion) 역시 tf2 없이는 구현하기 어렵다. 라이다 포인트 클라우드(Point Cloud)를 카메라 영상 좌표계로 변환해야 하기 때문이다. 이를 통해 다중모달 인지(Multimodal Perception)와 인공지능 융합(AI Fusion)이 가능해진다.

자율주행(Navigation), 로봇 팔 조작(Manipulation), 다중 로봇(Multi-Robot), 클라우드 로보틱스(Cloud Robotics), 디지털 트윈(Digital Twin), 시뮬레이션(Simulation), 체화형 인공지능(Embodied AI) 등 거의 모든 현대 로봇 시스템은 tf2에 의존한다. 특히 휴머노이드(Humanoid)는 수십 개 이상의 관절 좌표계를 동시에 관리해야 하므로 tf2의 중요성이 더욱 크다.

결론적으로 ROS2의 tf2와 좌표계(Coordinate Frame) 시스템은 단순한 좌표 변환 라이브러리가 아니다. 그것은 로봇이 공간을 이해하고, 센서를 통합하고, 위치를 추정하고, 자율주행을 수행하며, 물체를 조작하고, 인공지능 기반 추론을 수행하기 위한 **기하학적 신경계(Geometric Nervous System)** 이다. tf2는 현대 자율주행 로봇과 체화형 인공지능 시스템을 가능하게 하는 가장 중요한 기반 기술 중 하나라고 할 수 있다.

## 01.7 ROS2 Communication Debugging

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

# 01.07 ROS2 통신 디버깅 (ROS2 Communication Debugging)

**ROS2 통신 디버깅(ROS2 Communication Debugging)** 은 실제 로봇 개발에서 반드시 익혀야 하는 핵심 기술 중 하나이다. ROS2는 높은 확장성(Scalability)과 모듈성(Modularity)을 제공하는 분산형 로봇 플랫폼이지만, 이러한 분산 구조 때문에 다양한 통신 문제가 발생할 수 있다. 노드(Node)가 서로를 발견하지 못하거나, 메시지(Message)가 전달되지 않거나, 서비스 품질(QoS, Quality of Service) 설정이 충돌하거나, 네트워크 지연(Network Latency)으로 인해 시스템이 불안정해질 수 있다. 또한 DDS(데이터 분배 서비스, Data Distribution Service)의 동작 방식, 다중 컴퓨터(Multi-Machine) 환경, 시간 동기화(Time Synchronization) 문제 등도 복잡하게 얽혀 있다. 따라서 ROS2를 사용하는 로봇 엔지니어에게 통신 디버깅 능력은 필수 역량이라고 할 수 있다.

현대 자율주행 로봇은 본질적으로 분산 소프트웨어 시스템(Distributed Software System)이다. 전통적인 단일 프로그램(Monolithic Application)과 달리 ROS2 기반 로봇은 수많은 독립 노드가 토픽(Topic), 서비스(Service), 액션(Action), tf2 변환 네트워크(tf2 Transformation Network)를 통해 비동기적으로 통신한다. 이러한 노드들은 여러 프로세스(Process), CPU, GPU, 임베디드 시스템(Embedded System), 엣지 컴퓨터(Edge Computer), 클라우드 서버(Cloud Server), 무선 네트워크(Wireless Network)에 분산되어 동작한다. 시스템 규모가 커질수록 통신 상태를 직접 관찰하고 분석하기가 매우 어려워진다. ROS2의 디버깅 도구는 이러한 분산 시스템의 가시성(Observability)을 확보하기 위한 핵심 수단이다.

ROS2 통신 문제는 여러 계층(Layer)에서 발생할 수 있다. 응용 프로그램(Application) 수준의 문제일 수도 있고, ROS2 미들웨어(Middleware) 수준, DDS 전송 계층(Transport Layer), 운영체제(Operating System), 네트워크 인프라(Network Infrastructure), 하드웨어 동기화(Hardware Synchronization) 문제일 수도 있다. 따라서 효과적인 디버깅은 단순히 특정 노드만 보는 것이 아니라 전체 통신 파이프라인(Communication Pipeline)을 체계적으로 분석하는 접근이 필요하다.

가장 흔한 문제 중 하나는 노드 발견 실패(Node Discovery Failure)이다. ROS2는 ROS1과 달리 ROS 마스터(ROS Master)를 사용하지 않고 DDS 기반의 피어투피어(Peer-to-Peer) 발견 기능을 사용한다. 노드들은 자동으로 발행자(Publisher), 구독자(Subscriber), 서비스, 액션을 탐색한다. 그러나 방화벽(Firewall), 멀티캐스트(Multicast) 차단, DDS 설정 오류, ROS 도메인 ID(ROS_DOMAIN_ID) 불일치, VPN 설정 문제, 컨테이너(Container) 네트워크 분리 등으로 인해 발견이 실패할 수 있다.

특히 ROS_DOMAIN_ID 설정은 가장 먼저 확인해야 하는 항목 중 하나이다. 서로 다른 도메인 ID를 사용하는 노드들은 절대 서로를 발견할 수 없다. 실제 산업 현장에서는 여러 로봇 시스템을 분리하기 위해 의도적으로 다른 도메인을 사용하기도 한다. 그러나 설정 오류로 인해 통신이 되지 않는 경우도 매우 많다.

DDS 구현체(DDS Implementation)의 차이도 통신에 영향을 미친다. ROS2는 Fast DDS(FastDDS), 사이클론 DDS(CycloneDDS), RTI Connext DDS, 구름 DDS(GurumDDS) 등을 지원한다. 각 DDS는 발견 속도, 멀티캐스트 처리 방식, 지연 시간, 메모리 사용량, QoS 처리 방식에서 차이가 존재한다. 따라서 문제 해결 과정에서는 DDS 설정과 버전을 확인하는 것이 중요하다.

토픽은 ROS2 디버깅에서 가장 많이 분석되는 대상이다. 개발자는 현재 어떤 토픽이 활성화되어 있는지, 발행자가 정상적으로 메시지를 전송하고 있는지, 구독자가 이를 수신하고 있는지, 메시지 주기(Frequency)가 안정적인지 확인해야 한다. ROS2는 이를 위해 다양한 명령어를 제공한다.

예를 들어 \`ros2 topic list\` 명령은 현재 활성화된 토픽 목록을 보여준다. 특정 노드가 예상한 토픽을 생성하지 않는다면 즉시 확인할 수 있다. \`ros2 topic echo\` 명령은 실시간으로 메시지 내용을 출력한다. 이를 통해 센서 데이터, 속도 명령(Velocity Command), 위치 추정 결과(Localization Result), AI 탐지 결과(AI Detection Result)를 직접 확인할 수 있다.

\`ros2 topic hz\` 명령은 메시지 발행 주기(Message Frequency)를 측정한다. 자율주행, 센서 융합(Sensor Fusion), 위치 추정, 제어 시스템은 일정한 데이터 주기를 요구한다. 만약 10Hz로 동작해야 하는 센서가 실제로는 2Hz만 출력한다면 성능 문제가 발생할 수 있다. 이러한 경우 CPU 과부하(CPU Overload), DDS 병목(Bottleneck), 네트워크 혼잡(Network Congestion) 등을 의심해야 한다.

대역폭(Bandwidth) 분석도 중요하다. 고해상도 카메라, 라이다(LiDAR), 레이더(Radar), 깊이 카메라(Depth Camera)는 초당 수백 MB 이상의 데이터를 생성할 수 있다. 지나치게 많은 데이터는 DDS 계층이나 무선 네트워크를 포화시켜 패킷 손실(Packet Loss)을 유발할 수 있다. 따라서 통신 효율을 분석하고 최적화하는 작업이 필요하다.

메시지 타입(Message Type) 불일치 역시 흔한 문제이다. 발행자와 구독자는 동일한 메시지 정의(Message Definition)를 사용해야 한다. 예를 들어 한 노드는 \`geometry_msgs\`를 사용하고 다른 노드는 사용자 정의 메시지(Custom Message)를 기대한다면 통신이 실패할 수 있다. ROS2 도구는 이러한 타입 정보를 확인하는 기능도 제공한다.

QoS 디버깅은 ROS2 통신 분석의 핵심이다. QoS는 신뢰성(Reliability), 지속성(Durability), 히스토리(History), 마감 시간(Deadline), 생존성(Liveliness) 등을 제어한다. 발행자와 구독자의 QoS 설정이 서로 맞지 않으면 통신이 연결되지 않을 수 있다. 실제 산업 현장에서는 QoS 불일치가 가장 흔한 숨겨진 통신 오류 중 하나이다.

예를 들어 구독자는 Reliable 모드를 요구하지만 발행자는 Best Effort 모드를 사용하는 경우, DDS 구현체에 따라 연결이 실패할 수 있다. 따라서 현재 활성화된 QoS 정책을 확인하고 비교하는 작업이 중요하다.

서비스와 액션도 별도의 디버깅이 필요하다. 서비스 클라이언트(Service Client)가 서버(Service Server)를 찾지 못하거나, 요청(Request)이 시간 초과(Timeout)될 수 있다. 액션(Action)의 경우 목표(Goal)가 수락되지 않거나, 피드백(Feedback)이 전달되지 않거나, 결과(Result)가 반환되지 않을 수도 있다. ROS2는 서비스 목록 확인, 인터페이스 조회, 수동 서비스 호출 등의 기능을 제공한다.

특히 액션은 자율주행, 도킹(Docking), 물체 조작(Manipulation), 순찰(Patrol)과 같은 장시간 작업(Long-Duration Task)에 사용되기 때문에 디버깅의 중요성이 더욱 크다. 목표가 정상적으로 전달되는지, 피드백이 지속적으로 수신되는지, 취소(Cancel) 요청이 정상적으로 반영되는지 확인해야 한다.

tf2 디버깅도 매우 중요하다. 좌표 변환(Transform) 오류는 자율주행 시스템 전체를 불안정하게 만들 수 있다. 변환이 존재하지 않거나, 시간 정보가 일치하지 않거나, 부모-자식(Parent-Child) 관계가 잘못 정의된 경우 위치 추정, 센서 융합, 경로 계획이 모두 실패할 수 있다.

ROS2는 tf2_echo, tf2_monitor, view_frames와 같은 전용 디버깅 도구를 제공한다. 개발자는 이를 이용하여 좌표 트리(Frame Tree)를 시각화하고, 변환 지연 시간(Latency)을 측정하며, 누락된 좌표계를 찾을 수 있다. 또한 RViz를 이용하면 좌표계와 센서 데이터를 시각적으로 확인할 수 있다.

시간 동기화(Time Synchronization)는 분산 시스템에서 매우 중요하다. 여러 센서가 서로 다른 주기로 동작하기 때문에 시간 정보가 정확해야 한다. 잘못된 타임스탬프(Timestamp)는 센서 융합 오류, 위치 추정 오차, AI 추론 결과 불일치를 초래할 수 있다.

다중 컴퓨터 환경에서는 네트워크 시간 프로토콜(NTP, Network Time Protocol) 또는 정밀 시간 프로토콜(PTP, Precision Time Protocol)을 사용하여 시계를 동기화한다. 특히 고정밀 자율주행 시스템에서는 하드웨어 수준의 시간 동기화가 요구된다.

네트워크 디버깅(Network Debugging) 역시 필수적이다. DDS는 멀티캐스트를 활용하기 때문에 기업 네트워크, VPN, 무선 공유기, 컨테이너 네트워크 설정에 의해 차단될 수 있다. 따라서 라우팅 테이블(Routing Table), 방화벽 규칙(Firewall Rule), 멀티캐스트 설정 등을 점검해야 한다.

무선 네트워크(Wireless Network)는 추가적인 문제를 발생시킨다. 패킷 손실, 지연 시간 증가, 로밍(Roaming), 간섭(Interference)은 클라우드 로보틱스(Cloud Robotics), 원격 조작(Teleoperation), 플릿 관리(Fleet Management)에 큰 영향을 준다.

ROS2 백(Rosbag2)은 가장 강력한 디버깅 도구 중 하나이다. rosbag2는 모든 토픽 데이터를 기록 (Record)하고 재생(Replay)할 수 있다. 실제 환경에서 발생한 문제를 다시 재현하여 분석할 수 있기 때문에 매우 유용하다. 특히 자율주행 실패, AI 인식 오류, 센서 융합 문제를 반복적으로 분석하는 데 활용된다.

RViz는 시각적 디버깅(Visual Debugging)의 핵심 도구이다. 라이다 데이터, 카메라 영상, 점유 격자 지도 (Occupancy Grid Map), 경로(Path), tf2 좌표계, AI 객체 탐지 결과 등을 동시에 시각화할 수 있다. 숫자로 보기 어려운 문제를 직관적으로 분석할 수 있다.

로깅 시스템(Logging System)도 중요하다. ROS2는 디버그(Debug), 정보(Info), 경고(Warning), 오류 (Error), 치명적 오류 (Fatal) 수준의 로그를 제공한다. 개발자는 로그 레벨(Log Level)을 조정하여 문제를 보다 쉽게 추적할 수 있다.

최근의 체화형 인공지능(Embodied AI), 다중 로봇 시스템(Multi-Robot System), 클라우드 로보틱스, 플릿 학습(Fleet Learning), 에지-클라우드 동기화(Edge-Cloud Synchronization)는 모두 복잡한 통신 구조에 의존한다. 이러한 시스템에서는 통신 디버깅이 단순한 문제 해결이 아니라 전체 시스템의 안정성과 성능을 보장하기 위한 핵심 기술이 된다.

결론적으로 ROS2 통신 디버깅은 단순히 오류를 찾는 작업이 아니다. 그것은 분산 지능 시스템 (Distributed Intelligent System)의 내부를 관찰하고, 분석하고, 최적화하고, 안정화하는 **운영 가시성 인프라(Operational Observability Infrastructure)** 이다. 앞으로 휴머노이드(Humanoid), 물리형 인공지능(Physical AI), 자율주행 플랫폼, 클라우드 기반 로봇 시스템이 발전할수록 ROS2 통신 디버깅의 중요성은 더욱 커질 것이다.

## 01.8 ROS2 for Industrial AMR

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

# 01.08 산업용 자율이동로봇을 위한 ROS2 (ROS2 for Industrial AMR)

**산업용 자율이동로봇을 위한 ROS2(ROS2 for Industrial AMR)** 는 현대 산업용 자율이동로봇(AMR, Autonomous Mobile Robot)의 핵심 소프트웨어 아키텍처를 설명한다. ROS2는 원래 연구 및 학술 환경에서 시작되었지만, 현재는 공장(Factory), 물류센터(Logistics Center), 창고(Warehouse), 항만(Port), 스마트시티(Smart City), 플랜트(Plant), 대규모 산업 인프라(Infrastructure) 환경에서 운용되는 산업용 로봇의 사실상 표준 미들웨어(Middleware)로 자리 잡고 있다. 산업용 AMR은 확장성(Scalability), 모듈성(Modularity), 분산 통신(Distributed Communication), 실시간성(Real-Time Capability), 다중 센서 융합(Multi-Sensor Fusion), 플릿 관리(Fleet Management), 인공지능 기반 인지(AI Perception), 클라우드 연동(Cloud Connectivity), 안전 모니터링(Safety Monitoring), 그리고 장기 유지보수(Long-Term Maintainability)를 요구한다. ROS2는 이러한 요구사항을 충족할 수 있는 핵심 기술을 제공한다.

산업용 AMR은 교육용 로봇이나 연구용 프로토타입과는 매우 다르다. 실제 산업 현장의 로봇은 하루 수 시간 또는 수십 시간 동안 지속적으로 운용되며, 작업자(Worker), 지게차(Forklift), 생산 설비(Machinery), 선반(Rack), 차량(Vehicle), 팔레트(Pallet), 적재 장비(Loading Equipment), 그리고 예측하기 어려운 장애물(Unpredictable Obstacle)이 존재하는 환경에서 동작한다. 또한 조명 변화(Lighting Variation), 네트워크 상태(Network Condition), 기상 조건(Weather Condition), 바닥 상태(Floor Condition), 적재 하중(Payload Variation), 운영 일정(Operation Schedule) 등의 영향을 받는다. 따라서 산업용 로봇은 실험실 환경과 달리 불안정한 소프트웨어나 수동 개입(Manual Intervention)에 의존할 수 없다. ROS2는 단순한 개발 프레임워크를 넘어 산업용 로봇 지능을 지탱하는 운영 플랫폼(Operation Platform)의 역할을 수행한다.

ROS2의 가장 큰 특징 중 하나는 모듈형 분산 소프트웨어 아키텍처(Modular Distributed Software Architecture)이다. 산업용 AMR은 인지 시스템(Perception System), 자율주행 시스템(Navigation System), 위치 추정(Localization), 인공지능 추론 엔진(AI Inference Engine), 센서 융합(Sensor Fusion), 안전 시스템(Safety System), 플릿 관리(Fleet Management), 진단 시스템(Diagnostics), 클라우드 인터페이스(Cloud Interface), 하드웨어 드라이버(Hardware Driver) 등 수많은 구성 요소로 이루어진다. ROS2는 이들을 독립적인 노드(Node)와 패키지(Package) 형태로 구성할 수 있도록 하여 높은 확장성과 유지보수성을 제공한다.

산업용 AMR에서 ROS2가 중요한 이유 중 하나는 확장성이다. 실제 산업 시스템은 구축 후에도 지속적으로 변화한다. 새로운 센서가 추가될 수 있고, 인공지능 알고리즘이 업그레이드될 수 있으며, 클라우드 인터페이스가 변경될 수 있고, 작업 프로세스가 확장될 수 있다. ROS2는 노드 기반(Node-Based) 구조를 사용하기 때문에 기존 시스템을 전면 수정하지 않고도 새로운 기능을 추가할 수 있다. 센서 노드, AI 인지 노드, 자율주행 노드, 플릿 관리 노드가 서로 독립적으로 발전할 수 있다는 점은 산업 현장에서 매우 큰 장점이다.

분산 아키텍처(Distributed Architecture)는 현대 산업용 AMR에서 매우 중요한 요소이다. 대부분의 산업용 로봇은 하나의 컴퓨터만 사용하지 않는다. 저수준 모터 제어는 임베디드 컨트롤러(Embedded Controller)에서 수행되고, AI 기반 인지 처리는 GPU 기반 엣지 컴퓨터(Edge Computer)에서 수행되며, 클라우드 동기화는 별도의 통신 모듈에서 처리된다. ROS2는 DDS(데이터 분배 서비스, Data Distribution Service)를 기반으로 이러한 분산 컴퓨팅 환경을 자연스럽게 연결한다. 이를 통해 연산 부하를 분산시키고 장애 격리(Fault Isolation)를 실현할 수 있다.

DDS는 산업 환경에서 ROS2가 ROS1보다 훨씬 강력한 이유 중 하나이다. ROS1은 ROS 마스터(ROS Master)에 의존하는 중앙집중형 구조를 사용하였다. 반면 ROS2는 DDS 기반의 분산형 피어투피어 통신(Peer-to-Peer Communication)을 사용한다. 따라서 단일 장애점(Single Point of Failure)이 제거되며, 서비스 품질(QoS), 신뢰성(Reliability), 멀티캐스트(Multicast), 결정론적 통신(Deterministic Communication) 등을 지원할 수 있다. 생산 라인이나 물류 운영이 중단되면 큰 비용 손실이 발생하기 때문에 이러한 통신 안정성은 매우 중요하다.

특히 서비스 품질(QoS, Quality of Service) 관리는 산업용 로봇에서 필수적이다. 모든 데이터가 동일한 특성을 가지는 것은 아니다. 예를 들어 긴급 정지(Emergency Stop) 신호는 매우 짧은 지연 시간과 높은 신뢰성을 요구한다. 반면 카메라 영상은 일부 프레임 손실을 허용할 수 있다. 위치 추정 데이터는 일정한 주기성을 유지해야 한다. ROS2의 QoS 정책은 이러한 서로 다른 요구사항을 세밀하게 조정할 수 있도록 지원한다.

산업용 AMR은 강력한 실시간성(Real-Time Capability)도 요구한다. 모터 제어(Motion Control), 장애물 회피(Obstacle Avoidance), 긴급 제동(Emergency Braking), 안전 모니터링(Safety Monitoring)은 모두 밀리초(Millisecond) 단위의 응답성을 필요로 한다. ROS2는 실시간 리눅스(Real-Time Linux)와 연계할 수 있으며, 최적화된 실행기(Executor), DDS 기반 전송 구조, 메모리 관리 최적화 등을 통해 ROS1보다 훨씬 우수한 실시간 성능을 제공한다.

안전 통합(Safety Integration) 역시 ROS2의 중요한 역할이다. 산업용 로봇은 사람과 함께 작업하는 경우가 많다. 따라서 안전 라이다(Safety LiDAR), 비상 정지 회로(E-Stop Circuit), 안전 PLC(Programmable Logic Controller), 이중화 통신(Redundant Communication), 안전 제어기(Safety Controller) 등이 필요하다. ROS2는 안전 감시 노드, 자율주행 시스템, 장애물 감지 파이프라인, 플릿 관리 계층을 유기적으로 연결하여 안전 중심 아키텍처를 구성할 수 있다.

산업 환경은 자율주행에 매우 어려운 조건을 제공한다. 창고에는 반사면(Reflective Surface), 좁은 통로(Narrow Corridor), 움직이는 지게차(Moving Forklift), 수시로 변경되는 레이아웃(Layout Change), 낮은 조도(Poor Lighting)가 존재한다. 실외 환경에서는 비(Rain), 먼지(Dust), 안개(Fog), 눈(Snow), 진동(Vibration), 경사로(Slope), 넓은 개방 공간(Open Space)이 존재한다. 따라서 산업용 AMR은 다중 센서 융합(Multi-Sensor Fusion)에 크게 의존한다. 라이다(LiDAR), 카메라(Camera), 레이더(Radar), 위성항법시스템(GNSS, Global Navigation Satellite System), 관성측정장치(IMU, Inertial Measurement Unit), 휠 오도메트리(Wheel Odometry), 깊이 센서(Depth Sensor), 열화상 카메라(Thermal Camera), 초음파 센서(Ultrasonic Sensor)가 함께 사용된다. ROS2는 이러한 이기종 센서(Heterogeneous Sensor)를 통합하는 통신 기반을 제공한다.

tf2 좌표 변환 시스템(tf2 Transformation System)은 산업용 AMR의 핵심 요소이다. 자율주행과 센서 융합은 정확한 공간 정보(Spatial Information)에 의존한다. ROS2의 tf2는 지도 좌표계(Map Frame), 오도메트리 좌표계(Odometry Frame), 로봇 본체 좌표계(Base Frame), 센서 좌표계(Sensor Frame), 로봇 팔 좌표계(Manipulator Frame) 사이의 변환 관계를 지속적으로 관리한다. 실제 산업용 시스템에서는 수십 개 이상의 좌표계가 동시에 운영된다.

자율주행(Navigation)은 산업용 ROS2 활용의 핵심 분야이다. 내비게이션2(Navigation2, Nav2)는 위치 추정(Localization), 경로 계획(Path Planning), 장애물 회피(Obstacle Avoidance), 복구 동작(Recovery Behavior), 속도 제어(Velocity Control), 행동 트리(Behavior Tree)를 포함하는 완전한 자율주행 스택(Stack)을 제공한다. 산업용 AMR은 이러한 구조를 활용하여 장시간 안정적인 자율주행을 수행한다.

최근에는 행동 트리(Behavior Tree)가 산업용 자율성(Industrial Autonomy)의 핵심 구조로 자리 잡고 있다. 기존 상태 기계(State Machine)는 시스템이 복잡해질수록 유지보수가 어려워진다. 반면 행동 트리는 충전(Charging), 도킹(Docking), 엘리베이터 연동(Elevator Integration), 물류 운반(Logistics Handling), 플릿 협업(Fleet Coordination), 경로 재계획(Replanning) 등을 계층적으로 관리할 수 있다.

플릿 관리(Fleet Management)는 산업용 AMR의 필수 기능이다. 현대 물류센터는 수십 대에서 수백 대의 로봇을 동시에 운영한다. 플릿 관리 시스템은 작업 할당(Task Allocation), 교통 제어(Traffic Control), 충전 스케줄링(Charging Scheduling), 경로 최적화(Route Optimization), 혼잡 관리(Congestion Management), 유지보수 계획(Maintenance Planning)을 수행한다. ROS2는 네임스페이스(Namespace), DDS 도메인(DDS Domain), 클라우드 동기화(Cloud Synchronization)를 통해 이러한 대규모 다중 로봇 환경을 지원한다.

클라우드 로보틱스(Cloud Robotics)도 점점 중요해지고 있다. 산업용 AMR은 클라우드와 연결되어 원격 진단 (Remote Diagnostics), 예지 정비(Predictive Maintenance), AI 모델 업데이트(Model Update), 데이터 분석 (Data Analytics), OTA 업데이트(Over-The-Air Update)를 수행한다. ROS2는 엣지-클라우드(Edge-Cloud) 아키텍처와 자연스럽게 통합될 수 있도록 설계되어 있다.

인공지능(AI) 역시 산업용 AMR의 핵심 기술이 되었다. 객체 탐지(Object Detection), 의미론적 분할 (Semantic Segmentation), 팔레트 인식(Pallet Recognition), 작업자 탐지(Worker Detection), 이상 탐지(Anomaly Detection), 장면 이해(Scene Understanding)는 모두 딥러닝(Deep Learning)에 기반한다. ROS2는 GPU 기반 AI 노드와 자율주행 시스템을 실시간으로 연결하는 통신 플랫폼 역할을 수행한다.

특히 NVIDIA Jetson, RTX 기반 엣지 컴퓨터, 신경망처리장치(NPU, Neural Processing Unit), AI 가속기(AI Accelerator)는 ROS2 생태계에서 널리 사용된다. 힐스로보틱스와 같은 산업용 실외 자율주행 플랫폼에서 사용하는 Jetson Orin과 RTX A6000 Ada GPU 조합도 이러한 ROS2 기반 구조에 매우 적합하다.

산업용 AMR은 진단(Diagnostics)과 관측 가능성(Observability)도 중요하다. 센서 상태, 모터 온도, 배터리 상태, 통신 지연 시간, 위치 추정 신뢰도, AI 추론 안정성 등을 지속적으로 모니터링해야 한다. ROS2의 진단 프레임워크(Diagnostics Framework)는 이러한 상태 정보를 통합 관리할 수 있도록 지원한다.

결론적으로 ROS2는 단순한 로봇 개발 도구가 아니다. 그것은 산업용 AMR의 인지(Perception), 자율주행(Navigation), 안전(Safety), 인공지능(AI), 클라우드(Cloud), 플릿 지능(Fleet Intelligence), 엣지 컴퓨팅(Edge Computing)을 하나로 연결하는 **산업용 로봇의 디지털 신경계(Digital Nervous System)** 이다. 앞으로 체화형 인공지능(Embodied AI), 월드 모델(World Model), 다중 로봇 협업(Multi-Agent Collaboration)이 발전할수록 ROS2는 산업용 자율이동로봇의 핵심 소프트웨어 플랫폼으로서 더욱 중요한 역할을 수행하게 될 것이다.
