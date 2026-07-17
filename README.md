# AI 웹툰 창작소

이 프로젝트는 학교 현장에서 학생들의 창작 활동(국어, 창체, 역사 등)을 지원하기 위해 만들어진 **AI 웹툰 창작소**입니다. 학생이 쓴 긴 스토리를 입력하면 선택한 컷 수에 맞게 장면을 나누고, 각 컷별 요약과 AI 이미지 생성용 프롬프트를 자동으로 정리해줍니다.

## 주요 기능
- **Google 계정 로그인**: Firebase Authentication을 연동하여 학교 학생 및 교직원 구글 계정(`@sen.go.kr`, `@school.kr` 등)으로 안전하게 로그인할 수 있습니다.
- **장면 자동 분할**: 입력한 상세 스토리를 지정한 컷 수(4컷, 8컷, 12컷)에 맞게 자동으로 분할하고 요약합니다.
- **프롬프트 자동 생성**: 분할된 장면과 입력된 장르, 등장인물 정보를 바탕으로 AI 이미지 생성 도구에서 사용할 수 있는 프롬프트를 만들어줍니다.
- **실시간 데이터베이스 연동**: Firebase Firestore를 사용하여 생성한 웹툰 기획 기록이 실시간으로 클라우드에 저장되고 최근 기록에서 조회됩니다.

## 실행 방법
이 프로젝트는 별도의 빌드 과정이 필요 없는 순수 HTML/CSS/JS 기반의 정적 웹 애플리케이션입니다. 

1. 소스 코드를 다운로드 받거나 Clone 합니다.
2. 로컬 서버(VS Code Live Server 등)를 이용해 `index.html` 파일을 실행합니다. 
   *(주의: Firebase ES 모듈을 사용하므로 `file://` 프로토콜 대신 `http://` 로컬 서버 환경에서 실행해야 정상 작동합니다.)*

## Firebase 설정 방법
현재 코드는 Firebase 연동이 준비되어 있습니다. 본인의 Firebase 프로젝트와 연결하려면 아래 과정을 진행하세요.

1. [Firebase Console](https://console.firebase.google.com/)에서 새 프로젝트를 생성합니다.
2. **Authentication** 메뉴에서 **Google 로그인**을 활성화합니다.
3. **Firestore Database** 메뉴에서 데이터베이스를 생성합니다.
4. 프로젝트 설정(기어 아이콘)에서 '웹 앱'을 추가합니다.
5. 앱 추가 후 나타나는 `firebaseConfig` 객체의 값(API Key 등)을 복사합니다.
6. `index.html` 파일의 `<script type="module">` 내부에 있는 `firebaseConfig` 변수를 본인의 값으로 교체합니다.

## Firestore 보안 규칙 예시
테스트를 위해 Firestore 보안 규칙을 아래와 같이 임시로 설정할 수 있습니다. 실제 운영 환경에서는 반드시 인증된 사용자만 접근 가능하도록 규칙을 강화해야 합니다.

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```