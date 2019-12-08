# Bug Bounty Checklist

OTGv4를 기반으로 하는 버그 바운티 체크리스트입니다. 본 문서는 OTGv4 번역 문서가 아닙니다.<br>
개인적으로 정리하는 것이기에 검증되지 않았으며, 공신력 또한 없습니다. <br>
1차 작업중.. OTGv4 훑어보기, 정리 중. 체크리스트의 형태는 아직 X

## 정보 수집 (Information Gathering)

### OTG-INFO-001 검색 엔진에 노출된 정보 탐색

#### Checklist


#### 버그바운티 사례 



### OTG-INFO-002 웹서버 핑거프린팅

#### Checklist
* HTTP Response에서 Server 헤더를 확인 
* 에러 페이지에서 서버 정보를 유출하는지 확인

#### 버그바운티 사례
* [Out-of-date Version (Apache)](https://hackerone.com/reports/184877)
    * 취약점이 있는 버전의 웹서버를 사용한다는 점을 지적한다. 해당 버전의 웹서버에 공격 가능한 CVE를 나열하고, 설명도 덧붙였다. 


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