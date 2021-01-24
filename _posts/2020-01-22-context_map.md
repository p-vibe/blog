---
layout: post
title:  DDD - 컨텍스트 맵
date:   2021-01-22 07:50:00 +0800
categories: DDD
tag: architecture
sitemap :
  changefreq : daily
  priority : 1.0
---

컨택스트 맵은 통합의 소스 코드 구현을 사용하여 상세하게 나타낼 수 있다.

## 컨텍스트 맵이 필수적인 이유

우리 팀의 컨텍스트 맵은 우리 팀에게 의미가 있지, 다른 팀에게는 그닥 중요한 게 아닐 수 있다. 그들이 DDD를 구현하지 않을 수도 있고, 구현한다면 그들만의 관점에서 고유한 맵을 그려야한다.

초기에 컨텍스트 맵을 생각하지 않는다면 나중에 대참사가 일어날 수 있다. api를 제공해줄줄 알았는데 프로젝트 마무리단계에 그렇지 않는다는 걸 알게된 상황을 생각하자.

프로젝트에서 움직이고 있는 각 모델을 식별하여 바운디드 컨텍스트를 정의하자. 모델이 서로 만나는 지점을 나타내고, 어떤 의사소통이든 명시적으로 변환 관계를 정리하자.

프로젝트에 관한 가정을 맵의 형태로 나타낸다면 구분된 바운디드 컨텍스트를 생각해보게 되고, 관련된 용어 그룹을 모을 수 있을 것이고, 이는 강제적으로 언어적 경계를 인식해야만 하도록 할 것이다.

### 컨택스트 맵 그리기

컨텍스트 맵은 우선 현재를 매핑해야하지, 상상의 미래를 그려선 안된다. 추후 업데이트하면 된다. 컨텍스트 맵의 한 부분을 확대해 상세한 내용을 추가하고 싶을 때가 있는데, 이는 단순히 동일한 컨텍스트를 바라보는 또 다른 관점일 뿐이다. 다이어그램에 너무 많ㅇ느 세부 내용을 추가하는 일은 팀에겐 전혀 실질적 도움을 주지 못한다.

컨텍스트 맵은 엔터프라이즈 아키텍처나 시스템 위상 다이어그램이 아니다. 컨텍스트의 쉬운 예는 그냥 화이트보드에 다이어그램을 그리는 것이다.

### 프로젝트와 조직 관계

#### 바운디드 컨텍스트 간 통합 패턴

1. 파트너십: 두 가지 컨텍스트 안의 팀이 성공이나 실패를 함께한다면 협업 관계가 나타나야 한다. 이 팀들은 개발과 통합의 공동 관리를 위한 잘 조정된 기획 과정을 도입한다. 팀들은 양측 시스템 모두의 개발 요구를 수용할 수 있는 인터페이스를 만들어 나가기 위해 반드시 협업해야한다.

2. 공유 커널: 모델에서 공유된 부분과 이에 관련된 코드는 아주 가까운 상호의존성으ㅜㄹ 형성하는데, 이는 설계 작업을 도울수도 있고 약화시킬 수도 있다. 도메인 모델에서 팀이 공유하기로 동의한 부분집합 일부를 명시적 경계로 지정하자. 커널은 작게 유지하자. 이 명시적 공유는 특별한 상태에 놓이며, 다른 팀과의 상의 없이 변경해선 안 된다

3. 고객-공급자 개발: 다운스트림 팀의 요구를 수용해야 한다.

4. 순응주의자: 업스트림팀이 다운스트림 팀의 요구사항을 제공해주지 않으면 다운스트림은 속수무책이 된다. 다운스트림 팀은 맹목적으로 업스트림 팀의 모델을 준수해서, 바운디드 컨텍스트 사이에 나타나는 변환의 복잡성을 제거한다.

5. 부패 방지 계층: 변환 계층은 협조적인 팀 사이에서 잘 설계된 바운디드 컨텍스트를 연결할 때 간결하고 우아해질 수 있다. 그러나 공유 커널, 파트너, 고객-공급자 관계를 만들기에 제어나 의사소통이 적절하지 않다면 변환은 복잡해진다. 변환 계층은 좀 더 방어적 색깔을 띄게 된다. 다운스트림 클라이언트에는 분리 계층을 만들어서 업스트림 시스템의 기능을 다운스트림 자신이 소유한 도멩니 모델의 맥락에 맞춰 제공하라.

   부패 방지 계층의 각 타입에 따른 다운스트림 컨텍스트에(?) 도메인 서비스를 정의할 수 있다. 부패 방지 게층을 리파지토리 인터페이스 뒤에 위치시킬 수 있다.

6. 오픈 호스트 서비스: 서브시스템에 접근할 수 있도록 해주는 서비스 집합으로서의 프로토콜을 정의하자. 그래서 여러분과 통합해야 하는 모든 이가 사용할 수 있도록 해주자. 특정 요구사항이 있지 않는한 이 프로토콜을 강화/확장해서 새로운 통합 요구 사항에 대응하자. 예외적 상황은 일회성 변환기를 사용해 프로토콜을 추가함으로써, 공유된 프로토콜이 단순함과 일관성을 유지하도록 하자.

   이 패턴은 바운디드 컨텍스트 클라이언트와 상호작용하는 rest 기반 리소스로 구현할 수 있다. 일반적으로 오픈 호스트 서비스를 rpc api로 생각하지만, 메시지 교환을 사용해 구현할수도 있다.

7. 발행된 언어: 두 바운디드 컨텍스트 모델 사이의 변환은 공통 언어를 필요로 한다. 발행된 언어는 오픈 호스트 서비스와 종종 결합한다. 이룰 구현하는 방법으로 대개 xml 스키마를 활용한다. 

8. 분리된 방법: 만약 두 기능성 집합 사이에 유의미한 관계가 없다면 이들은 서로가 완전히 분리돼야 한다. 바운디드 컨텍스트가 다른 것과 아무 연관이 없음을 선포해서 개발자가 작은 범위 내에서 단순하고 전문화된 솔루션을 찾도록 해주자.

9. 큰 진흙공: 여러 모델이 서로 뒤섞이고 경계는 일정하지 않은 상황이 있다. 이런 어지러운 상황 전체를 아우르는 경계를 큰 진흙공이라고 부른다. 이 컨텍스트 안에 세련된 모델링을 적용해보려 애쓰지 말고, 시스템이 다른 컨텍스트 안으로 제못대로 퍼져나가려는 경향에 주의하자.

팀의 통합에선 오픈 호스트 서비스와 발행된 언어를 사용한다. 아마 부패 방지 계층도 사용할 것이다.

다운스트림 모델이 업스트림 모델과 완전히 독립적으로 작동할 수 있지는 않더라도, 직접적인 실시간 종속성을 크게 제한하는 방향으로 설계해야 한다.

의존하고 있는 상태가 이미 오리의 로컬 시스템에 있다면 훨씬 큰 자율성을 달성할 수 있다. 의존 객체 모두를 캐싱하는 방법을 떠올릴 수도 있지만, ddd를 사용할 땐 일반적으로 이를 선택하지 않는다. 대신 외부 모델에서 변환된 로컬 도메인 깨체를 만든 후 필요한 최소한의 상태만 로컬 모델에 유지한다. 이는 우리가 데이터를 동기화해야 하는 필요성을 줄여줄 뿐 아니라 개념을 올바르게 모델링했는지에 대한 문제이기도 하다.
