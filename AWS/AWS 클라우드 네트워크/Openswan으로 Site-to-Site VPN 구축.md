## 목표
Openswan과 Site-to-Site VPN을 이용하여 가상의 외부 네트워크와 AWS 네트워크를 연결함으로써 더욱 확장된 네트워크를 구축한다.

## 아키텍처 다이어그램

![VPN](https://user-images.githubusercontent.com/67897827/160837599-fca2bd19-8558-4191-b8c5-7a8b08cca70a.jpg)

## 실습 과정
* 외부 네트워크로 가정하는 도쿄 리전에 VPC및 내부 구성요소들 생성
* vpc-d에 Customer Gateway 생성 (서울리전에서 Customer Gateway생성이 진행되야됨)
* vpc-a에 Virtual Private Gateway 생성 (서울리전에서 Customer Gateway생성이 진행되야됨)
* VPN Connection(vpc-ad-vpn) 생성
* EC2(public-ec2-d)에 Openswan설치
* VPN Configuration(Tunnel,PSK등) 설정
* Route table Route 업데이트
* VPN 테스트
