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

(실습)  
NAT gateway 생성 -> 퍼블릭 서브넷에 연결유형 퍼블릭으로 탄력적 IP없으면 할당하기 -> 그다음에 프라이빗 서브넷 라우팅 테이블로 가서 라우팅 편집눌러서 0.0.0.0/0은
NAT gateway로 가게 만들어주기 이렇게 하면됨!

시험 나옴!! 겁나 중요!!

![image](https://user-images.githubusercontent.com/67897827/189825408-3db1383c-cee7-4a8d-9184-25d1e69da703.png)


ELB
![image](https://user-images.githubusercontent.com/67897827/189841295-9f8aaf84-5584-48d7-b00f-f5c2175aa314.png)

GWLB

![image](https://user-images.githubusercontent.com/67897827/189842658-6babb6d5-a8a7-4572-9dca-f6029b999606.png)

그림에서 보면 파란선이 인터넷으로부터 애플리케이션 서버까지 가는 과정이고 주황선이 그 반대이다. 즉 바로가지 않고 GWLB 엔드포인트갓다가 GWLB통해 보안검사를 받고
서버로 들어가는 것이다.(GWLB는 보안 관련 어플라이언스와 연관)

![image](https://user-images.githubusercontent.com/67897827/189843950-3e618306-0f3e-48b3-ada3-df761e792ba2.png)

ELB에선 리스너 라는 것이있는데, 이건 클라이언트와 로드밸런서간 연결을 위한 프로토콜 및 포트번호 구성하고, 로드밸런서와 대상그룹간 프로토콜 및 포트번호 구성

즉 클라이언트가 로드밸런서로 접속하게 되면 타겟으로 라우팅하게 된다.

타겟그룹 생성
1. 대상유형에는 EC2인스턴스(및 EC2 Auto scaling), IP 주소, Lambda 함수
2. 프로토콜을 지정할 수 있다.
3. 상태검사가 있는데 타겟이 정상인지 비정상인지 확인하는 기능이다.
4. 고른 프로토콜에 있어 속성을 설정할 수 있다.
  - 프로토콜이 HTTP라고 가정 했을때
    - 등록 취소 지연 : 이건 만약 오토스케일링으로 인스턴스가 갑자기 줄었는데 그 사이에 어떤 클라이언트가 접속해서 뭘 하고 있으면 그거 끝날때 까지 기다려줘야 된다.
    - 느린 시작 기간 : 타겟그룹으로 되자마자 모든 요청 공유를 받게 되는데 이거 하면 이 기간동안 요청 사항이 선형적으로 증가한다.
    - 알고리즘 : 라우드 로빈은 일정 시간마다 라우팅 변경하는 것이고, 최소 미해결 요청은 처리하고 있는 요청이 가장 적은 대상에게 라우팅 하는 것이다.
    - 고정 : 클라이언트가 세션을 유지한 상태라면 요청을 동일한 인스턴스로 고정 할 수 있는 기능이 고정이다.
속성이 TCP/UDP일땐 밑의 속성 참조
![image](https://user-images.githubusercontent.com/67897827/189849351-5eca3a89-f267-40b0-864a-48c67832c472.png)


(실습)  
 EC2 만들건데 밑에 웹서버 생성 스크립트를 고급 세부정보에 넣주기, 태그추가에 Name EC2_01, 보안그룹 새 보안그룹 생성에서 이름과 설명에 web_access 하고 SSH규칙지우고 
 HTTP추가 + 0.0.0.0/0 해주기 후 키페어 없이 계속해서 만들기 -> 확인하기 위해 인스턴스 가서 퍼블릭 IP주소를 구글창에 치기

#!/bin/bash
yum update -y
yum install httpd -y
systemctl enable httpd.service
systemctl start httpd.service
cd /var/www/html
echo "안녕하세요. 김태혁 입니다. $(hostname -f)" > index.html

Application Load Balancer  
• 최소 2개 이상의 가용영역을 지정 해야 함(NLB는 1개)  
• 서브넷당 사용가능한 IP 주소가 최소 8개 필요  
• 권장하는 최소 CIDR 블록은 /27 넷마스크 (예: 10.0.0.0/27)  
• 리스너는 HTTP , HTTPS를 사용 할 수 있음  
• HTTPS 프로토콜 사용시 SSL/TLS 인증서를 배포해야 함  
• 인증서는 ACM(AWS Certificate Manager)사용 또는 클라이언트 인증서 사용 가능  
• 소스 IP 전달(Source IP Preservation)사용을 위해서는 X-Forwarded-For 헤더를 사용  
• WebSocket 기능 사용을 위해서는 Stickiness를 활성화 해야 함  

![image](https://user-images.githubusercontent.com/67897827/189853733-6ee6f9d4-694a-4607-b512-1aa58e9bf50c.png)

긍까 리스너 규칙은 로드 밸런서 하는데 IF ~면 THEN ~해라 

![image](https://user-images.githubusercontent.com/67897827/189854421-c3c49524-c0e0-43ad-9de4-8ff64a895fbf.png)

![image](https://user-images.githubusercontent.com/67897827/189854599-c659b943-8bfc-4490-8572-4a5a0f8a5a02.png)

(실습)  
대상그룹에서 대상그룹 생성 -> 대상유형은 인스턴스 -> 대상그룹 이름에 HTTP-TG-A 프로토콜은 HTTP, VPC는 디폴트 버전은 그대로, 상태검사도 HTTP 경로 / 냅두기 -> 대상등록
에서 사용가능한 인스턴스에 우리가 만든 두개 같이 눌러서 아래에 보류중인거 누르기 후 대상그룹 생성 
-> 그 다음 로드 밸런서 클릭해서 생성 누르고 유형을 ALB로 -> 이름은 ALB로 체계는 인터넷경계로 IPv4로, 네트워크 매핑에선 최소 2개이상 가용영역 선택 보안그룹은 아까 만든 web_access(앞에건지우고), 리스너 및 라우팅은 HTTP선택 전달대상에 아까 만든 HTTP-TG-A선택 그후 로드밸런서 생성

로드 밸런서 가게 되면 아래 설명에 DNS이름이있는데 이거 복사해서 구글에 쳐서 접속하는거다 그러면 연결된 대상그룹이 있는 인스턴스 접속

Network Load Balancer  
• Millions of Concurrent Users/Requests Per Second 의 경우에 적합한 로드 발란서  
• 서브넷당 사용가능한 IP 주소가 최소 8개 필요  
• 권장하는 최소 CIDR 블록은 /27 넷마스크 (예: 10.0.0.0/27)  
• 고정 IP 주소 할당 가능!!!!!!!! 
• 클라이언트 IP 주소 전달 가능 (Source IP Preservation)!!!!!!!  
• WebSocket 지원!!!!!!  
• ALB처럼 리스너 규칙 설정 없음!!!!!! 하지만 추가할 수는 있다!!!!!!  
• 리스너 프로토콜은 TCP, TCP/UDP, UDP, TLS를 사용 할 수 있음  
• TLS 프로토콜 사용시 SSL/TLS 인증서를 배포해야 함  
• 인증서는 ACM(AWS Certificate Manager)사용 또는 클라이언트 인증서 사용 가능  

(실습)  
고정 IP 주소 사용하기 위해 탄력적 IP 하나 생성 -> 대상그룹 생성-> 대상유형 인스턴스 이름은 TCP-TG-A,프로토콜은 TCP 상태검사도 TCP-> EC2 두개 눌러 대상등록
-> 그 다음에 로드밸런서 생성 NLB클릭 -> 이름은 NLB,인터넷경계,주소는IPv4,네트워크매핑은 최소 1개이상 서브넷만 선택해도됨 또 여기서 고정 IP할당하기 위해
IPv4주소에 탄력적 주소 사용 누르기, 리스너 및 라우팅에서 TCP누르고 대상그룹 눌러주기 후 생성
