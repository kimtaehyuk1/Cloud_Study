AWS Organizations  
• 여러 AWS 계정을 중앙에서 관리  
• Organizations는 글로벌 서비스  
• 전체 계정을 관리하는 계정을 관리계정(Master Account)라고 함  
• 그 외의 계정은 멤버 계정이라고 부름  
• 조직관리를 위해 OU(Organization Unit)이라는 조직 단위로 그룹화 하여 관리  
• 그룹마다 서비스 제어 정책(SCP, Service Control Policy)를 적용해서 액세스를 제한 해야 하는 서비스를 제어할 수 있음  
• 계정을 통합하면 결제를 한곳으로 통합 가능하고 볼륨 가격 할인을 받을 수 있음  


![image](https://user-images.githubusercontent.com/67897827/182325925-37e5e31b-e2f7-4b06-a5bd-8d7172d47ac8.png)

![image](https://user-images.githubusercontent.com/67897827/182326136-0943463d-f080-4dcc-b9dd-d968c0ab692d.png)

(실습)  
AWS Organization입력 AWS 계정추가 -> 기존 AWS계정 초대,계정아이디 붙여넣기,초대전송 -> 초대받은 계정에서 Organization들어가 옆탭에 초대들어가서 수락하면됨 ->
다시 본계정으로와서 목록보게되면 추가되있다. -> 계층구조눌러 root클릭해서 하위항목 관리계정옆에 작업눌러서 ou인 조직단위를 새로생성 누르기 -> 이름,조직단위생성(2개만들기)
-> 만들려진거 클릭해서 또 작업에 새로생성 누르면 그 밑으로 들어간다. -> 다시 목록클릭해 초대받은 계정클릭해서 작업에 이동눌러서 만든 ou중 갈곳정하기

왼쪽 정책 클릭 -> 서비스제어정책 눌러 활성화 -> 정책생성 -> 이름,오른쪽아래 작업추가EC2검색해 선택하고 all action선택,그다음2.리소스추가,리소스유형은 all리소스 후 정책생성
-> 이걸 만든 ou중 적용할 ou클릭해서 정책탭 클릭해서 연결눌러서 정책연결
