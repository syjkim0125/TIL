# Optional Properties와 Class Validator

## Optional Properties
- Property가 정의는 되어 있으나, 모든 Property가 '반드시' 필요하지는 않을 때, 사용한다.
- 예시
    + 1. 사용자는 결제할 때, 결제 수단을 선택할 수 있다.
    + 2. 결제 수단은 '카드'일 수도 '예치금'일 수도 있다.
    + 3. 결제 수단은 반드시 하나이다.

### 예시 코드
```
public readonly paymentMethod: {
    cardName?: string;
    depositId?: string;
}
```
- 이 부분은 paymentMethod가 Card냐 Deposit이냐에 따라서 이메일에 두 가지가 다르게 표시되어야 했는데, 통합이벤트에서 타입에 따라 들어올 수도 아닐 수도 있기 때문에 optional property를 썼었다.
  -> 근데 이렇게 하면 다음과 같은 오류가 있다.

인터페이스가 이런 형태로 구성되었을 때,아래의 객체 모두 유효하게 됩니다.
```
    { cardName: undefined, depositId: undefined }
    { cardName: 'CARD', depositId: 'deposit_id }
```
그러나 이런 객체는 유효하지 않아야 하고, paymentMethod를 참조하는 코드에서 저런 입력이 없을 것이라고 기대할겁니당 따라서, 인터페이스 상속을 통해서 타입을 더 명확하게 할 수 있도록 구성하는 것이 좋을 듯 합니다.

예를 들면,
```
interface PaymentMethod {
      type: Constant.PaymentMethodType
    }
    
    interface Card extends PaymentMethod {
      cardName: string;
    }
    
    interface Deposit extends PaymentMethod {
      depositId: string;
    }
    
    public readonly paymentMethod: Card | Deposit;
```

→ 이와 같이 인터페이스 상속을 함으로써, 타입을 명확하게 지정한 다음에 다음과 같은 메소드를 추가한다.

```
    export function isCard(paymentMethod: PaymentMethod): paymentMethod is Card {
      return paymentMethod.type === Constant.PaymentMethodType.CARD;
    }
    
    export function isDeposit(paymentMethod: PaymentMethod): paymentMethod is Deposit {
      return paymentMethod.type === Constant.PaymentMethodType.DEPOSIT;
    }
```

→ 이렇게 하면 다음과 같이 property에 접근할 수 있게 된다.
```
    if (SendPaymentCompletedEmailCommand.isCard(command.paymentMethod)) {
          const { cardName } = command.paymentMethod;
          cardNameContent = cardName;
        } else if (SendPaymentCompletedEmailCommand.isDeposit(command.paymentMethod)) {
          const { depositId } = command.paymentMethod;
          depositBalance = await this.notificationDomainService.getBalance(depositId);
        } else {
          throw new UnprocessableEntityException();
        }
```

### 결론
optional을 쓸 때 주의할 것은, 타입을 명확하게 나타낼 수 없을지도 모른다는 점과 둘 중에 한 Property만 필요할 경우에는 적절하지 않을 수 있다.
반드시 필요하지 않은 Property들에 대해서 고려하도록 하자.
