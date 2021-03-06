---
layout: post
title: "JAVA: There Are Taxes on Inheritance"
author: "without inheritance M"
---



숙제를 하던 중 무분별한 클래스 상속을 사용하여 시정할 것을 권고받았습니다. 같은 실수를 방지하기 위해 이를 명확히 이해하고자 정리했습니다. 

{% highlight markdown %}
1. 상속을 사용하게 된 이유
2. 코드 재사용을 위해 상속을 사용하면 발생하는 문제점
3. 상속의 의미
4. 수정된 해결 방안 도출
{% endhighlight %}


<br>
<br>
<br>


## 1
**상속을 사용하게 된 이유**는 <ins>중복 코드가 발생</ins>했기 때문입니다. 해당 코드를 재사용하기 위하여 중복되는 내용들을 임의의 A 클래스에 묶고 이 코드가 필요한 모든 클래스가 A를 상속하여 필요한 메소드를 사용하도록 했습니다. 

<br>
<br>
<br>

## 2
하지만 **코드 재사용을 위해 상속을 사용하면 발생하는 문제점**이 있었습니다. 처음에는 두어 번의 중복 코드 발생으로 이를 상위 클래스로 옮겼습니다. 그런데 상위 클래스를 상속하는 하위 클래스가 많아지면서 상위 클래스의 메소드가 제공하는 기능과 <ins>매우 유사하지만 완전히 일치하지는 않는 코드가 필요한 경우</ins>가 생긴 것입니다. 이런 경우 해결 방법은 두 가지입니다.

  첫 번째는 상위 클래스의 메소드를 수정하여 두 가지 변형을 모두 충족하는 메소드로 만드는 것입니다. 조건문을 이용해 분기하는 방식일 확률이 높습니다.

 두 번째는 하위 클래스에서 해당 메소드를 재정의하는 것입니다.

 첫 번째 방법을 선택하면 비슷한 경우가 또 발생할 경우 상위 메소드를 계속해서 수정해야 합니다. 상위 클래스 메소드가 수정되면 이를 상속받는 모든 하위 클래스에서도 그 메소드 변화에 영향을 받아 한 차례 수정 작업을 거쳐야 합니다. 수정 작업이 매우 번거롭습니다.

 두 번째 방법은 중복되는 코드를 반복하지 않으려던 상속의 목적이 무효해집니다. 메소드 재정의를 할 때 유사한 코드를 다시 작성해야 하기 때문입니다.

 이로써 코드 재사용을 위해서 클래스 간의 상속 구조를 만드는 것은 적절하지 않다는 것을 알게 되었습니다. 그렇다면 상속은 어떤 경우에 사용하는 것일까요?

<br>
<br>
<br>

## 3 
**상속의 의미**가 무엇인지에 대하여 먼저 정리해 보겠습니다.

  
 다음은 오라클에서 제공하는 자바 API Document에서 _대충_ 번역한 내용입니다.
 >자바 언어에서는 클래스들이 다른 클래스들로부터 파생될 수 있다. 그에 의해 필드와 메소드들을 이어받는다. 다른 클래스로부터 파생된 클래스를 subclass라고 하고, subclass가 파생된 클래스를 superclass라고 한다. superclass가 없는 Object를 제외한 모든 클래스는 단 하나의 직접적인 superclass를 갖는다. 명시된 다른 superclass가 없는 클래스들은 모두 Object의 subclass이다. 클래스들은 다른 클래스에서 파생된 클래스로부터 다시 파생될 수 있다. 이런 식으로 무한히 파생되는 것이 가능한데, 결국 최상위 클래스는 Object이다.

정의는 위와 같이 단순합니다. 하지만 단순히 필드와 메소드를 가져온다는 의미에서 생각을 멈춰서는 안 됩니다. 이는 클래스 간에 의존 관계를 만드는 것이기 때문입니다. 

 다음은 Effective Java 2nd Edition/item 16:Favor composition over inheritance 일부 번역입니다. 필요하다고 생각되는 문단만 뽑아서 전문은 아닙니다.

> 상속은 코드 재사용을 위한 강력한 방법이지만 언제나 가장 좋은 선택인 것은 아니다. 부적절하게 사용하면 깨지기 쉬운 소프트웨어를 만들게 된다. 수퍼클래스와 서브클래스를 같은 패키지 안에 두고 같은 프로그래머가 이를 관리하도록 하는 것이 안전하다. 또한 이 상속 구조의 설계는 문서화되는 편이 좋다. 일반적인 완성된 클래스를 패키지 구분을 넘어서 상속하는 것은 위험하다. 메소드 호출과는 다르게 상속은 캡슐화를 위반한다. 다른 말로, subclass는 구현의 내용을 superclass에 의존한다. superclass의 구현은 계속 변경될 수 있다. 그렇다면 subclass 내용 또한 코드를 건드리지 않았음에도 깨지게 된다. 결과적으로 subclass는 superclas와의 의존 관계에 의해 개정되어야 한다.


 저자는 위와 같은 이유로 inheritance보다 composition 관계를 맺을 것을 권합니다. composition이란 간단히 말해, 코드를 사용하고자 하는 클래스를 필드로 두어 그 클래스로부터 필요한 내용을 빌리는 것을 말합니다. 예를 들어 Factory 클래스에서 Worker 클래스의 work와 takeRest 메소드가 다시 사용되어야 한다면, Factory가 Worker를 상속하는 것이 아니라 Worker를 필드로 두어 composition 관계를 맺어 메소드를 호출하는 것입니다.

 이러한 예로부터 상속 관계를 설계할 때 고려해야 할 점이 명확해졌습니다. 코드 재사용의 문제에서 클래스 간의 관계를 정의할 때, 클래스의 본질을 먼저 생각해야 한다는 것입니다. Factory 클래스가 책임을 수행할 때 Worker 클래스의 모든 필드와 메소드가 필요하다고 해도 코드 내용을 떠나, Factory와 Worker는 상속 관계가 될 수 없습니다. 클래스의 본질이 상이하기 때문입니다. 이와 다르게 Engineer와 Designer가 Worker를 상속한다는 것은 보다 일리가 있겠습니다.

<br>
<br>
<br>

## 4
공부한 것을 토대로 **수정된 해결 방안**은 동일한 코드의 재사용은 delegation-composition으로, 유사하지만 동일하지 않은 코드는 interface 구현으로 해결한다는 것입니다.

 처음에 발생한 '중복 코드'의 문제부터 다시 생각해 보겠습니다. 이를 처리하기 전에 발생한 '중복 코드'가 어떤 것인지 먼저 파악해 봅시다.

 기본적으로 하나의 클래스는 한 가지의 책임만 가집니다. 만약 두 개 이상의 클래스가 동일한 작업을 하고 있다면, 한 사람이 담당해야 할 작업에 두 사람이 동시에 배치되어 있거나 세 명의 노동력이 필요한 작업을 두 명이 과로하여 감당하고 있을 것입니다. 전자의 경우에는 한 명을 해고하는 것이, 후자의 경우에는 한 명을 더 고용하는 것이 합리적입니다.

 기존의 두 클래스가 명확히 다른 책임을 갖는다고 가정하겠습니다. 그렇다면 중복 코드의 내용은 두 클래스 각각이 가진 책임과는 성격이 다를 것이 명백합니다. 그리고 중복된 코드들이 한 개 이상의 책임 단위로 분류될 것입니다. 분류된 기준에 따라 새로운 클래스를 만들어 중복 코드들을 담고 기존의 두 클래스가 새로운 클래스에게 필요할 때마다 작업을 요청할 수 있도록 하면 중복 코드를 걷어낼 수 있습니다. 이렇게 다른 클래스로 책임을 이전하는 것을 '위임'이라는 의미 그대로 delegation이라고 표현합니다.

 중복되는 코드들은 제 3의 클래스로 넘어갔습니다. 하지만 아직 '유사하지만 완전히 일치하지는 않는 작업'에 대한 처리가 남아 있습니다. 이를 생각하기 앞서 그 작업을 수행하는 메소드의 어떤 부분이 유사하며 어떤 부분이 일치하지 않는 것인지를 구분해야 합니다.

 메소드는 크게 선언부와 구현부로 나누어집니다. 선언부에는 반환 타입, 매개 변수가 선언되고 구현부에는 메소드의 수행할 작업이 서술됩니다. 일반화하려는 두 메소드의 선언부(반환 타입, 매개 변수)가 일치할 때, 다시 말해 메소드의 input과 output의 함의가 같을 때 메소드의 규격이 같다고 판단할 수 있습니다. 글 전반부에 모호하게 언급되었던 '유사하지만 일치하지는 않는 코드'라는 것은 선언부는 같으나 구현부에 차이가 있는 메소드를 가리킵니다. 이와 같이 공통적으로 갖춰야 할 메소드의 규격은 인터페이스에 정의하고 클래스들이 인터페이스를 각각의 특성에 맞게 구현할 수 있습니다.



 * **tip** '매개 변수'와 '인자'의 차이
  : 인자(argument)는 메소드 외부에서 메소드로 전달하는 값이며, 매개 변수(parameter)란 메소드 내에서 인자 값을 전달받아 가지고 있는 변수를 지칭한다. 예를 들어, 메소드 내에서 인자 값을 복사하여 가지고 있는 매개 변수를 수정하면 인자 값에 영향이 없지만 인자의 레퍼런스로 연결된 매개 변수를 수정하면 인자 값에도 영향을 끼칠 수 있다. 
>
