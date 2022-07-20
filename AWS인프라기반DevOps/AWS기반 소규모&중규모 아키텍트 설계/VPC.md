VPC의 특징
* 계정 생성 시 default로 VPC를 만들어 줌
* EC2, RDS, S3 등의 서비스 활용 가능
* 서브넷 구성
* 보안 설정(IP block, inbound outbound 설정)
* VPC Peering(VPC 간의 연결)
* IP 대역 지정 가능
* VPC는 하나의 Region에만 속할 수 있음(다른 Region으로 확장 불가능)

VPC의 구성요소
![image](https://user-images.githubusercontent.com/67897827/179925121-564abc40-4455-40f9-9e1f-fd2a8130c1f0.png)
1. Availability Zone
- 하나의 리젼안에 포함되어 있다. 리젼안에 AZ가 2개이상 있는데(일정거리 떨어져있음) 이는 물리적인 피해가 있을때 다른거 동작시키려고
- 각 계정의 AZ는 다른 계정의 AZ와 다른 아이디를 부여받음(내가 다른사람과 똑같이 AZ1이어도 사실은 다른거)

2. Subnet(CIDR)
- 하나의 AZ에서만 생성 가능, 하나의 AZ에는 여러개의 서브넷 생성 가능
- private 서브넷은 인터넷에 접근 불가능한 서브넷으로 VPC내부에서만 소통
- public 서브넷은 인터넷에 접근 가능한 서브넷

3. Internet Gateway(VPC내부와 인터넷과 소통이 필요한 매개체)
- private 서브넷은 IGW(인터넷게이트웨이)와 연결되어 있지 않다.

4. Route Table (VPC안에있는 객체들간에 혹은 VPC인터넷 외부간의 네트워킹 위해 필요 테이블)
- 트래픽이 어디로 가야 할지 알려주는 테이블(local인 내부로 가야되는지 외부로 인터넷게이트웨이로 가야되는지 나태내주는 이정표)
- VPC생성시 자동으로 만들어짐

5. Network Access Control List/security group (NACL임 이게 보안을 담당)(인바운드,아웃바운드 규칙은 여기서)
- NACL은 Stateless한 보안검문소, SG은 Stateful한 보안검문소이다.
![image](https://user-images.githubusercontent.com/67897827/180002968-ff150f47-7c26-477d-bbaa-f47d2a8b21d7.png)
그림에 대해서 설명하면 지금EC2는 SG이 설정되어 있고, 인바운드 즉 포트 80으로 들어오는건 허용하고 나가는건 none모두 허용하지 않겠다인데 SG는 
Stateful하기 때문에 방금 80으로 들오오게 시작한 1025는 기억해서 그놈은 내가 허락해주겠다는 것이다. 하지만 Stateless는 반대로 아웃바운드 none이므로 아무도 못나간다
즉 NACL이 더 엄격하다는 것이다.
- Access Block(인바운드올떄 막은 놈들)은 NACL에서만 가능하다.

6. NAT(Network Address Translation) instance/NAT gateway
![image](https://user-images.githubusercontent.com/67897827/179946401-1a4626d8-fd4b-46a5-88b3-b00582002a69.png)
- private서브넷이라 할지라도 여기에 SQL등등 깔려있으면 깔때도 그렇고 업데이트 할때도 그렇고,종종 인터넷과 연결이 될 필요가 있다. 
이럴때 public 서브넷으로 우회한다 public 서브넷안에있는 NAT gateway나 NAT instance(이건 걍 EC2라 생각해도 무방한데 private에서 우회해서 사용할때 EC2를 NAT instance라 한다)을
활용한다.
- 정확히는 private 서브넷에서 NACL거쳐 라우터테이블에서 트래픽을 퍼블릭 서브넷으로 쏴줌 그래서 퍼블릭 서브넷안의 NAT gateway로 접근하고 이놈이 인터넷 게이트웨이로 트래픽 쏴줌

7. Bastion host
인터넷상에있는 관리자가 private 서브넷상의 인스턴스들 가지고 작업을 해야되는데 access가 불가능한 상태이다. 이걸 해결하기 위해 아까 NAT과는 반대의 개념인데
이건 인테넷에서 private서브넷으로 접근하기 위한방법으로 쉽게 Public 서브넷에 위치하는 EC2라고 생각
![image](https://user-images.githubusercontent.com/67897827/179949269-639ef815-bd0b-415c-be9e-e3f3da4fcc77.png)

8. VPC endpoint
- 핵심 : 서비스에 비공개로 연결 하고 싶을때, 이거 사용하면 Public IP주소가 필요가 없다.
- 즉 AWS의 여러 서비스들과 VPC를 연결시켜주는 중간 매개체 
- 필요한이유 : 결국이놈도 Private subnet같은 경우 격리된공간인데 여기서 aws의 다양한 서비스들을 연결할 수 있도록 지원하는 서비스이다.
- Interface Endpoint : Private ip를 만들어 서비스로 연결해줌(SQS, SNS, Kinesis, Sagemaker 등 지원)
- Gateway Endpoint : 라우팅 테이블에서 경로의 대상으로 지정하여 사용(S3, Dynamodb 지원)
![image](https://user-images.githubusercontent.com/67897827/179950890-f4031426-5ea9-4816-a17a-cb6ea444f3eb.png)

위 그림에서 Gateway Endpoint를 예를들면 private에서 S3에 접근하고 싶으면 라우터테이블로 가서 거기서 Gateway Endpoint로 쏴주고 여기서 S3로 쏴준다.

 ※ 실습 후 참고 내용 ※
1. VPC 만듬 -> 그 후 저 VPC랑 연결한 서브넷을 2개 만들었다 = VPC만들면 라우팅 테이블 만들어지는데 이상황은 서브넷 2개가 다 저 라우팅테이블 하나에 연결되있다.
-> 우리는 서브넷마다 따로 라우터 테이블 있기 원하기 때문에 라우팅 테이블가서 한개더 만들고 작업들어가서 서브넷 연결 편집들어가서 연결
-> 그 후 퍼블릭 라우팅은 기본 셋팅 빼고 외부와 연결되있어야 되기때문에 로컬로들어가는거 외에는(0.0.0.0/0이라고 하면 모든ip뜻하는데 어짜피 순서상 위에서 
로컬로 가는것이 먼저 수행되고 나니까) 우리가 라우팅 테이블에서 인터넷게이트웨이로 보내줘야된다는 라우팅 추가 해줘야한다.(라우팅 편집에서)

(VPC 설정할때 사이드블록에 10.0.0.0/16 이라는 뜻은 여기 내에 있는 IP가 들어오게되면 일루 보내라)
(VPC만들면 NACL과 Route Table은 자동으로 만들어짐, 서브넷 설정할때 10.0.0.0/24 랑 다르게 하려면 10.0.1.0/24 이렇게 해야된다. 
서브넷이기 때문에 24로 고정시킨 그 부분이 달라야된다.)

2. 인터넷게이트웨이 만들어줘야함->만들면 작업들어가서 VPC연결-> 위에도 언급했지만 퍼블릭 서브넷 라우팅테이블에는 인터넷게이트웨이 가는거 설정해주기

3. NACL도 우리같은경우 서브넷별로 1개있어야되니까 1개더 만들자 -> 서브넷연결필수 -> NACL은 인바운드 아웃바운드 규칙 설정을 해야된다. 여기선 규칙번호라는게
있는데 이것은 작을수록 우선순위대로 보겠다는 것이다. 예를하나들자면 규칙번호 100에 유형 22 허용, 규칙번호 101에 유형 모든유형 거부, 규칙번호 200에 유형 80 허용
이라고 가정하면 22포트러 오는 트래픽은 허용하고 규칙번호 101에서 모든포트 거부했으니까 규칙번호 200에서 80들어오는거 허용한거는 묵살되는것이다.
NACL은 Stateless하기때문에 인바운드 아웃바운드 규칙 밖에 안봄으로 세밀하게 설정해야된다. 아웃바운드에서는 포트범위를 1024-65535라고 해야되는데 그 이유는
들어오는 포트는 정해지지만 나가는것은 임의로 나가기 때문에 저 포트들을 다 허용해야 그 중에 하나 걸리는 것이다.







