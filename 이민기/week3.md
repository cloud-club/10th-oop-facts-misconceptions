## 추상화

- 현실에서 출발하되 불필요한 부분을 도려내가면서 사물의 놀라운 본질을 드러나게 하는 과정
- 어떤 양상, 세부 사항, 구조를 좀 더 명확하게 이해하기 위해 특정 절차나 물체를 의도적으로 생략하거나 감춤으로써 복잡도를 극복하는 방법
- 모든 경우에 추상화의 목적은 복잡성을 이해하기 쉬운 수준으로 단순화하는 것이라는 점을 기억하라

핵심 → 객체를 쉽게 이해하기 위한 목적

생물 vs 무생물 / 동물 vs 식물 / 인간 vs 동물 / 남자 vs 여자

### 객체 지향과 추상화는 어떤 관련이 있을까?

<img width="600" alt="image" src="https://github.com/user-attachments/assets/31c6e8a8-60ec-48a5-ad3f-535128ece153" />

시스템은 여러 개의 객체들이 존재하고 상호작용을 하는데 이러한 것들을 모두 이해하기는 어렵기 때문에 최대한 단순화(추상화)시켜 시스템의 본질을 쉽게 이해하기 위한 것 같다.

### 타입 - 분류, 추상화, 일반화 & 특수화

<img width="600" alt="image" src="https://github.com/user-attachments/assets/3dced028-79e3-4e6f-8dca-f1eac0ec3066" />

책에서 ‘적절한 분류 체계는 애플리케이션을 다루는 개발자의 머릿속에 객체를 쉽게 찾고 조작할 수 있는 정신적인 지도를 제공한다는 것이다’ 라고 말하고 있다.

꼭 애플리케이션이 아니더라도 일상에서도 분류를 통해 지도를 그리고 있다 (위 갈매기 예시)

사람은 객체를 볼 때 큰 분류부터 인식하여 점점 구체적으로 들어간다.

우리는 갈매기를 찾을 때 새라는 큰 범위에서 찾아가듯이 객체도 큰 범위부터 좁혀 들어가면 해당 객체의 기능을 쉽게 이해할 수 있다.

---

## 행동이 우선이다

**다형성**

<img width="776" height="38" alt="image" src="https://github.com/user-attachments/assets/3971a5e2-4a1a-492f-b4ff-099f25fe737b" />

- 넓이를 구할 수 있는 도형 → 원, 직사각형

```java
interface Shape {
    double area();  // 넓이를 계산해서 반환한다
}
```

- 원은 넓이를 계산하기 위해 **반지름**을 저장

```java
class Circle implements Shape {
    private double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}
```

- 직사각형은 **가로와 세로**를 저장

```java
class Rectangle implements Shape {
    private double width;
    private double height;

    Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double area() {
        return width * height;
    }
}
```

- 내부 데이터와 계산 방법은 달라도, 둘 다 ‘자신의 넓이를 계산한다’는 책임을 수행

```java
Shape first = new Circle(3); // 원의 반지름
Shape second = new Rectangle(4, 5); // 직사각형의 가로 세로

System.out.println(first.area());
System.out.println(second.area());
```

| 구분 | 원 객체 | 직사각형 객체 |
| --- | --- | --- |
| 내부 데이터 | 반지름 | 가로, 세로 |
| 받는 요청 | `area()` | `area()` |
| 수행하는 책임 | 자신의 넓이 계산 | 자신의 넓이 계산 |
| 계산 방법 | π × 반지름² | 가로 × 세로 |
| 공통 타입 | `Shape` | `Shape` |

청소기와 빗자루는 먼지를 없앤다는 동일한 책임을 가지고 있다. 먼지를 빨아드리거나 빗질을 통해 먼지를 쓸어 담는(`객체의 내부 표현`) 방식은 다르지만 청소라는 행동은 동일하다. 이것들은 청소 용품이란 동일한 타입으로 볼 수 있다.

---

## Responsibility Driven Design

**책임을 찾고 책임을 수행할 적절한 객체를 찾아 책임을 할당하는 방식으로 협력을 설계하는 방법**

- 핵심 개념
    - 역할
    - 책임
    - 협력

#### 설계 방식

1. **할 일 찾기**
    
    사용 시나리오를 통해 시스템이 해야 할 일을 파악한다.
    
    ```java
    주문 금액 계산
    재고 확보
    결제 승인
    주문 확정
    ```
    
2. **필요한 책임 나누기**
    
    해야 할 일을 ‘알아야 하는 것’과 ‘수행해야 하는 것’으로 구체화한다.
    
3. **책임을 맡을 객체 정하기**
    
    각 책임을 수행하기에 적절한 객체를 찾거나 만든다.
    
    | 객체 | 책임 |
    | --- | --- |
    | `Order` | 금액 계산과 주문 상태 관리 |
    | `Inventory` | 재고 확보 |
    | `PaymentProcessor` | 결제 처리 |
    | `Checkout` | 전체 주문 흐름 조정 |
4. **협력과 메시지 정하기**
    
    객체들이 누구에게 어떤 요청을 보내야 하는지 정한다.
    
    ```java
    class Checkout {
        void placeOrder(
            Order order,
            Inventory inventory,
            PaymentProcessor payment
        ) {
            // Order에게 금액 계산을 요청
            int amount = order.calculateTotal();
    
            // Inventory에게 재고 확보를 요청
            inventory.reserve(order);
    
            // PaymentProcessor에게 결제를 요청
            payment.approve(amount);
    
            // Order에게 주문 확정을 요청
            order.confirm();
        }
    }
    ```
    
5. **데이터와 구현 구체화하기**
    
    책임 수행에 필요한 필드, 자료구조, 알고리즘을 정한다. 예를 들어 상품 한 종류를 주문하는 `Order`는 다음처럼 구현할 수 있다.
    
    ```java
    class Order {
        private int unitPrice;
        private int quantity;
        private boolean confirmed;
    
        Order(int unitPrice, int quantity) {
            this.unitPrice = unitPrice;
            this.quantity = quantity;
        }
    
        int calculateTotal() {
            return unitPrice * quantity;
        }
    
        void confirm() {
            confirmed = true;
        }
    }
    ```
    
6. **시나리오로 검토하고 수정하기**
    - 객체 간 협력으로 요구사항을 충족하는지 확인하고, 책임과 객체를 조정한다.
    - 1만 원짜리 상품을 2개 주문하면 **2만 원 계산 → 재고 확보 → 결제 승인 → 주문 확정**으로 이어지는지 확인한다. 재고 부족이나 결제 거절 상황도 검토하며 책임과 협력을 보완한다.
