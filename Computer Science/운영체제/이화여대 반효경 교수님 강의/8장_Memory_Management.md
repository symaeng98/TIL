# [8장] Memory Management

## Logical vs Physical Memory

- Logical address (Virtual address)
    - 프로세스마다 **독립적으로** 가지는 주소 공간
    - 0번지부터 시작
    - CPU가 바라보는 주소
- Physical address
    - 메모리에 실제로 올라가는 위치

⇒ Logical address → Physical address로 **주소 변환**이 이루어져야 함

## 주소 바인딩 (Address Binding)

![](image/8장/address_binding.png)

### Compile time binding

- 물리적 메모리 주소가 컴파일 시 알려짐
    - 위의 그림에서 Logical address가 Physical address가 됨
    - Physical memory에 비어있는 주소들이 있을 때도 정해진 주소로 올리게 됨 (비효율적)

### Load time binding

- 물리적 메모리 주소가 실행 시에 결정됨

### Run time binding

- 경우에 따라서 다른 메모리 주소로 바뀔 수도 있음
- **MMU**가 binding을 도와줌

### CPU가 바라보는 메모리는 놀랍게도 logical address이다.

- 위의 경우에, 컴파일 했을 때 명령어가 “Add 20, 30”인데, 이때 20과 30 자체를 실제 메모리 주소로 바꾸려면 다시 컴파일을 해야한다.
- CPU가 20, 30에 있는 것을 달라고 요청을 하면, 그때마다 메모리 주소 변환을 통해 해당 값을 가져오는 것이다.

## MMU (Memory Management Unit)

![](image/8장/mmu.png)

- MMU는 relocation register와 limit register로 주소 변환을 한다.
- 시작 위치를 정해두고, 그에 해당하는 offset만큼 계산해서 가져옴
- limit register는 프로세스 크기보다 더 큰 주소의 값을 요청할 때를 대비한다.

## Dynamic Loading

- 프로그램이 필요할 때마다 메모리에 올리는 것
- 운영체제가 해주는게 아니라, 프로그래머가 하는 것
    - 운영체제가 해주는 것은 **페이징**

## Swapping

- 프로세스를 일시적으로 메모리에서 backing store(HDD)로 쫓겨나는 것
- 중기 스케줄러가 priority를 기준으로 swap out 시킴

## Dynamic Linking

- 라이브러리가 프로그램 실행 파일 코드에 포함이 되는 것이 아니라, 라이브러리가 실행될 때 해당 라이브러리를 찾아서 실행함

---

## Allocation of Physical Memory

연속 할당

- Fixed partition allocation
- Variable partition allocation

불연속 할당

- Paging
- Segmentation
- Paged Segmentation

## 연속 할당

하나의 프로세스가 메모리 공간에 연속적으로 할당함

![](image/8장/contiguous_allocation.png)

### 고정 분할 방식 (Fixed partition)

- 프로그램 A가 분할 1에 들어가고, 프로그램 B는 크기가 조금 커서 분할 2에는 들어가지 못하고 분할 3에 들어간다 하자
    - 이때 분할 1과 분할 3 사이의 **외부에 낭비되는 공간**을 **External Fragmentation**, 분할 3 **내부에 낭비되는 공간**을 **Internal Fragmentation**이라고 한다.

### 가변 분할 방식 (Variable partition)

- 프로그램 크기들이 균일하지 않아서, External Fragmentation은 생긴다.
- 프로세스의 크기를 hole에 넣을 때 어떻게 넣는지에 대한 방법이 여럿 있다.
    - First-fit: Size가 n 이상인 것 중 최초로 찾아지는 hole에 할당
    - Best-fit: Size가 n 이상인 것 중 가장 작은 hole에 할당
    - Worst-fit: 가장 큰 hole에 할당