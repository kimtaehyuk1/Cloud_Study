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

