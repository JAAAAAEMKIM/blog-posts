# 토이 프로젝트용 API 서버 올리기

## 목적
FE 토이프로젝트를 위해 여러가지를 해보고 싶은데, API 서버가 있으면 좋을 것 같은 상황이 많았다.
  - ex) react-query 등 서버 state를 관리해주는 라이브러리들을 실제로 써보기

이럴 때 Mock을 써도 되지만, 직접 리모트에 올려보며 FE 프로젝트 운영과 관련된 작업들도 배울 수 있다고 생각했다.
- ex) 도커, EC2, Nginx 등

그러기 위해 간단히 이것저것 집어넣을 수 있는 BE 서버를 올려보기로 했다.

## 사용 스택

1. loopback4
   API 앱을 쉽게 만들 수 있고 Typescript를 지원한다.
   예전에 튜토리얼로 만들어둔 앱이 있기 때문에 그걸 조금 수정만 해서 사용하기로 했다.

2. MongoDB
   Document 기반 NoSQL의 대표적인 예
   다루기 편해서 선택했다.

3. AWS EC2
   가장 기본적이고 대표적인 AWS 인스턴스이고,
   AWS Free tier에서도 월 750시간을 사용할 수 있기 때문에 선택했다.

4. 도커
   컨테이너 방식의 이점을 위해

## 과정
1. Loopback4 어플리케이션 만들기
2. MongoDB 설치 및 연결하기
3. EC2 인스턴스 만들기
4. EC2에 MongoDB 및 앱 설치
5. 실행해보기
6. nginx 설치 및 실행


### 1. Loopback4 어플리케이션 만들기

Loopback4 공식 문서의 todo 예제를 따라 만들었다.
https://loopback.io/doc/en/lb4/todo-tutorial.html

혹시나 샘플앱을 EC2에 올리는걸 해보고 싶다면 위의 튜토리얼이 만들어져있는 리포지토리를 클론해서 진행해보면 된다.

### 2. MongoDB 설치 및 연결하기

MongoDB 공식 문서를 참고하여 설치해준다.
https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-os-x/

로컬에서 실행은 다음 커맨드를 사용한다.

```shell
brew services start mongodb-community@6.0
```
기본적으로 27017 포트에서 해당 프로세스가 실행된다.
포트를 확인해보면 실행중인 것을 알 수 있다.

MongoDB Compass라는 GUI를 제공하기 때문에 편하게 접속해볼 수 있다.

![MongoDB Compass](https://user-images.githubusercontent.com/43107046/222969315-123c5b1a-348f-4936-9851-0599e3f4bf2c.png)

정지는 다음 커맨드를 사용한다.
```sh
brew services stop mongodb-community@6.0
```

MongoDB가 설치되었으니 Loopback 앱과 연결시켜준다.
기존 튜토리얼에서는 인메모리 DB를 사용했었다. MongoDB와 연결을 위해서는 datasource 추가가 필요하다.

loopback4는 친절히 MongoDB 연결에 대한 문서도 제공한다.
https://loopback.io/doc/en/lb4/Connecting-to-MongoDB.html

먼저 커넥터를 설치해준다.

```sh
npm i loopback-connector-mongodb
```

그 후 차근차근 아래와 같이 진행하면 새로운 datasource가 만들어진다.

```sh
$ lb4 datasource
? Datasource name: mongo
? Select the connector for db:
  ...
  Redis key-value connector (supported by StrongLoop)
❯ MongoDB (supported by StrongLoop)
  MySQL (supported by StrongLoop)
  ...
? Connection String url to override other settings (eg: mongodb://username:passw
ord@hostname:port/database):
? host: localhost
? port: 27017
? user:
? password: [hidden]
? database: demo
? Feature supported by MongoDB v3.1.0 and above: Yes

Datasource Db was created in src/datasources/
```

Repository에서 새로운 데이터 소스를 사용하도록 DBDataSource를 MongoDataSource로 변경해준다. (!!재확인하기)

이제 yarn start를 해보면 MongoDB와 연결되어 실행되는 것을 알 수 있다.
post를 통해 데이터가 제대로 들어가는지 테스트 후 MongoDB Compass에서 확인할 수 있다.


### 3. EC2 인스턴스 만들기

EC2 인스턴스를 만들고 앱을 배포한다.
VPC, security group 등 생소한 내용들이 많은데 설명이 잘 되어있는 곳들이 많기 때문에 참고하여 진행했다.

토이용이 아니었다면, 각각 서비스를 분리했을텐데 EC2의 프리티어로는 한 개에 다 넣고 돌려야 추가 요금이 발생하지 않기 때문에 하나만 생성해주었다. 여력이 되면 여기는 개선해볼 예정이다.

### 4. EC2에 MongoDB 및 앱 설치

로컬에서처럼 여기도 mongodb를 설치해준다. - linux에 설치하기 참고

BE 앱도 클론해와서 build 해준다.
빌드 결과만 가져오도록 개선할 예정인데, 일단 돌아가게 하는게 목표라 이런 방식을 선택했다.

### 4. 실행해보기

mongod를 systemctl로 실행해준다.
실행된 후 BE앱을 실행시켜서 connection을 만들어줘야한다.

docker환경에서 실행되기 때문에 이를 맞춰줘야했다.

### 5. nginx 설치 및 실행

Nginx를 통해서 http 포트로 들어온 요청을 3000으로 넘겨준다. 다른 포트는 외부로 열리지 않게 되어있다.

/api 경로를 받기로 했다.

그럼 완성!



## 추가 개발 계획

https로 연결하도록 하기
배포 프로세스 자동화 - git pull로 당기고 있는데 깃헙액션같은걸 쓰는 방식으로 바꾸도록 실험 중


## 마무리

하기전에는 과정을 다 안다고 생각하고 금방할 줄 알았다.
하지만 역시나 항상 그렇듯이 세세한 곳들에서 시행착오를 겪으며 생각보다 오래걸렸다.
개념만 익히기보단 직접해보는게 중요한 것 같다.

이 토이앱을 기반으로 여러가지 FE 프로젝트를 진행해봐야겠다.





태그: #ec2 #토이프로젝트 #BE 튜토리얼 #API 서버 배포하기 #loopback4 #mongodb #개발 공부 