![image](https://user-images.githubusercontent.com/67897827/182187544-0484ac98-0b75-4877-9085-c3e380eeb616.png)

![image](https://user-images.githubusercontent.com/67897827/182187841-b8d1b3c9-f44e-40c0-91b9-375412a9816c.png)

Consumer가 SQS한테 메시지 요청하면 그러며 SQS가 메시지를 보내주고, 그 후 컨슈머가 메시지 다 처리했다고 응답하면 그 뒤에 SQS큐에서 메시지 삭제됨  

![image](https://user-images.githubusercontent.com/67897827/182188239-50fa1181-7215-48ab-800d-986eb72156ff.png)

일반적으로 SQS는 컨슈머가 소비했다고 응답올때까지 계속 메시지 보냄

![image](https://user-images.githubusercontent.com/67897827/182189533-dbeb82b5-4247-46a4-8154-24ecb5bccbe4.png)

(실습)  
SQS 서비스에서 대기열 생성 -> 표준대기열,이름,구성은일단냅두고,액세스정책은 기본, (배달못한편지대기열 수신수 지정할수있다), 생성

동작을 확인하기위해 오른쪽 상단에 메시지 전송 및 수신 클릭 -> 메시지 본문 입력해서 메시지전송 눌르면 메시지가 Queue에 올라가게 된다. 그러면 수신에서 메시지 폴링하면(이것은
컨시머 애플리케이션이 이 메시지를 요청하는 동작임)
