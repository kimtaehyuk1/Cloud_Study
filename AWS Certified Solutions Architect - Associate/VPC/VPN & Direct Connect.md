![image](https://user-images.githubusercontent.com/67897827/182573844-15b7a83c-3ec6-403f-b655-48f9cef2b33c.png)

![image](https://user-images.githubusercontent.com/67897827/182574155-a89a9f03-bd73-432e-9f2c-6a6b89b61277.png)


![image](https://user-images.githubusercontent.com/67897827/182574680-dcda804d-b8df-401b-be83-9bf94e7c2880.png)

Customer Gateway는 실제 물리적인 게이트웨이가 아니라 고객의 물리적인 라우터정보를 AWS콘솔에서 입력해주는 것이다.

![image](https://user-images.githubusercontent.com/67897827/182575100-37a59f7c-4ce4-4881-8581-80f1f7fcafd3.png)

VPN은 인터넷연결을 통해 프라이빗 네트워크를 설정해주는가 반면에 DX는 AWS에서 제공하는 전용선으로 프라이빗 네트워크를 구성하는것이다.

![image](https://user-images.githubusercontent.com/67897827/182575394-33ff008f-7a14-4f4e-99df-61fc10a678ea.png)

![image](https://user-images.githubusercontent.com/67897827/182575574-48164321-16b7-4266-96f4-f80fde94f087.png)


![image](https://user-images.githubusercontent.com/67897827/182576791-0e0fed15-dd61-4475-98bf-3ef029979a7c.png)

EC2에서 아웃바운드 트래픽은 유료이고, 인터넷에서 EC2로의 접속은 무료이다. EC2에서 ELB로 나가는 비용은 같은 가용영역인 경우에 무료이고 ELB에서 EC2로 들어오는것도 무료이다.
ELB에서 인터넷으로 나가고 들어오는 비용은 유료이다. 인터넷에서 DX로 들어오는건 무료이고, DX에서 인터넷으로 나가는 비용은 유료이다. S3같은 퍼블릭 서비스에서 
CloudFront로 나가는 비용은 무료이고, CloudFront에서 퍼블릭 서비스로 오는건 유료이다. 또 외부에서 CloudFront로 접속하는 비용은 무료이고, CloudFront에서 외부로 나가는
비용은 유료이다.

!!!!!!따라서 S3버킷에서 CloudFront로 컨텐츠를 캐싱하고 외부에서 CloudFront로 접속하게 되면 직접 S3버킷으로 접속하는 것보다 비용 절감된다.!!!!!!!!!!!1
