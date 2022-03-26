
![nat](https://user-images.githubusercontent.com/67897827/160227613-c6a09d2a-8315-41a0-9995-8c6b9e469b8d.PNG)


## 실습단계
* (1) Private subnet 생성 및 Public subnet과의 차이점 이해
* (2) Bastion host를 통한 Private EC2 접속
* (3) NAT Instance를 통한 Private EC2의 외부 통신 구성
* (4) NAT Gateway를 통한 Private EC2의 외부 통신 구성

## 필기
* private subnet은 외부와 직접적으로 통신하지 못한다.
* private ec2에 접근하기위해서 public ec2가 중계 서버역활을 하는데 이것을 Bastion host 라고 한다. 배스쳔 호스트는 내부 네트워크와 외부 네트워크를 연결하는 게이트웨어 역활을
하는 일종의 중계 느낌이다.
* 
