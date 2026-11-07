G3 Lesson 7 Vocabulary Lab — Netlify 배포 안내

1. 이 폴더 전체(index.html + images 폴더)를 Netlify Drop에 끌어다 놓으세요.
   https://app.netlify.com/drop
2. 사이트 주소를 학생에게 QR로 공유합니다.
3. HTML만 열어도 단어 학습, 퀴즈, 복습, 로컬 저장은 작동합니다.

중요: 교사 대시보드의 서로 다른 기기 실시간 집계는 백엔드 없이는 불가능합니다.
현재 index.html의 FIREBASE_CONFIG가 비어 있으므로 대시보드는 로컬 기록만 보여줍니다.
실시간 집계를 활성화하려면 다음을 설정해야 합니다.

[Firebase Realtime Database 연결]
1. https://console.firebase.google.com 에서 프로젝트를 만들고 Authentication을 활성화합니다.
2. Authentication > Sign-in method에서 Anonymous와 Email/Password를 활성화합니다.
3. Users에서 교사용 이메일/비밀번호 계정을 만들고 교사 UID를 확인합니다.
4. Realtime Database를 만들고 아래의 보안 규칙을 적용합니다.
5. index.html의 FIREBASE_CONFIG에 apiKey, databaseURL, teacherUID를 입력합니다.
6. 다시 Netlify에 업로드합니다.
7. 교사 대시보드에서 교사 이메일/비밀번호로 로그인하면 약 4초마다 학생 기록이 갱신됩니다.

Realtime Database Rules (TEACHER_UID_HERE를 실제 교사 UID로 교체):
{
  "rules": {
    "sessions": {
      ".read": "auth != null && auth.uid === 'TEACHER_UID_HERE'",
      "$uid": {
        ".write": "auth != null && auth.uid === $uid",
        ".validate": "newData.hasChildren(['student', 'stage'])"
      }
    }
  }
}

보안 안내: 학생 이름은 개인정보입니다. 학생 실명 대신 학번/별칭을 사용하고, 학교 개인정보 정책을 확인하세요.
교사 계정 비밀번호는 HTML 파일에 입력하거나 저장하지 마세요. 브라우저 교사 로그인에서만 입력하세요.
교사 대시보드는 인증되지 않은 학생에게 서버의 전체 기록을 공개하지 않습니다.
Firebase의 무료 사용량/네트워크 제한 및 교내 인터넷 환경을 확인하세요.
TTS는 브라우저 정책상 최초 학생 상호작용 후 자동 재생될 수 있으며 en-US 음성이 설치되지 않으면 해당 기기에서 음성이 제한될 수 있습니다.

자료 출처: 첨부된 G3 Lesson 7 Technology in Our Lives 어휘 활동지의 40개 영어 표현.
영어 예문/한국어 해석/관련 형태/오답 문장은 새로 작성한 학습 콘텐츠입니다.
원본 활동지의 일부 연습 예문을 그대로 옮긴 것이 아닙니다.

수정 사항: 로컬 이미지 제거, 단어별 Google 이미지 검색 직접 링크, Stage 1/2 예문 해석 3지선다(정답 균형 배치). Google 이미지는 사용자가 링크를 누를 때만 새 탭에서 열립니다.
