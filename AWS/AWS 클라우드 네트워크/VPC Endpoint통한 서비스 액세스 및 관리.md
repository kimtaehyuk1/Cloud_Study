## 아키텍처 이미지

![endpoint](https://user-images.githubusercontent.com/67897827/160245504-0800a7c5-a207-4591-a6cc-3697bd7a4eca.PNG)

* Endpoint를 사용하면 vpc외부로 트래픽이 이동하지 않더라도 private영역에서 vpc외부에있는 AWS 서비스와 직접 연결해 그 서비스를 사용 할 수 있다.
* 이렇게 되면 트래픽의 효율이나 특히 보안상의 이점을 가짐.

## 실습 단계

1. 실습을 하기 앞에 IAM Role 이나 S3 버켓을 환경 구성하기
2. Private EC2의 S3 접속을 위한 Gateway endpoint 생성
3. Private EC2의 CloudFormation 접속을 위한 Interface endpoint 생성 및 커맨드로 CloudFormation 사용하기
