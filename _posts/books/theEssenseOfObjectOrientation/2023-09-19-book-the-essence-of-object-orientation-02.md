---
title: "[객체지향의 사실과 오해] 02 이상한 나라의 객체"

excerpt: "객체지향의 사실과 오해"
categories: [Book, 객체지향의 사실과 오해]
tags: [Book, 객체지향의 사실과 오해]
date: 2023-09-19
last_modified_at: 2023-09-19
render_with_liquid: false
---
# 02 이상한 나라의 객체

1장에선 커피를 주문하는 일련의 과정으로 객체지향에 대해 소개했다면, 2장에서는 엘리자베스 스펠크와 필립 켈만의 실험, 이상한 나라의 앨리스 그리고 버트란드 마이어의 기계 은유로 객체를 보다 자세히 소개한다.

## 객체

**__엘리자베스 스펠크와 필립 켈만의 실험 - 아기의 인지 능력_**

아기가 두 개의 막대기를 함께 움직일 경우 하나의 막대기로 인식하는 실험을 통해 인간은 본능적으로 세상을 독립적이고 식별 가능한 객체의 집합으로 바라본다는 점을 알 수 있다. 이때 인지 능력은 물리적 한계 뿐만 아니라 개념적으로 경계 지을 수 있는 추상적인 사물까지 포함하며, 인간은 좀 더 단순한 객체들로 주변을 분해함으로써 자신이 몸담고 있는 복잡한 세상을 이해하고 극복하려고 노력한다.

객체지향 패러다임은 소프트웨어의 세계 역시 인간이 인지할 수 있는 다양한 소프트웨어 객체들이 모여 이뤄져 있다는 믿음에서 출발한다.

**__이상한 나라의 앨리스_**

이상한 나라의 앨리스에서, 앨리스라는 객체가 음료, 버섯 케이크 등을 먹는 **행동**을 취했을 때, 앨리스의 키라는 **상태**가 변화(커지거나 작아짐)하는 것을 통해 다음의 사실을 알 수 있다.

- 객체는 상태는 변경가능하다.
  ```앨리스의 키가 변한다.```
- 상태를 결정하는 것은 행동이지만 행동의 결과를 결정하는 것은 상태이다.
  (행동의 결과는 상태에 의존적이다.)
  ```앨리스의 현재 키가 130cm이고 케이크를 먹을 경우 150cm가 더 커진다고 할 때, 앨리스의 키는 130(현재 키) + 150 = 280(케이크를 먹은 후의 키)cm 가 된다.  ```
- 어떤 행동의 성공 여부는 이전에 어떤 행동들이 발생했는지에 영향을 받는다.
  즉, 행동 간의 순서가 중요하며 결과에 영향을 미친다.
  ```앨리스가 문을 통과하기 위해 음료나 케이크를 먹은 것처럼, 문을 통과하는 행동을 하려면 음료나 케이크를 먹는 행동이 선행되어야 한다.```
- 행동에 의해 상태가 변경되더라도 객체는 유일한 존재로 식별 가능하다.
  ```앨리스의 키가 변하는 것과 무관하게 앨리스가 앨리스라는 사실은 변하지 않는다.```

책에서는 객체를 다음과 같이 정의한다.

> **객체**란 식별 가능한 개체 또는 사물이다. 객체는 자동차처럼 만질 수 있는 구체적인 사물일 수도 있고, 시간처럼 추상적인 개념일 수도 있다. 객체는 구별가능한 식별자, 특징적인 행동, 변경 가능한 상태를 가진다. 소프트웨어 안에서 객체는 저장된 상태와 실행 가능한 코드를 통해 구현된다.

### 상태

> **상태**는 특정 시점에 객체가 가지고 있는 정보의 집합으로 객체의 구조적 특징을 표현한다. 객체의 상태는 객체에 존재하는 정적인 프로퍼티와 동적인 프로퍼티 값으로 구성된다. 객체의 프로퍼티는 단순한 값(속성, attribute)과 다른 객체를 참조하는 링크로 구분할 수 있다.

객체가 주변 환경과의 상호작용에 어떻게 반응하는가는 그 시점까지 객체에 어떤 일이 발생했느냐에 좌우된다. 그러나 일반적으로 과거에 발생한 행동의 이력을 통해 현재 발생한 행동의 결과를 판단하는 방식은 복잡하고 번거로우며 이해하기 어려우므로 이를 단순히 기술하기 위해 상태를 고안했다.
상태를 이용하면 과거의 모든 행동 이력을 설명하지 않고도 행동의 결과를 쉽게 예측하고 설명할 수 있다. 상태를 이용하면 과거에 얽매이지 않고 현재를 기반으로 객체의 행동방식을 이해할 수 있다. 상태는 근본적으로 세상의 복잡성을 완화하고 인지 과부하를 줄일 수 있는 중요한 개념이다.

객체의 상태는 단순한 값과 객체의 조합으로 표현할 수 있는데,
객체의 상태를 구성하는 모든 특징을 통틀어 객체의 **프로퍼티(property)** 라고 하며, 이는 변경되지 않고 고정되기 때문에 **정적**이다.
반면 **프로퍼티 값(property value)** 는 시간이 흐름에 따라 변경되기 때문에 **동적**이다.

```
앨리스의 키가 130cm라고 했을 때, 키는 프로퍼티, 130cm라는 수치는 프로퍼티 값이 된다.
```

객체와 객체 사이의 의미 있는 연결을 **링크(link)** 라고 한다. 객체와 객체 사이에는 링크를 통해서만 메시지를 주고받을 수 있다. 링크는 객체가 다른 객체를 참조할 수 있다는 것을 의미하며, 이것은 일반적으로 한 객체가 다른 객체의 식별자를 알고 있는 것으로 표현된다.

```
앨리스와 앨리스가 마신 음료 모두 '객체'이며, 둘은 연결되어 있다.
```

### 행동

> **행동**이란 외부의 요청 또는 수신된 메시지에 응답하기 위해 동작하고 반응하는 활동이다. 행동의 결과로 객체는 자신의 상태를 변경하거나 다른 객체에게 메시지를 전달할 수 있다. 객체는 행동을 통해 다른 객체와의 협력에 참여하므로 행동은 외부에 가시적이어야 한다.

객체의 상태는 저절로 변경되지 않으며 오직 객체의 자발적인 행동을 통해 변경된다. 객체는 자신에게 주어진 책임을 완수하기 위해 다른 객체를 이용하고 다른 객체에게 서비스를 제공하는데, 이때 객체가 어떤 행동을 하도록 만드는 것은 객체가 외부로부터 수신한 메시지이다. 객체가 취하는 행동은 여러 부수 효과를 초래하는데, 자기 자신의 상태뿐만 아니라 다른 객체의 상태 변경을 유발(행동 내에서 협력하는 다른 객체에 대한 메세지를 전송)할 수도 있다.

**상태 캡슐화**
객체는 상태를 캡슐 안에 감춰둔 채 외부로 노출하지 않는다. 객체가 외부에 노출하는 것은 행동뿐이며, 외부에서 객체에 접근할 수 있는 유일한 방법 역시 행동 뿐이다.
객체에게 메시지를 전달하는 외부의 객체는 메시지를 수신하는 객체의 상태가 변경된다는 사실조차 알지 못한다.

상태를 외부에 노출시키지 않고 행동을 경계로 캡슐화하는 것은 객체의 자율성을 높이고 협력을 유연하고 간결하게 만든다.

### 식별자

객체가 식별 가능하다는 것은 객체를 서로 구별할 수 있는 특정한 프로퍼티가 객체 안에 존재한다는 것을 의미한다. 이 프로퍼티를 식별자라고 하며 모든 객체는 식별자를 가지고 이를 이용해 객체를 구별할 수 있다.

> **동등성(equality)** 상태를 이용해 두 값이 같은지 판단할 수 있는 성질
> **동일성(identical)** 식별자를 기반으로 객체가 같은지를 판단할 수 있는 성질

숫자, 날짜, 문자열 등의 **값(value)** 은 상태가 변하지 않기 때문에 **불변 상태**를 가진다고 말하며 두 인스턴스의 **상태가 같다면 두 인스턴스를 같은 것으로 판단**한다. 즉, 값이 같은지 여부는 상태가 같은지를 이용해 판단한다. 값의 상태는 결코 변하지 않기 때문에 어떤 시점에 동일한 타입의 두 값이 같다면 두 값은 항상 동등한 상태를 유지하는 것이다.

**객체**는 시간에 따라 변경되는 상태를 포함하며, 행동을 통해 상태를 변경하므로 **가변 상태**를 가진다고 말하며, 타입이 같은 두** 객체의 상태가 완전히 똑같더라도 두 객체는 독립적인 별개의 객체**로 다뤄야 한다. 한편, 두** 객체의 상태가 다르더라도 식별자가 같다면 두 객체를 같은 객체로 판단**할 수 있다. 객체의 상태는 가변적이므로 값과 같이 상태를 기반으로 객체의 동일성을 판단할 수 없으며 상태 변경과는 독립적인 별도의 식별자를 이용해 동일성을 판단해야 하는 것이다.

**__버트란드 마이어 - 기계 은유_**

객체의 행동 대부분은 쿼리와 명령으로 구성된다.

- **쿼리(query)** 객체의 상태를 조회하는 작업
- **명령(command)** 객체의 상태를 변경하는 작업

마이어는 다음의 기계를 객체에 비유한다.

```
내부를 볼 수 없으며 블랙박스이다
외부에는 사각형과 원형의 버튼이 있다.
사각형 버튼은 객체의 상태를 변경할 수 있는 버튼이며, 
원형 버튼은 객체의 상태를 조회할 수 있는 버튼이다. 
```

객체 기계와 소프트웨어 객체의 대응 관계는 다음과 같다.

- 객체 기계의 버튼을 눌러 상태 변경/조회를 요청 - 객체의 행동을 유발하기 위해 메시지를 요청
- 버턴을 누르는 것은 사용자이지만 버튼에 따라 동작하는 방식을 결정하는 것은 기계 - 전달된 메시지에 따라 스스로 판단하고 결정하는 자율적인 객체의 특성
- 사각형 버튼(객체 상태 변경) - 명령
- 원형 버튼(객체 상태 조회) - 쿼리
- 사용자는 객체가 제공하는 명령 버튼과 쿼리 버튼으로 구성된 인터페이스를 통해서만 객체에 접근 가능 - 객체의 캡슐화 강조

이렇듯 객체를 기계로서 바라보는 관점은 상태, 행동, 식별자에 대한 시각적인 이미지를 제공하고 캡슐화와 메시지를 통한 협력 관계를 매우 효과적으로 설명한다.

**상태를 결정하고 행동을 나중에 결정하는 방법이 설계에 미치는 영향**

1. 캡슐화 저해 - 상태가 객체 내부로 깔끔하게 캡슐화되지 못하고 공용 인터페이스에 노출될 확률이 높아짐
2. 협력에 적합하지 못한 객체 창조
3. 객체의 재사용성이 저하

**결론**: 행동에 초점을 맞춰야 함. 객체지향 설계는 애플리케이션에 필요한 협력을 생각하고 협력에 참여하는 데 필요한 행동을 생각한 후 행동을 수행할 객체를 선택하는 방식으로 수행되어야 한다. **행동이 상태를 결정한다**

## 의인화와 은유

2장의 마지막으로, 1장을 시작하며 다루었던 "객체지향 세계는 실세계의 모방"과 연관 짓는 설명에 대해 보다 구체적으로 반박한다. 모방과 추상화라는 개념만으로는 현실 객체와 소프트웨어 객체 사이의 관계를 깔끔하게 설명할 수 없으며 소프트웨어는 실세계와는 특성이 전혀 다른 어떤 것임을 강조한다.

### 의인화

> 현실의 객체보다 더 많은 일을 할 수 있는 소프트웨어 객체의 특징 __레베카 워프스브록_

현실 속 객체와 소프트웨어 객체의 가장 큰 차이점이며, 현실 속의 수동적인 존재가 소프트웨어 객체로 구현될 때는 능동적으로 변한다는 것. 즉, 현실 객체가 가지지 못한 추가적인 능력을 보유하게 되는 것을 의미한다.

```
현실 속의 전화기는 스스로 통화 버튼을 누를 수 없으며 계좌는 스스로 금액을 이체할 수 없다.
```

소프트웨어 속 객체지향 세계는 현실의 모습을 참조할 뿐, 궁극적으로는 실세계와 다른 새로운 체계를 창조하는 것이며 오히려 객체지향 세계의 객체는 현실 속의 객체보다 더 많은 특징과 능력을 보유한다.

현실 세계의 객체를 자세히 관찰하고 그 중 소프트웨어 객체에 적합한 속성만 추려내라는 전통적인 조언은 오히려 객체지향 애플리케이션이 현실의 구조를 정확하게 반영해야 한다는 오해만 심어줄 뿐이다.

### 은유

> 실제로는 적용되지 않는 한 가지 개념을 이용해 다른 개념을 서술하는 대화의 한 형태로, 현실 세계와 객체지향 세계 사이의 관계를 좀 더 명확하게 설명할 수 있는 단어.

현실 속의 객체의 의미 일부가 소프트웨어 객체로 전달되기 때문에 프로그램 내의 객체는 현실 속의 객체에 대한 은유라고 볼 수 있다.

은유 관계에 있는 실제 객체의 이름을 소프트웨어 객체의 이름으로 사용하면 표현적 차이를 줄여 소프트웨어의 구조를 쉽게 예측할 수 있고, 이해와 유지보수가 쉬운 소프트웨어를 만들 수 있다.

**결론적으로** 우리는 객체지향 설계자로서 현실을 모방할 것이 아니라 자유로이 창조해야 하며, 창조한 객체의 특성을 상기시킬 수 없다면 현실 속의 객체의 이름을 이용해 객체를 모사하면 된다.

---

## ++

### 책임-주도 설계(Responsibility-Driven Design, RDD)

책임을 찾고 책임을 수행할 적절한 객체를 찾아 책임을 할당하는 방식으로 협력을 설계하는 방법 [Link](https://thisblogfor.me/computerscience/OOP/5%EC%9E%A5/)
