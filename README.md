<h1>개발일지</h1>

<h3>23.06.04.</h3>

- 사이드 프로젝트 "iMymeMine" 시작
- 테이블 구성(유저, 영화 데이터 테이블)
- 유저 데이터 추가시 트리거 개발
- 앱 추가
- 페이지 추가

<h3>23.06.05.</h3>

- 영화 데이터 추가시 트리거 개발
- admin, test 계정 생성 및 각 테스트 완료
- 기본 기능 구현 완료
- 현재 Oracle account 로그인 설정, 추후 USER 테이블과 연동되도록 변경 필요

<h3>23.06.06.</h3>

- 모바일 브라우저로 테스트시 별점 터치 이슈 발견, 우선 사이즈 키워보고 추후 이슈 재확인할 것

<h3>23.06.07.</h3>

- 회원가입 페이지 생성 및 개발
- 로그인 페이지에 회원가입 바로가기 버튼 추가
- USER 테이블 컬럼 추가(이름, 생년월일, 성별)

<h3>23.06.08.</h3>

- css 적용 방식 변경: 내부 스타일시트(APEX Custom Css) -> 외부 스타일시트(Static Application Files내 파일 하나로 통합 관리)
- 구글 로그인 추가
  > 구글 OAuth2 등록 https://www.joinc.co.kr/w/man/12/oAuth2/Google
  > 
  > Oracle APEX에서 적용 https://doyensys.com/blogs/apex-login-using-google-social-sign-in/

- 구글 OAuth 생성 관련 시행착오
  > 구글 OAuth 생성시 "승인된 자바스크립트 출처" 부분의 URI는 생략해야함
  > 
  > 구글 OAuth 생성시 승인된 리디렉션 URI에 "https://apexea.oracle.com/pls/apex/apex_authentication.callback" 가 아니라 apexea 대신 apex로 작성했더니 오류 해결됨

<h3>23.06.09.</h3>

- 앱 이름 변경: IMYMEMINE이라는 쇼핑몰 발견하여 i'Mmmn(아임ㅁㅁ)으로 변경, ㅁㅁ까지 상호명 가능한지 찾아볼 것
- 로고 제작
- 오류 발견: 구글 OAuth 생성 후 credential 생성하고 social login에는 성공하였으나 이때 로그인 페이지로 연동해두면 앱 실행하자마자 구글 로그인 페이지로 리디렉션되기 때문에 로그인 화면에서 구글 로그인용 버튼을 하나 두고 페이지 하나 추가하여 추가된 페이지로 구글 로그인 리디렉션되도록 방법 모색

<h3>23.06.12.</h3>

- 오류 발견: 로그아웃이 안됨. 로그인 리디렉션 오류 해결을 위해서 수정 중에 로그인/로그아웃 관련 앱 세팅이 깨진 것으로 추측 
  > 로그아웃을 할 때, 구글 로그아웃이 되도록 하면 안됨. 브라우저 자체의 구글이 로그아웃되어버림. 모바일 앱으로 개발 예정이기 때문에 로그인 세션 유지는 최대한 오래되어야 함. 자동 로그아웃을 빈번하게 의도할 필요없다고 판단


<h3>23.06.13.</h3>

- 로그아웃 해결: APP > Shared Components > Authentication Schemes > Post-Logout URL 부분이 Home Page와 URL을 수정하다보니 깨진 것 같다는 추측으로 URL 선택 후 로그인 페이지(f?p=&APP_ID.:9999) URL을 입력하여 해결
- 앱 시작하자마자 구글 로그인 뜨는 이슈는 앱 시작 URL을 로그인 화면으로 변경하는 것으로 해결 예정(방법 찾는 중)
- Google 계정으로 계속하기 버튼에 Dynamic Action 추가. 캐시로 아이디, 비밀번호 입력되어있는 내용으로 로그인이 되어, 해당 아이템 값이 있는 경우 clear 후 submit page 되도록 처리.
