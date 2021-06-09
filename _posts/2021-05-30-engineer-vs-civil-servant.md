---
title: "개발자와 공무원"
excerpt: "CentOS 7에서 Selenium 설정 및 발생 가능 에러 정리"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/install-selenium-on-centos.png
categories:
  - Development 
tags:
  - Development-Skills
last_modified_at: 2020-12-07T08:06:00-05:00
published: false
---

## 요즘 상황을 보며 드는 위화감

비전공자의 개발자 전향에 대하여
요즘은 대부분 장밋빛 미래를 말한다.

나도 체감을 하는 것이
구글링을 하다 보이는 개발 학원 광고글이 많아졌고
개발자 전향에 대한 의견을 묻는 지인들도 늘었다.

특히 개발관련 학원 광고문구는 정말 달콤하다.
저 과정을 수료만 하면
- 수요가 많아 취업이 잘 될것 같고
- 전문직이라 연봉도 괜찮을것 같다

근데 이와 비슷한 경험
뭔가 수능볼 때도 겪었던 것 같기도 하고 그렇다.
이 학원의 이 강사 강의만 들으면
연고대도 갈 수 있을것 같은 기분이 들던...


## 이 글을 읽지 않아도 되는 사람
개발 자체에 흥미가 있는 사람은
이 글을 읽지 않아도 된다.

사람이란게 결국 가슴이 시키는 일을 해야 하는것 같다.
개발이 끌리고 해보고 싶어 혹은 개발에 적성을 찾은 것 같다 그러면
해야지 다른 방법이 없다.

혹은 소거법으로 개발자의 길을 고른 사람들도 
이 글을 읽지 않아도 된다.

소거법은 나의 경우로,
원래 전공이었던 외국어에 별 흥미가 없었고
경영,경제는 팀플 많아 싫어서 
외국어 회화와 팀플 없이 오로지 컴퓨터만 바라보는 이 직업을 골랐다.


## 비전공에서 개발 배우면 정말 취업이 잘되나?

개발자 채용이 늘어난 것은 팩트이다.
거의 모든 기업은 자사의 웹이나 앱을 가지고 있고 
농업 조차도 IT와 시너지를 내려 하고 있다.
또한 빅데이터, 인공지능은 이제 태동하는 시기이기도 하다.

"농부심-농산물 데이터 시각화 서비스"
- 인증서 업데이트좀..들어갈때마다 warning뜸..
https://www.nongbushim.com/PriceSearch.html

이것은 마치
과거 산업혁명으로 농업 인구가 줄고
공장 일자리가 늘어난 것과 비슷한 상황이다.
"이전 인터뷰 참고글"

일자리가 늘어난 것은 맞지만
비전공 개발자의 일자리 상황은 괜찮지 않은 것이 문제다.

직접 구직 활동을 해보지않더라도
채용사이트 서칭으로도 알 수 있다.
스타트업 채용 사이트인 "원티드"에서 보면 
다른 업종에 비해 개발자 일자리가 훨씬 많다.

그런데 "신입"은 찾아보기 힘들다.
이전 글에도 언급했듯
늘어난 일자리는 "경력직 개발자" 자리이다.

학원광고에 달리는 초봉4천이 넘는 경우는
대기업 신입이거나 경력직을 말하는 것이다.

그냥 신입 취업도 힘든데
비전공자 신입의 상황은 어떻겠는가?

취업을 하려면 경력이 쌓일 때까지 
실수령액 월 200만원이 안되는 연봉을 감수해야 한다.
(연봉으로는 2천 중반 정도)
또한 이러한 일자리도 
대부분 수도권에 몰려있다.

적은 연봉에 정년보장이 된 일자리도 아닌데
수도권 생활비를 감당하며 버티는 것은 쉬운 일이 아니다.

일본취업을 목표로 하는 경우는 사정이 그나마 낫다.
일본은 그래도 신입을 뽑는다.
그럼에도 월200이 안되는 급여와 
도쿄, 오사카에 몰려있는 일자리로
생활비 감당이 힘든 것은 똑같다.


## 이 꽉깨물고 경력자가 되기 까지...
"약속의 9회 사진"

약속의 9회...는 아니고
약속의 3년차라는 말이 있다.

3년 정도 경력이 쌓이면
그동안 안 보이던 코드구조들도 보이기 시작하고
이직을 하면 연봉도 많이 뛰는 시기라는 말이다.

나도 비전공으로 커리어를 시작할때
약속의 3년차를 믿었다.

근데 이 3년이라는 시간이
생각보다 힘들다.

개발일이 사무직과 제일 다른 점은
오롯이 혼자서 해 나가야한다는 말이다.

마치 처음 영어 에세이를 쓰는 것과 같다.
선배들에게 두괄식으로 쓰라는 조언을 받았다고 하더라도
직접 에세이를 쓰는 것은 나 자신이다.
문제는 이제 막 알파벳을 뗐는데
두괄식은 차치하더라도 문법에 맞게 적는 것조차 쉽지 않다는 것이다.
(개발이라는게 점 하나 잘못 찍어도 에러가 나지 않는가?)

따라서 실력 부족으로 매일 야근을 해야 하고
퇴근해서도 개인공부를 해야 한다.

쉬는 시간 없이 3년을 오롯이 갈아넣다보면
좋아했던 개발도 싫어지는 순간이 온다.

또한 간절히 본전 생각이 난다.
"이 시간에 다른 일을 했다면 뭘 하든 성공했겠다"

이게 나만 힘든 줄 알았는데 또 아니다.
처음 30명 정도 시작한 동기들은
3년 정도 지나니 5명 정도 남아있었다.

## 경력자가 되고 나면 조금은 나아질까?
단언컨데 아니라고 할 수 있다.

3년 정도 실력이 쌓이면 
이제 좀 할만하지 않을까 싶지만

1년차에는 1년차 수준의 어려운 버그들이
3년차에는 3년차 수준의 어려운 버그들이 
기다리고있다.

후배들이 해결못한 문제들이
나에게 들어온다.
실력이 쌓일수록 마주치는 문제들의 수준도 같이 쌓인다.

결론은 경력이 쌓여도
삶의 많은 시간들을 오롯이 갈아넣어야 하는 상황은 
나아지지 않는다.

10년차 이상은 어떨까?
회사에서 선배들을 보건대 더 힘들다고 할 수 있다.

후배들이 풀지못한 여러 문제들이 올라오는 것과 더불어
관리직도 겸해야 한다.
관리직이라는 것은 문제가 생기면 책임도 져야 한다는 것을 뜻한다.

1N년차 선배가 말한 썰이 아직도 생각이 난다.
"야근하고 퇴근해서, 맥주잔 손에 들고 버그를 해결하다가
너무 피곤해서 그대로 잠들었는데
정신을 차리고보니 손에 맥주잔이 그대로 들려있더라"

이쯤되면 들이는 시간대비 연봉이 적다는 생각이 든다.


## 그럼 문과는 뭘로 먹고살라는 말이냐?
나의 좁은 식견으로 보건대
가까운 미래에 남는 직업군은 3개이다.

- 전문직
- 공무원
- 개인사업자(유튜브 등도 포함)

인공지능의 발전으로 사무직은 줄어들 것이고
서비스직조차도 위협받을 것이다.

- 과거 엑셀의 등장으로 얼마나 많은 사무직 인원이 줄어들었는가?
- 최근 맥도날드만가도 키오스크가 생기고나서 매장직원이 늘었는가 줄었는가?

그런데...
전문직과 사업자는 그렇다치더라도
공무원이야말로 사무직의 끝판왕아닌가?

공무원의 직위는 헌법으로 보장되어있다.
따라서 이미 채용된 공무원을 해고하려면 많은 제약이 따르기때문에
기계로 대체된다 하더라도
채용 인원을 줄이는 방식으로 인원을 조정할 가능성이 크다.
실제로 초등학교 교사를 보더라도
이미 채용된 인원을 자르지 않고
교대 정원을 줄이고 있다.

또한 사무직 공무원 이외에
나라의 기반과 관련된 공무원(경찰,소방,복지 등)은
기계로 대체하기도 어렵고 민영화하기도 어렵다.

전기,수도,가스가 민영화된 일본을 보면 안다.
한달 전기료 만원 안넘는 내가
일본가서 한달 2~3천엔 정도의 전기료가 나온다.
(애초에 기본료가 900엔 정도)

따라서 문과가 먹고살 길을 현실적으로 이야기하자면
특정 분야의 전문인이 되거나
개인 사업을 하거나
아니면 이악물고 공직사회에 진입하는 방법이 있다고 할 수 있다.


분량조절 실패로 다음글에 이어적겠습니다.


## ----------------

## title : 공무원vs개발자, 개발자는 전문직인가? 

이전글에 이어진다 
### 이전글 링크 

## 개발자는 전문직인가?
이전 글에 미래는
사업, 전문직, 공무원 이라고 했다.
** 이전글 읽기

그럼 개발자는 전문직인가?
감히 말하건대 대부분은 아니다.
전문직의 조건
- 시험 등을 통과해서 공식적으로 보장되는 지위를 가진 사람
- 한 분야에 뛰어난 능력을 가진 사람
둘 중 하나 이상 만족하면 난 전문가라고 생각한다.
의사는 두개를 다 만족했고.

개발자는 첫번째 조건을 만족하지 못한다.
공식적인 시험이 없고 걍 난 개발자야 하면 개발자다.

일반 개발자는 왜 미래에 힘들까?
얼마전 마소에서 만든 그 코딩해주는 프로그램
즉, 어느 기능 자체를 만드는 것은 기술적으로 가능한 일이며,
설계서대로 만들기만 해서는 기계가 다한다.
즉 설계서를 만드는 일까지를 잘 해야 전문직이 될 것이다.


그럼 뛰어난 능력을 가진 사람이어야 하는데 이게 어렵다.
개발은 쌓아가는 그런 것이 아니다.
난 의사를 모르지만
사람 몸의 구조는 바뀌지 않아.
의사로 치면 몸이 조금씩 바뀐다.
얼마전엔 심장이 여기 있었는데 이젠 다른 곳에 있고
지배적인 플랫폼(ex 스마트폰)이 나오면
아예 몸이 다른 것으로 바뀌는 셈이다.

그래서 개발자들은 퇴근후에 자기개발을 한다.
이건 플러스 알파가 아니라
런닝머신 위에서 뒤로 안가기 위한 싸움이다.

플러스 알파가 되려면 더 열심히 해야한다.
젊어서는 퇴근해도 자기개발하고 그런데,
나이들고 가정이 생기고 그러면? 
너의 삶도 챙겨야하지않겠니?

모든 연령대에서 지금 막 개발 전환할 때처럼
아침부터 저녁까지 개발에 몰두할 시간이 되는 것은 아니다.


## 들이는 노력 대비 공무원, 다시 공무원 눈을 ...
공무원 얘기를 하면 
나라 미래가 어둡다느니 그런 이야기 달린다.
100% 맞는 말이지만 현실적으로 생각해보니 이런 결론이다.

전문직 개발자가 되는 시간과 비용을 
공무원 시험에 투자해서 공무원이 되면 
많은 것을 보장받는다.
- 정년까지 먹고살 수 있음.


공무원 변호사의 것을 봐라
공시난이도는 수능보다 높지만,  기출만 보면 커버가 가능한. 100점 맞는 시험이 아니다.
(일반행정 기준 점점 그것도 아니다)


기출 범위를 넘어서면 그야말로 헬게이트
그러나 아직 전산직은 그런 것은 아니다.

또한 
그리고 은퇴 세대이긴 해. 정부 정책도 그렇긴 하지만
확실히 경쟁률이 준 것은 팩트다.
최근 이전 세대의 은퇴 시기와 맞물려
경쟁률이 떨어지고 있는 추세이다.

정말 우연찮게도
마지막 차가 왔다는 느낌을 지울 수 없다.


## 현직자는 어떻게 생각하는가?
okky에 올라온 글
대부분 공무원이다. 현직자조차도 이렇게 생각.

그리고 심지어는 경력직 공무원이라고
개발일을 하다가 공무원으로 전향할 수도 있다.

**사이트 첨부



## 결론 
나중에 풀긴 하겠지만,
본인이 수능 점수를 어느정도 잘 받았다면
난 공무원이 더 나아보인다.
그리고 개발자 하려는 이유가 단지 취업이 더 잘 되 보인다 라면.
되기 물론 어렵지? 근데 들이는 노력은 차이가 없어.

아무 곳이나 들어갈 것이냐, 좀더 시간을 써서 들어갈 것이냐.
총량의 들인 노력은 거의 비슷할 것이다.




## 개요
[이전 포스팅](https://donggyuu.github.io/development/like-click-bot)에서는 Local에서 셀레니움을 이용해 간단한 봇을 만들어보았습니다. 봇을 서버에 올려 정해진 시간에 동작하도록 하면 수동으로 실행하는 것보다 간단하겠죠.   
본 포스팅에서는 CentOS7서버에서 셀레니움 봇을 동작하도록 하는 설정을 하겠습니다.

## Chrome headless
리눅스 상에서 동작할 것이므로 브라우저의 화면이 보일 필요가 없습니다. Chrome headless는 브라우저 화면의 표시 없이 브라우저를 실행하도록 해줍니다.   
headless의 설정을 위해 [아래의 코드](https://github.com/donggyuu/like-click-bot/blob/master/like-click-bot.py#L70)를 추가했습니다.   
```python
options = webdriver.ChromeOptions()
# 브라우저 창 안 띄우겠다
options.add_argument('headless')
# 보통의 FHD화면을 가정
options.add_argument('window-size=1920x1080')
# gpu를 사용하지 않도록 설정
options.add_argument("disable-gpu")
# headless탐지 방지를 위해 UA를 임의로 설정
options.add_argument("user-agent=Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6)")
```
특정 서비스는 접속 로그의 UA를 보고 사람인지 로봇인지를 판단합니다. 따라서 로봇으로 인식되지 않도록 임의의 UA도 설정해 주었습니다. 

## CentOS 설정
미리 준비해둔 CentOS서버에 접속합니다. 저는 Lightsail를 사용하므로 Lightsail에 접속하겠습니다.
```bash
# 권한으로 접속이 안되면 sudo를 붙여본다
ssh -i /Users/don/aws_keypair_seoul.pem centos@xx.xx.xx.xx
```

CentOS7에 python2는 이미 설치되어 있지만 대세를 따라  python3을 사용하겠습니다.  
python3의 설치는 yum을 사용하는 방법, 직접 다운로드하는 방법이 있습니다. 개인적으로 패키지 관리툴의 사용을 선호하므로 yum으로 설치하겠습니다([이곳](https://www.liquidweb.com/kb/how-to-install-python-3-on-centos-7/)을 참고하였습니다). 다만 현 시점에서 [yum은 python3.6까지만 지원](https://stackoverflow.com/questions/50408941/recommended-way-to-install-pip3-on-centos7)한다는 단점이 있습니다.   

```bash
# python2 is already installed on CentOS7
$ python --version
-----------------------
Python 2.7.5

# install python3
sudo yum update -y
sudo yum install -y python3

# confirmation
$ python3 --version
-----------------------
Python 3.6.8
```

[pip은 python3.6부터는 같이 포함](https://linuxize.com/post/how-to-install-pip-on-centos-7)되어있기 때문에 따로 설치하지 않아도 됩니다. 
pip을 사용하여 Selenium을 설치합니다. 명령어 "python3.6 -m pip"을 사용하겠습니다. 

```bash
sudo python3.6 -m pip install selenium
--------------------------------------------
Successfully installed selenium-3.141.0 urllib3-1.26.2
```
[이전 포스팅](https://donggyuu.github.io/development/like-click-bot)에서 만든 봇은 yml에서 설정값을 읽어옵니다. 따라서 yaml관련 라이브러리도 설치해줍니다. 
```bash
sudo python3.6 -m pip install pyyaml
--------------------------------------------
Successfully installed pyyaml-5.3.1
```

이제 봇을 위치시킬 경로를 만들어줍니다.
```bash
mkdir -pv /home/centos/app/like-click-bot
--------------------------------------------
mkdir: created directory ‘/home/centos/app/like-click-bot’
```

## Selenium bot 실행
로컬에 다운로드해둔 봇을 CentOS7서버로 업로드합니다(아래는 Lightsail을 사용하는 경우의 scp 명령어).   
```bash
sudo scp -i /Users/don/aws_keypair_seoul.pem -r /Users/don/like-click-bot centos@xx.xx.xx.x:/home/centos/app/
```

CentOS에서 봇이 잘 업로드 되었음을 확인합니다.  
```bash
# file check
$ pwd; ll
/home/centos/app/like-click-bot
total 16332
-rwxr-xr-x. 1 centos centos 14057472 Dec  7 09:07 chromedriver
-rw-r--r--. 1 centos centos       76 Dec  7 09:06 config.yml
-rw-r--r--. 1 centos centos     3129 Dec  7 09:06 like-click-bot.py
-rw-r--r--. 1 centos centos      135 Dec  7 09:06 README.md
```

최종적으로 Selenium을 사용한 봇을 실행시켜봅니다. 
```bash
python3 like-click-bot.py
```

## 에러 핸들링  
진행하다보면 2가지 에러가 나올 수 있습니다. 아래의 조치를 취한 후 재실행해 봅시다.  

### case_1
```bash
... 생략 ...
OSError: [Errno 8] Exec format error
```
[위의 에러](https://stackoverflow.com/questions/38833589/oserror-errno-8-exec-format-error-selenium)는 운영체제와 다른 chromedriver를 사용했을때 발생합니다. [여기](https://sites.google.com/a/chromium.org/chromedriver/downloads)에서 Linux용으로 다운받아 사용하면 됩니다.

### case_2
```bash
... 생략 ...
selenium.common.exceptions.WebDriverException: Message: Service ./chromedriver unexpectedly exited. Status code was: 127
```
위의 에러는 크롬이 설치되어있지 않을때 발생하는 에러입니다. chromedriver를 사용하기 위해서는 당연하게 Chrome도 필요합니다([참고](https://blog.miyam.net/104)).  
아래와 같이 yum으로 크롬 브라우저를 설치해줍니다.
```bash
# 환경 설정
sudo vi /etc/yum.repos.d/google-chrome.repo
-----------------------------------
[google-chrome] 
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64
enabled=1
gpgcheck=0
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
-----------------------------------

# install
sudo yum install google-chrome-stable

# check
# chromedriver의 버전과 같아야 함
[centos@ip-172-26-3-214 yum.repos.d]$ google-chrome --version
Google Chrome 87.0.4280.88
```

### case_3
```bash
...생략...
selenium.common.exceptions.WebDriverException: Message: unknown error: session deleted because of page crash
from unknown error: cannot determine loading status
from tab crashed
  (Session info: headless chrome=87.0.4280.88)
```
서버 사양이 부족하면 발생하는 에러입니다. 크롬은 메모리를 많이 잡아먹기에, 적어도 ram 1g 이상의 서버를 사용하는 것이 좋습니다. 위의 에러는 ram 512mb에서 자주 발생하였습니다.  