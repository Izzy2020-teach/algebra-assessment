# 포물선과 선형변환 탐구 수행평가

고급 대수 Ⅱ. 행렬과 선형변환 수행평가(100점) 웹 앱입니다. 학생은 자기가 고른 대상에서 포물선의 식을 행렬(가우스 소거법·역행렬·행렬식)로 구하고, 그 곡선을 선형변환(대칭·닮음·회전, 합성·역변환)으로 옮겨 문양을 만든 뒤 검증합니다. 교사는 작성 현황을 실시간으로 보며 채점합니다.

> **지금 상태 (2026-11-04)**: 원본 앱(전문 수학 삼각함수·미적분 모델링 탐구, Blis-logic/modeling-assessment 커밋 14ca30d)을 복사한 단계입니다. 화면과 활동지는 아직 원본 그대로이고, 배포하지 않았습니다.

## 구성

- **화면**: `index.html`(+ `config.js`, `mathtool.js`)을 GitHub Pages로 배포합니다. 학생은 구글 계정 없이 아이디·비밀번호로 접속합니다.
- **예비 화면**: 학교망이 `github.io`를 막을 때를 위해 같은 화면을 Apps Script 주소(`/exec`)로도 엽니다(`apps-script/app.html`, 자동 생성).
- **저장 서버**: `apps-script/` 폴더의 `.gs` 파일들(계정·저장·채점·문서·학생 폴더·동의서·백업)을 교사 개인 Gmail의 Apps Script에 올려 웹 앱으로 배포합니다. 기록은 그 스프레드시트와 드라이브에 쌓입니다.
- **연결**: `config.js`의 `apiUrl`에 Apps Script 웹 앱 주소를 넣습니다(지금은 비어 있음).

## 설치 (처음 한 번)

`.gs` 파일이 여러 개라서 편집기에 붙여 넣는 대신 **clasp**(구글 Apps Script 명령줄 도구)로 올립니다. 순서와 주의할 점은 `CLAUDE.md`의 '저장 서버 배포 (clasp)'에 있습니다. 요약하면:

1. Node.js·git 설치, `npm i -g @google/clasp`
2. https://script.google.com/home/usersettings 에서 Google Apps Script API 켜기 → `clasp login`
3. `clasp create --type sheets --title "<새 스프레드시트 제목>" --rootDir apps-script`
4. `node tools/build-gas.js` → `clasp push` → 편집기에서 `setup`, `installBackup` 실행(권한 허용)
5. `clasp deploy` → `/exec?ping=1` 확인 → `config.js`에 주소 → 다시 `node tools/build-gas.js`
6. 커밋·`git push`, GitHub **Settings → Pages**(main / root) 켜기
7. `clasp push` → `clasp deploy -i <배포ID>`
8. 교사 가입 코드(`설정` 시트)로 교사 계정을 만들고, 시험 학생으로 제출·채점·PDF까지 한 번 해 본 뒤 시험 계정을 지웁니다.

## 운영

- **비밀번호**: 암호화해 저장하므로 교사도 볼 수 없습니다. 잊은 학생은 교사가 [계정·비밀번호 관리 → 비밀번호 초기화]로 임시 비밀번호를 정해 줍니다.
- **실시간**: 학생 글은 입력을 멈추고 2~3초 뒤 저장되고, 교사 대시보드는 10초마다 새로 고칩니다.
- **기록**: `기록`(작성률 등), `채점`(요소별 점수·총점) 시트. 매일 백업 사본이 7일 동안 보관됩니다.
- **작성 마감**: 대시보드에서 켜면 서버가 학생 저장을 막습니다.
- **AI 활용**: 학생은 단계마다 AI를 어디에 어떻게 썼는지 적습니다. 확인 없이 그대로 붙여 넣은 내용은 감점될 수 있습니다(평가계획 참고).

## 고칠 때

화면을 고친 뒤: `node tools/build-gas.js` → 커밋·`git push`(Pages 반영 확인) → `clasp push` → `clasp deploy -i <배포ID>`. `app.html`은 직접 고치지 않습니다.

저장소가 공개여도 학생 기록과 비밀번호는 스프레드시트에만 있고 저장소에는 올라가지 않습니다.
