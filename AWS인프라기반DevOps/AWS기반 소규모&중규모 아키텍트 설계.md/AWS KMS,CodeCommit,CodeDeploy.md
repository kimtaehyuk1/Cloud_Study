## 개념먼저 잡기

![image](https://user-images.githubusercontent.com/67897827/190658262-6a1b27aa-c77c-4e27-97cf-ef9c37e0560f.png)

Client-side 암호화  
• 내가 직접 암호화 키를 관리(클라이언트가 암호화 키 관리하는게 핵심포인트--> 그래야 암호화도 하고 복호화도 한다.)  
• 필요하다면 AWS의 KMS를 활용할 수 있음( 암호화 키를 관리하기 어렵기에 그것을 해주는게 KMS tool이다.)  
Server-side 암호화  
• AWS가 알아서 서버에 저장된 데이터를 암호화시켜놓음  
• 암호화 키를 자동으로 관리  
• S3, RDS, DynamoDB, Redshift 등은 모두 암호화 기능을 기본으로 갖추고 있음  
• 이들은 암호화 키를 관리하기 위해 AWS의 KMS를 우리가 모르는 사이에 사용함  

![image](https://user-images.githubusercontent.com/67897827/190659166-905a8f71-1a30-4f31-ace1-978521aeb3cf.png)
(우린 클라이언트 사이드 암호화 하는거다)

![image](https://user-images.githubusercontent.com/67897827/190660249-99b7cb34-f82d-4117-856e-a1f2d3343b44.png)

그림 설명  

• CMK는 생성된 region을 떠나지 않는다(region 설정 필수)
• CMK를 통해선 최대 4KB의 데이터만 암호화할 수 있다.
• 더 큰 데이터를 암호화할 땐 CMK가 아닌 data key를 활용한다.
  --> 우리가 암호화할 데이터가 4kb이상일텐데 그럼 왜 CMK가 있느냐? 
  -> 그림에서 보이는것 같이 data key는 CMK가 암호화 알고리즘 통해 생성이된다. 그러면 data key 2개가 생성된다. 한개는 원본 data key, 다른 하나는 이걸 암호화 시켜논 data key
     그러면 원본 data key를 통해서 우리가 암호화시키고 싶은 실제 데이터를 암호화시키고 이걸 삭제시킨다.그러면 암호화된 data key만 남는데 그걸로는 암호화 된 데이터를 복호화
     시킬 수 없다보니까, CMK를 통해서 암호화된 data key를 원본 data key로 바꿔준다 이걸 활용해 암호화 데이터를 복호화 시키는 것이다. 복호화 시켰으면 원본 키는 또 삭제됨
     즉 !!!! 암호화된키와 암호화된 데이터 같이 저장해 놔야 털렸을때도 지장없다 짜피 우린 CMK가지고 있으니까 암호화 키에서 원본다시 만들 수 있으니까 갠춴
     
     그러니까 엄청큰 데이터를 암호화하기 위해 data key를 사용하는데 이건 CMK에서 나온거고 이 data key도 CMK로 암호화하는거다. 아~ 그래서 필요하구만
     
• KMS는 CMK만 관리하고 data key는 관리하지 않는다.!!!!!!!!!!!!!!!!!!!!!핵심!!!!!!!!!!!!!!!!!!!!1
• Data keys는 cmk를 통해서 생성된다.
• 하나의 cmk를 통해 여러 개의 data keys를 생성할 수 있다.
• 일단 큰 데이터를 암호화하기 위해 plaintext data key를 활용해서 암호화
• 암호화가 끝난 뒤엔 plaintext data key를 삭제

![image](https://user-images.githubusercontent.com/67897827/190660404-b4f216c4-50e2-46ff-bf85-a06ab3d5ef82.png)


팁! 회사들은 3중 방어체계이다 첫번째로 S3같은경우도 자체적으로 서버사이드에서 암호기능을 가지고 있다. 두번째 클라이언트 사이드에서 데이터를 data key로 암호화한다.
세번째 data key도 CMK로 암호화한다.!!(이 말뜻이 이해가 되야됨!)


(실습)  

우선 KMS를 쓸려면 AWS Encryption SDK를 써야된다.

AWS KMS 가서 키생성 -> 키유형은 대칭키(비대칭은 복잡해짐) 고급옵션은 KMS,단일리전 키 -> 레이블추가에서 별칭은 first-cmk 하고 다음 -> 키 관리 권한 정의는 IAM에 있는 
것들인데 누구한테 관리자 권한을 부여할거냐? 다음 -> 사용자도 누가 사용할건지 고르기 다음 -> 검토단계는 손안대로 완료 -> 인스턴스 만들기 후 putty로 그 인스턴스 들어가기
-> sudo apt update(업데이트하는거), sudo apt install awscli(cli까는거), aws configure(이건 KMS에서 키생성할때 관리자와 사용자 고른놈의 IAM ID와 시크릿키로하기 또 KMS에서
고른 리전도 넣어주기) -> 그 다음으로 파이썬 깔기 sudo apt install python3-pip 그후 pip install aws-encrytion-sdk 쳐주기 -> vi main.py로 파일하나 만들거다.
그다음 그 파일에 https://github.com/aws/aws-encryption-sdk-python 여기있는 내용 붙여줄건데(내가 깃허브에서 찾은거) 각각 내용설명은 영상보기


(메모)
사용자는 암호화된 데이터를 복호화만 시킬수 있고 관리자는 그거 외에 모든 권한을 다 가지고 있다.

## AWS CodeCommit

![image](https://user-images.githubusercontent.com/67897827/190842555-e5020d4f-2e17-4a02-a68f-24e37c713930.png)

깃쓴느것과 AWS CodeCommit쓰는것의 차이는 아무래도 S3에 commit해서 저장해놓면 암호화가 다되니까 또 IAM을 통해서 관리 권한을 세세하게 관리 할 수있다. 

(실습)  

먼저 IAM유저 등록하기 (이름 codecommit_master,프로그래밍방식,기존정책연결하는데 검색에 codecommit쳐서 fullaccess로 -> IAM가서 방금 만든 사용자 들어가면 보안자격증명에서
git부분에 보면 자격증명생성해서 다운로드 해놓기 -> EC2하나 만들고 putty로 연결 후 sudo apt update(업데이트하는거), sudo apt install awscli(cli까는거), aws configure(여 부분은 IAM사용자에대한 ID와 시크릿값, 리전도 넣주기) -> 그다음 codecommit들어가서 레포지토리생성(이름은 firstrepo 후 생성) 만들어지면 옆에 HTTPS 복사해놓기 -> 다시 putty
창으로가서 git clone (아까 HTTPS 붙여놓은거) 이거하면 아마 user name과 패스워드 치라고 나오는데 그때 다운받았던거 쳐주면 된다. 그후에 cd fastrepo로 들어가기 vi test.txt 만들고 안에서 hello라고 치기 후 git config --local user.name "hyuk", git config --local user.email 98taehyukkim@naver.com (이 둘은 어떤 유저가 했는지에대한 증명) -> 후 git add . (현재 디렉토리에 있는 변경된거 codecommit에 추가) 후 git commit -m "test.txt" 후에 git push -u origin 쳐주고 또 다운받은 ID,비번 쳐주면된다.


## AWS CodeDeploy

![image](https://user-images.githubusercontent.com/67897827/190844044-53064617-d1c4-4c3c-b08b-acac31d927e9.png)
배포 자동화 해주는거

만약 코드에 문제점이 생겼다. 그럼 문제점 수정하고 배포하는데 까지 시간이 걸리는데 CodeDeploy사용하면 무정지 즉 실제 서버 사용하는 유저가 꺼지지 않고 다시 배포될수 있는

![image](https://user-images.githubusercontent.com/67897827/190844061-8c1625f5-14cd-4da4-ae40-02733fb587f3.png)

그림에서와 같이 1. Comput platform은 앞선 그림과 같이 EC2,온프레미스,Lambda,ECS통해 배포할거냐 2. Deployment Types&Groups 이건 로드밸런싱 대상그룹처럼 배포그룹이다.
3. IAM & Service Role은 그냥 권한 설정 




