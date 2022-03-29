## VPC Peering
* VPC Peering을 이용하여 동일 Account 및 Region 내에 위치한 2개의 서로 다른 VPC를 연결하고 통신이 가능한 네트워크를 구성해 본다.

![피어링](https://user-images.githubusercontent.com/67897827/160443187-13ffdf9e-6ea9-4da4-b821-77c6335da7d8.PNG)

(1) 실습환경 구성(추가 VPC 네트워크 생성)
* VPC 생성
* Internet Gateway 생성
* Subnet 생성
* Route table 생성
* EC2 생성(private-ec2-a,private-ec2-b Security group ICMP 트래픽 Inbound허용하돌고 업데이트

(2) Peering connection 및 Route table 구성
* Peering connection 생성
* 각각 서브넷에서 다른 서브넷 경로 업데이트
