* 도커가 나온 이유
- 개발 환경을 그대로 복제해서 같은 환경에서 배포하고 싶다.
- 같은 서버에서 개발 환경을 여러개로 분리하고 싶다(같은서버내 다른 프로그램마다 독립적이어야지 서로 영향을 안미친다.)

가상환경이 저 두가지의 니즈를 어느정도 충족시키기는 하지만 비효율성이 생긴다! 어느부분이냐?
가상환경은 서버내에서 프로그램이 독립적으로 돌아가려면 일정 비율로 컴퓨팅파워를 분배해야된다. 즉 30대 70으로 나눴다고 가정했을때 30이 바쁘고 70이 놀고있어도
자동적으로 인지하지 못하고 70은 계속 놀고있는것이다. 

* 하지만 도커같은 경우에는 유동적으로 컴퓨팅 자원을 공유해서 쓴다 알아서 많이 쓰고 있는거 도와준다.
* 도커에서는 독립적인 프로그램들을 컨테이너 형태로 환경을 구분해준다라고 한다.
* 이미지를 통해 '같은 환경'의 가상 컴퓨터(컨테이너)를 무한히 생성할 수 있다.(auto scaling) ex)만약 개발자가 퇴사할때 이미지 남겨주면 설령 버젼을 다른것이라해도
남겨진 이미지 연장선에서 개발을 진행하면 된다.

* Docker-Compose
이거는 만약 컨테이너가 마이크로서비스라 서로 컨테이너간 유기적인 관계가 있을때(상호작용 해야 될 때) 그 위에 Docker-Compose라해서 이 컨테이너들을 포함할 수 있고,
또 이것을 통해서 컨테이너의 관계도 정의 할 수 있다.(필요하면 실습코드 보기)

django와 nginx에 도커파일 만들어서 컨테이너 실행해 웹서버 돌리는것은 영상참조 ( 필기 다 했는데 날라감 ㅠ)

※ AWS ECR(Elastic Container Registry)에 컨테이너 업로드
옵션1.
![image](https://user-images.githubusercontent.com/67897827/180708489-c2d66fb2-683c-4412-accf-8bd660779f1f.png)
개발을하고-> 도커이미지를 만들고-> 그 이미지를 외부 Registry에 등록을한다.(그 저장소는 Docker Registry일수도 있고, AWS ECR일수도 있다)->등록된 이미지를
ECS(Elastic Container Service)라는 서비스를 통해서 여러가지 EC2에 배포를 한다.

(실습)  
EC2만들기(Ubuntu로,Name:docker-ECR) -> PUTTY열고 빈칸에 ubuntu@만든EC2 퍼블릭IPv4 DNS 붙여넣기하고 SSH Auth에 allow체크해주고 밑에 ppk붙여넣기
-> curl -fsSL https://get.docker.com/ | sudo sh 명령어로 도커 다운받고 sudo usermod -aG docker $USER로 권한설정하고 mkdir docker-sever파일 만들어주고 cd로 가기
-> git clone (깃허브 올려놓은 code에 SSH에 주소있는거 복사해서 붙여넣기).git -> ls하면 fastcampus_test 있을거고 cd로 들어가기-> vi Dockerfile 만들어주기 insert모드에서

FROM python:3.6.7

ENV PYTHONUNBUFFERED 1

RUN apt-get -y update
RUN apt-get -y install vim

RUN mkdir /srv/docker-server
ADD . /srv/docker-server

WORKDIR /srv/docker-server

RUN pip install --upgrade pip
RUN pip install -r requirements.txt

EXPOSE 8000
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

이거 쳐주고 wq로 저장해서 그 후 도커파일을 통해서 이미지 생성하는 docker build -t ecr/django . 이거 하면 permission deny나오는데 putty창 꺼서 다시 드가서 다시 fastcampus_test 로 드가서 build명령어 똑같이 다시 치면 실행 된다. -> 다음으로 도커에 tag달아줘야 된다 docker tag (만든 이미지 ID) (AWS ECS 리포지토리에서 만든 URI)
이렇게 명령어 쳐주기! 이 말은 이 이미지를 리포지토리에 올릴거라는 tag다는거다 (참고로 docker image 치면 만든 이미지 ID 볼수 있다)
-> aws ecr get-login --no-include-email --region us-east-2 이건 ecr에 접근하고 싶다는 커맨드라이다 이게 바로 AWS CLI인데 아마 안되고 sudo apt install awscli 나올거다
이거 쳐서 실행해서 다시 저거 쳐주면 되는데 또 aws configure 이라는것이 뜨면서 니가 aws사용 유저를 알려라고 뜰거다(밑에 설명) 다하고나서 다시 명령어 치기
-> 긴 문자열이 생길거다 그거 그대로 복사해서 복붙해서 쳐주기 그럼 로그인 성공 뜬다 -> 마지막으로 내가 만든 이미지를 저장소에 푸쉬하기위해 docker push (AWS ECS 리포지토리에서 만든 URI) 

--------------------------------여기까지는 ECR에 이미지 올린것이고 이제 클러스터를 생성 해줘야 된다--------------------------------------------------------------
클러스터는 하나의 구분된 영역이라 생각하기 비슷한얘들끼리 묶어놓은 집합체라고 생각
전체적인 프로세스는 클러스터를 생성하고 작업을 생성한다 작업은 클러스터 내부에있는 세부적인 행위이고 사실적으로 작업안에 여러가지 컨테이너가 섞여 있다! 이 작업을
실행을 시키면 서버가 수행이 된다라고 생각하기.

AWS ECS에 드가서 클러스터 생성 -> 네트워킹 전용 누르기(AWS Fargate사용할거니까) -> 이름은 fargate-cluster,컨테이너 인사이트 활성화 후 생성 -> 왼쪽 작업 정의에서
작업 생성 -> fargate누르기 -> 작업정의이름은 fargate-task, 작업메모리 0.5GB 작업CPU는 0.25, 컨테이너 정의에서 추가 눌러서 컨테이너 이름은 django1 이미지는 내가 
만들었던 ECR 레지스트리에가서 이미지 URI복사해서 이미지칸에 복붙, 메모리제한은 128,포트매핑은 8000포트로 이렇게만하고 다음 , 마지막 생성 누르기 그 후에
만들어진 작업을 실행 누르기(시작 유형은 fargate,클러스터는 방금 만들었던거 누르고,클러스터 VPC는 그냥 기본적인거 눌러서 있는거 누르고 서브넷도 눌러서있는 3개 다 눌러주기
, 보안그룹은 편집을 눌러서 인바운드 규칙 추가해서 8000번포트를 추가하겠다(customtcp TCP 8000)근데 나는 만들어놓거 있으니까 그거 쓰기 후 작업실행 누르기 그러면 배포가
된것이다. (작업에 세부정보보면 네트워크쪽에 퍼블릭IP있는데 거기에 배포가 된거다 즉 처음 이미지를 만들기위해서 EC2에서 작업을 해야되지만, 그 이후엔 ECS쓰면 된다
즉 EC2없이 바로 URL로 배포된거다)


(옵션 1 실행하기 위한 저장소 생성)  
AWS ECS드가서 왼쪽 바에 리포지토리 클릭 생성 누르기 -> 리포지토리 이름에 / fargate_django 이름 쓰고 생성(하나의 폴더 즉 저장소가 생긴거다)

(옵션 1 실행하기 위한 aws configure)  
aws IAM 드가기 -> 왼쪽바에 사용자 드가기 사용자추가해서 이름을 awscli라고 이름짓고 액세스 유형을 프로그램방식 체크 -> 권한 설정을 기존정책에 연결눌러 AdministratorAccess
누르기 -> 키는 name:admin 후 다음눌러 사용자 만들기하면 액세스 키 ID가 나와있다 putty창에 ID와 시크릿키 치고 나머지물어보는건 걍 엔터 치면 날 증명한거다 그 후 다시 aws 명령어 쳐보기



옵션2.
![image](https://user-images.githubusercontent.com/67897827/180709641-bbee6831-6bc2-413d-9536-dd47c3a538aa.png)
등록소에 저장하는거 까지는 똑같은데 여기서 빼서 배포를 하는 방식이 다르다 이전까지 배포할때 EC2서버를 만들어서 거기다가(EC2를 거쳐서 배포를 했는데)
그 EC2가 빠지고 마치 ECS와 EC2를 동시에 활용하는것 같은 효과를 주는것이다.


여기 실습은 영상통해서 참고하기

실습
![image](https://user-images.githubusercontent.com/67897827/180710228-e655cb3d-fac9-4c49-8698-ff5dd48b878e.png)




※ AWS CLI  
- AWS 명령줄 인터페이스(CLI)는 AWS 서비스를 관리하는 통합 도구입니다. 도구 하나만다운로드하여 구성하면 여러 AWS 서비스를 명령줄에서 제어하고 스크립트를 통해
자동화할 수 있습니다.(GUI를 하면 편하기는 한데 벙거롭다. 자동화를 못한다.)  
- AWS CLI는 Amazon S3에서 효율적으로 파일을 보내고 받을 수 있는 간단한 새 파일명령 세트를 제공합니다  

CLI 명령어(모르면 예를들어 ecr repository 생성 cli쳐서 문서 찾아보기)  
ECS에서 레포지토리 생성하는 명령어  
aws ecr create-repository --repository-name hello-cli --region us-east-2  
클러스터 생성부터 작업 생성의 CLI는 (영상참조) https://docs.aws.amazon.com/ko_kr/AmazonECS/latest/developerguide/ECS_AWSCLI_Fargate.html 여기 참조해서 따라하기  



