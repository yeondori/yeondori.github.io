---
title: "[객체지향의 사실과 오해] 06 객체 지도"

excerpt: "객체지향의 사실과 오해"
categories: [Book, 객체지향의 사실과 오해]
tags: [Book, 객체지향의 사실과 오해]
date: 2023-09-26
last_modified_at: 2023-09-26
render_with_liquid: false
---

# **06 객체 지도**

6장은 지도로 기능과 구조를 개략적으로 설명하고 정기예금 도메인 모델을 예시로 기능과 구조의 통합을 설명한다.

> 길을 모를 때 어떻게 가야 할까?

1. **다른 사람에게 길을 물어본다** 면 이는 기능적이고 해결책 지향적인 접근법이다. 길을 알려주는 사람은 경로를 상세히 설명해야 하며, 이 방법은 일반적이지도 재사용 가능하지도 않다.
2. **지도를 본다** 면 이는 구조적이고 문제 지향적인 접근법이다. 지도는 실세계의 지형을 기반으로 만들어진 추상화 모델로 길을 찾는 데 필요한 풍부한 정보가 함축되어 있고,다양한 목적을 위한 재사용이 가능하다(범용적이다).

지도가 범용적인 이유는 사람들이 원하는 '**기능**'에 비해 지도에 표시된 '**구조**'가 안정적이기 때문이다. 지도를 사용하는 사람들의 요구사항은 항상 바뀌지만 지도는 기능에 비해 상대적으로 잘 변하지 않는 안정적인 지형 정보를 기반으로 하고 있기 때문에 이 모든 요구사항을 수용할 수 있다.
이는 기능이 아니라 **구조**를 기반으로 모델을 구축하는 편이 보다 범용적이고 이해하기 쉬우며 변경에 안정적이라는 점을 시사한다. **자주 변경되는 기능이 아닌, 안정적인 구조를 따라 역할, 책임 그리고 협력을 구성하라**는 것이 이번 장의 주제이다.

## **기능 설계 VS 구조 설계**

모든 소프트웨어 제품의 설계는 기능 측면의 설계와 구조 측면의 설계, 두 가지 측면으로 존재한다. **기능 측면의 설계**는 제품이 사용자를 위해 무엇을 할 수 있는지에 초점을 맞추는 반면 **구조 측면의 설계**는 제품의 형태가 어떠해야 하는지에 초점을 맞춘다. 설계는 이 두 가지 측면이 조화를 이루도록 해야 한다.
소프트웨어를 개발하는 일차적인 이유는 사용자에게 훌륭한 기능을 제공하기 위해서이므로 개발 초기 단계에서는 사용자가 원하는 것이 무엇이고 그것을 만족시키기 위해 어떤 기능을 제공해야 하는지에 초점을 맞춰야 한다. 훌륭한 기능이 훌륭한 소프트웨어를 만드는 충분조건이라면, 훌륭한 구조는 필요조건이라고 할 수 있다.
성공적인 소프트웨어는 훌륭한 기능을 제공하는 동시에 사용자가 원하는 새로운 기능을 빠르고 안정적으로 추가할 수 있어야 한다. 사람들의 요구사항은 예측 불가능하므로 이를 대비할 수 있는 가장 좋은 방법은 변경을 선택할 수 있는 선택의 여지를 설계에 마련하는 것이다.
즉, 깔끔하고 단순하며 유지보수가 쉬운 설계는 사용자의 변하는 요구사항을 반영할 수 있도록 쉽게 확장 가능한 소프트웨어를 창조할 수 있는 기반이 된다.

> 설계를 하는 목적은 나중에 설계하는 것을 허용하는 것이며, 설계의 일차적인 목표는 변경에 소요되는 비용을 낮추는 것이다 _Metz 2012

전통적인 기능 분해(functional decomposition)는 자주 변경되는 기능을 중심으로 설계한 후 구조가 기능에 따르게 하기 때문에, 기능 변경에 취약하다.
반면 객체지향 접근방법은 자주 변경되지 않는 안정적인 객체 구조를 바탕으로 시스템의 기능을 객체 간의 책임으로 분배해, 기능이 변경되더라도 객체 간의 구조는 그대로 유지되고 변경을 수용할 수 있는 유연한 소프트웨어를 만들 수 있는 기반을 제공한다.

### **기능과 구조**

객체지향 설계를 위해서는 기능과 구조가 모두 필요하다. **기능**은 사용자의 목표를 만족시키기 위해 책임을 수행하는 시스템의 행위로 표현되며, 기능을 수집하고 표현하기 위한 기법을 **유스케이스 모델링** 이라 한다.
**구조**는 시스템의 기능을 구현하기 위한 기반으로, 사용자나 이해관계자들이 도메인에 관해 생각하는 개념과 개념간의 관계로 표현한다. 구조를 수집하고 표현하기 위한 기법은 **도메인 모델링**이라 한다.

### **구조**

앞서 언급된 도메인 모델링의 결과를 **도메인 모델**이라고 한다.
소프트웨어를 사용하는 사람들은 자신이 관심을 가진 특정한 분야의 문제를 해결하기 위해 소프트웨어를 사용하는데, 이처럼 사용자가 프로그램을 사용하는 대상 분야를 **도메인**이라고 한다.
**모델**이란 지식을 선택적으로 단순화하고 의식적으로 구조화한 형태로, 간단히 말해 대상을 추상화하고 단순화한 것이다. 이 두가지 개념을 연결해 설명하면, **도메인 모델**이란 사용자가 프로그램을 사용하는 대상 영역에 관한 지식을 선택적으로 단순화하고 의식적으로 구조화한 형태다.
도메인 모델은 소프트웨어가 목적하는 영역 내의 개념과 개념 간의 관계, 다양한 규칙이나 제약 등을 깊게 추상화한 것이다.

도메인 모델은 단순히 다이어그램이 아니며 이해관계자들이 바라보는 **멘탈 모델(Mental Model)** 이다. 멘탈 모델은 사람들이 자기 자신, 다른 사람, 환경, 자신이 상호작용하는 사물들에 대해 갖는 모델으로, 도널드 노먼은 제품을 설계할 때 제품에 관한 모든 것이 사용자가 제품에 갖고 있는 멘탈 모델과 정확히 일치해야 한다고 주장한다.
멘탈 모델은 사용자가 가지고 있는 **사용자 모델**과 디자이너가 가지고 있는 **디자인 모델**, 그리고 최종 제품인 **시스템 이미지**의 세 가지로 구분되며 사용자 모델과 디자인 모델이 동일하다면 이상적이지만 사용자와 설계자는 직접적인 상호작용이 불가능하므로 최종 제품인 시스템 자체를 통해서만 의사소통할 수 있다.
따라서 설계자는 디자인 모델을 기반으로 한 시스템 이미지가 사용자 모델을 정확하게 반영하도록 노력하여야 하는데, 도메인 모델은 이 세 가지를 포괄하도록 추상화한 소프트웨어 모델이다.

다시 말하면 도메인 모델이란 사용자들이 도메인을 바라보는 관점이며, 설계자가 시스템의 구조를 바라보는 관점인 동시에 소프트웨어 안에 구현된 코드의 모습 그 자체이다. 객체지향을 사용하면 사용자들이 이해하고 있는 도메인의 구조와 최대한 유사하게 코드를 구조화할 수 있는데, 이러한 특성을 **연결완전성, 표현의 차이**라고 한다.
주의해야 할 점은 소프트웨어 객체는 현실 객체의 모방이 아닌 은유 기반의 재창조이며 소프트웨어 객체는 현실 객체가 갖지 못한 특성을 가질 수도 있고 현실 객체가 하지 못하는 행동을 할 수도 있다는 점이다. 이러한 의미적 거리를 **표현적 차이, 의미적 차이**라고 하며, 핵심은 은유를 통해 현실 객체와 소프트웨어 객체 사이의 차이를 최대한 줄이는 것이다.
대부분의 소프트웨어 도메인은 현실에 존재하지 않는 가상의 세계를 대상으로 하므로, 우리는 사용자가 도메인에 대해 생각하는 개념들, 즉, 도메인 모델을 은유해야 한다.

도메인 기반으로 코드를 작성해야 하는 첫번째 이유는 사용자가 도메인을 바라보는 관점을 그대로 코드에 반영할 수 있게 한다는 것이며, 두번째 이유는 도메인 모델이 제공하는 구조가 상대적으로 안정적이라는 것이다.

소프트웨어는 항상 **변경**을 의식해야 한다. 사용자 모델에 포함된 개념과 규칙은 비교적 변경될 확률이 적기 때문에 사용자 모델을 기반으로 설계와 코드를 만들어야 변경에 쉽게 대처할 수 있을 가능성이 커진다.
안정적인 구조를 기반으로 자주 변경되는 기능을 배치함으로써 기능의 변경에 대해 안정적인 소프트웨어를 구현할 수 있게 된다.

### **기능**

기능적 요구사항이란 시스템이 사용자에게 제공해야 하는 기능의 목록을 정리한 것이다. 시스템이 사용자에게 기능을 제공하는 이유는 사용자들이 시스템을 통해 달성하고자 하는 목표가 존재하기 때문이고, 훌륭한 기능적 요구사항을 얻기 위해서는 목표를 가진 **사용자**와 사용자의 목표를 만족시키기 위해 일련의 절차를 수행하는 **시스템** 간의 **상호작용** 관점에서 시스템을 바라보아야 한다.
유스케이스 모델링의 결과를 **유스케이스**라고 하며, 유스케이스는 사용자의 목표를 달성하기 위해 사용자와 시스템 간에 이뤄지는 상호작용의 흐름을 텍스트로 정리한 것이다. 유스케이스는 사용자들의 목표를 중심으로 시스템의 기능적인 요구사항들을 이야기 형식으로 묶을 수 있는데, 산발적으로 흩어진 기능에 목표라는 문맥을 제공함으로써 각 기능이 유기적인 관계를 지닌 체계를 이룰 수 있게 한다는 점에서 가치가 있다.

다음은 앨리스터 코오번의 유스케이스 정기예금 이자 계산 유스케이스이다.

```
유스케이스명: 중도 해지 이자액을 계산한다.

일차 액터: 예금주

주요 성공 시나리오:
    1. 예금주가 정기예금 계좌를 선택한다.
    2. 시스템은 정기예금 계좌 정보를 보여준다.
    3. 예금주가 금일 기준으로 에금을 해지할 경우 지급받을 수 있는 이자 계산을 요청한다.
    4. 시스템은 중도 해지 시 지급받을 수 있는 이자를 계산한 후 결과를 사용자에게 제공한다.
    
확장: 
    3a. 사용자는 해지 일자 다른 일자로 입력할 수 있다.
```

예시를 통해 볼 수 있는 유스케이스의 특성은 다음과 같다.

1. 유스케이스는 사용자와 시스템 간의 상호작용을 보여주는 **텍스트**다.

    : 유스케이스는 다이어그램이 아니며, 핵심은 사용자와 시스템 간의 상호작용을 일련의 이야기 흐름으로 표현하는 것이다.


2. 유스케이스는 하나의 시나리오가 아닌 **여러 시나리오들의 집합**이다. 

    : 이자 계산 유스케이스는 계좌 선택과 당일까지의 이자 계산, 계좌 선택과 특정일까지의 이자 계산으로 두 개의 시나리오를 포함하고 있다. 즉, 하나의 시나리오가 아니라 이자액 계산이라는 사용자의 목표와 관련된 모든 시나리오의 집합인 것이다. 여기서 시나리오를 **유스케이스 인스턴스**라고도 한다.


3. 유스케이스는 단순한 피처(feature) 목록과 다르다.

    : 피처는 시스템이 수행해야 하는 기능의 목록을 단순하게 나열한 것으로, 예시에서는 '시스템을 정기예금 정보를 보여준다', '시스템은 당일이나 현재의 이자를 계산한다' 가 피처가 된다. 
      서로 연관이 없어보이는 두 피처를 '중도 해지 이자액을 계산한다'는 유스케이스로 묶어 문맥을 얻을 수 있게 되는데, 이처럼 유스케이스는 단순히 기능을 나열하는 것이 아니라 이야기를 통해 연관된 기능들을 함께 묶을 수 있다는 강점이 있다.


4. 유스케이스는 사용자 인터페이스와 관련된 세부 정보를 포함하지 말아야 한다.

    : 유스케이스는 자주 변경되는 사용자 인터페이스 요소는 배제하고 사용자 관점에서 시스템의 행위에 초점을 맞추는데, 이러한 형식을 **본질적인 유스케이스(essential use case)** 라고 한다.


5. 유스케이스는 내부 설계와 관련된 정보를 포함하지 않는다.
    
    : 유스케이스는 시스템의 내부 구조나 실행 메커니즘에 관한 어떤 정보도 제공하지 않고, 단지 사용자가 시스템을 통해 무엇을 얻을 수 있고 어떻게 상호작용할 수 있느냐에 관한 정보만 기술한다. 유스케이스는 객체지향 이외의 패러다임에도 적용 가능하며, 객체지향 또한 유스케이스 이외의 방법으로 요구사항을 명시할 수 있다.
      유스케이스는 객체의 구조나 책임에 대한 어떤 정보도 제공하지 않는다. 


### **기능과 구조의 통합**

시스템은 사용자와 만나는 경계에서 사용자의 목표를 만족시키기 위해 사용자와의 협력에 참여하는 커다란 객체다. 
사용자에게 시스템이 수행하기로 약속한 기능은 결국 시스템의 책임으로 볼 수 있고, 시스템에 할당된 책임은 시스템 안의 작은 규모의 객체들이 수행해야 하는 더 작은 규모의 책임으로 세분화된다. 
이때 우리는 도메인 모델에 포함된 개념을 은유하는 소프트웨어 객체를 선택한다. 객체의 이름은 도메인 모델에 포함된 개념으로부터 차용하고, 책임은 도메인 모델에 정의한 개념의 정의에 부합하도록 할당한다.
`이자를 계산하는 책임을 가진 객체는 이자율이 되며, 이자는 이자율에 의해 생성된다. ` 협력을 완성하는 데 필요한 메시지를 식별하면서 객체들에게 책임을 할당해 나가고 협력에 참여하는 객체를 구현하기 위해 클래스를 추가하고 속성과 메서드를 구현하면 시스템의 기능이 완성된다.

유스케이스는 사용자에게 제공할 기능을 시스템의 책임으로 보게 함으로써 객체 간의 안정적 구조에 책임을 분배할 수 있게 하며 도메인 모델은 기능을 수용하기 위해 은유할 수 있는 안정적인 구조를 제공한다. 책임-주도 설계 방법은 시스템의 기능을 역할과 책임을 수행하는 객체들의 협력관계로 보게 함으로써 기능과 구조를 통합한다. 
유스케이스에서 출발해 객체들의 협력으로 이어지는 일련의 흐름은 객체 안에 다른 객체를 포함하는 재귀적 합성이라는 객체지향의 기본 개념을 잘 보여주며 객체에 대한 재귀는 객체지향의 개념을 모든 추상화 수준에서 적용 가능하게 하는 동시에, 객체지향 패러다임을 어떤 곳에서든 일관성 있게 적용할 수 있게 한다.

도메인 모델이 안정적인 이유는 도메인 모델을 구성하는 개념은 비즈니스가 없어지거나 완전히 개편되지 않는 한 안정적으로 유지되며, 개념관의 관계 또한 비즈니스 규칙을 기반으로 하기 때문에 비즈니스 정책이 크게 변경되지 않는 한 안정적으로 유지되기 때문이다. 
따라서 변경에 대한 파급효과를 최소화하고 요구사항 변경에 유연하게 대응할 수 있는 시스템을 구축할 수 있게 하며, 이는 객체지향이 긴응에 변경에 대해 좀 더 유연하게 대처할 수 있는 패러다임으로 일컬어지는 이유이다.

객체지향의 가장 큰 장점은 도메인을 모델링하기 위한 기법과 도메인을 프로그래밍하기 위해 사용하는 기법이 동일해, 도메인 모델링에서 사용한 객체와 개념을 프로그래밍 설계에서의 객체와 클래스로 매끄럽게 변환할 수 있다는 것이다. (**연결완전성**)

이때, 연결완전성의 역방향 역시 성립하는데 도메인 모델과 코드 모두 동일한 모델링 패러다임을 공유하기 때문에 코드의 변경으로부터 도메인 모델의 변경 사항을 유추할 수 있고 이를 **가역성(reversibility)**라고 한다.

정리하자면, 안정적인 도메인 모델을 기반으로 시스템의 기능을 구현하고 도메인 모델과 코드를 밀접하게 연관시키기 위해 노력하는 것이 유지보수가 쉽고 유연한 객체지향 시스템을 만드는 첫걸음이다!

