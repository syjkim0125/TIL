# Integration Event와 Json

## Integration Event
- 마이크로 서비스간에 이벤트를 주고 받기 위해 사용한다.

## 이벤트와 관련된 패턴
- Observer 패턴과 Pub-Sub 패턴
    + Observer패턴은 Subject와 서로 인지하고 있다. 하지만, Pub-Sub은 서로를 전혀 몰라도 상관없다.
      - Observer패턴의 경우 Subject에 Observer를 등록하고 Subject가 직접 Observer에 알려주어야 한다.
      Pub-Sub패턴의 경우 Publisher가 Subscriber의 위치나 존재를 알 필요 없이 Message Queue와 같은 Broker 역할을 하는 중간지점에 메시지를 던져놓기만 하면 된다.
      반대로 Subscriber 역시 Publisher의 위치나 존재를 알 필요 없이 Broker에 할당된 작업만 모니터링 하다가 할당 받아서 작업하면 되기 때문에 Publisher와 Subscriber가 서로 알 필요 없다.

    + Observer패턴보다 Pub-Sub패턴이 더 결합도가 낮다.
      - Pub-Sub은 서로의 존재를 알 필요가 없기 때문

    + Observer패턴은 대부분 동기 방식이고, Pub-Sub패턴은 대부분 비동기 방식이다.
      - Broker로 Message Queue를 많이 사용하기 때문이다.

    + Observer패턴은 단일 도메인 하에서 구현되어야 하나 Pub-Sub패턴은 크로스 도메인 상황에서도 구현 가능하다.
      - 역시 중간 매개체인 Broker가 있기 때문에 어플리케이션의 도메인이 다르더라도 Message Queue(Broker)에 접근만 가능하다면 처리가 가능하기 때문.




### Integration Event with Pub-Sub Pattern
- 통합 이벤트를 AWS의 SNS에 게시하는 방식으로 사용하고 있는데, Json 객체로 주고 받는다.

## 착각한 부분
- 무심코 통합이벤트는 escaping되면서 다 string으로 넘어오는 줄 알았다. 근데 json객체로 그대로 넘어오기 때문에 타입은 그대로 지켜진다.

### Json
- JSON은 자바스크립트의 객체 표기법으로부터 파생된 부분 집합이다. 따라서 JSON 데이터는 다음과 같은 자바스크립트 객체 표기법에 따른 구조로 구성된다.

1. JSON 데이터는 이름과 값의 쌍으로 이루어집니다.

2. JSON 데이터는 쉼표(,)로 나열됩니다.

3. 객체(object)는 중괄호({})로 둘러쌓아 표현합니다.

4. 배열(array)은 대괄호([])로 둘러쌓아 표현합니다.

### Escape
- \를 사용해서 어떤 부호나 문자열을 본연의 뜻이 아니게 쓸 수 있게 한 걸 말한다.

ex) \" -> 자바스크립트에서의 "가 아니라 문자열 자체의 "로 쓰인다.

## 결론
통합 이벤트를 AWS의 SNS에 Publish할 때 이스케이핑되는데, 그렇다고 해서 타입이 바뀌는게 아니다. (나는 string으로 다 바뀌는 줄 알았다...)
number로 넘어오면 그대로 number이다.

## Refer
- Observer & Pub-Sub
https://jistol.github.io/software%20engineering/2018/04/11/observer-pubsub-pattern/
- Json
http://tcpschool.com/json/json_basic_structure
- escape
https://falsy.me/%EC%9D%B4%EC%8A%A4%EC%BC%80%EC%9D%B4%ED%94%84-%EB%AC%B8%EC%9E%90%EC%9D%98-json-parse-%EC%98%A4%EB%A5%98%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B4%85%EB%8B%88%EB%8B%A4/
