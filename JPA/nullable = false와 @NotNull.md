// 수정 필요
### nullable = false vs @NotNull

nullable=false: null을 넣은 엔티티를 생성한 된 뒤 Repository에 전달되고,

이 값이 DB에 넘어간 뒤에 예외가 발생해 위험한 오류를 맞을 수 있다.

하지만 @NotNull을 사용하면 **SQL을 날리기 전에 null인게 있으면 ConstraintViolationException을 발생**시킨다!