# ER Diagram

## ✒︎ Entity Relationship Diagram
실재(존재) 관계 도표, 데이터들의 관계를 나타내는 도표


## ✒︎ 용어 및 기호 정리

### ❆ 부모 테이블, 자식 테이블
A테이블의 PK를 B테이블이 가지고 있다면 A가 부모, B가 자식 


### ❆ 실선(식별 관계)과 점선(비식별 관계)
- 실선: 자식 테이블이 부모 테이블의 pk를 기본키로 사용
- 점선: 자식 테이블이 부모 테이블의 pk를 기본키로 사용하지 않음


### ❆ 연관 관계

![erd관계](/img/Database/erd관계.png)
출처:
https://inpa.tistory.com/entry/ERD-CLOUD-%E2%98%81%EF%B8%8F-ERD-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8%EC%9D%84-%EC%98%A8%EB%9D%BC%EC%9D%B8%EC%97%90%EC%84%9C-%EA%B7%B8%EB%A0%A4%EB%B3%B4%EC%9E%90

- 줄이 하나면 '일', 여러개면 '다'를 의미하며 테이블의 관계를 나타냄
  
일대일 / 일대다 / ~~다대다~~ 과 같이 나태날 수 있으며, 다대다 관계는 지양한다.
  
A는 B를 {}
- 포함할 수 있다
- ~할 수 있다, 될 수 있다
- 담을 수 있다
- 매칭할 수 있다
이렇게 말 만들어 보면 파악하기 수월하다.
  

예를들어보면 주문 엔티티와 상품 엔티티가 있다고 할때... 
1. 하나의 주문에 여러 상품을 담을 수 있고
2. 하나의 상품은 여러 주문에 포함될 수 있다.
즉, 다대다 관계인데, 이것은 테이블을 하나 사이에 끼워주는 식으로 일대다 관계로 풀어줘야 한다.

'주문 - 주문상품 - 상품' 이런식으로 테이블을 추가해주면..
1. 하나의 주문에 여러개의 주문 상품이 담길 수 있으며, 하나의 주문상품은 하나의 주문에 속한다. (일대다)
2. 하나의 주문상품은 하나의 상품에 매칭되며, 하나의 상품은 여러 주문상품이 될 수 있다. (일대다)


