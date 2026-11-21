# ClaudeClaw (모스카 포크) — 빠른 시작 가이드

이 저장소는 [moazbuilds/claudeclaw](https://github.com/moazbuilds/claudeclaw)의 포크입니다.
원본과 다른 점:

- **Windows 수정:** `claude.cmd`를 거치지 않고 `claude.exe`를 직접 실행합니다. (텔레그램 여러 줄 메시지 잘림 방지)
- **긴 작업 지원:** 한 번 실행 제한이 5분이 아니라 **15분**이고, `settings.json`의 `sessionTimeoutMs`로 바꿀 수 있습니다.

## 준비물

| 항목 | 설치 확인 | 설치 방법 |
| --- | --- | --- |
| Claude Code | `claude --version` | https://claude.com/claude-code |
| Bun | `bun --version` | https://bun.sh (Windows: `powershell -c "irm bun.sh/install.ps1 | iex"`) |
| Node.js (LTS) | `node --version` | https://nodejs.org |
| 텔레그램 계정 | — | 봇을 쓸 경우 |

## 1. 설치 (5분)

터미널에서:

```bash
claude plugin marketplace add moskalabs/claudeclaw
claude plugin install claudeclaw
```

## 2. 작업 폴더 만들기 (중요)

ClaudeClaw는 **현재 폴더**에 설정과 로그를 저장합니다.
**홈 폴더(`C:\Users\이름`)에서는 시작이 막혀 있습니다.** 전용 폴더를 만드세요.

```bash
mkdir D:\code\claudeclaw-work
cd D:\code\claudeclaw-work
claude
```

## 3. 시작

Claude Code 안에서:

```
/claudeclaw:start
```

설정 마법사가 모델, 하트비트, 텔레그램, 보안을 물어봅니다. 아래 "4. 텔레그램 연결"을 먼저 준비해 두면 한 번에 끝납니다.

설정을 직접 하고 싶으면 `.claude/claudeclaw/settings.json`을 만들고
[`settings.example.json`](settings.example.json)을 복사해서 값만 바꾸세요. (`allowedUserIds`는 따옴표 없는 숫자입니다.)

## 4. 텔레그램 연결

1. 텔레그램에서 **@BotFather** → `/newbot` → 봇 이름 정하기 → **토큰** 받기
2. **@userinfobot**에게 아무 말이나 보내서 내 **숫자 ID** 확인
3. `settings.json`의 `telegram.token`에 토큰, `telegram.allowedUserIds`에 내 ID를 넣기
4. 만든 봇에게 메시지를 보내면 응답이 옵니다.

> `allowedUserIds`에 넣은 사람만 봇을 쓸 수 있습니다. 비워 두지 마세요.
> **토큰은 절대 GitHub에 올리지 마세요.** `.claude/claudeclaw/settings.json`은 커밋하지 않습니다.

## 5. 자주 바꾸는 설정

| 설정 | 설명 |
| --- | --- |
| `model` | 기본 모델 (`opus`, `sonnet`, `haiku`) |
| `timezone` | 예: `Asia/Seoul` |
| `sessionTimeoutMs` | 한 번 실행 제한(ms). 기본 900000 = 15분 |
| `security.level` | `locked` / `strict` / `moderate` / `unrestricted` (처음엔 `moderate` 권장) |
| `heartbeat.enabled` | 주기적 자동 점검 (기본 꺼짐) |

설정을 바꾸면 **데몬을 껐다 켜야** 적용됩니다.

## 6. 문제가 생기면

| 증상 | 확인할 것 |
| --- | --- |
| `/claudeclaw:start`가 "CRITICAL BLOCKER" | 홈 폴더에서 시작함 → 전용 폴더에서 `claude` 다시 실행 |
| 텔레그램 응답이 없음 | 토큰, `allowedUserIds`, 데몬이 켜져 있는지, 인터넷 연결 |
| 긴 작업이 중간에 끊김 | `sessionTimeoutMs` 값 키우기 |
| Windows에서 여러 줄 메시지가 잘림 | 이 포크 코드를 쓰는지 확인 (`claude plugin update claudeclaw`) |
| 업데이트가 필요함 | `claude plugin marketplace update` 후 `claude plugin update claudeclaw` |

로그는 `.claude/claudeclaw/logs/`에 있습니다.

---

*작성: 2026-11-21. 이 포크는 원본보다 커밋이 뒤처져 있습니다. 새 PC에서 처음부터 설치해 검증한 문서는 아니므로, 막히는 부분이 있으면 이 문서를 고쳐 주세요.*
