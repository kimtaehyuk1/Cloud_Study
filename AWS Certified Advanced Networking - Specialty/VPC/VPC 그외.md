NAT

private 서브넷에서 인터넷과 통신을 하려면 NAT 인스턴스나 NAT게이트웨이가 필요해서 프라이빗 IP를 퍼블릭 IP로 바꿔줘야만 인터넷과 통신이 가능하다.

NAT Instance
![image](https://user-images.githubusercontent.com/67897827/189818763-831c2385-d237-49ea-bf51-3dae0f594e14.png)

그림으로 설명하면 그림에 프라이빗 IP인 100.1.200.10은 프라이빗 라우팅테이블에서 NAT instance로 가는것을 보고 퍼블릭 서브넷안에 있는 NAT Instance로 가서 퍼블릭 IP로
변환 된 후에 다시 IGW(인터넷게이트웨이)를 경유해 밖으로 나갈 수 있는것이다.

NAT Instance  
• EC2인스턴스에 NAT 기능을 설치  
• 사용자가 직접 EC2에 NAT 기능을 구성해야 함  
• EC2가 문제가 생기면 NAT 기능이 동작 안 함  
• 많은 트래픽이 발생해 NAT 요청이 많으면 EC2의 CPU, Memory를 많이 사용하게 됨  
• 인스턴스 유형이 지원하는 네트워크 대역폭에 따라 처리량이 다름  
• NAT Gateway에 비해 더 많은 기능을 구현 할 수 있음!!!!!!!!  
  - 예, Squid Proxy를 설정하여 특정 URL로만 액세스 제한 가능(DNS Filtering)  
• EC2 인스턴스 이므로 Security Group 적용 가능!!!!!!!!!!  
• NAT 인스턴스 사용시 EC2 ENI의 원본/대상 확인(Source/Destination Check) 비활성화 해야함!!!!!  
  - 활성화가 되어 있으면 인스턴스가 보내거나 받는 트래픽이 원본 또는 대상이 아니므로 트래픽이 Drop됨  

NAT Gateway

![image](https://user-images.githubusercontent.com/67897827/189820243-148b8fd5-1be4-476c-8c96-2e62b434c5a2.png)

그림과 같이 위와 NAT instance설명 그림과 메커니즘은 똑같은데, 퍼블릭 서브넷에 NAT Gateway만들고 탄력적IP부여해 퍼블릭 IP만들어준다.

NAT Gateway  
• NAT Instance 처럼 EC2에 구성하지 않고 AWS에서 제공하고 관리하는 서비스  
• 고가용성을 지원  
• 최대 45Gbps까지 자동으로 대역폭이 확장 됨  
• Port Forwarding 등의 사용자가 추가 기능을 구현할 수 없음!!!!!!!!!!!!  
• NAT Gateway에 Security Group 적용 불가!!!!!!!!  
• NAT Gateway 생성시 Elastic IP를 지정해야 하며 하나의 Elastic IP 주소만 NAT 게이트웨이에 연결 가능!!!!  
• NAT 게이트웨이는 포트 1024 - 65535를 사용하므로 NACL 인바운드 트래픽에서 해당 포트가 허용되어 있어야 트래픽 수신가능  

















