`Duck`과 `Person` 예시 코드 (파이썬)

서로 상속 관계가 없는 전혀 다른 클래스인 `Duck`과 `Person`이 있다고 가정해 보겠습니다.
python

```python
class Duck:
    def quack(self):
        print("꽥꽥!")

    def swim(self):
        print("오리가 수영합니다.")


class Person:
    def quack(self):
        print("사람이 오리흉내를 내며 꽥꽥! 소리를 냅니다.")

    def swim(self):
        print("사람이 수영합니다.")


# 매개변수로 전달된 객체가 quack()과 swim() 메서드를 가지고 있다면 그대로 실행함
def make_it_swim_and_quack(duck):
    duck.quack()
    duck.swim()


# 1. Duck 객체 전달
duck = Duck()
make_it_swim_and_quack(duck)
# 출력:
# 꽥꽥!
# 오리가 수영합니다.

# 2. Person 객체 전달 (Duck을 상속받지 않음!)
person = Person()
make_it_swim_and_quack(person)
# 출력:
# 사람이 오리흉내를 내며 꽥꽥! 소리를 냅니다.
# 사람이 수영합니다.
```


핵심 설명

- **상속 관계 없음**: `Person` 클래스는 `Duck` 클래스를 상속받지도 않았고, 공통 부모 클래스도 없습니다.
- **동작 중심의 판단**: `make_it_swim_and_quack()` 함수는 `thing`이 오리(`Duck`)인지 아닌지(타입이 무엇인지)를 따지지 않습니다.
- **결과**: `thing` 객체가 `quack()`과 `swim()`이라는 메서드를 가지고 있기만 하면, 함수는 에러 없이 그대로 동작합니다. 

즉, `Person`이어도 오리처럼 행동(`quack`, `swim`)할 수 있다면 오리처럼 취급하는 것이 덕 타이핑입니다.