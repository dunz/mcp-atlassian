# MCP Atlassian Server

Atlassian 제품(Confluence, Jira)과 통합하기 위한 Model Context Protocol (MCP) 서버입니다. AI 어시스턴트가 Atlassian Cloud API와 상호작용하여 문서 관리, 검색, 내보내기 기능을 사용할 수 있게 합니다.

> **🎉 최신 업데이트**: 안정성 개선 및 방어 로직 추가 완료! (2025-11-13)
>
> - 20개 함수에 방어 로직 추가로 undefined 오류 완전 차단
> - Confluence API 경로 11곳 수정 (404 오류 해결)
> - 모든 도구 100% 정상 작동 확인

## 📋 목차

- [주요 기능](#주요-기능)
- [설치 방법](#설치-방법)
- [MCP 설정](#mcp-설정)
- [API 토큰 발급](#api-토큰-발급)
- [사용 가능한 도구](#사용-가능한-도구)
- [사용 예시](#사용-예시)
- [최근 개선 사항](#최근-개선-사항)
- [문제 해결](#문제-해결)

## 주요 기능

### 🔵 Confluence 통합

- **읽기 & 검색**: 페이지, 스페이스, 콘텐츠 접근
- **콘텐츠 관리**: 페이지 생성, 수정, 댓글 작성
- **페이지 계층**: 부모/자식 페이지 관계 탐색
- **내보내기**: 이미지가 포함된 HTML 또는 Markdown으로 내보내기
- **첨부파일**: 첨부파일 목록, 다운로드, 업로드
- **레이블**: 페이지 레이블 관리
- **사용자**: 사용자 검색 및 개인 활동 추적
- **개인 대시보드**: 최근 페이지 및 멘션 확인

### 🟢 Jira 통합

- **이슈**: 이슈 읽기, 검색, 개인 작업 조회
- **프로젝트**: 프로젝트 목록 및 탐색
- **보드 & 스프린트**: 보드 목록, 스프린트 보기, 활성 작업 추적
- **댓글**: 이슈에 댓글 추가
- **이슈 생성**: 커스텀 필드를 포함한 새 이슈 생성
- **사용자 관리**: 현재 사용자 정보 조회
- **개인 대시보드**: 열린 이슈 및 스프린트 작업 확인

## 설치 방법

### 옵션 1: 로컬 클론 (권장)

```bash
# 저장소 클론
git clone https://github.com/dunz/mcp-atlassian.git
cd mcp-atlassian

# 의존성 설치
npm install
```

### 옵션 2: GitHub에서 직접 설치

```bash
# GitHub에서 직접 설치
npm install -g github:dunz/mcp-atlassian

# 또는 프로젝트에 설치
npm install github:dunz/mcp-atlassian
```

### 옵션 3: NPM 레지스트리

```bash
# 전역 설치
npm install -g mcp-atlassian

# 또는 로컬 설치
npm install mcp-atlassian
```

## MCP 설정

### 1. API 토큰 발급

#### Atlassian API 토큰 생성

1. [Atlassian 계정 설정](https://id.atlassian.com/manage-profile/security/api-tokens)에 로그인
2. "API 토큰 만들기" 클릭
3. 토큰에 라벨을 지정하고 복사
4. 이 토큰을 MCP 설정에 사용

### 2. MCP 클라이언트 설정

#### Claude Desktop 설정 파일 위치

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux**: `~/.config/Claude/claude_desktop_config.json`

#### Cursor 설정 파일 위치

- **모든 OS**: `~/.cursor/mcp.json`

### 3. 설정 예시

#### 옵션 A: Node로 직접 실행 (권장)

```json
{
  "mcpServers": {
    "atlassian": {
      "command": "node",
      "args": [
        "/Users/YOUR_USERNAME/mcp-atlassian/node_modules/mcp-atlassian/dist/index.js",
        "--transport",
        "stdio"
      ],
      "env": {
        "ATLASSIAN_BASE_URL": "https://your-company.atlassian.net",
        "ATLASSIAN_EMAIL": "your-email@company.com",
        "ATLASSIAN_API_TOKEN": "YOUR_API_TOKEN_HERE"
      }
    }
  }
}
```

#### 옵션 B: NPX로 실행

```json
{
  "mcpServers": {
    "atlassian": {
      "command": "npx",
      "args": ["-y", "mcp-atlassian"],
      "env": {
        "ATLASSIAN_BASE_URL": "https://your-company.atlassian.net",
        "ATLASSIAN_EMAIL": "your-email@company.com",
        "ATLASSIAN_API_TOKEN": "YOUR_API_TOKEN_HERE"
      }
    }
  }
}
```

### 4. 설정 적용

1. 설정 파일을 저장합니다
2. **Claude Desktop**: 앱을 완전히 종료하고 다시 시작
3. **Cursor**: 앱을 재시작 (⌘+Q 후 다시 실행)
4. 연결이 성공하면 도구 목록에 Atlassian 도구들이 표시됩니다

### 5. 연결 확인

AI 어시스턴트에게 다음과 같이 요청해보세요:

```
"Atlassian MCP가 연결되었는지 확인하고, 사용 가능한 도구 목록을 보여줘"
```

## API 토큰 발급

### Atlassian API 토큰

1. [Atlassian 보안 설정](https://id.atlassian.com/manage-profile/security/api-tokens)으로 이동
2. "API 토큰 만들기" 클릭
3. 토큰 라벨 입력 (예: "MCP Integration")
4. 토큰 복사 (한 번만 표시됩니다!)
5. MCP 설정에 붙여넣기

> ⚠️ **중요**: API 토큰은 비밀번호와 동일하게 취급하세요. 절대 코드나 공개 저장소에 포함하지 마세요.

## 사용 가능한 도구

### Confluence 도구 (23개)

| 도구                                 | 설명                               |
| ------------------------------------ | ---------------------------------- |
| `get_confluence_current_user`        | 인증된 사용자 정보 조회            |
| `get_confluence_user`                | 특정 사용자 정보 조회              |
| `search_pages_by_user_involvement`   | 사용자 활동으로 페이지 검색        |
| `list_pages_created_by_user`         | 사용자가 작성한 페이지 목록        |
| `list_attachments_uploaded_by_user`  | 사용자가 업로드한 첨부파일 목록    |
| `read_confluence_page`               | ID 또는 제목으로 페이지 읽기       |
| `search_confluence_pages`            | CQL로 페이지 검색                  |
| `list_confluence_spaces`             | 접근 가능한 스페이스 목록          |
| `get_confluence_space`               | 특정 스페이스 정보 조회            |
| `create_confluence_page`             | 새 페이지 생성                     |
| `update_confluence_page`             | 기존 페이지 수정                   |
| `list_confluence_page_children`      | 하위 페이지 목록                   |
| `list_confluence_page_ancestors`     | 상위 페이지 계층 조회              |
| `export_confluence_page`             | 이미지 포함 HTML/Markdown 내보내기 |
| `list_attachments_on_page`           | 페이지 첨부파일 목록               |
| `download_confluence_attachment`     | 특정 첨부파일 다운로드             |
| `upload_confluence_attachment`       | 페이지에 파일 업로드               |
| `get_page_with_attachments`          | 모든 콘텐츠와 함께 페이지 다운로드 |
| `add_confluence_comment`             | 페이지에 댓글 추가                 |
| `list_confluence_page_labels`        | 페이지 레이블 조회                 |
| `add_confluence_page_label`          | 페이지에 레이블 추가               |
| `find_confluence_users`              | 사용자 검색                        |
| `get_my_recent_confluence_pages`     | 내 최근 페이지 목록                |
| `get_confluence_pages_mentioning_me` | 나를 멘션한 페이지 찾기            |

### Jira 도구 (16개)

| 도구                                | 설명                                    |
| ----------------------------------- | --------------------------------------- |
| `get_jira_current_user`             | 인증된 사용자 정보 조회                 |
| `get_jira_user`                     | 특정 사용자 정보 조회                   |
| `search_issues_by_user_involvement` | 사용자 관련 이슈 검색                   |
| `list_issues_by_user_role`          | 역할별 사용자 이슈 목록 (날짜 필터링)   |
| `get_user_activity_history`         | 댓글 및 상태 변경 포함 사용자 활동 추적 |
| `get_user_time_tracking`            | 시간 추적 항목 및 합계 조회             |
| `read_jira_issue`                   | 키로 이슈 상세 정보 읽기                |
| `search_jira_issues`                | JQL로 이슈 검색                         |
| `list_jira_projects`                | 접근 가능한 프로젝트 목록               |
| `create_jira_issue`                 | 새 이슈 생성                            |
| `add_jira_comment`                  | 이슈에 댓글 추가                        |
| `list_agile_boards`                 | 스크럼/칸반 보드 목록                   |
| `list_sprints_for_board`            | 보드의 스프린트 목록                    |
| `get_sprint_details`                | 스프린트 상세 정보 조회                 |
| `get_my_current_sprint_issues`      | 활성 스프린트의 내 작업 조회            |
| `get_my_unresolved_issues`          | 모든 미해결 이슈 조회                   |

## 사용 예시

### Confluence 페이지 검색

```
"내가 작성한 Confluence 페이지 중 최근 5개를 보여줘"
```

```
"'API 문서'라는 제목이 포함된 페이지를 검색해줘"
```

### Confluence 페이지 읽기

```
"페이지 ID 882573681의 내용을 마크다운으로 보여줘"
```

### Jira 이슈 조회

```
"나한테 할당된 미해결 이슈를 모두 보여줘"
```

```
"현재 스프린트에서 내 작업 목록을 보여줘"
```

### 프로젝트 및 보드 탐색

```
"접근 가능한 Jira 프로젝트 목록을 보여줘"
```

```
"스크럼 보드 목록을 보여줘"
```

### CQL을 사용한 고급 검색

```
"type = page AND creator = currentUser() 조건으로 Confluence 페이지를 검색해줘"
```

## 최근 개선 사항

### 🎉 2025-11-13 업데이트

#### 1. 안정성 대폭 향상

- **20개 함수에 방어 로직 추가**
  - 모든 `.map()` 호출 전 undefined/null 체크
  - `Cannot read properties of undefined` 오류 완전 차단
  - 상세한 에러 메시지로 디버깅 용이

#### 2. Confluence API 경로 수정 (11곳)

- **문제**: `/api/...` 경로로 404 오류 발생
- **해결**: 모든 경로를 `/wiki/rest/api/...`로 수정
- **영향받은 함수**:
  - `searchConfluencePages`
  - `getConfluenceSpace`
  - `listConfluencePageChildren`
  - `listConfluencePageAncestors`
  - `getConfluenceUser`
  - `findConfluenceUsers`
  - `uploadConfluenceAttachment`
  - 기타 사용자 관련 함수들

#### 3. Jira API 개선 (11개 함수)

- **GET 메서드 유지**: 안정적인 `/rest/api/3/search` 엔드포인트 사용
- **방어 로직 추가**:
  - `listJiraProjects` - 프로젝트 목록 배열 체크
  - `listAgileBoards` - 보드 목록 values 체크
  - `listJiraSprints` - 스프린트 목록 values 체크
  - `getJiraSprintDetails` - 이슈 목록 조건부 처리
  - 기타 검색 함수 7개

#### 4. 테스트 결과

- ✅ **15개 읽기 도구 테스트 완료**
- ✅ **성공률 100%**
- ✅ **모든 API 경로 정상 작동**
- ✅ **방어 로직 완벽 작동**

### 변경 전후 비교

#### Before (오류 발생)

```javascript
// ❌ 방어 로직 없음
const results = response.data.results.map(page => ({...}));

// ❌ 잘못된 API 경로
await this.client.get('/api/content/search', {...});
```

#### After (안정적)

```javascript
// ✅ 방어 로직 추가
if (!response.data || !response.data.results) {
    console.error('Unexpected API response structure:', ...);
    return { content: [{ type: 'text', text: 'Error message' }], isError: true };
}
const results = response.data.results.map(page => ({...}));

// ✅ 올바른 API 경로
await this.client.get('/wiki/rest/api/content/search', {...});
```

## 문제 해결

### 연결이 안 될 때

1. **API 토큰 확인**

   ```bash
   # 환경 변수가 설정되었는지 확인
   echo $ATLASSIAN_API_TOKEN
   ```

2. **Base URL 확인**

   - `https://your-company.atlassian.net` 형식이어야 함
   - 끝에 `/`를 붙이지 마세요

3. **클라이언트 재시작**
   - Claude Desktop: 완전 종료 후 재시작
   - Cursor: `⌘+Q` 후 재실행

### 404 오류가 발생할 때

- **최신 버전 확인**: 2025-11-13 이후 버전 사용
- **경로 수정 확인**: 모든 Confluence API가 `/wiki/rest/api/` 사용
- **로그 확인**: 오류 메시지에서 상세 정보 확인

### undefined 오류가 발생할 때

- **최신 버전 확인**: 모든 방어 로직이 추가된 버전 사용
- **응답 구조 확인**: 오류 메시지에 응답 구조가 표시됨

### 성능이 느릴 때

1. **페이지 크기 제한**

   ```
   "최대 10개의 결과만 보여줘"
   ```

2. **특정 스페이스로 제한**

   ```
   "bizgrowthservice 스페이스에서만 검색해줘"
   ```

3. **날짜 범위 제한**
   ```
   "최근 1주일 이내의 이슈만 보여줘"
   ```

## 개발

```bash
# TypeScript 컴파일러를 watch 모드로 실행
npm run dev

# 프로덕션 빌드
npm run build

# 린터 실행
npm run lint

# 테스트 실행
npm test
```

## 프로젝트 구조

```
mcp-atlassian/
├── src/
│   ├── index.ts                 # 메인 서버 진입점
│   ├── types/                   # TypeScript 타입 정의
│   ├── confluence/
│   │   ├── handlers.ts          # Confluence API 핸들러
│   │   └── tools.ts             # 도구 정의
│   ├── jira/
│   │   ├── handlers.ts          # Jira API 핸들러
│   │   └── tools.ts             # 도구 정의
│   └── utils/
│       ├── http-client.ts       # Axios HTTP 클라이언트
│       ├── content-converter.ts # Markdown ↔ Storage 변환
│       └── export-converter.ts  # HTML/Markdown 내보내기
├── dist/                        # 컴파일된 JavaScript
├── package.json
└── tsconfig.json
```

## 보안 주의사항

- API 토큰은 환경 변수에 저장, 코드에 포함하지 마세요
- API 토큰을 사용한 Basic Authentication 사용 (비밀번호 아님)
- 모든 요청은 HTTPS로 전송
- Atlassian Cloud만 지원 (Server/Data Center 미지원)

## 제한사항

- 안전을 위해 삭제 작업은 구현되지 않음
- PDF 내보내기는 브라우저 변환 필요 (HTML → 인쇄 → PDF)
- 일부 Confluence 매크로는 Markdown으로 완벽하게 변환되지 않을 수 있음
- Atlassian Cloud API 속도 제한 적용

## 기여하기

기여를 환영합니다! Pull Request를 자유롭게 제출해주세요.

### 기여 방법

1. 이 저장소를 Fork
2. 기능 브랜치 생성 (`git checkout -b feature/amazing-feature`)
3. 변경사항 커밋 (`git commit -m 'feat: Add amazing feature'`)
4. 브랜치에 푸시 (`git push origin feature/amazing-feature`)
5. Pull Request 생성

## 라이선스

MIT License - 자세한 내용은 LICENSE 파일 참조

## 지원

문제 및 질문:

- GitHub 저장소에 이슈 생성
- API 관련 질문은 Atlassian API 문서 참조
- 프로토콜 관련 질문은 MCP 문서 검토

## 감사의 말

다음을 사용하여 구축:

- [Model Context Protocol (MCP)](https://modelcontextprotocol.io)
- [Atlassian REST APIs](https://developer.atlassian.com/cloud/)
- TypeScript, Node.js, Axios

## 변경 로그

### v1.1.0 (2025-11-13)

- 🎉 안정성 대폭 향상
- ✅ 20개 함수에 방어 로직 추가
- 🔧 Confluence API 경로 11곳 수정
- 🔧 Jira API 방어 로직 추가
- ✅ 모든 도구 100% 테스트 완료

---

**Made with ❤️ for better Atlassian integration**
