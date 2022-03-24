
![VPC](https://user-images.githubusercontent.com/67897827/159907582-75a96777-e9b3-4e80-82eb-e0ffa3c5c4e7.PNG)

## 순서
* 1. Custom VPC를 만든다
* 2. 그 안에 서브넷을 생성한다.
* 3. 서브넷안에 EC2 인스턴스를 만든다.
* 4. 외부와 통신 가능한지 테스트
* 5. 인터넷 게이트웨이와 라우트 테이블 생성 후 라우트 정보 업데이트해 EC2 인스턴스에 접근

**참고로 publick-subnet-a의 IPv4 CDIR block은 vpc의 IPv4 CDIR block보다 안으로 들어와야 된다.**
**3.까지하면 통신은 안된다 그냥 ec2가 놓여질 수 있는 환경만 만든거지 이거와 통신할 수 있는 경로는 만들지 않았다.**
