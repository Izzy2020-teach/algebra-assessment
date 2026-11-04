# 포물선과 선형변환 탐구 수행평가 웹 앱

고급 대수(Ⅱ. 행렬과 선형변환) 수행평가(100점) 웹 앱. 사용자는 수학 교사이며 한국어로 대화한다. 교사는 개발 도구가 처음이므로 무엇을 왜 하는지 쉬운 말로 설명한다.
원본: Blis-logic/modeling-assessment (커밋 14ca30d, 전문 수학 삼각함수·미적분 모델링 탐구 앱)에서 2026-11-04 복사해 시작했다. 원본 운영 서버·스프레드시트와의 연결(`.clasp.json`, `apiUrl`, git 기록)은 복사 직후 끊었다.

## 반드시 지킬 작업 규칙 (모든 작업·세션 공통)

1. **코드의 추가·수정은 승인을 받은 뒤에만 한다.** 고치기 전에 무엇을, 어느 파일에서, 왜 바꾸는지
   먼저 알리고 승인을 받는다. 조사·읽기·로컬 시험은 승인 없이 해도 된다.
   커밋·push·clasp push·clasp deploy도 각각 승인을 받는다.
2. **학생 작성 결과를 바꿀 가능성을 검토하고 보고한다.** 승인을 요청할 때와 마칠 때 모두 적는다.
   - 저장된 학생 기록(`doc`), 채점, 계정, Drive 파일에 영향이 있는지
   - 칸 이름·데이터 구조·계산식·자동 저장·문서 생성이 바뀌어 답이 사라지거나 다르게 보이거나
     다시 계산되거나 덮어써질 수 있는지
   - 영향이 있으면 범위, 옛 기록 호환 처리, 되돌리는 방법을, 없으면 "학생 작성 결과 변경 없음"과 근거를

## 함께 볼 문서 (이 폴더의 상위 폴더)

- `../진행_체크리스트.md`: 7단계 진행 체크리스트와 **결정 기록**(날짜·이유). 새 결정은 여기에 적는다.
- `../활동지_초안.md`: 4단계 활동지 칸·힌트·도구, 교사용 검산 예시 (교사 승인 v1)
- `../평가계획_초안.md`: 100점 평가요소·수준별 기준·AI 감점 규칙 (교사 승인 v1)

## 현재 상태 (2026-11-04)

- 이름 문구(11/4)와 **첫 덩어리(11/4)**를 마쳤다: `STAGES` 4단계·칸 설명·주제 예시·평가요소 이름, 단계 수에 묶인 곳은 `STAGES.length`로, 엔진은 이차함수 하나(`MODEL_PARAMS=["a","b","c"]`, `quadBody`), 단계마다 AI 칸(`AI_BLOCK`)과 위쪽 안내(`AI_NOTICE`), `DATA_GOAL=10`, 보고서 머리글.
- **둘째 덩어리(11/4)**: `matrix` 도구(`mxSolve`·`gaussJordan`·`det3`, 분수 `QF`/소수 `FF`, 저장 `calc.pick`·`calc.mxShow`)와 `transform` 도구(`tfState`·`tfSVG`·`tfCalcHTML`, 저장 `calc.tf`·`calc.rep`·`calc.tfShow`). 변환 종류를 바꾸면 `data-rerender`로 화면을 다시 그린다. 행렬 LaTeX는 `texRows`(첨가행렬은 `\Bigg(`+`array`, 나머지는 `pmatrix`).
- **셋째 덩어리(11/4)**: 배점은 화면 `ELEMENT_MAX=[20,30,30,20]`·`LEVEL_PTS`(수준 A~E 점수)·`levelOf`와 서버 `GRD_MAX`·`GRD_LEVELS`(Code.gs) 두 곳에 같게 둔다. 채점 시트 머리글(`GRD_HEAD`) 요소 이름도 새 이름. 점수 저장 형식(`scores.s1~s4`)은 원본 그대로.
- 수식 키보드(11/4): `KEYS` 갈래를 기본·행렬·변환·이차함수·검증·문자·기호·숫자로 바꿨다(옛 오차율 단추 제거, 새 식은 이차함수·검증 갈래).
- 남은 것: 미제출·최저점 처리(학교 규정 확인 후), 배포.
- 원본의 미분 변화율 표·구분구적법·도함수 그래프 코드(`rate`, `riemann*`, `derivCharts`, `logTry`)는 활동지에서 뺐지만 함수는 아직 남아 있다(쓰지 않음).
- 아직 배포하지 않았다. 서버(Apps Script)·스프레드시트·GitHub 원격·Pages 주소 모두 없다. 학생 기록이 없으므로 지금은 칸 이름을 바꿔도 학생 영향이 없다(배포 전에 확정).

## 이 영역의 결정 사항

- 대상: 학생 3명, 노트북·크롬북·PC, 3~4차시, 이번 학기
- 성취기준: [12고대02-01]~[12고대02-06] 전부를 한 탐구에서 다룬다
- 탐구: **포물선을 찾고, 변환으로 디자인하기** (두 탐구를 하나로 합침, 11/4 교사 결정)
  - ① 탐구 설계 ② 곡선 식 구하기(세 점 → 연립일차방정식 → $AX=B$ → 가우스 소거법·역행렬·행렬식) ③ 변환으로 디자인(대칭·닮음·회전, 합성·역변환, 넓이 $|\det|$배) ④ 검증·성찰
  - 중심 개념: 행렬식 ≠ 0 ⇔ 역행렬 존재 ⇔ 하나로 정해지고 되돌릴 수 있다
- 모델: 이차함수 $y=ax^2+bx+c$만. 행렬은 교과서 범위대로 3차 정사각행렬까지, 변환은 평면(2차)만
- **최소제곱(전치행렬 $A^TA$)은 쓰지 않는다**: 교과서에 없어 교육과정 밖
- 계산: 학생이 손계산 과정을 적고, 앱 도구로 확인(도구는 학생이 적은 뒤 펼쳐 보는 방식)
- 자료 목표: 10개(제안 상태)
- 검증 지표: 원본 오차율 $\frac{|y-f(x)|}{y_{\max}-y_{\min}}\times100$을 쓸지 아직 미정
- 평가: 100점, 요소 4개 — 탐구 설계와 자료 수집 20 / 연립일차방정식과 행렬 30 / 선형변환과 디자인 30 / 검증·성찰과 AI 활용 20. 각 요소 A~E 수준(30점: 30·25·20·15·10, 20점: 20·17·14·11·8)
- AI: 사용 허용, 단계마다 기록 칸(`s1_ai`~`s4_ai`, 어느 칸·물은 것·AI 답·내가 확인/고친 것). 확인 없이 그대로 붙여 넣으면 감점 가능(해당 요소 한 수준 하향 등), 감점 전 소명 기회. AI 판별 프로그램 결과만으로 판단하지 않는다
- 서버 계정: 교사 **개인 Gmail**. 동의서: 충북온라인학교, 해당 학년도 말까지 보유

## 원본에서 바꿀 곳

### 영역에 묶인 부분 (`index.html` 중심)
| 무엇 | 이름 | 할 일 |
|---|---|---|
| 활동지 | `STAGES` | 4단계, `../활동지_초안.md`의 칸 이름(`s1_obj` 등) 그대로 |
| 칸 설명 | `FIELD_HELP`, `GLOSS` | 새 칸에 맞춰 |
| 주제 예시 | `CARDS` | 분수 물줄기, 던진 공, 다리 아치 등 |
| 평가요소 | `ELEMENTS`, `GRD_HEAD`(`Code.gs`) | 새 이름 4개 |
| 모델 | `FORMS`, `FORM_TEX`, `fval` 등 엔진 | 이차함수 하나. `switch`의 `default`가 마지막 형태로 떨어지므로 빠뜨림 주의 |
| 전용 도구 | `rate`·`riemann`, `riemannHTML`, `RATE_ROWS`, `derivCharts` | 빼고, **행렬 확인 도구**(가우스 소거·역행렬·행렬식)와 **변환 그리기 도구**(좌표평면, 행렬 2~3개 합성, 행렬식, 그림 저장)를 새로 만든다 |
| 키보드 | `mathtool.js`의 `KEYS` | '삼각함수'·'모델 식' 갈래를 행렬 입력(`pmatrix`, `\det`, $A^{-1}$) 위주로 |
| 문서 | `buildSpec`의 `toolParts`, `DOC_HEADER`, `DOC_INTRO`, `DOC_STD` | 고급 대수·성취기준 |
| 이름 문구 | `FOLDER_NAME`, `BACKUP_FOLDER_NAME`, `CONSENT_FOLDER_NAME`, 문서·PDF 이름, `service`, `<title>`, `.brand` | '모델링탐구'를 새 이름으로. **원본과 같은 Gmail이면 폴더가 섞이므로 반드시 바꾼다** |

### 숫자로 박힌 곳
| 바꾸는 것 | 함께 고칠 곳 |
|---|---|
| 단계 수 5 → 4 | `/5*100` 네 곳(`dashHTML`, 저장 요약, `submitHTML`, `statusHTML`), `SHORT`, `STAGE_LOOK`('나의 수행' 포함 6 → 5개), `buildSpec`·`footNav`의 `si===4` → `si===3`, `progress`·`stageFields`의 단계 번호 예외 |
| 요소 배점 10 → 30·30·20·20 | `gradeForm`의 `[10,8,6,4,2]`·`max="10"`, 알림판의 `/ 10`, 총점 표시. 요소 개수(4)는 그대로라 `[1,2,3,4]` 자리는 유지 |
| 자료 목표 20 → 10 | `DATA_GOAL` + 안내 글의 '20개' |
| 머리글 배열 | `REC_HEAD`, `GRD_HEAD`는 새 시트에서 시작하므로 지금 바꿔도 된다 |

### 원본에서 넘어온 남은 과제 (함께 고치면 좋음)
- ~~수식 키보드 '오차율' 단추가 옛 식을 넣는다~~ → 11/4 고침
- `api()`의 `fetch`에 제한 시간이 없어 서버 무응답 때 '불러오는 중'에서 멈춘다
- 자료표 중간에 줄을 넣으면 x 자동 채우기가 아래 줄과 겹칠 수 있다(`dg*`)

## 파일

| 파일 | 역할 |
|---|---|
| `index.html` | 화면 전체. GitHub Pages: https://izzy2020-teach.github.io/algebra-assessment/ (저장소 Izzy2020-teach/algebra-assessment, main / root, push 후 1~2분 뒤 반영) |
| `config.js` | `apiUrl` = Apps Script 웹 앱 주소(끝이 `/exec`). 서버는 g03535@cberi.go.kr 계정, 배포 ID `AKfycbw3_zWWdOd_vexJVgqSrYkXTrhEldBwGy3pt_aVlFA0vfbX00g1qNlhQcIU37NR8IlAVw` — 갱신은 항상 `clasp deploy -i <이 ID>` |
| `mathtool.js` | 수식 입력기(MathLive 0.110.0, 필요할 때만 불러옴) + 갈래별 키보드 + 기호 그리기(기기 안에서 동작, API 없음) |
| `apps-script/Code.gs` | 저장 서버. `doPost` JSON API, 계정·저장·채점·공지. 시트 `설정`·`계정`·`기록`·`채점`·`문서` |
| `apps-script/Docs.gs` | 화면이 보낸 문서 명세(spec)를 구글 문서로 그리고 PDF로 내보냄. 문항이 바뀌어도 고칠 필요 없음 |
| `apps-script/Files.gs` | 학생별 폴더 `[학번_이름]`: 보고서·자료·풀이 사진(50KB 이하)·평가 결과 문서(`writeGradeDoc_`, 요소 수·배점 확인 필요) |
| `apps-script/Math.gs` | 문서에 수식 그림 넣기(MathJax SVG → PNG, 대체 텍스트 = LaTeX) |
| `apps-script/Consent.gs` | 개인정보 동의 팝업·서명·동의서 문서. `CONSENT_ORG`·`CONSENT_KEEP`·`CONSENT_VER`는 여기 한 곳 |
| `apps-script/Backup.gs` | 매일 기록 스프레드시트 사본(7일 보관), `installBackup` 1회 실행 |
| `apps-script/Version.gs` | **자동 생성**, 화면 버전 번호 |
| `apps-script/app.html` | **자동 생성**(`node tools/build-gas.js`): index+config+mathtool을 합친 예비 화면. 직접 고치지 않는다 |
| `apps-script/appsscript.json` | 매니페스트(실행 계정 나, 액세스 `ANYONE_ANONYMOUS`) |
| `.claude/launch.json` | 로컬 시험 서버 `npx serve -l 5173 .` |
| `design/student-mockups.html` | 원본 디자인 시안(적용안 = B 배치 + A 색·폰트) |

**계산은 전부 화면(`index.html`)에서 한다.** 수학 엔진은 `index.html`에만 있고 `.gs`에는 없다. 서버는 화면이 보낸 요약(`summary.progress`, `n`, `formA`, `errA`)을 시트에 적기만 한다. 학생 답은 `기록` 시트 '데이터' 열의 JSON(`doc`)이다:
`doc = { v, info:{sid,name,title}, ans:{칸 이름:값}, data:{xs,ys,grid}, models, calc, checks, tools, photos, files, act, status, updatedAt, submittedAt, reopenAt }`

## 원본에서 그대로 쓰는 구조 (바꾸지 않는다)

- 가입·로그인: 학생은 한글 이름 2~10자 + 학번 숫자 5자리, 교사는 가입 코드. 규칙은 서버 `apiSignup_`과 화면 가입 폼 양쪽에 있다. 비밀번호는 salt+해시로 `계정` 시트에만.
- 자동 저장·충돌·손상 방지: `apiSave_`, `base`/`CONFLICT`, `parseParts_`/`CORRUPT`, `CHUNK`(4만 자씩 나눠 저장). **바꾸지 않는다.**
- 교사 대시보드·공지(설정 시트 `공지 목록` JSON)·마감·다시 열기·평가 결과 알림(알림판 + 팝업).
- 활동 시간 기록 `doc.act`. 개인정보 동의는 학생만, 서버가 `consent`를 보낼 때만 팝업(옛 서버와도 안전). 서버는 동의 없이 `save`를 막지 않는다.
- 칸 규칙: 칸 이름 `s<단계>_<이름>`, 저장은 `doc.ans[칸]`. 입력칸에 `placeholder=" "`가 있어야 빈칸 초록 표시가 된다. 라벨의 수식은 `$…$` + `tx()`. 칸 종류 `math`(수식만)·`longmath`(글+수식+사진, `-AI를 활용함-` 단추). 블록 `{n,t,hint,star,opt,f:[[키,라벨,종류,옵션]],table,tool,check,help,chart,last}`, `opt`는 작성률에서 빠진다.
- 학생 화면·보고서에는 배점을 쓰지 않는다. 교사 채점 칸과 공개한 점수에만 점수를 보인다.

## 업데이트 안전장치

- **버전 번호**: `build-gas.js`가 화면 파일이 바뀔 때만 `APP_VER`를 새로 매겨 `index.html`과 `Version.gs`에 적는다. 화면은 서버 번호가 더 새로우면 [저장하고 새로고침] 알림을 띄운다. 그래서 **화면(Pages) → 서버(deploy -i)** 순서로 배포한다.
- 서버 응답에 값이 있을 때만 새 화면 기능이 켜지게 하면 어느 쪽을 먼저 올려도 안전하다(동의 팝업 방식).
- 기록·채점·계정 시트에 열을 끼워 넣거나 데이터 칸을 고치지 않는다(정렬·필터·숨기기는 괜찮음).
- 계산식을 바꿔도 학생이 칸에 넣은 옛 값은 그대로다. 공지와 채점 재확인을 함께 한다.
- 공개 상태에서 점수를 고치면 저장마다 학생 팝업이 뜬다. 여러 번 손볼 때는 공개를 끄고 고친 뒤 마지막에 공개한다.

## 시험 방법

- 로컬 화면: `.claude/launch.json`의 설정으로 띄우고, 서버 호출을 가짜로 바꿔 시험한다(실제 기록에 시험 데이터가 섞이지 않게). 시험 전용 스프레드시트에 연결해도 된다.
- 엔진·도구: 교사용 검산 예시(`../활동지_초안.md` 끝)로 확인한다. 예: 세 점 $(0,1),(1,2),(2,5)$ → $y=x^2+1$, $\det A=-2$.
- 렌더링: 모든 단계를 빈 기록과 꽉 찬 기록으로 그려 오류를 확인한다. 휴대폰 폭(≤560px)도 본다.
- 문서: 긴 수식·빈칸·사진·그래프·변환 그림이 든 기록으로 실제 PDF를 받아 본다.
- 외부 실패: 구글 서비스 일시 오류는 가짜 객체로 '정상 / 1번 실패 후 성공 / 모두 실패'를 시험한다.
- 린트: 저장소에 린터가 없으므로 임시 폴더에서 ESLint를 돌려 새 경고만 확인한다.

## 저장 서버 배포 (clasp)

1. `npm i -g @google/clasp`
2. 교사가 https://script.google.com/home/usersettings 에서 **Google Apps Script API**를 켠다(개인 Gmail).
3. `clasp login` — 브라우저 로그인은 교사가 직접 한다.
4. `clasp create --type sheets --title "<새 스프레드시트 제목>" --rootDir apps-script` → 덮어써진 `appsscript.json`은 저장소 버전으로 되돌린다.
5. `node tools/build-gas.js` → `clasp push`
6. 편집기(`clasp open-script`)에서 교사가 **setup**을 실행해 권한을 허용하고, **installBackup**도 실행한다. `설정` 시트에 교사 가입 코드가 생긴다.
7. `clasp deploy` → `https://script.google.com/macros/s/<배포ID>/exec`. 시크릿 창에서 `/exec?ping=1`이 `{"ok":true,...}`인지 확인한다.
8. `config.js`의 `apiUrl`에 주소를 넣고 `node tools/build-gas.js` → 커밋·`git push`, GitHub Pages 켜기(main / root).
9. `clasp push` → `clasp deploy -i <배포ID>`(기존 배포 갱신. `-i` 없이 하면 주소가 바뀐다).
10. 리허설: 교사 가입 → 시험 학생으로 처음부터 제출·채점·공개·PDF까지 → 시험 계정 삭제.

화면을 고친 뒤 순서: `node tools/build-gas.js` → 커밋·`git push` → Pages 반영 확인 → `clasp push` → `clasp deploy -i <배포ID>`.

## 주의 · 기술 함정

- `.clasp.json`은 새 서버를 만들 때 생긴다(커밋해도 됨). 자격 증명 `~/.clasprc.json`은 절대 커밋하지 않는다.
- `/dev` 주소는 학생에게 동작하지 않는다. 항상 `/exec`. 액세스는 반드시 `ANYONE_ANONYMOUS`.
- 권한 범위를 늘렸으면 `setup`으로 권한 허용 **후** `deploy -i`. 순서가 바뀌면 학생 기능이 멈출 수 있다.
- 학교망이 `github.io`를 막을 수 있다 → 예비 주소 `/exec`를 함께 안내. `/exec`는 iframe 안이라 `localStorage`가 막힐 수 있어 모든 접근을 try/catch로 감싼다.
- 학교망이 TLS를 가로채면 Node/npm에 `NODE_OPTIONS=--use-system-ca`를 쓴다. TLS 검증을 끄지 않는다.
- 구글 문서에 빈 글자 삽입 금지: `setText('')`, `appendText('')`, `appendParagraph('')` 대신 `removeChild`와 `' '`.
- 구글 문서 그림 넣기 일시 실패: 다시 시도 후 건너뛰고 다음 반영 때 다시 그린다(원본에 구현됨).
- MathLive 0.110: `math-field`를 지웠다 다시 만들면 내부 오류 → 하나를 재사용(`#mt-park`), 포커스는 `focusMF`.
- 긴 수식은 바깥 `+`·`−`에서 나눈다(`MathTool.splitLong`, 화면과 보고서 그림 모두). `{}`, `\left…\right`, `\begin…\end`(행렬) 안에서는 나누지 않는다(11/4, 회전행렬이 깨지던 버그를 고침).
- 로컬 시험 서버: 상위 폴더 `.claude/launch.json`의 `algebra-app`(내려받기 없는 `node .claude/static-server.js app 5173`). 학교망에서 `npx serve` 내려받기가 안 됐다.
- 셀 글자 수 한도 5만 자 → `CHUNK`로 나눠 저장(이미 처리됨).
