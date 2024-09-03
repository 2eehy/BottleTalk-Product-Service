# 프로젝트 소개

BottleTalk는 위스키에 대한 정보를 제공하고, 위스키에 대한 생각을 나눌 수 있는 사용자들의 피드 공간을 제공하는 서비스입니다. 2030 세대를 타겟으로 혼술, 홈술, 하이볼 등을 즐기는 이들을 위한 커뮤니티를 지향하며,**마이크로서비스 아키텍처(MSA)** 기반으로 설계되어 확장성과 유연성을 확보했습니다.


# 주요 기능

사용자 맞춤 인기 검색어와 상품 추천 기능
비 로그인 사용자에게는 공통적인 추천 인기 검색어와 인기 상품 정보를 제공합니다.
로그인한 사용자의 성별과 생년에 따라, 맞춤형 인기 검색어와 인기 상품 정보가 제공됩니다.

크롤링 진행상황 공유를 위한 Slack Notification크롤링 시작, 진행 중, 완료 등 진행 상황을 쉽게 알 수 있도록 Slack 알림이 구현되어 있습니다.
ElastiCache를 사용한 사용자 정보 캐싱로그인한 사용자의 성별 및 생년을 캐싱하여 사용자 활동 로그에 추가됩니다.

# 기술 스택
<img width="1067" alt="image" src="https://github.com/user-attachments/assets/34c52ace-0d63-4955-b765-df3cff1a497a">

# 시스템 아키텍처
<img width="1080" alt="image" src="https://github.com/user-attachments/assets/c883c87e-f7f0-45cd-ad56-e01707a906e5">

# 데이터 ETL 파이프라인
<img width="753" alt="image" src="https://github.com/user-attachments/assets/3baa27b2-fb24-4e7a-b80f-26b2d2837d28">

# 데이터 크롤링 아키텍처
<img width="563" alt="image" src="https://github.com/user-attachments/assets/cc2563fc-7eff-4cb9-9107-e8b7df10b03d">

# WBS
<img width="483" alt="image" src="https://github.com/user-attachments/assets/f7b2f888-cebf-4fba-b9c8-10022a664367">
<br>효율적 의사소통과 지속적인 개선을 위해 노션의 칸반보드를 활용하여 테스크단위로 업무를 진행 (해야할일, 진행중, 완료 단계로 구분)

# 담당 업무
- **Java High Level REST Client**를 활용한 **ElasticSearch** 연결 및 **ProductAPI** 개발,
- **AWS Cognito**와 **Google Cloud API**를 활용한 **로그인과 회원가입** 구현,
- **Docker**를 활용한 공용 ElasticSearch와 Kibana 인스턴스 구현및 배포,
- **Jasypt**를 활용한 환경변수 암호화
- **AWS Amplify**를 사용한 로그인 구성 AWS Cognito 사용 **간소화**
- 서비스별 **Logstash Agent** 구성
- **Github Actions** 를 이용한 **yml** 파일 자동 생성

  
# Installation


1. 로컬 환경에서 실행시 resources 에 secrets.yml 파일 추가 필요(카카오톡 공유)
   <br>logback-spring.xml 의 11-12 라인 확인후 local-path 로 변경필요


2. 서버에서 실행시 
```
gradle bootRun --args='--spring.profiles.active=dev' 
혹은

java -jar your-application.jar --spring.profiles.active=dev 
으로 실행
```

3. 인스턴스에 logstash 설치후 
logstash.conf 확인 필요
sudo vi /etc/logstash/conf.d/logstash.conf

```

input {
    file {
        path => "/home/ec2-user/product-logs/application-*.log"
        start_position => "beginning"
        codec => "json"

    }
}

filter {
    json {
        source => "message"
        remove_field => "message"
    }

}

output {
        elasticsearch {
                hosts => "null"
                user => "null"
                password => "null"
                index => "null"
        }

        stdout {
          id => "logstash"
          codec => rubydebug
        }
}

```


5. api test

   | 메서드 | URL                               | 설명             |
   |-----|-----------------------------------|----------------|
   | GET | /v1/api/products                  | 전체 위스키정보를 조회   |
   | GET | /v1/api/products/{id}             | 특정 위스키정보를 조회   |
   | GET | /v1/api/products?search={keyword} | 키워드로 위스키목록을 조회 |
   | GET | /v1/api/products/top5Keywords     | 인기검색어 TOP5 조회  |
   | GET | /v1/api/products/top5Products     | 인기삼풍 TOP5 조회   |





