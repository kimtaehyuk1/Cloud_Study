![image](https://user-images.githubusercontent.com/67897827/182538518-21b90465-ec81-4dec-81c0-badc7c82e9a8.png)

NAT Instance  
• EC2인스턴스에 NAT 기능을 설치  
• 사용자가 직접 EC2에 NAT 기능을 구성해야 함  
• EC2가 문제가 생기면 NAT 기능이 동작 안 함  
• 많은 트래픽이 발생해 NAT 요청이 많으면 EC2의 CPU, Memory를 많이 사용하게 됨  
• 인스턴스 유형이 지원하는 네트워크 대역폭에 따라 처리량이 다름  
• NAT Gateway에 비해 더 많은 기능을 구현 할 수 있음  
• EC2 인스턴스 이므로 Security Group 적용 가능  

![image](https://user-images.githubusercontent.com/67897827/182538964-bcfc6df9-0952-49a0-853e-c9ac4d74a48a.png)

NAT Gateway  
• NAT Instance 처럼 EC2에 구성하지 않고 AWS에서 제공하고 관리하는 서비스  
• 고가용성을 지원(여러 가용영역에 배포가 되기 때문에)  
• 최대 45Gbps까지 자동으로 대역폭이 확장 됨  
• Port Forwarding 등의 사용자가 추가 기능을 구현할 수 없음(NAT instance는 사용자지정 소프트웨어이기에 더 많은 기능 지원한데비해)  
• NAT Gateway에 Security Group 적용 불가  
• NAT Gateway 생성시 Elastic IP를 지정해야 하며 하나의 Elastic IP 주소만 NAT 게이트웨이에 연결 가능  

NAT는 퍼블릭 서브넷에 만들어 줘야된다.

(실습)  
NAT 게이트웨이 만들기전에 여기에 연결될 탄력적IP먼저 만들어준다 -> NAT게이트웨이가서 생성누르기 -> 이름,서브넷은 퍼블릭에,탄력적IP할당 후 생성 하고 이후에 라우팅테이블을
통해서 연결해주기위해 프라이빗라우팅테이블 선택하고 아래 라우팅탭에 편집눌러 라우팅 추가 0.0.0.0/0 대상에 NAT 게이트웨이 선택 후 변경사항 저장 이러면 프라이빗 서브넷에도
인터넷이 접속된다.

![image](https://user-images.githubusercontent.com/67897827/182540129-2ae23248-66ff-4dae-8399-c15a08c7aa43.png)

![image](https://user-images.githubusercontent.com/67897827/182542043-3b36240f-7ffb-43c1-9e05-8bb3e8b23e35.png)

![image](https://user-images.githubusercontent.com/67897827/182542204-7d619486-5447-4b28-87ad-04a307a3d195.png)


AWS PrivateLink  
• VPC와 서비스 간에 프라이빗 연결을 제공하는 기술  
VPC 엔드포인트  
• 인터넷을 통하지 않고 AWS 서비스에 프라이빗하게 연결할 수 있는 VPC의 진입점  
✓ 게이트웨이 엔드포인트  
✓ 인터페이스 엔드포인트  
✓ Gateway Load Balancer 엔드포인트  
엔드포인트 서비스 (AWS PrivateLink)  
• VPC 내에 있는 애플리케이션 또는 서비스  
• 다른 AWS 계정의 VPC Endpoint에서 엔드포인트 서비스(AWS PrivateLink)로 연결  


![image](https://user-images.githubusercontent.com/67897827/182547187-26125b49-90c8-4330-aef3-84b9b432fccd.png)

위의 S3 와 DynamoDB 이 두가지 서비스만 VPC EndPoint에 Gateway Endpoints를 생성해서 연결 할 수 있다. 다른 AWS서비스는 인터페이스 엔드포인트를 사용해서

![image](https://user-images.githubusercontent.com/67897827/182548465-c90ba8c2-a7fb-4978-b35c-16f9b8d55a3f.png)


![image](https://user-images.githubusercontent.com/67897827/182551479-55410a59-8a65-4430-9a49-cd70566fc39d.png)

Peering방법은 1대1이라 계정많으면 다 일일이 연결해줘야되고 고객사와 IP대역이 겹치면 peering설정 못할것이다.

![image](https://user-images.githubusercontent.com/67897827/182553057-ff755755-2e3c-4471-95d4-0e66e93d1900.png)

VPC Endpoint Service(AWS PrivateLink)는 서비스 프로바이더가 EC2인스턴스앞에 NLB나 GLB를 설치하고 이것은 Endpoint Service로 만들어준다. 이젠 커스터머에서 
VPC Endpoint에 인터페이스 엔드포인트를 만들어준다. 그러면 VPC Endpoint에 네트워크 인터페이스가 생성된다. 이것과 옆에 엔드포인트 서비스를 서로 연결해준다 그러면 밴더에
엔드포인트 서비스 내에 있는 NLB뒤에 있는 application을 사용할수 있고, 이렇게 연결하면 여러명의 커스터머도 인터페이스 엔드포인트만 생성하면 다 엔드포인트 서비스에 연결
가능해서 app쓸수있다.
