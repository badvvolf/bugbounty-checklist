# Bug Bounty Checklist

OTGv4를 기반으로 하는 버그 바운티 체크리스트입니다. 본 문서는 OTGv4 번역 문서가 아닙니다.<br>
개인적으로 정리하는 것이기에 검증되지 않았으며, 공신력 또한 없습니다. <br>
1차 작업중.. OTGv4 훑어보기, 정리 중. 체크리스트의 형태는 아직 X <br>
번역 참고 : https://ok-chklist.readthedocs.io/ko/latest/index.html

## 정보 수집 (Information Gathering)

### OTG-INFO-001 검색 엔진에 노출된 정보 탐색

#### Checklist


#### 버그바운티 사례 
이 종류는 바운티를 잘 안 준다. 

* [Securing "Reset password" pages from bots](https://hackerone.com/reports/43807)
	* 민감한 페이지가 서치 엔진 검색 결과에 나왔다. 
* [Insecure Direct Object Reference on API without API key](https://hackerone.com/reports/284963)
	* 구글에서 찾은 URL을 이용하여 API 키 없이 API를 호출할 수 있었다.
	* `site:*.api.도메인.com` 와 같은 쿼리로 구글링
* [Research papers on yelp are getting indexed by google bots](https://hackerone.com/reports/207435) 

### OTG-INFO-002 웹서버 핑거프린팅

#### Checklist
* 일반적인 HTTP Response에서 Server 헤더를 확인
* 에러 페이지에서 서버 정보를 유출하는지 확인
* HTTP 헤더 필드 순서를 통해 유추
* 400 응답 시 함께 오는 HTTP 헤더 필드 종류를 통해 유추

#### Tools
* [httprint](https://net-square.com/httprint.html) : 오픈소스 X, 개인 무료. Last update : 2005
* [httprecon](https://www.computec.ch/projekte/httprecon/) : 오픈소스, HTTPS 가능, 핑거프린트 데이터베이스 얻을 수 있음, Last update : 2009

#### 버그바운티 사례
* [Out-of-date Version (Apache)](https://hackerone.com/reports/184877)
	* 취약점이 있는 버전의 웹서버를 사용한다는 점을 지적한다. 해당 버전의 웹서버에 공격 가능한 CVE를 나열하고, 설명도 덧붙였다. 
* [RCE and Complete Server Takeover of http://www.█████.starbucks.com.sg/](https://hackerone.com/reports/502758)
	* 스택 트레이스로 얻은 웹서버 버전으로 유효한 CVE를 찾아 RCE 

### OTG-INFO-003 정보 노출을 확인하기 위해 웹 서버 메타 파일 확인 

#### Checklist
* robots.txt 파일 확인
* robots.txt에서 크롤링 금지한 파일의 meta 태그 체크

#### Tools

#### 버그바운티 사례




## 입력 유효성 테스트

### OTG-INPVAL-001 반사형 XSS

#### Checklist
* Input Vector 찾기
	* GET 파라미터
	* HTTP 헤더
	* POST 데이터
		* Form 값
		* Hidden Form 값
		* 미리 정의된/또는 선택한 버튼 값
* 필터링 우회 여부 체크
	* ```>, <, &, /, `, ', " ``` 와 같은 HTML 특수문자
	* ```script, javascript, img``` 와 같은 문자열
	* ```\n, \r, \uXXXX ``` 

#### 버그바운티 사례

    
## 에러 처리

### OTG-ERR-002 스택 트레이스 분석

#### Checklist

#### 버그바운티 사례
* [RCE and Complete Server Takeover of http://www.█████.starbucks.com.sg/](https://hackerone.com/reports/502758)
	* 스택 트레이스로 얻은 웹서버 버전으로 유효한 CVE를 찾아 RCE 