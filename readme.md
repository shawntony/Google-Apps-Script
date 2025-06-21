# VSCode Google Apps Script 개발 환경 구축

> VSCode에서 Google Apps Script를 효율적으로 개발하기 위한 완전한 가이드

[![Node.js](https://img.shields.io/badge/Node.js->=14.0.0-green.svg)](https://nodejs.org/)
[![CLASP](https://img.shields.io/badge/CLASP-latest-blue.svg)](https://github.com/google/clasp)
[![VSCode](https://img.shields.io/badge/VSCode-latest-blue.svg)](https://code.visualstudio.com/)

## 📋 목차

- [개요](#개요)
- [사전 준비](#사전-준비)
- [설치 가이드](#설치-가이드)
- [VSCode 설정](#vscode-설정)
- [확장 프로그램](#확장-프로그램)
- [실습 가이드](#실습-가이드)
- [예제 프로젝트](#예제-프로젝트)
- [명령어 참고](#명령어-참고)
- [문제 해결](#문제-해결)

## 개요

Google Apps Script를 VSCode에서 개발하기 위해 Google의 공식 도구인 **CLASP**(Command Line Apps Script Projects)를 활용합니다.

### 주요 장점
- 🎯 **로컬 개발 환경** - VSCode의 강력한 에디팅 기능 활용
- 🔄 **버전 관리** - Git을 통한 소스코드 관리
- 🛠️ **자동완성** - TypeScript 지원 및 IntelliSense
- 📦 **확장성** - npm 패키지 및 외부 라이브러리 사용 가능

## 사전 준비

다음 항목들이 설치되어 있어야 합니다:

- [ ] **Google 계정** (Gmail)
- [ ] **Node.js** (v14 이상) - [다운로드](https://nodejs.org/)
- [ ] **VSCode** - [다운로드](https://code.visualstudio.com/)

### 설치 확인

```bash
node -v    # Node.js 버전 확인
npm -v     # npm 버전 확인
code -v    # VSCode 버전 확인
```

## 설치 가이드

### 1. CLASP 설치

```bash
# CLASP 전역 설치
npm install -g @google/clasp

# 설치 확인
clasp -v
```

### 2. Google Apps Script API 활성화

1. [Google Apps Script API 설정](https://script.google.com/home/usersettings) 접속
2. **"Google Apps Script API"**를 **"On"**으로 변경

### 3. CLASP 로그인

```bash
clasp login
```

브라우저에서 Google 계정 인증 후 권한 허용

### 4. 프로젝트 초기화

```bash
# 새 프로젝트 폴더 생성
mkdir my-gas-project
cd my-gas-project

# npm 초기화
npm init -y

# GAS 프로젝트 생성
clasp create --type standalone --title "My GAS Project"
```

## VSCode 설정

### 프로젝트 열기

```bash
# VSCode에서 프로젝트 열기
code .
```

### 타입 지원 설치

```bash
# Google Apps Script 타입 정의 설치
npm install --save-dev @types/google-apps-script
```

### VSCode 설정 파일

`.vscode/settings.json` 파일 생성:

```json
{
  "files.associations": {
    "*.gs": "javascript"
  },
  "emmet.includeLanguages": {
    "javascript": "javascriptreact"
  },
  "google-apps-script.clasp.enable": true,
  "google-apps-script.snippets.enable": true
}
```

## 확장 프로그램

### 필수 확장 프로그램

다음 확장 프로그램들을 설치하세요:

1. **Google Apps Script** (labnol.google-apps-script)
   - 구문 강조, 자동완성, 코드 스니펫 제공

2. **JavaScript (ES6) code snippets**
   - JavaScript ES6 코드 스니펫

3. **Prettier** 
   - 코드 자동 포맷팅

4. **ESLint**
   - 코드 품질 검사

### 확장 프로그램 설치 방법

1. `Ctrl + Shift + X` - Extensions 열기
2. "Google Apps Script" 검색
3. 설치 버튼 클릭

### 커스텀 스니펫 설정

`Ctrl + Shift + P` → "Preferences: Configure User Snippets" → "javascript.json":

```json
{
  "Gmail Send Email": {
    "prefix": "gas-gmail",
    "body": [
      "function sendEmail() {",
      "  const recipient = '${1:email@example.com}';",
      "  const subject = '${2:Subject}';",
      "  const body = '${3:Email body}';",
      "  ",
      "  GmailApp.sendEmail(recipient, subject, body);",
      "  console.log('Email sent!');",
      "}"
    ],
    "description": "Gmail 이메일 발송 함수"
  },
  "Sheets Read Data": {
    "prefix": "gas-sheets",
    "body": [
      "function readSheetData() {",
      "  const sheetId = '${1:SHEET_ID}';",
      "  const spreadsheet = SpreadsheetApp.openById(sheetId);",
      "  const sheet = spreadsheet.getActiveSheet();",
      "  const data = sheet.getDataRange().getValues();",
      "  ",
      "  console.log(data);",
      "  return data;",
      "}"
    ],
    "description": "Google Sheets 데이터 읽기"
  }
}
```

## 실습 가이드

### 1. 첫 번째 함수 작성

`Code.js` 파일 생성:

```javascript
/**
 * Hello World 함수
 */
function myFunction() {
  console.log('Hello from VSCode!');
  return 'Hello, Google Apps Script!';
}

/**
 * Google Sheets 읽기 예제
 */
function readSheet() {
  // 본인의 Sheets ID로 변경하세요
  const sheetId = 'YOUR_SHEET_ID';
  const spreadsheet = SpreadsheetApp.openById(sheetId);
  const sheet = spreadsheet.getActiveSheet();
  const data = sheet.getDataRange().getValues();
  
  console.log('Sheet data:', data);
  return data;
}
```

### 2. 코드 업로드 및 실행

```bash
# 코드를 Google Apps Script 서버로 업로드
clasp push

# 브라우저에서 프로젝트 열기
clasp open
```

Google Apps Script 에디터에서 함수를 선택하고 실행 버튼(▶️) 클릭

### 3. 로그 확인

```bash
# 실행 로그 확인
clasp logs

# 실시간 로그 모니터링
clasp logs --watch
```

## 예제 프로젝트

### 일일 업무 리포트 자동화

완전한 예제 프로젝트를 만들어보겠습니다.

#### 프로젝트 구조

```
daily-report-automation/
├── src/
│   ├── main.js           # 메인 실행 함수
│   ├── gmailHandler.js   # Gmail 처리
│   ├── sheetsHandler.js  # Sheets 처리
│   ├── calendarHandler.js # Calendar 처리
│   └── reportGenerator.js # 리포트 생성
├── config/
│   └── settings.js       # 설정값
├── utils/
│   └── helpers.js        # 유틸리티 함수
├── .claspignore          # 업로드 제외 파일
├── clasp.json           # CLASP 설정
└── package.json         # npm 설정
```

#### 주요 기능

- 📧 **Gmail 자동 수집** - 특정 라벨의 이메일 분석
- 📊 **Google Sheets 연동** - 데이터 자동 정리
- 📅 **Calendar 통합** - 일정 요약
- 📱 **자동 리포트 발송** - HTML 리포트 이메일 발송

#### config/settings.js

```javascript
const CONFIG = {
  REPORT_SHEET_ID: 'YOUR_SHEET_ID_HERE',
  REPORT_SHEET_NAME: 'DailyReport',
  GMAIL_LABELS: ['업무', '중요', '프로젝트'],
  REPORT_RECIPIENTS: ['manager@company.com'],
  TIMEZONE: 'Asia/Seoul'
};
```

#### src/main.js

```javascript
function runDailyReportAutomation() {
  console.log('=== 일일 리포트 자동화 시작 ===');
  
  try {
    // 데이터 수집
    const emailData = getTodayEmails();
    const calendarData = getTodayCalendarEvents();
    
    // Sheets에 저장
    saveReportToSheets(emailData, calendarData);
    
    // 리포트 생성 및 발송
    generateAndSendDailyReport(emailData, calendarData);
    
    console.log('=== 자동화 완료 ===');
    
  } catch (error) {
    console.error('오류 발생:', error);
  }
}
```

#### 자동화 트리거 설정

1. Google Apps Script 에디터에서 ⏰ "트리거" 메뉴
2. "트리거 추가" 클릭
3. 설정:
   - **함수**: `runDailyReportAutomation`
   - **이벤트 소스**: 시간 기반
   - **트리거 유형**: 일 타이머
   - **시간**: 오후 6-7시

## 명령어 참고

### 기본 CLASP 명령어

| 명령어 | 설명 |
|--------|------|
| `clasp login` | Google 계정 로그인 |
| `clasp logout` | 로그아웃 |
| `clasp create` | 새 프로젝트 생성 |
| `clasp clone <script-id>` | 기존 프로젝트 복제 |
| `clasp push` | 로컬 → 서버 업로드 |
| `clasp pull` | 서버 → 로컬 다운로드 |
| `clasp open` | 브라우저에서 프로젝트 열기 |
| `clasp deploy` | 프로젝트 배포 |
| `clasp logs` | 실행 로그 확인 |
| `clasp status` | 프로젝트 상태 확인 |

### 개발 워크플로우

```bash
# 1. 로컬에서 코드 작성 (VSCode)
# 2. 서버로 업로드
clasp push

# 3. 브라우저에서 테스트
clasp open

# 4. 로그 확인
clasp logs

# 5. 필요시 서버 변경사항 동기화
clasp pull
```

### .claspignore 설정

프로젝트 루트에 `.claspignore` 파일 생성:

```
node_modules/**
.git/**
*.md
package.json
package-lock.json
.env
.vscode/**
```

## 문제 해결

### 자주 발생하는 오류

#### 1. "clasp 명령어를 찾을 수 없음" (Windows)

```bash
# npm 전역 경로 확인
npm config get prefix

# PATH 환경변수에 다음 경로 추가
# C:\Users\사용자명\.npm-global
# 또는 C:\Users\사용자명\AppData\Roaming\npm
```

#### 2. "Permission denied" 오류

```bash
# 다시 로그인
clasp login
```

#### 3. "Script not found" 오류

`clasp.json`의 `scriptId`가 올바른지 확인:

```json
{
  "scriptId": "1ABC...XYZ",
  "rootDir": "./src"
}
```

#### 4. "Push failed" 오류

```bash
# 서버의 최신 버전 먼저 다운로드
clasp pull
```

### 디버깅 방법

```bash
# 상세 로그 확인
clasp logs --simplified

# 실시간 로그 모니터링
clasp logs --watch

# 프로젝트 정보 확인
clasp status
```

### VSCode 확장 프로그램 문제

1. **자동완성이 작동하지 않는 경우:**
   ```bash
   npm install --save-dev @types/google-apps-script
   ```

2. **구문 강조가 되지 않는 경우:**
   - 파일 확장자가 `.gs` 또는 `.js`인지 확인
   - VSCode 설정에서 파일 연결 확인

3. **확장 프로그램 재시작:**
   - `Ctrl + Shift + P` → "Developer: Reload Window"

## 고급 기능

### TypeScript 사용

```bash
# TypeScript 설치
npm install -g typescript
npm install --save-dev @types/google-apps-script

# tsconfig.json 생성
echo '{
  "compilerOptions": {
    "lib": ["es2015"],
    "experimentalDecorators": true
  }
}' > tsconfig.json
```

### 환경별 설정 관리

```javascript
// config.js
const CONFIG = {
  DEVELOPMENT: {
    SHEET_ID: 'DEV_SHEET_ID',
    DEBUG: true
  },
  PRODUCTION: {
    SHEET_ID: 'PROD_SHEET_ID', 
    DEBUG: false
  }
};

const ENV = 'DEVELOPMENT'; // 환경 변경
const config = CONFIG[ENV];
```

### Git 연동

```bash
# Git 초기화
git init
git add .
git commit -m "Initial commit"

# GitHub 연동
git remote add origin <repository-url>
git push -u origin main
```

## 추가 리소스

- 📚 [Google Apps Script 공식 문서](https://developers.google.com/apps-script)
- 🛠️ [CLASP GitHub](https://github.com/google/clasp)
- 💡 [Google Apps Script 예제](https://developers.google.com/apps-script/samples)
- 🎯 [VSCode 확장 프로그램](https://marketplace.visualstudio.com/items?itemName=labnol.google-apps-script)

## 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다.

---

**🎉 이제 VSCode에서 Google Apps Script를 효율적으로 개발할 수 있습니다!**

문제가 발생하거나 질문이 있으시면 Issues를 통해 문의해주세요.
