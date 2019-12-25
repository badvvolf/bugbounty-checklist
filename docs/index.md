# Bug Bounty Checklist

**버그바운티 참고 문서 덤프로 변질되고 있는 체크리스트.. 일단 정보를 모으는 중** <br>
개인 참고, 정리용 문서. 체크리스트는 추후에.. <br>
번역 참고 https://ok-chklist.readthedocs.io/ko/latest/index.html

---

## 정보 수집 (Information Gathering)

### OTG-INFO-001 검색 엔진에 노출된 정보 탐색

#### Checklist

#### 기존 Tools
* 서치 엔진

#### 자동화 아이디어
* 자동 검색보다는 편하게 검색 가능하도록 쿼리를 모아놓고 버튼만 누르면 검색하는 게 좋겠음

#### 버그바운티 사례 
이 종류는 바운티를 잘 안 준다. 

* [Securing "Reset password" pages from bots](https://hackerone.com/reports/43807)
	* 민감한 페이지가 서치 엔진 검색 결과에 나왔다. 
* [Insecure Direct Object Reference on API without API key](https://hackerone.com/reports/284963)
	* 구글에서 찾은 URL을 이용하여 API 키 없이 API를 호출할 수 있었다.
	* `site:*.api.도메인.com` 와 같은 쿼리로 구글링
* [Research papers on yelp are getting indexed by google bots](https://hackerone.com/reports/207435) 

---

### OTG-INFO-002 웹서버 핑거프린팅

#### Checklist
* 일반적인 HTTP Response에서 Server 헤더를 확인
* 에러 페이지에서 서버 정보를 유출하는지 확인
* HTTP 헤더 필드 순서를 통해 유추
* 400 응답 시 함께 오는 HTTP 헤더 필드 종류를 통해 유추

#### 기존 Tools
* [httprint](https://net-square.com/httprint.html)
	* 오픈소스 X, 개인 무료. Last update : 2005
* [httprecon](https://www.computec.ch/projekte/httprecon/)
	* 오픈소스, HTTPS 가능, 핑거프린트 데이터베이스 얻을 수 있음, Last update : 2009
* [software-version-reporter](https://github.com/portswigger/software-version-reporter)
	* 오픈소스, Burpsuite 플러그인.

#### 자동화 아이디어
* 핑거프린팅으로 버전 정보를 뽑으면 CVE 추천까지 해주기

#### 버그바운티 사례
* [Out-of-date Version (Apache)](https://hackerone.com/reports/184877)
	* 취약점이 있는 버전의 웹서버를 사용한다는 점을 지적한다. 해당 버전의 웹서버에 공격 가능한 CVE를 나열하고, 설명도 덧붙였다. 
* [RCE and Complete Server Takeover of http://www.█████.starbucks.com.sg/](https://hackerone.com/reports/502758)
	* 스택 트레이스로 얻은 웹서버 버전으로 유효한 CVE를 찾아 RCE 

---

### OTG-INFO-003 정보 노출을 확인하기 위해 웹 서버 메타 파일 확인 

#### Checklist
* robots.txt 파일 확인
* robots.txt에서 크롤링 금지한 웹페이지의 meta 태그 체크
* 민감한 데이터가 있는 웹페이지의 meta 태그 체크

#### 기존 Tools

#### 버그바운티 사례
* [Securing sensitive pages from SearchBots](https://hackerone.com/reports/3986)
	* meta 태그 noindex, nofollow 태그가 걸려있지 않은 점을 지적함.
	* 특히 GET 파라미터에 토큰을 넣어 보내는 경우 주의해야 한다 지적

---

### OTG-INFO-004 웹 서버의 어플리케이션 확인

#### Checklist
* IP- > 도메인 찾기, 또는 역으로

#### 기존 Tools
* [Sublist3r](https://github.com/aboul3la/Sublist3r)
	* 서브도메인 열거

#### 자동화 아이디어
* Subdomain takeover 취약점 찾기
	* 서치엔진/DNS 쿼리를 이용하여 서브도메인을 찾고, takeover 가능한지 찾는다. 
	* 검색을 통해 나온 서브도메인 + bruteforce를 이용하여 DNS 쿼리 
	* 자주 사용하는 도메인 문구를 이용하여 쿼리

#### 버그바운티 사례
* [Sub Domain Take over](https://hackerone.com/reports/111078)
	* Broken Link Hijacking으로 버그 리포트
* [Bulgaria - Subdomain takeover of mail.starbucks.bg](https://hackerone.com/reports/736863)
	* `*.starbucks.*` 를 스캔하여 mail.starbucks.bg가 주인없는 서비스를 가리키고 있다는 것을 확인

---
### OTG-INFO-005 주석이나 메타데이터 확인

#### Checklist

#### 기존 Tools

#### 자동화 아이디어
* 주석만 뽑고, 중요해보이는 데이터를 추천

#### 버그바운티 사례

---

### OTG-INFO-006 어플리케이션 엔트리포인트 식별

#### Checklist
* GET 파라미터, 쿠키 설정, HTTP 에러 발생 페이지, 독특한 HTTP 메소드 사용 지점, 커스텀 HTTP 헤더 등

---

### OTG-INFO-007 경로 매핑

#### 기존 Tools
* Burpsuite 
* 크롤러

#### 버그바운티 사례
* [Real Time Error Logs Through Debug Information](https://hackerone.com/reports/503283)
	* slackb.com/debug 에 접속하니 디버깅 정보를 볼 수 있더라 보고

---

### OTG-INFO-008 웹 어플리케이션 프레임워크 핑거프린팅

#### Checklist
* 프레임워크 특유의 HTTP 헤더, 경로, 쿠키, HTML 코드로 프레임워크 식별, 버전 식별

#### 기존 Tools
* [Whatweb](https://www.morningstarsecurity.com/research/whatweb)
	* 오픈소스, 현재도 업데이트 됨, 칼리 기본 툴
* [BlindElephant](http://blindelephant.sourceforge.net/)
	* 오픈소스, 오래됨
* **[Wappalyzer](https://www.wappalyzer.com/)**
	* 오픈소스, 현재도 업데이트 됨
	* 간편함, 웹에서 도메인을 검색하여 핑거프린트 검색 가능
	* 브라우저 익스텐션으로 사용 가능

---

### OTG-INFO-009 웹 어플리케이션 핑거프린팅

#### Checklist
* 어플리케이션 특유의 쿠키, HTML 코드, 파일 또는 디렉토리 이름
* Dirbusting 으로 가능성 있는 파일/디렉토리 탐색

#### 기존 Tools
* [Whatweb](https://www.morningstarsecurity.com/research/whatweb)
	* 오픈소스, 현재도 업데이트 됨, 칼리 기본 툴
* [BlindElephant](http://blindelephant.sourceforge.net/)
	* 오픈소스, 오래됨
* **[Wappalyzer](https://www.wappalyzer.com/)**
	* 오픈소스, 현재도 업데이트 됨
	* 간편함, 웹에서 도메인을 검색하여 핑거프린트 검색 가능
	* 브라우저 익스텐션으로 사용 가능

---

### OTG-INFO-010 어플리케이션 아키텍쳐 매핑
#### Checklist
* 방화벽, 리버스 프록시, 로드 밸런서, DB 등 아키텍쳐 조사
	* HTTP 헤더나 TCP 반응을 보고 판단할 수 있음

---

### OTG-CONFIG-001 네트워크/인프라 설정 테스트

* 알려진 취약점, 관리 도구, 인증 시스템 등에 대한 테스트

#### 버그바운티 사례
* [Out-of-date Version (Apache)](https://hackerone.com/reports/184877)
	* 취약점이 있는 버전의 웹서버를 사용한다는 점을 지적한다. 해당 버전의 웹서버에 공격 가능한 CVE를 나열하고, 설명도 덧붙였다. 

---

### OTG-CONFIG-002 어플리케이션 플랫폼 설정 테스트

* 플랫폼 설치시 딸려오는 샘플 프로그램이나 디폴트 설정을 체크한다. 권한 설정이나 방화벽 설정, 로깅 설정 등

#### 버그바운티 사례
* [Real Time Error Logs Through Debug Information](https://hackerone.com/reports/503283)
	* slackb.com/debug 에 접속하니 디버깅 정보를 볼 수 있더라 보고

---

### OTG-CONFIG-003 파일 확장자 핸들링 테스트

* 파일 확장자를 통해 서버에서 사용하는 기술 스택을 추측할 수 있다. 
* 민감한 정보를 담는 확장자를 열 수 있는지 테스트한다. 

---

### OTG-CONFIG-004 백업 및 참조되지 않은 파일 테스트
* 이름을 변경한 파일, 프로그램이 자동으로 생성한 백업(스냅샷), 잊힌 파일 등에 접근 가능한지 알아본다. 
* 백업은 원본 파일의 확장자와 다른 확장자로 변하기때문에(ex : .old) 일반적인 문자열로 전송될 수도 있다. 서버 코드 유출 가능.

---

### OTG-CONFIG-005 인프라와 어플리케이션 관리자 인터페이스 확인
* 관리자 페이지에 접속할 수 있는지 확인한다. 
* 디폴트 위치, 브루트포스, 어드민용 포트 등 테스트
* 기본 ID/PW 테스트

---

### OTG-CONFIG-006 HTTP 메소드 테스트
* 여러가지 HTTP 메소드 사용가능한지 체크
* TRACE, OPTIONS 등 활용
* 임의로 생성한 메소드가 있는지 체크 

---

### OTG-CONFIG-007 HSTS 체크
* HTTP Strict Transport Security(HSTS) 헤더 체크
	* 이 도메인은 HTTPS만 지원한다는 의미
	* 
#### 버그바운티 사례
* [SSO through odnoklassniki uses http rather than https](https://hackerone.com/reports/703759)
	* 로그인 페이지에서 odnoklassniki로 로그인을 누를 시 HTTP로 리다이렉트할 URL을 전송한다. 이를 이용하여 피해자의 PC에서 공격자의 계정으로 로그인할 수 있었다. 
	* 단순히 HSTS가 없다는 것 만으로는 제보가 안 되지만, 이 경우 HSTS가 있어도 공격 가능성이 있었음. 

---
## 입력 유효성 테스트

### OTG-INPVAL-001 Reflected XSS

#### Checklist
* Input Vector 찾기
	* GET 파라미터
	* HTTP 헤더
	* POST 데이터
		* Form 값
		* Hidden Form 값
		* 미리 정의된/또는 선택한 버튼 값
	* **툴을 이용하여 Input Vector 리스팅**
* 필터링 우회 여부 체크
	* ```>, <, &, /, `, ', " ``` 와 같은 HTML 특수문자
	* ```script, javascript, img``` 와 같은 문자열
	* ```\n, \r, \uXXXX ``` 
	* **퍼저를 이용하여 페이로드 테스트**
#### 자동화 아이디어
* 주요 특수문자/키워드 필터링 여부 판단하고, XSS 가능성 높은 순으로 추천하기 
	* 수동으로 XSS 테스트하기 전에 도움을 주는 용도

#### 버그바운티 사례
* [XSS vulnerable parameter in a location hash](https://hackerone.com/reports/146336)
	* GET 파라미터로 받은 값을 로그로 찍는 스크립트에 XSS가 가능했다.
* [DOM XSS at https://www.thx.com in IE/Edge browser](https://hackerone.com/reports/702981)
	* 현재 페이지 URL을 window.location.href로 가져와서 이용한다. IE나 Edge 브라우저는 window.location.href에 인코딩을 하지 않기 때문에 `https://www.thx.com/#'><img src=x onerror=alert(document.domain)>` 와 같은 URL 입력 시 XSS 가능했다.
* [Reflected Cross site Scripting (XSS) on www.starbucks.com ](https://hackerone.com/reports/438240)
	* HTTP 파라미터에 Return URL을 자바스크립트 스키마로 넣었다.
* [Reflected XSS in pubg.com](https://hackerone.com/reports/751870)
	* GET 파라미터로 받은 값을 이용하여 "더 보기" 버튼을 만들었다. 파라미터는 검색 쿼리같음. 이 파라미터를 필터링 없이 이용하여 XSS가 가능했음.

---

### OTG-INPVAL-002 Stored XSS

#### Checklist

#### 버그바운티 사례
* [Stored XSS in vanilla](https://hackerone.com/reports/496405)
* [Cross-site Scripting (XSS) - Stored in RDoc wiki pages](https://hackerone.com/reports/662287)
	* 필터링 부족으로 인해 클릭재킹, 피싱 공격 가능. 또한 XSS도 가능했음.
* [Stored XSS in Snapmatic + R★Editor comments](https://hackerone.com/reports/309531)
	* 여러 우회 방법을 이용하여 XSS 가능했음. 

---
### OTG-INPVAL-005 SQL 인젝션

#### Checklist

#### 기존 Tools
* Burpsuite Intruder

#### 버그바운티 사례
* [SQL Injection on sctrack.email.uber.com.cn](https://hackerone.com/reports/150156)
	* json 데이터를 base64로 인코딩하여 전송하는 데이터에서 SQLi가 가능했다.

---

### OTG-INPVAL-016 HTTP Splitting/Smuggling

#### Checklist

#### 버그바운티 사례
* [HTTP Request Smuggling on vpn.lob.com](https://hackerone.com/reports/694604)
* [Multiple HTTP Smuggling reports](https://hackerone.com/reports/648434)
	* 여러 프로그램의 HTTP sumggling CVE 제시

---

### OTG-INPVAL-013 OS 커맨드 인젝션

#### Checklist

#### 버그바운티 사례
* [Local files could be overwritten in GitLab, leading to remote command execution](https://hackerone.com/reports/587854)

---

### OTG-INPVAL-014 버퍼 오버플로우

#### Checklist

#### 버그바운티 사례
* [Security check failure or stack buffer overrun (crash)](https://hackerone.com/reports/481335)

---

## 에러 처리

### OTG-ERR-002 스택 트레이스 분석

#### Checklist

#### 버그바운티 사례
* [RCE and Complete Server Takeover of http://www.█████.starbucks.com.sg/](https://hackerone.com/reports/502758)
	* 스택 트레이스로 얻은 웹서버 버전으로 유효한 CVE를 찾아 RCE 

---

## 암호화 테스트 

### OTG-CRYPST-003 민감 데이터 암호화되지 않은 채널 전송 
#### 버그바운티 사례
* [Invitation reminder emails contain insecure links](https://hackerone.com/reports/327674)
	* 메일에 http 링크가 있다는 점을 지적. (https가 아니라고)
* [SSO through odnoklassniki uses http rather than https](https://hackerone.com/reports/703759)
	* 로그인 페이지에서 odnoklassniki로 로그인을 누를 시 HTTP로 리다이렉트할 URL을 전송한다. 이를 이용하여 피해자의 PC에서 공격자의 계정으로 로그인할 수 있었다.
	* 단순히 HSTS가 없다는 것 만으로는 제보가 안 되지만, 이 경우 HSTS가 있어도 공격 가능성이 있었음. 

---

## 비즈니스 로직 테스트

### OTG-BUSLOGIC-008 예기치 않은 파일 형식 업로드

#### Checklist
* 필터링 우회 
	* 띄어쓰기, 특수문자 끼워넣기

#### 버그바운티 사례 
* [Webshell via File Upload on ecjobs.starbucks.com.cn](https://hackerone.com/reports/506646)
	* 파일 이름의 마지막(확장자 뒤)에 띄어쓰기를 포함하여 필터링 우회
* [XXE at ecjobs.starbucks.com.cn/retail/hxpublic_v6/hxdynamicpage6.aspx](https://hackerone.com/reports/500515)
	* 파일 업로드 취약점을 이용해 XML 파일 업로드, XXE 까지 가능

---

## 클라이언트측 테스트

### OTG-CLIENT-005 CSS Injection 테스트

#### 버그바운티 사례 
* [CSS Injection to disable app & potential message exfil](https://hackerone.com/reports/679969)
	* 사용자에게 입력받은 값으로 테마를 변경하는 기능에 CSS 인젝션 가능
