![image](https://user-images.githubusercontent.com/67897827/181439754-b46a201b-96c1-4663-b7cc-2ed14677e10e.png)

Elastic File System (EFS) – 성능 및 스토리지 클래스

스토리지 클래스  
• 표준 스토리지 (Standard) - 3개의 가용영역에 데이터 저장, 자주 액세스하는 파일을 저장하는 데 사용  
• 표준 IA (Standard Infrequent Access) - 3개의 가용영역에 데이터 저장, 자주 액세스하지 않는 파일을 저장하는 데 사용  
• One Zone - 1개의 가용영역에 데이터 저장, 자주 액세스하는 파일을 저장하는 데 사용  
• One Zone IA (One Zone Infrequent Access) - 1개의 가용영역에 데이터 저장, 자주 액세스하지 않는 파일을 저장하는데 사용  
• EFS 수명주기 관리 정책 또는 EFS 지능형 계층화(Intelligent-Tiering)을 사용해 자주 사용하지 않는 데이터를 다른 스토리지 클래스로 자동 전환 가능

성능 모드 : I/O, 읽기 쓰기 속도  
• 기본 범용 성능 모드 (General Purpose performance mode) – 일반적인 I/O 성능  
• 최대 I/O 성능 모드 (Max I/O performance mode) – 높은 성능의 처리가 필요한 빅데이터 분석 애플리케이션 등에 사용

처리량 모드 : 파일 시스템의 처리량(MiB/s)  
• 기본 버스팅 처리량 모드 – 파일 용량이 커짐에 따라 자동으로 처리량을 확장  
• 프로비저닝된 모드 - 저장된 데이터의 양과 상관없이 고정으로 처리량을 지정  

(실습)  
EFS은 보안그룹을 거치니까 보안그룹 먼저 생성 이름:efs_sg,설명도똑같이,인바운드규칙에 NFS선택하고 대상은 EC2인스턴스 보안그룹 대상으로(ssh_web_sg)후 보안그룹 생성
-> EFS파일시스템 생성,이름efs,사용자지정 클릭해서 옵션한번 보고 다음-> 각 가용영역에 대해 보안그룹은 방금 생성한 보안그룹으로 바꾸기->정책 냅두고 생성

EC2인스턴스에가서 만든거랑 연결해야된다. 인스턴스 하나 더 만드는데 만드는 과정에서 인스턴스 구성에서 파일시스템 추가,추가보안그룹은 체크 해제->스토리지냅두고-> 태그에
Name EC2_EFS 보안그룹 선택은 만들어논 ssh_web_sg후 시작하기

또 기존에 있던 EC2에 EFS스토리지를 마운틴 한다 우선 파워셀 접속해서 ssh명령어로 EC2에 접근(자세 명령어는 EC2꺼보기) 접속됫다 가정-> 젤먼저 EFS 클라이언트 설치
명령어: sudo yum install -y amazon-efs-utils -> AWS에서 EFS가서 생성 EFS클릭해서 오른쪽 상단 연결 누르면 EFS탑재 헬퍼에있는 명령어 사용 할껀데 우선 mkdir로 efs만들어주고
그 파일에 명령어 입력 cd efs로가서 sudo touch inostudy.txt해서 파일한개 만들어 주기 -> 그다음으로 이거 전에 생성한 EFS(바로위)로 ssh접속해서 루트 디렉도리에서 ll루르면
-> cd mnt -> cd efs -> cd fs1 -> ll누르면 아까 만든 inostudy.txt파일이 있다.

이렇게 두개의 인스턴스가 동일한 파일시스템에 마운트 된것을 확인!
