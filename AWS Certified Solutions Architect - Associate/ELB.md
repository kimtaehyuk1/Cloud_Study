
![image](https://user-images.githubusercontent.com/67897827/181186839-55d984db-112a-4967-a1ff-329ecf665f14.png)

Load Balancer 개요
• 트래픽을 분산하는 서비스  
• EC2 인스턴스, 컨테이너, IP 주소 등 여러 대상으로 자동으로 분산 가능  
• 비정상 대상을 감지하면, 해당 대상으로 트래픽 라우팅을 중단하고 대상이 다시 정상으로 감지되면 트래픽을 해당 대상으로 다시 라우팅  

![image](https://user-images.githubusercontent.com/67897827/181187940-169c635d-59c7-4bd1-97de-8966356fa53c.png)

![image](https://user-images.githubusercontent.com/67897827/181188770-adead07f-3c27-42f8-96e6-ea9eed2151ac.png)

즉 클라이언트가 로드밸런서로 접속하게되면 리스너가 프로토콜 및 포트번호를 확인해서 로드밸런싱을 대상그룹으로 전달하게 된다.


AWS 실습에서 대상그룹 선택할때
1. 타겟그룹에 인스턴가 있는데 인스턴스에는 EC2 인스턴스와 EC2에 오토 스케일링 그룹이 있을 수 있다. 두번째로 IP주소를 선택할 수 있고 마지막으로 Lamda를 선택할 수 있다.
2. 각 LB에 맞는 프로토콜 선택
3. 상태검사
4. 속성(HTTP/HTTPS)속성값 지정
 - 등록 취소 지연 : Auto Scaling 축소하게 되면 접속 EC2가 종료되는데, 바로 종료하지 않고, 접속된 EC2의 용건이 끝날때까지 일정시간 대기 했다가 끊는것  
 - 느린 시작 기간 : Auto Scaling으로 EC2추가 하게 되면 트래픽 할당을 과도하게 받지 않고, 적절하게 할당량을 늘려주는 기능  
 - 알고리즘( 라운드 로빈 : 일정시간마다 라우팅을 변경, 최소 미해결 요청 : 처리하고 있는 요청이 가장 적은 대상에게 라우팅)  
 - 고정 : 클라이언트가 세션을 유지한 상태라면 모든 요청을 동일한 인스턴스로 유지하는 기능  
4-2 속성(TCP/UDP/TLS)속성값 지정  
 - 등록 해제시 연결 종료 : 등록 해제 지연에 도달 했을 때 NLB가 활성 연결을 종료
 - 클라이언트 IP 주소 보존 : 들어오는 모든 트래픽의 클라이언트 IP를 애플리케이션으로 전달하는 기능  

Application Load Balancer(ALB)
• HTTP/HTTPS 요청을 로드 밸런싱 해야 하는 경우 사용  
• 웹 애플리케이션 처리에 적합  
• ★★★ 리스너 규칙을 기반으로 라우팅 설정이 가능함  
• ★★★ 데이터 전송 보안을 위한 HTTPS 프로토콜 사용시 SSL/TLS 인증서를 배포해야 함  
• 인증서는 ACM(AWS Certificate Manager)사용 또는 클라이언트 인증서 사용 가능  

![image](https://user-images.githubusercontent.com/67897827/181197923-7f32fdce-7bd5-4d34-acf1-c7bafb755e71.png)  


Network Load Balancer
• TCP, UDP, TLS 요청을 로드밸런싱 해야 하는 경우에 사용  
• ★★★(빠름)고도의 성능이 요구되거나 대기 시간이 낮아야 하는 애플리케이션에 적합  
• 게임 등의 수백만의 동시 사용자 처리에 적합  
• ★★★ 고정 IP 주소 할당 가능  
• ★★★ 클라이언트 IP 주소 전달 가능 (Source IP Preservation)  
• ALB처럼 리스너 규칙 설정 없음  
• 리스너 프로토콜은 TCP, TCP/UDP, UDP, TLS를 사용 할 수 있음  
• 데이터 전송보안을 위한 TLS 프로토콜 사용시 SSL/TLS 인증서를 배포해야 함  
• 인증서는 ACM(AWS Certificate Manager)사용 또는 클라이언트 인증서 사용 가능  



(실습)
인스턴스 시작->인스턴스 구성에서 인스턴스 개수 2개 나머지 냅두고 고급 세부 정보에 스크립트붙일 수 있는데 이건 인스턴스 실행 될때 사용자데이터에 입력된 스크립트가 자동으로
실행된다(여기는 웹서버를 생성하는 스크립트를 붙여준다)  
#!/bin/bash
yum update -y
yum install httpd -y
systemctl enable httpd.service
systemctl start httpd.service
cd /var/www/html
echo "안녕하세요. 이노스터디 입니다. $(hostname -f)" > index.html  
->스토리지 추가 냅두고 ->  태그 추가에 Name EC2_Web -> 새보안그룹 만들어서 SSH지우고 HTTP와 HTTPS추가 보안그룹 이름은 web_access_sg 이렇게 되면 인스턴스 만든거  
인스턴스에서 퍼블릭 IP복사해서 URL창에 치면 나온다  

이후에 로드밸런싱칸에 대상그룹생성 -> 대상 유형은 인스턴스, 그룹이름은 TG-ALB, 프로토콜은 HTTP 80번 포트, 나머지 냅두기 -> 대상그룹 연결할 인스턴스에 방금 생성한
두개의 인스턴스 눌러주고 아래에 보류 중인 것으로 포함 클릭 후 대상그룹 생성 똑같이 TG-NLB로 한개더 만들기(프로토콜은 TCP하기)  
-> 로드밸런서 생성 -> ALB선택 -> 이름 ALB,인터넷경계,IPv4, 네트워크매핑에서 가용용역 4개 다 선택,보안그룹은 web_access_sg이거 선택 리스너및라우팅은 HTTP 80 TG-ALB  
나머지냅두고 생성눌르기 그러고 나서 로드밸런싱 DNS로 구글 접속해보면 그 안에(대상그룹)안 인스턴스들이 번갈아 가면서 뜬다  

만약 HTTPS를 사용하려면 로드밸런서 들어가서 리스너 추가눌러서 -> 프로토콜 HTTPS로 바꾸고 SSL인증서에서 ACM에서 인증서 발급받아 사용하기  

NLB생성은 위에 자세히 써논것과 유사해서 따라하면 되는데 탄력적IP생성하기 위해(NLB는 고정IP가능하니까) 탄력적IP칸에가서 생성한다음 로드밸런싱생성 과정에서
네트워크 매핑에 가용영역 클릭하게 되면 IP주소 할당에서 탄력적 IP로 할당하기

