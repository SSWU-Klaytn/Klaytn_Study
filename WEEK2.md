# Klaytn-study-2

## 섹션3. 클레이튼 덧셈게임 개발 with Klaytn Tools

### 1. 덧셈 게임 개요

- 사용 언어
```
1. 클레이튼 caver.js = 이더리움 web3.js
2. 이더리움의 비잔티움 버전 fork
3. 스마트계약에 많이 쓰이는 솔리디티 언어 사용
4. 트러플 프레임워크
```

- 사용 툴
```
IDE:스마트계약을 테스트할 수 있는 곳
wallet:계정관리
scope:트랜잭션 정보를 찾아볼 수 있는 곳 
```

- 과정
1. 운영자계정으로 로그인
2. keystore파일과 비밀번호를 통해 계정 인증
3. 컨트랙에 KLAY보내기를 통해 이벤트에 사용 될 총 금액을 정함(컨트랙 주소로 보내는 이유는 이벤트에 사용할 금액 유저들에게
투명하게 공개하기 위함)
4. 트랜잭션이 완료되면서 유저들에게 이벤트에 쓸 금액을 보여줌
5. 3초안에 유저들이 맞추면 0.1KLAY를 유저계정에게 보냄
6. 확인버튼을 통해 송금받을 수 있음
7. 트랜잭션완료되면 컨트랙의 잔액이 깍임


- 장점
	- 트랜잭션 처리되는 속도 빠름 
	- 방금 생성한 트랜잭션 정보를 투명하게 열람 가능(Klaytn scope으로 이동)


### 2. 스마트계약1- 컨트랙으로 KLAY 송금

1. 컨트랙의 이름을 additiongame으로 설정
2. 배포하는 순간 운영자 계정의 주소를 저장할 수 있는 상태변수를 만들고 생성자를 만든다.
```
  constructor() public{
	owner=msg.sender;
		}
```
    - 생성자의 역할 : 초기화
    - msg.sender : 배포하는데에 쓰이고 있는 계정, 현재 이 컨트랙을 호출하고 있는 사람. > owner 상태변수를 사용하여 블록체인에 영원히 저장
3. environment(환경설정)을 BAOBAB으로 진행
4. 배포
	- 컴파일 후, deploy하면 additiongame 이 배포됨.
	- txhash: 배포하면서 생긴 트랜잭션 해쉬 정보
	- contractaddress: 트랜잭션이 어느 주소에 배포가 되었는지 볼 수 있음

### 3. 스마트계약2- 컨트랙의 잔액 불러오기

1. 생성자 안에 함수 코드 작성
```
function getBalance() public  view             return(uint)
 함수        이름      가시성   타입(읽기 전용)  uint타입을 리턴
 
 return address(this).balance;
             ㄴadditiongame 자신의 트랜잭션
```
 
2. owner 계정에서 컨트랙 주소로 klay 송금
```
function deposit() public payable{}
계정에서 solidity함수로 klay를 보낼 때 항상 payable 타입을 붙여야함
그래야 함수에서 돈을 받을 수 있음 
```

3. 유효성 검사
조건문을 사용하여 함수를 불러 온 계정이 ower계정인지 확인 

* payable 타입을 사용하여 deposit()하려면 Tx Value 값을 1로 수정



### 4. 스마트계약3- 컨트랙에서 사용자 계정으로 KLAY 보내기
덧셈게임에서 정답을 맞췄을 때, 정답을 맞춘 사용자가 직접 이 함수를 호출해서 돈을 받아가는 구조

1. function transfer(uint _value) public return (bool) {
 인자로 사용자에게 넘길 klay값을 받음(프론트앤드에서 peb로 변환돼서 넘어오는 것)

2. 유효성 검사
컨트랙에 있는 금액이 사용자가에 보낼 값만큼 있는지 확인
require(getbalance() >= _value)
잔액 불러오는 코드          인자로 받은 value 값과 비교

3. msg.sender.transfer(_value);
사용자가 이 함수 를 사용하여 value만큼을 전송하는 코드

4. return true; 성공여부 알려줌


*컨트랙에서 보내는 KLAY값은 peb로 변환하여야 함
변환사이트 사용





