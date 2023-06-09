<h1>아이디에이션</h1>

> - 최근 영화관 방문이 줄어들고 있고, i'Mmmn은 개인이 본 영화를 기록해두기 위함 앱이다. 따라서 주요 OTT 서비스로 본 영화를 기록할 일이 많은 것이고, 연동 가능한지 PoC해볼 것.

---

<h1>개발일지</h1>

<details><summary><h2>2023년 6월</h2></summary>
 <details><summary><h3>23.06.04.</h3></summary>
  
  - 사이드 프로젝트 "iMymeMine" 시작
  - 테이블 구성(유저, 영화 데이터 테이블)
  - 유저 데이터 추가시 트리거 개발
  - 앱 추가
  - 페이지 추가
 </details>
 <details><summary><h3>23.06.05.</h3></summary>
 
  - 영화 데이터 추가시 트리거 개발
  - admin, test 계정 생성 및 각 테스트 완료
  - 기본 기능 구현 완료
  - 현재 Oracle account 로그인 설정, 추후 USER 테이블과 연동되도록 변경 필요
 </details>
 <details><summary><h3>23.06.06.</h3></summary>
  
  - 모바일 브라우저로 테스트시 별점 터치 이슈 발견, 우선 사이즈 키워보고 추후 이슈 재확인할 것
 </details>
 <details><summary><h3>23.06.07.</h3></summary>
  
  - 회원가입 페이지 생성 및 개발
  - 로그인 페이지에 회원가입 바로가기 버튼 추가
  - USER 테이블 컬럼 추가(이름, 생년월일, 성별)
 </details>
 <details><summary><h3>23.06.08.</h3></summary>
 
  - css 적용 방식 변경: 내부 스타일시트(APEX Custom Css) -> 외부 스타일시트(Static Application Files내 파일 하나로 통합 관리)
  - 구글 로그인 추가
    * 구글 OAuth2 등록 https://www.joinc.co.kr/w/man/12/oAuth2/Google
    * Oracle APEX에서 적용 https://doyensys.com/blogs/apex-login-using-google-social-sign-in/
  
  - 구글 OAuth 생성 관련 시행착오
    * 구글 OAuth 생성시 "승인된 자바스크립트 출처" 부분의 URI는 생략해야함
    * 구글 OAuth 생성시 승인된 리디렉션 URI에 "https://apexea.oracle.com/pls/apex/apex_authentication.callback" 가 아니라 apexea 대신 apex로 작성했더니 오류 해결됨
 </details>
 <details><summary><h3>23.06.09.</h3></summary>
  
  - 앱 이름 변경: IMYMEMINE이라는 쇼핑몰 발견하여 i'Mmmn(아임ㅁㅁ)으로 변경, ㅁㅁ까지 상호명 가능한지 찾아볼 것
  - 로고 제작
  - 오류 발견: 구글 OAuth 생성 후 credential 생성하고 social login에는 성공하였으나 이때 로그인 페이지로 연동해두면 앱 실행하자마자 구글 로그인 페이지로 리디렉션되기 때문에 로그인 화면에서 구글 로그인용 버튼을 하나 두고 페이지 하나 추가하여 추가된 페이지로 구글 로그인 리디렉션되도록 방법 모색
 </details>
 <details><summary><h3>23.06.12.</h3></summary>
  
  - 오류 발견: 로그아웃이 안됨. 로그인 리디렉션 오류 해결을 위해서 수정 중에 로그인/로그아웃 관련 앱 세팅이 깨진 것으로 추측 
    * 로그아웃을 할 때, 구글 로그아웃이 되도록 하면 안됨. 브라우저 자체의 구글이 로그아웃되어버림. 모바일 앱으로 개발 예정이기 때문에 로그인 세션 유지는 최대한 오래되어야 함. 자동 로그아웃을 빈번하게 의도할 필요없다고 판단
 </details>
 <details><summary><h3>23.06.13.</h3></summary>
  
  - 로그아웃 해결: APP > Shared Components > Authentication Schemes > Post-Logout URL 부분이 Home Page와 URL을 수정하다보니 깨진 것 같다는 추측으로 URL 선택 후 로그인 페이지(f?p=&APP_ID.:9999) URL을 입력하여 해결
  - 앱 시작하자마자 구글 로그인 뜨는 이슈는 앱 시작 URL을 로그인 화면으로 변경하는 것으로 해결 예정(방법 찾는 중)
  - Google 계정으로 계속하기 버튼에 Dynamic Action 추가. 캐시로 아이디, 비밀번호 입력되어있는 내용으로 로그인이 되어, 해당 아이템 값이 있는 경우 clear 후 submit page 되도록 처리.
 </details>
 <details><summary><h3>23.06.14.</h3></summary>
  
  - 구글 로그인 계정으로 영화 데이터 추가시 I_MOVIE 테이블에는 정상적으로 데이터가 들어가며, USER_ID에는 구글 계정의 name이 들어가는 것 확인
  - 대신 구글 로그인 계정은 I_USER 테이블에 데이터가 없음 -> I_USER 테이블에 데이터가 없는 로그인 정보로 메인화면 접근시 I_USER에 회원가입 정보를 트리거로 넣을지, 구글 계정의 회원가입 정보를 따로 연동할 수 있는지 확인 후 더 이슈없을 방법으로 선택할 예정
    * 유저 데이터이기 때문에 안정성부터 고려할 것
    * 트리거로 선택하면 I_USER 테이블에 구글 계정 여부에 대한 컬럼 추가할 것
 </details>
 <details><summary><h3>23.06.15.</h3></summary>
  
  - 영화 관련 오픈 API 발견 https://www.kobis.or.kr/kobisopenapi/homepg/apiservice/searchServiceInfo.do
  - 영화 페이지에 들어갔을 때, 모바일 특성상 심플해야할 것 같아서 보여주는 컬럼 변경(축소)
  - 테이블 변경사항(I_MOVIE)
    * 컬럼 변경 CINEMA -> PLACE
    * 컬럼 NOTE 추가
    * 테이블 수정에 따른 UI 및 Query 변경
  
  - 영화 추가 팝업 Drawer에서 Wizard Modal Dialog로 변경
  - 팝업 내 입력창들 길이 줄여서 스크롤 생기지 않고 한 눈에 모두 들어올 수 있도록 변경할 것
  - 영화본 장소 select list로 추가하도록 변경. CGV 등 선택시 아이콘으로 보여지도록 할 예정
 </details>
 <details><summary><h3>23.06.16.</h3></summary>
  
  - 로그인 방식을 ver1.0.0에서는 회원가입만 두는게 관리, 테스트에 용이할 것으로 판단. 로그인 방식 수정하고 유저 테이블 데이터로 회원가입 관리 가능하도록 수정할 것
  - CSS 추가: 푸터의 release 1.0.0 diplay none 처리.
   * css 파일명을 변경하여 적용 다시 해야함.
 </details>
 <details><summary><h3>23.06.17.</h3></summary>
  
  - 앱에서 사용 중인 css 파일 주소 변경 완료
  - 로그인 credential 변경(구글 -> DB)
  - 구글 로그인 버튼 안 보이게 변경
 </details>
 <details><summary><h3>23.06.18.</h3></summary>
  
  - 계정이 막히는 현상을 계속 해결하지 못해서 결국 챗GPT와 대화까지 했는데, 권한 문제라는 심증이 생김. Oracle APEX의 워크스페이스 하나만 발급받는 것이 아니라 DBA 권한이 있는, DB부터 발급받아야 할 것 같아서 시도 중.
    * OCI 계정 생성 https://team-okitoki.github.io/getting-started/free-oci-promotions/
    * OCI 주소 https://signup.cloud.oracle.com/
 </details>
 <details><summary><h3>23.06.19.</h3></summary>
  
  - OCI Autonomous DB 생성 완료(APEX)
 </details>
 <details><summary><h3>23.06.20.</h3></summary>
  
  - instance 생성 -> Launch APEX 로 Admin(internal 접근 성공)
  - Workspace(im) 생성 완료
  - i'Mmmn 앱 다시 개발
     * 앱 생성: 기본 제공 옵션들 모두 추가, 테스트 예정
     * css 파일 생성 및 적용 완료
 </details>
 <details><summary><h3>23.06.21.</h3></summary>
  
  - DB로 로그인은 Authenticate Scheme을 Custom으로 두고 함수로 로그인하는 구조로 해결 예정: https://blogs.ontoorsolutions.com/post/custom-authentication-oracle-apex/
 </details>
 <details><summary><h3>23.06.22.</h3></summary>
  
  - 영화 목록/추가 페이지 각각 생성, 페이지 상세 세팅 완료
  - 영화진흥위원회 회원가입 완료, 오픈 API 분석 시작
  - 영화진흥위원회 API 인증키 발급 완료: 일 3000회 제한(1인 최대 키 2개 발급 가능) -> 추가 필요시 문의하여 호출횟수 제한을 높이거나, 인증키 갯수를 n개로 늘려서 3000*n번 사용 가능하도록 변경 필요
  - Postman에 영화목록 API 테스트 및 설명 추가 완료
  - Postman에 영화 상세정보 API 테스트 및 설명 추가 완료
    * 해당 API는 필수값으로 영화코드가 있어서 우선 활용하지 않는 것으로 분류
  - 영화 오픈 API 목록
    * 일별 박스오피스(검토)
    * 주간/주말 박스오피스(검토)
    * 공통코드 조회(사용 X)
    * 영화목록(사용)
    * 영화 상세정보(사용 X)
    * 영화사목록(사용 X)
    * 영화사 상세정보(사용 X)
    * 영화인목록(사용 X)
    * 영화인 상세정보(사용 X)
   - Postman에 일별 박스오피스, 주간/주말 박스오피스 API 테스트 및 설명 추가 완료
     * 일별 API의 경우, 10:00경 당일 데이터 리턴 안됨.
     * 주간/주말 API는 주말 날짜에만 동작. -> 주간은 왜 안 뜨는지 다시 확인 예정
 </details>
 <details><summary><h3>23.06.23~28.</h3></summary>
  
  - 회사 제안서 작성으로 스킵..
  - 네이버 오픈 API 중에도 영화 API 있는 것 확인. 찾아볼 것.
 </details>
 <details> 
  <summary><h3>23.06.29.</h3></summary>
  
  - 여러 login credential 사용 방법(+홈 URL로 이동했을 때, 첫 화면에 구글 로그인 안 뜨게 하는 방법): 기존 로그인 credential로 current 변경 후 다른 로그인은 버튼 만들고, 해당 버튼 클릭시 이동 페이지 설정 후, 설정 창에서 하단의 advanced에 APEX_AUTHENTICATION(?) 사용하면 해당 버튼을 통해서만 해서만 다른 로그인 방법 가능 -> 이제 로그아웃시 딱 해당 앱만 로그아웃되도록 하는 방법 찾기
 </details>
 <details><summary><h3>23.06.30.</h3></summary>
  
  - 예시 데이터 추가, 영화 추가 페이지 리포트의 정렬 변경
  - 영화 추가 페이지 내 검색 바 외 기능 제외
  - 검색 버튼 css 변경 추가하였으나 반영 안되서 확인 필요
 </details>
</details>
<details>
 <summary><h2>2023년 7월</h2></summary>
  <details><summary><h3>23.07.01.</h3></summary>
   
   - 영화 페이지 내 검색버튼의 텍스트 변경
     * 해당 서치바가 포함된 interactive report의 attribute에서 Search button Label에 입력
   - 검색 버튼 css 수정: class가 a-Button a-IRR-button a-IRR-button--search으로 되어있어서 사이 공백 없애고 .으로 연결
  </details>
  <details><summary><h3>23.07.04.</h3></summary>
  
  - 버튼명 변경
  - css 추가: Static ID 주고 css 파일 적용시 동작하지 않음. 확인 필요.
  </details>
  <details><summary><h3>23.07.05.</h3></summary>
  
  - 네이버 뉴스 API 서비스 중단되었다고 함. 확인 필요.
  </details>
  <details><summary><h3>23.07.06.</h3></summary>

  - 영화 추가 버튼 css 수정 완료.
    * Static ID는 ID로 css 줄 때 #으로 작성. class는 .으로 작성.
  - 추가 버튼 텍스트를 아이콘으로 변경. Universal Theme에서 Floating Button 찾아서 대체할 예정.
    * Dracle APEX Universal Theme: https://apex.oracle.com/pls/apex/r/apex_pm/ut/getting-started
  </details>
</details>
