CloudWatch  
• AWS 클라우드 리소스와 AWS에서 실행되는 애플리케이션을 위한 모니터링 서비스  
• 지표를 수집 및 추적하고 로그 파일을 수집 및 모니터링하고 경보를 설정  
• Amazon EC2 인스턴스, Amazon DynamoDB 테이블, Amazon RDS DB 인스턴스 같은 AWS 리소스뿐만 아
니라 애플리케이션과 서비스에서 생성된 사용자 정의 지표 및 애플리케이션에서 생성된 모든 로그 파일을모니터링  
• 시스템 전반의 리소스 사용률, 애플리케이션 성능, 운영 상태를 파악  

CloudWatch  
지표(Metrics)  
• AWS 클라우드 리소스 및 AWS에서 실행하는 애플리케이션을 모니터링  
• CPU사용량, 네트워크 사용량 등의 AWS 서비스에 대한 측정값  
• AWS 제품 및 서비스에 대한 지표가 자동으로 제공되며 자체 애플리케이션 및 서비스에서 생성된 사용자 정의 지표도모니터링  
대시보드(Dashboard)  
• AWS 리소스 및 사용자 정의 지표의 그래프를 한눈에 볼 수 있는 대시보드 기능  
로그(Logs)  
• 애플리케이션에 대한 로그를 수집하는 기능  
• Lambda, CloudTrail, ECS, API Gateway 등의 AWS서비스에 대한 로그를 수집  
• AWS서비스 이외에도 Log Agent를 설치하여 로그를 수집 가능  
• 로그를 S3, Kinesis Data Stream, Kinesis Data Firehose, AWS Lambda로 전송 가능  
경보(Alarms)  
• 지표값에 대한 알림을 생성하는 기능  
• 예, Amazon EC2 인스턴스 CPU 사용률, Amazon ELB 요청 지연 시간, Amazon DynamoDB 테이블 처리량, Amazon SQS 대기열 길이, AWS 청구서 요금에 대한 알림  
• 생성된 알림을 이메일을 전송하거나, SQS 대기열에 게시하거나, Amazon EC2 인스턴스를 중단 또는 종료하거나, Auto Scaling 정책을 실행하도록!!!! 경보를 설정  

