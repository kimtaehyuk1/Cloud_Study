![image](https://user-images.githubusercontent.com/67897827/181710738-1bdda4cc-ff70-4949-b928-cdc711c97a68.png)

![image](https://user-images.githubusercontent.com/67897827/181711108-7692c5e6-dcb4-4208-a996-07a80184fa6e.png)

![image](https://user-images.githubusercontent.com/67897827/181711493-0bd362ef-3b48-4cc9-adfd-0d8696773079.png)

(실습)  
EC2를 서울과 미국에 한개씩 만든다(웹서버생성 스크립트 넣주기) -> global accelerator서비스들어가서 만들기 누르기 -> 이름짓고,타입은 스탠다드-> 다음
리스너는 80에 TCP,Client affinity는 전에 접속서버 접속해주는건데 이거 일단 none -> 엔드포인트그룹에 리전을 우린 서울과미국에 만들었으니까 추가해서 두개 해주기 -> 
엔드포인트 그룹에 리소스연결해주기 위해 add endpoint클릭해서 인스턴스라 고르고 만든거 고르기 후 만들기 누르기

이렇게해서 만들어진 accelerator DNS name으로 접속하면 나는 서울에서 이거 하기에 가까운 서울리전께 뜰것이다.
