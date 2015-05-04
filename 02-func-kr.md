---
layout: page
title: 파이썬 프로그래밍
subtitle: 함수 만들기
minutes: 20
---
> ## 학습 목표
>
> *   함수를 정의한다.
> *   매개변수를 갖는 함수 정의한다.
> *   콜스택(Call Stack)이 무엇이며 왜 프로그램이 콜스택을 사용하는지 설명한다.
> *   콜스택을 사용해서 중첩된 함수 호출 실행을 추적한다.

다각형을 한번만 그리고자 한다면, 가장 단순한 방법은 지난 학습에서 했듯이 코드를 3줄 타이핑하는 것이다.

~~~ {.input}
>>> for side in range(6):
...     turtle.forward(50)
...     turtle.left(60)
... 
~~~

하지만, 다양하게 많은 다각형을 그리고자 한다면, 다음과 같이 타이핑하고 싶을 것이다.

~~~ {.input}
>>> polygon(6, 50, 60)
~~~

그리고 파이썬이 나머지를 알아서 해결하도록 하고 싶다.
이 작업을 단계별로 만들어 가자.

첫째로, 고정된 크기를 갖는 정사각형을 그리는 **함수**를 새로 생성하자.

~~~ {.input}
>>> def square():
...     for side in range(4):
...         turtle.forward(60)
...         turtle.left(90)
...
~~~

상기 명령어를 실행하면 파이썬이 어떤 것도 출력하지 않고,
거북이는 움지이지 않는다. 왜냐하면, 실제로 파이썬에 어떤 것도 수행하도록 명령하지 않았기 때문이다.
--- 대신에 *만약 명령하면* 새로운 작업을 어떻게 수행하는지에 대한 것만 전달했다.
새로운 명령을 수행하는데, 거북이 메쏘드를 호출하는 것과 동일하게 함수를 **호출(call)**한다.

~~~ {.input}
>>> square()
~~~

![square1](fig/square1.png)

방금전에 발생한 것이 다음에 정리되어 있다.

1.  `def`를 사용해서 새로운 함수를 정의할 때, 첫번째 행 아래 들여쓰기된 명령어-- 함수 **몸통(body)** --를 파이썬이 받아서 메모리에 저장한다.
2.  함수를 호출할 때는, 파이썬은 함수가 정의된 것을 찾아서 해당 함수 명령어를 실행한다.

![square2](fig/square2.png)

지금까지 줄곧 그려왔던 동일한 정사각형을 그리는 함수는 그다지 유용하지 않다.
하지만, 화면을 깨끗이 하고, 다음을 실행하자.

~~~ {.input}
>>> for petal in range(8):
...     square()
...     turtle.left(45)
...
~~~

출력결과는 각각의 위쪽에 놓여진 8개 정사각형이 된다.

![eight squares](fig/eight_squares.png)

다른 방식으로도 동일한 것을 그릴 수 있다 --- 사실 [후반부 수업]()에서 다룰 예정이다 ---
하지만, 이미 앞에서 함수에 명령어를 넣는 것이 다른 코드를 더 읽기 좋게 한다는 것을 봤다.
또한 작성한 것을 재사용(re-use)할 수 있게 한다: 반복되는 같은 행을 재입력(re-enter)하지 않고,
원할 때마다 정사각형을 이제는 그릴 수 있다.

물론, 정사각형 크기를 변경할 수만 있다면 더욱 유용할 것이다. 크기변경하는 함수를 다시 정의하자.

~~~ {.input}
def square(length):
...     for side in range(4):
...         turtle.forward(length)
...         turtle.left(90)
...
~~~

함수 정의를 입력하게 되면, 이전 작성한 함수 자리를 차지한다.

![square 20, square 30](fig/square20_30.png)

함수내부에, `square` 함수는 `length`라는 **매개변수(parameter)**를 갖는다.
변수 `side`가 루프에서 현재 위치를 추적한 것처럼, `length`는 호출될 때 `square`에 전달된 값을 저장한다.
예를 들어, 만약 다음과 같이 코드를 작성한다면:

~~~ {.input}
>>> square(20)
~~~

`length`는 값이 20을 갖게 된다.
만약 다음과 같이 작성한다면:

~~~ {.input}
>>> square(30)
~~~

`length`는 값이 30이 된다.
거북이를 이동할 때, 앞으로 고정된 크기만큼 전진하라고는 하지 않았다.
대신에, `turtle.forward(length)`와 같이 작성해서 `length` 픽셀만큼 거북이를 이동시킨다.
함수호출 `square(30)`은 한편에 30 픽셀 크기 정사각형을 그리게 하고, `square(90)`은
3배 더큰 정사각형을 그리게한다.
다음과 같이 이미지를 그릴 수 있게 한다.

~~~ {.input}
>>> for size in [20, 40, 60]:
...     square(size)
...     left(15)
...
~~~

![square left 15](fig/square_left_15.png)

그런데, 뭔가 현실적으로 이제 루프에 값을 사용하고 있다는 것에 주목하라.
앞선 루프에 있는 숫자 리스트는 중요하지 않다 --- 이들 숫자 존재이유는 루프를 고정된 횟수만큼 반복하게 만드는 것이다.
여기서 숫자 20, 40, 60 이 `square`에 전달되어 정사각형을 원하는 크기만큼 키우는데 있다. 

만약 한개 매개변수가 잘 동작한다면, 두개 혹은 세개 매개변수는 어떨까?

~~~ {.input}
>>> def polygon(sides, length, angle):
...     for s in range(sides):
...         turtle.forward(length)
...         turtle.left(angle)
...
~~~

다시 한번, 실제로 함수를 호출하기 전까지 아무것도 일어나지 않는다:

~~~ {.input}
>>> polygon(6, 30, 60)
~~~

하지만, 상기와 같이 호출하면 거북이가 다각형을 그린다.

![hexagon](fig/hexagon.png)

그리고, 만약 다음과 같이 호출하면, 

~~~ {.input}
>>> polygon(8, 30, 135)
~~~

8각형을 그리게 된다.

![octagon](fig/octagon.png)

> ## 변수 이름(variable name) {.callout}
>
> 유의미한 변수명과 함수에 `s`의 사용에 대해서 설명하라.

함수를 작성하게 되면, 다른 함수에서 작성한 함수를 사용하지 않을 이유가 없다.
앞에서 다각형을 그린 것처럼 "flower"를 생성하는 함수가 다음에 있다.

~~~ {.input}
>>> def flower(petal_sides, petal_angle, poly_sides, poly_length, poly_angle):
...     for s in range(petal_sides):
...         polygon(poly_sides, poly_length, poly_angle)
...         turtle.left(petal_angle)
...
~~~

`flower` 함수는 매개변수 5개를 받는다. 
첫번째 두 매개변수는 전체 꽃그리기를 제어하고, 나머지 세 매개변수는 각 꽃잎 형상을 제어한다.

![eight-squares-flower](fig/eight-squares-flower.png)

앞에서 한 것처럼 8각형 꽃을 상기 함수를 사용해서 그릴 수 있다:

~~~ {.input}
>>> flower(8, 45, 4, 50, 90)
~~~

상기 코드가 의미하는 바는 "정사각형 8개를 그리는데, 크기는 각각 50 픽셀이고, 정사각형 사이는 45도 회전한다"

![triangle-flower](fig/triangle-flower.png)

상기 함수를 사용해서 다음과 같은 형상도 그릴 수 있다.

~~~ {.input
>>> flower(3, 120, 3, 50, 120)
~~~

두함수를 나란히 놓고 잠시동안 루프를 살펴보자:

~~~ {.python}
def polygon(sides, length, angle):
    for s in range(sides):
        turtle.forward(length)
        turtle.left(angle)

def flower(petal_sides, petal_angle, poly_sides, poly_length, poly_angle):
    for s in range(petal_sides):
        polygon(poly_sides, poly_length, poly_angle)
        turtle.left(petal_angle)
~~~

두 함수 모두 `s`로 불리는 변수를 루프 변수로 사용하지만, 이름만 같은뿐 다른 변수다 ---
우연히 동일한 이름이 되었다. 이것이 동작하는 것을 이해하기 위해서, 좀더 단순한 ㅎ마수를 정의하자.

~~~ {.python}
>>> def double(value):
...     return value * 2
...
~~~

`return` 명령어는 함수가 호출자에게 무슨 값을 반환할지 알려준다.

~~~ {.input}
>>> print double(3)
~~~
~~~ {.output}
4
~~~

`double`을 사용하는 또다른 함수가 다음에 있다:

~~~ {.input}
>>> def one_plus_one(value):
...     first = value + 1
...     second = double(first)
...     return second + 1
...
~~~

그리고, `one_plus_one(10)`을 호출할 때, 무슨 일이 발생하는지 다음에 나와있다.

1.  **콜스택**에 파이썬이 값 10을 놓는데 함수 호출에 전달되는 값이 또다른 함수 위에 쌓인다는 
    사실로부터 이름을 얻게 된다.

![call-stack-01](fig/call-stack-01.png)

2.  그리고 나서 새로운 변수 `first`를 생성하는데 **스택프레임(stack frame)**은 
    `one_plus_one` 함수 내부에서 발생하는 것을 추적하고 `first`에 값 11 (10 + 1)을 대입한다.

![call-stack-02](fig/call-stack-02.png)

3.  이제 `one_plus_one`가 `double`을 호출하고 나서, 
    파이썬이 또다른 스택 프레임을 생성하고 값 11을 전달한다.

![call-stack-03](fig/call-stack-03.png)

    이제 `value`라는 이름을 갖는 변수가 두개 있음을 주목한다.
    하지만, 서로 다른 스택프레임에 있기 때문에, 실제로는 다른 변수다 --- 우연히 같은 이름을 갖게된 것이다.

4.  `double`은 22 (11 * 2)를 반환한다. 그래서 파이썬이 해당 함수호출을 추적하는데 사용한 스택프레임을 버리고 
    다시 그 밑에 함수를 사용하기 시작한다. 그 스택프레임에는 `second`라고 새로 생성된 변수에 22 값을 놓는다:

![call-stack-04](fig/call-stack-04.png)

5.  마침내 함수 `one_plus_one`가 23(22+1)을 반환한다. 그래서 파이썬이 함수 호출 스택프레임을 버리고 나서
    최종 결과를 출력한다.

[pythontutor.com](http://goo.gl/nPfnnt)에서 콜스택에 대한 자세한 정보 확인합니다.

아마도 진행과정을 일일이 따라가는 것은 엄청난 수고지만, 절대적으로 필요하다.
프로그램에 모든 변수가 한 장소에 저장되어 있다면 어떤 모습일지 상상해보자.
새로운 함수를 작성할 때마다, 우연히 잘못된 변수를 참조하지 않도록 다른 누군가 사용한 변수명을 사용하지 않도록 주의해야만 한다.
주의를 하려면, 호출하는 모든 함수에 있는 단하나의 줄도 빠짐없이 읽어야만 한다.
설사 이와 같은 작업을 수행할 수 있을 정도로 인내심을 가질 수 있지만,
함수를 작성한 누군가 다시 코드를 수정하고 변수 이름을 변경하는 순간 바로 작성한 프로그램은 곧 망가진다. 

여기 핵심적인 아이디어가 **캡슐화(encapsulation)**다.
그리고, 올바른 이해할 수 있는 프로그램을 작성하는 것이 매우 중요하다.
함수의 역할은 몇가지 연산작업을 하나로 바꿔주는 것이다.
매번 무언가를 수행할 때마다 수십개 혹은 수백개 문장 대신에 단하나 함수 호출로 생각을 단순화하는데 있다.
이것이 가능한 것은 함수가 서로 서로 간섭하지 않는 것이다; 만약 서로 간섭하게 된다면,
다시한번 세세한 부분에 주의를 기울여야 하고, 급격하게 머리속 단기 기억장소에 부하를 주게 된다.

