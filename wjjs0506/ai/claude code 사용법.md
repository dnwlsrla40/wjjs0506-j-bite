# Claude Code 사용법

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

claude code의 응답 형식에 대해서 설정한다.

