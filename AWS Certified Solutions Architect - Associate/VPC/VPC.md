VPC (Virtual Private Cloud)  
• AWS의 가상 네트워크  
• AWS서비스의 네트워크 연결을 제어하는 기능  
• AWS계정을 생성하면 기본VPC네트워크가 생성됨  
• 기본 VPC는 인터넷과 연결되어 있고 EC2 인스턴스를 생성하면 기본VPC에 연결  
• VPC는 Private 네트워크이므로 Private IP 대역을 사용  
10.0.0.0 – 10.255.255.255 (10.0.0.0/8)  
172.16.0.0 – 172.31.255.255 (172.16.0.0/12)  
192.168.0.0 – 192.168.255.255 (192.168.0.0/16)  

![image](https://user-images.githubusercontent.com/67897827/182393300-46741577-2254-4a2f-b066-a1d6aaf436fa.png)


![image](https://user-images.githubusercontent.com/67897827/182393692-8487b319-aaf8-49f7-9c3a-9e4c83ec57d7.png)
라우팅 테이블이 없으면 서로다른 서브넷 인스턴스끼리 통신할 수가 없다.

(실습 과정 요약)
![image](https://user-images.githubusercontent.com/67897827/182396207-1347667c-d7f2-4b74-946a-85b9b5618ce6.png)
VPC를 만들어주면 NACL과,라우팅테이블이 만들어지고 서브넷을 만들건데 한개는 퍼블릭,한개는 프라이빗으로 만들것이다. 자동으로 생성된 라우팅 테이블 말고 1개더 생성해야된다. 
그 후 라우팅테이블을 통해 둘을 연결해준다. (이러면 퍼블릭과 프라이빗이 통신가능) 그 후 인터넷게이트웨이를 만들어주고 이놈은 퍼블릭서브넷에만 통신이 되도록 라우팅한다. 
그 후 퍼블릭서브넷에 EC2인스턴스를 하나만들고,프라이빗도 하나 만들어준다.  

![image](https://user-images.githubusercontent.com/67897827/182397055-62ef6747-5361-4f08-a11c-fbe3e448d09d.png)

![image](https://user-images.githubusercontent.com/67897827/182401276-9f7dbd3b-000a-4f9c-b4bc-1dad1bcace9b.png)


걍 중간 노트 필기  
라우팅테이블 만들어 주면 중간탭에 서브넷연결 편집눌러서 맞는 서브넷과 연결 또 인터넷게이트웨이를 만들건데 이건 퍼블릭 서브넷에만 해주기위함이니까 퍼블릭서브넷과 연결된
라우팅테이블에 0.0.0.0/0은 IGW로 가게해준다. 또 인터넷게이트웨이 만들어주면 오른쪽 상단 작업에서 VPC연결 그 후 라우팅테이블 탭에서 퍼블릭라우팅 테이블 선택해서 중간탭에
라우팅에서 라우팅편집눌러서 0.0.0.0/0 인터넷게이트웨이 선택 그 후 EC2만드는데 과정에서 우리가 만든 VPC로 특히 퍼블릭서브넷안에 EC2만들때는 퍼블릭IP할당 활성화 후 계속
다음 해준다음에 보안그룹새로 만들고 월래있는 기존규칙 지우고 규칙추가해서 사용자지정은 모든 TCP 허용하는데 대상은 0.0.0.0/0(실제에선 모든 트래픽 허용하면 큰일남) 똑같이
프라이빗서브넷에도 EC2만들어주기


보안그룹은 EC2한개에 대한 방어벽이고 NACL은 서브넷전체를 지키는 방화벽느낌
![image](https://user-images.githubusercontent.com/67897827/182405217-20e490a2-4914-4516-ace9-8596274bb4ff.png)

![image](https://user-images.githubusercontent.com/67897827/182405530-ff5120f1-b2a6-41eb-b1bb-38583f50a16b.png)

![image](https://user-images.githubusercontent.com/67897827/182406237-e4d4b158-43e7-4a69-ae37-dfe8f5f2de84.png)

![image](https://user-images.githubusercontent.com/67897827/182407344-3a2026ce-bcb4-49b2-af32-3639d14a9720.png)

![image](https://user-images.githubusercontent.com/67897827/182407759-816405a6-f376-49d6-a445-903643576a59.png)


