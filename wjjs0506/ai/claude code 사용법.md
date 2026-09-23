# Claude Code 사용법

## 설정 파일 (settings.json)

Claude Code에 적용이 될 설정 파일
  - 모델
  - 권한
  - 스타일
  - etc...

### 우선 순위

1순위 : 엔터프라이즈 관리 정책 설정
  - Claude code가 제공하는 각 OS에 따른 엔터프라이즈 관리 정
  - managed-settings.json 파일
    - mac : 

2순위 : 프로젝트 설정
  - 클로드 코드 하위 각 프로젝트 별 설정
    - .claude/setting.json 파일

3순위 : 사용자 개인 설정
  - 사용자가 제공하는 프로젝트 통합 설정
    - ~/.claude/settings.json 파일

### 프로젝트 설정 분리

- 팀과 공유할 설정(git upload) : .claude/settings.json에 관리
- 개인 설정 (local) : .claude/settgins.**local**.json에 관리 (.gitignore에 추가하여 사용)

### 공식 문서

https://code.claude.com/docs/ko/settings

## 메모리 관리(=행동 지침) (claude.md && memory.md)

Claude Code가 세션이 끊겼다 다시 시작해도 가지고 있을 수 있는 메모리 파일들. 보통 행동기록지침 내용을 작성한다.  
새로운 세션이 시작되면 해당 파일을 먼저 읽어 행동 지침을 파악하고 사용자의 요구사항에 맞는 행동을 수행한다.

`/memory`명령어를 통해 현재 실행중인 나의 메모리 파일들을 확인하고 열 수 있다.

### 권장사항

1. **각 지침은 검증 가능하며 구체적일 수록 좋다**
   - "코드를 적절히 포맷해줘" -> "2칸 들여쓰기 해줘"
   - "이메일 주소를 검증하는 함수를 구현해줘" -> "validateEmail 함수를 작성해줘 예제 테스트 케이스는 user@example.com은 true, invalid는 false, user@.com은 false야 구현 후 테스트 진행해줘"
2. **모든 규칙은 일관성이 유지되어야 한다**
   - 규칙이 서로 모순되면 임의로 하나를 선택해서 적용할 확률이 높아진다.
   - 아래 메모리 설정 유형들 내에서도 각 규칙이 일관성을 유지할 수 있도록 관리한다.
   - claudeMdExcludes를 통해 작업과 관련없는 다른 CLAUDE.md 파일을 건너 뛸 수 있도록 한다.
3. **md파일 형식을 통한 구조로 CLAUDE CODE가 읽기 쉽도록 한다.**
   - 중요한 내용은 헤더와 글머리 기호등을 활용하여 인식하기 쉽도록 한다.
4. **가능한 md 파일의 길이가 500자 이내로 해야 CLAUDE가 명확히 이해한다.**
   - 아래 규칙 가져오기로 Import해 전략적으로 규칙을 설정한다.

### 규칙 가져오기(Claude Import)

다른 프로젝트에서 사용하는 규칙을 참조할 경우 아래와 같이 설정한다.

`@path/to/import` 와 같이 "@" 이후 파일 경로(상대/절대 경로 모두 가능)와 파일명을 통해 작성한다. (그 외에 가져오기를 안하지만 인용이 필요한 경우 ``(백틱)으로 감싸면 Claude가 인식하지 못한다.)

```
프로젝트 개요는 @README를 참조하고 이 프로젝트의 사용 가능한 npm 명령어는 @package.json을 참조하십시오.

# 추가 지침
- git 워크플로우 @docs/git-instructions.md
```

### 메모리 설정 유형

각 메모리 파일에 YAML Frontmatter로 경로를 추가하면 적용되는 파일 범위를 따로 지정 할 수 있다.

```
---
paths:
  - "src/api/**/*.ts"
---
```

- 엔터프라이즈 정책 지침 메모리
  - 회사의 규정과 같이 전 인원이 지켜야할 코딩 표준, 보안 규정 준수와 같은 요구사항
  - 위치 : OS별로 상이
    - mac : /Library/Application/Support/ClaudeCode/CLAUDE.md
    - linux : /etc/claude-code/CLAUDE.md
    - window : C:\ProgramFiles\ClaudeCode\CLAUDE.md
- 프로젝트 지침 메모리
  - 프로젝트 폴더 내에 적용되는 행동 규칙
    - 아키텍처, 코딩 표준, 환경, git 규칙 등에 대한 내용 포함
  - 위치 : 프로젝트 하위 ./CLAUDE.md (.claude 하위도 가능)
- 프로젝트 규칙 (프로젝트 메모리에 포함)
  - 언어별 가이드라인, 테스트 규칙, API 표준 등
  - 위치 : 프로젝트 하위 ./.claude/rules/*.md
    - rules 폴더 밑에 있는 것이 중요
    ```
    your-project/
    ├── .claude/
    │   ├── CLAUDE.md           # 주 프로젝트 지침
    │   └── rules/
    │       ├── code-style.md   # 코드 스타일 가이드라인
    │       ├── testing.md      # 테스트 규칙
    │       └── security.md     # 보안 요구사항
    ```
- 사용자 지침 메모리
  - 개인 코드 스타일 선호, 단축키 등을 설정
  - 위치 : ~(사용자 개인 경로)/.claude/CLAUDE.md
- 프로젝트 로컬 지침 메모리(로컬)
  - 샌드박스 URL, 선호하는 데이터 등 로컬 작업에 필요한 정보 입력
  - 위치 : ./CLAUDE.local.md

### Memory.md

Claude가 각 프로젝트 프롬프트를 수행하면서 동작 중 기록이 필요한 사항들을 기록하는 파일

새로운 세션이 시작 시 시스템 프롬프트에서 처음 200줄을 로드해 사용한다.

## Ultra Think

claude code가 더 깊이 있는 사고를 할 수 있게 해주는 설정으로 프롬프트에서 명령어를 같이 실행해주면 수행 가능하다.

명령어에 따라 사용 가능한 토큰 예산을 세팅하고 수행한다.

1. think : 4,000 tokens
2. megathink : 10,000 tokens
3. ultrathink : maximum budget

해당 작업 진행 시 시각적 파일(이미지등의 자료)를 첨부하면 더 깊이 있는 응답을 받을 수 있다

> 💡ultrathink 키워드 변경 (Extended Thinking)  
> 26년 1월 16일부터 ultrathink 명령어는 deprecated되었고, default로 적용되게 변경되었다.  
> - 사고 토큰의 기본 값 : 31,999
>
> 사고 토큰의 기본 값을 변경하고 싶은 경우, ".claude" 내의 `settings.json` 파일에서 아래 내용의 수정하면 된다.
> ```json
>  "env": {
>    "MAX_THINKING_TOKENS": "변경할 토큰 값"
>  }
> ```


## Status Line

아래 사진과 같이 claude code의 작업 중인 창의 현재 세션의 사용자 정의 상태 표시줄을 구성할 수 있다.

아래 사진은 모델, 컨텍스트 사용량, 비용으로 구성된 Status Line이다.

![statusline](../assets/images/ai/claude%20code%20사용법/statusline.png)

### 적용 방법

claude code 창에서 `/statusline 명령(ex. 모델, 컨텍스트 사용량, 비용을 색상을 구분해서 나타내줘)`을 수행하여 status line 기능을 추가한다.

#### 수동구성

사용자 설정(`~/.claude/settings.json`) 파일에서 프로젝트 설정에 statusLine 필드를 주가하고 설정을 추가한다.

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline.sh",
    "padding": 2
  }
}
```

> 💡 Window 환경에서 적용이 안될 경우  
> status line의 경우 linux 기준으로 실행하므로 winodw 환경이라면 명령 시 자신의 환경을 명시해주는 것이 좋다
>
> 💡 적용이 안될 경우 (MAC / Winodw 동일)  
> claude code cli 환경에서는 json 파일을 읽기 위해 jq 명령어 도구(조회,핉터링,파싱 등)을 호출하는데 해당 기능에 문제가 발생하여 안될 경우가 있다.
>
> 이때에는 "jq 없거나 못쓰면 대체 파싱하여 진행해줘"와 같은 명령어를 추가하여 실행하면 jq를 사용하지 않고 설정해주게 된다. -> 이 경우 sh 파일 내 명령어가 jq를 사용했을 때 보다 상대적으로 길어질 확률이 높다.

**공식 문서**  

https://code.claude.com/docs/ko/statusline

## Output Style

Claude Code의 응답 형식에 대해서 설정한다. 
(핵심 기능 - 파일 읽기/쓰기/스크립트 실행/지식 등은 변경하지 않고 응답 방식 - 어조/역할/출력 형식만 변경)

Claude를 단순한 '소프트웨어 엔지니어'가 아닌 다른 역할(예: 설명자, 글쓰기 보조 등)로 작동시키고 싶을 때 사용하거나 같은 말투나 형식을 유지해달라고 반복해서 프롬프트를 입력해야 할 때 적용하는 것이 좋다

- Default : 출력 스타일이 설정되지 않아 Claude Code의 표준 출력 스타일
- Proactive : 주로 일상적인 결정을 주로 할 때 사용을 추천하며, 데이터 삭제 공유 또는 시스템 변경 작업 외의 대화에서 accept mode로 동작
- Explanatory : Default와 동일한 방식으로 작업을 수행하지만, 해당 작업을 수행하는 이유에 대해서 짧은 설명을 Insight 레이블로 추가한다. 
  > ★ Insight ─────────────────────────────────────
  > - Every route in this repo goes through the withAuth wrapper, so the new endpoint gets session checks without its own middleware.
  > - Rate limits are set per route in limits.ts, which is why this change adds an entry there rather than a global default. 
  > ───────────────────────────────────────
- Learning : Explanatory 스타일처럼 설명(Insight)를 제공하고 일부 코드를 사용자가 구현하도록 요청한다. 구현해야할 부분은 `//TODO(Human)`으로 위치를 표시해준다.
  > ● Learn by Doing
  > 
  > Context: The upload form is in place and calls validateFile() before accepting a file. Size and type checks work for images, but the switch statement has no handling for documents yet.
  > 
  > Your Task: In upload.js, implement the case "document" branch inside validateFile(). Look for TODO(human).
  >
  > Guidance: Decide on a size limit for documents and whether the file extension has to match the MIME type. Return {valid: boolean, error?: string}.

### 스타일 변경

#### 대화형 prompt

`/output-style ${mode}`를 통해 원하는 output-style을 설정한다.

#### 설정 파일 편집

`settings.json`에서 아래 내용을 추가한다.

```
{
  "outputStyle": ${mode}
}
```

### 사용자 설정 출력 스타일

프로젝트 공통으로 출력을 지정할 경우 아래 경로에 .md 파일을 등록해두면 적용된다.

- 사용자: ~/.claude/output-styles
  
- 프로젝트: .claude/output-styles

#### Frontmatter (메타데이터)

- name : 사용자 설정 출력 스타일의 이름(없으면 파일 이름으로 등록)
- description : 출력 스타일 설명
- keep-coding-instructions : claude code의 default 코딩 지침을 따를 지 여부 (소스 변경시 테스트 여부, 변경 감지 등)

```
---
name: Diagrams first
description: Lead every explanation with a diagram
keep-coding-instructions: true
---
```

#### 본문

적용할 스타일의 상세 내용을 markdown 형식으로 작성해주면 된다.

```
When explaining code, architecture, or data flow, start with a Mermaid diagram showing the structure, then explain in prose.

## Diagram conventions

Use `flowchart TD` for control flow and `sequenceDiagram` for request paths. Keep diagrams under 15 nodes.
```

#### 공식 문서

https://code.claude.com/docs/ko/output-styles