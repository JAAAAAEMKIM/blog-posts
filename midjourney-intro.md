# Midjourney AI

홈페이지: https://www.midjourney.com/home/?callbackUrl=%2Fapp%2F

text 기반 입력을 통해 그림을 그려주는 AI 서비스.
디스코드를 통해 무료로 쉽게 이용이 가능하다.

아래 링크를 통해 discord에 들어갈 수 있다. 
https://discord.gg/midjourney

디스코드 계정이 있으면 바로 사용할 수 있다. 없으면 가입해야한다.

## 예시

명령어를 통해 `여자친구와 제주도 여행을 간 삼성맨`을 그려달라고 했을 때 나온 결과물

![삼성맨](https://user-images.githubusercontent.com/43107046/226108696-9f4e9120-bd9b-4953-a421-90814c2b95d6.png)

꽤나 사람같이 잘 그려주는 것을 볼 수 있다.

## 사용법

가이드 문서
https://docs.midjourney.com/docs/quick-start

1. Midjourney 디스코드에 들어간다.


2. 좌측 목록에서 `newbies-#` 채널로 들어간다.
  ![디스코드 채널 목록](https://user-images.githubusercontent.com/43107046/226108522-96c97d06-b10e-4e31-9191-e6d7f4e823ba.png)
  (채널이 보이지 않는다면 새로고침 해보면 보인다.)

3. `/imagine` 명령어를 입력하고 인자로 이미지에 대한 설명을 입력해준다.
  ex) `/imagine A monkey drinking green milkshake on a yellow car.`
  한글은 잘 안되는 것 같다. 영어로 테스트했을 때는 잘 되었고, 영어가 익숙하지 않다면 `deepL`같은 번역기를 돌려 입력해보자(얘도 AI기반 번역)

4. 필요 시 패러미터를 넘겨줄 수도 있다.
  ex) `/imagine A monkey drinking green milkshake on a yellow car. --q 2 --ar 16:9 --chaos 60`
  패러미터에 따라 같은 인풋에도 완전히 다른 결과가 나올 수 있다.

  > 패러미터 설명
  > --v: 버전
  > --q: 퀄리티. 값이 높을 수록 오래걸리지만 높은 퀄리티의 결과
  > --chaos <0-100>: 값이 높을 수록 예상치 못한 결과가 나올 수 있다.
  > --no: 빼고 싶은 값. ex) 식물이 없는 결과를 원할 때 `--no plants` 
  > 등 기본적인 패러미터들을 주로 사용했다.
  >
  > 완전한 목록은 https://docs.midjourney.com/docs/parameter-list

5. 이미지 variation 또는 upscale
  처음 결과가 나오면 아래와 같이 4컷의 이미지가 생성된다.
  ![결과 예시](https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_DiscordInterface%281%29.png)

  `U1-U4`: 1-4 중 마음에 드는 이미지를 선택해서 upscale 시켜서 좋은 화질의 이미지를 얻을 수 있다.
  `V1-V4`: 1-4 중 마음에 드는 이미지를 기반으로 variation을 준 결과를 다시 4컷 얻을 수 있다.
  새로고침버튼: 새로 결과를 뽑을 수 있다.

  
### 추가) 커스텀 디스코드 서버에서 사용하기

`newbies-#` 채널은 사람들이 많이 사용하다보니 내가 하고싶은걸 시켜두고 다른 걸 보고 있다보면 놓칠 수도 있다. 그럴 때는 자기 자신의 서버에 MidJourney 봇을 초대하여 똑같이 사용할 수 있다.

1. 채팅창에서 MidJourney Bot의 프로필사진을 클릭한다.
![MidJourney Bot 프로필](https://user-images.githubusercontent.com/43107046/226109943-21652118-5b60-435f-bac5-a2326eee5c3f.png)

2. 서버에 추가 버튼을 눌러 내 채널에 추가한다.

3. /imagine을 사용하면 뜨는 권한을 수락 후 사용한다.


AI로 점점 다양한 것을 할 수 있게 되는게 재밌는 것 같다.
다음엔 또 어떤 멋진 AI가 나올지 기대된다.
  


#이미지 생성 AI #그림 AI #midjourney #미드저니 #노블AI #dall-e #이미지 생성 #text-to-image #인공지능 #chatgpt
