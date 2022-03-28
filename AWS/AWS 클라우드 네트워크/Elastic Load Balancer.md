## Elastic Load Balancer를 통한 이중화 네트워크 구성

## 아키텍처 다이어그램
![load balancer](https://user-images.githubusercontent.com/67897827/160380960-cd26ae42-07cb-457d-a28a-93334c9e5151.PNG)

## 과정
(1) 실습 환경 구성
* Subnet(public-subnet-a2)생성
* EC2생성
* Security group rule 추가(HTTP 트래픽 inbound허용)
* 아파치 웹 서버 설치
* HTTP 파일 생성

(2) Target group 생성 및 Application Load Balancer구성
* Target group 생성
* Application Load Balancer구성
* 웹 브라우저에서 Load Balancer 테스트

(3) Application Load Balancer의 경로기반 라우팅 구성
* HTML파일 추가 생성
* Target group 생성
* Listener 업데이트
* 웹 브라우저에서 Load Balancer테스트

(4) Target group 생성 및 Network Load Balancer 구성
* Target group 생성
* Network Load Balancer 구성
* 웹 브라우저에서 Load Balancer 테스트
