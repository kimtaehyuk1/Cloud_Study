## Security group과 Network ACL

* 우선 어떤 트래픽이 VPC 내부로 들어올때, 서브넷 레벨에서 Network ACL의 인바운드 룰 통과할 수 있는지 검증받음
* 그 후에 인스턴스 레벨에서 security group에 인바운드 룰 통과 검증 받음 이때 security group은 트래픽 상태/정보 저장 할 수 있는 특성 가지고 있다.
* 그렇기 때문에 인바운드 룰 통과되면 아웃바운드 관계 없이 통과시킴
* security group에 나온놈은 다시 Network ACL에 다다라서, 이때 Network ACL는 stateless이기 때문에 다시 검증한다.



이번 실습 아키텍처 다이어그램

![Network ACL](https://user-images.githubusercontent.com/67897827/159921787-0cb16af9-b2c5-4ca9-8244-4d342cf0752c.PNG)

설명 : 동일 VPC내 서로다른 서브넷에 위치한 ec2 인스턴스들이 security group 과 Network ACL의 설정 규칙에 따라서 어떻게 통신이 이뤄지는지 확인.

* Network ACL은 rule number가 낮을 수록 우선순위를 갖는다.
