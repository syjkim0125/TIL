# Optional Properties와 Class Validator

## Optional Properties
- Property가 정의는 되어 있으나, 모든 Property가 '반드시' 필요하지는 않을 때, 사용한다.
- 예시
    + 1. 사용자가 결제하면 결제와 관련된 이메일이 온다.
    + 2. 결제 관련 이메일에는 사용자가 선택한 결제 수단이 보여진다.
    + 3. 결제 수단은 '카드'이거나 '예치금'이다.
    + 4. 예치금이 보여질 때는 예치금의 잔액도 표시가 되어야 한다.

    -> 3, 4번의 경우에 optional을 선택하게 된다.

### 예시 코드
```
import { IsInstance, IsEnum, IsNumber, IsOptional } from 'class-validator';
import { DomainEvent } from '@packages/ddd';
import { ClassValidatorWrapper } from '@packages/util/validator';

import { PaymentStatus } from '@apps/payment/payment/constant';

import { PaymentId } from '@apps/payment/payment/domain/payment/model/PaymentId';

export class PaymentConfirmedEvent extends DomainEvent {
  @IsInstance(PaymentId)
  public readonly paymentId: PaymentId;

  @IsEnum(PaymentStatus)
  public readonly prevStatus: PaymentStatus;

  @IsOptional() // 이게 빠져있어서 계속 테스트에서 Validation 오류를 뿜었다.
  @IsNumber()
  public readonly balance?: number;

  constructor(paymentId: PaymentId, prevStatus: PaymentStatus, balance?: number) {
    super();
    this.paymentId = paymentId;
    this.prevStatus = prevStatus;
    this.balance = balance;

    ClassValidatorWrapper.validate(this);
  }
}
```
- balance는 예치금의 잔액을 의미한다.

결제가 완료된 후, 결제 완료 도메인 이벤트로 balance가 넘어오게 되는데 card일 때는 이 balance가 필요가 없으므로 optional을 사용하였다.
다만, 여기서 주의할 점은 class validator인데 @IsNumber()만 사용하면, optional이더라도 validation check를 하기 때문에 오류를 낸다.
따라서, @IsOptional()을 반드시 붙여줘야 한다.

### 결론
optional이라고 해서 그 부분이 아예 없는게 아니라, undefined로 된다. 
-> 이것도 validation check를 하므로, Isnumber() 같은게 있으면 밸리데이션 오류를 낸다. 
undefined라고 아예 없는 취급 하지 말자.