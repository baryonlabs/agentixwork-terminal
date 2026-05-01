> 이 문서는 Claude가 번역했어요. 개선할 부분이 있다면 PR을 보내주세요.

<h1 align="center">AgentixWork Terminal</h1>
<p align="center"><strong>한국어 정의:</strong> AgentixWork Terminal은 여러 CLI 코딩 에이전트를 동시에 실행하고, 어느 작업이 사용자의 확인을 기다리는지 관찰하며, 파일 편집과 브라우저 테스트까지 이어주는 <strong>에이전트 운영 터미널</strong>입니다. cmux를 기반으로 하되, 새 프로젝트 시작, 파일 탐색, Neovim 실행, 작업 상태 관리 같은 AgentixWork 워크플로우를 더해 갑니다.</p>

> Fork 공지: AgentixWork Terminal은 [manaflow-ai/cmux](https://github.com/manaflow-ai/cmux)를 fork한 저장소입니다. 업스트림 저작권 표시와 라이선스를 유지하면서 프로젝트 생성, 파일 편집, Neovim, 향후 AI 에이전트 워크스페이스 제어를 위한 AgentixWork 워크플로우 레이어를 추가합니다. 자세한 내용은 [NOTICE.agentixwork.md](NOTICE.agentixwork.md), [docs/agentixwork-terminal-roadmap.md](docs/agentixwork-terminal-roadmap.md), [docs/agentixwork-cli.md](docs/agentixwork-cli.md)를 보세요.

<p align="center">
  <a href="https://github.com/manaflow-ai/cmux/releases/latest/download/cmux-macos.dmg">
    <img src="./docs/assets/macos-badge.png" alt="macOS용 cmux 다운로드" width="180" />
  </a>
</p>

<p align="center">
  <a href="README.md">English</a> | <a href="README.ja.md">日本語</a> | <a href="README.zh-CN.md">简体中文</a> | <a href="README.zh-TW.md">繁體中文</a> | 한국어 | <a href="README.de.md">Deutsch</a> | <a href="README.es.md">Español</a> | <a href="README.fr.md">Français</a> | <a href="README.it.md">Italiano</a> | <a href="README.da.md">Dansk</a> | <a href="README.pl.md">Polski</a> | <a href="README.ru.md">Русский</a> | <a href="README.bs.md">Bosanski</a> | <a href="README.ar.md">العربية</a> | <a href="README.no.md">Norsk</a> | <a href="README.pt-BR.md">Português (Brasil)</a> | <a href="README.th.md">ไทย</a> | <a href="README.tr.md">Türkçe</a> | <a href="README.km.md">ភាសាខ្មែរ</a> | <a href="README.uk.md">Українська</a>
</p>

<p align="center">
  <a href="https://x.com/manaflowai"><img src="https://img.shields.io/badge/@manaflow-555?logo=x" alt="X / Twitter" /></a>
  <a href="https://discord.gg/xsgFEVrWCZ"><img src="https://img.shields.io/badge/Discord-555?logo=discord" alt="Discord" /></a>
  <a href="https://github.com/manaflow-ai/cmux"><img src="https://img.shields.io/github/stars/manaflow-ai/cmux?style=flat&logo=github&label=stars&color=4c71f2" alt="GitHub stars" /></a>
</p>

<p align="center">
  <img src="./docs/assets/main-first-image.png" alt="cmux 스크린샷" width="900" />
</p>

<p align="center">
  <a href="https://www.youtube.com/watch?v=i-WxO5YUTOs">▶ 데모 영상</a> · <a href="https://cmux.com/blog/zen-of-cmux">The Zen of cmux</a>
</p>

## 정의

AgentixWork Terminal은 Claude Code, Codex, OpenCode, Gemini CLI 같은 터미널 기반 코딩 에이전트를 여러 개 병렬로 실행하는 사람을 위한 **에이전트 운영 터미널**입니다.

터미널을 작업의 중심에 두고, 그 주변에 필요한 운영 레이어를 더합니다. 새 프로젝트 생성, 파일 탐색기 접근, Neovim 실행, 브라우저 패널, 주의가 필요한 에이전트 알림, 워크스페이스 상태, 향후 agent/session 제어를 하나의 흐름으로 묶는 것이 목표입니다.

**English definition:** AgentixWork Terminal is an **agent operations terminal**: a cmux-based workspace for running, supervising, and editing across multiple terminal-native coding agents while keeping the terminal as the primary work surface.

## 새 프로젝트 시작

AgentixWork는 새 코딩 워크스페이스를 한 명령으로 시작할 수 있도록 작은 CLI 프로토타입을 추가합니다.

```bash
Prototypes/agentixwork-cli/agentixwork new my-app
```

기본적으로 단순한 이름은 `~/dev` 아래에 생성되고, `git`이 있으면 Git 저장소로 초기화되며, `cmux` CLI가 설치되어 있으면 해당 폴더를 바로 엽니다. 폴더를 만들고 바로 Neovim으로 들어가려면 다음처럼 실행합니다.

```bash
Prototypes/agentixwork-cli/agentixwork new my-app --nvim
```

기본 상위 폴더를 바꾸려면 `AGENTIXWORK_PROJECTS_DIR`를 설정합니다.

```bash
export AGENTIXWORK_PROJECTS_DIR="$HOME/work"
```

## 기능

<table>
<tr>
<td width="40%" valign="middle">
<h3>알림 링</h3>
코딩 에이전트가 입력을 기다리면 패널에 파란색 링이 뜨고 탭이 강조돼요
</td>
<td width="60%">
<img src="./docs/assets/ko/notification-rings.png" alt="알림 링" width="100%" />
</td>
</tr>
<tr>
<td width="40%" valign="middle">
<h3>알림 패널</h3>
대기 중인 알림을 한곳에서 확인하고, 가장 최근 읽지 않은 알림으로 바로 이동할 수 있어요
</td>
<td width="60%">
<img src="./docs/assets/ko/sidebar-notification-badge.png" alt="사이드바 알림 배지" width="100%" />
</td>
</tr>
<tr>
<td width="40%" valign="middle">
<h3>내장 브라우저</h3>
<a href="https://github.com/vercel-labs/agent-browser">agent-browser</a>에서 포팅된 스크립팅 API를 갖춘 브라우저를 터미널 옆에 띄울 수 있어요
</td>
<td width="60%">
<img src="./docs/assets/ko/built-in-browser.png" alt="내장 브라우저" width="100%" />
</td>
</tr>
<tr>
<td width="40%" valign="middle">
<h3>세로 + 가로 탭</h3>
사이드바에서 git 브랜치, 연결된 PR 상태/번호, 작업 디렉토리, 수신 포트, 최근 알림 텍스트를 한눈에 볼 수 있어요. 수평·수직 분할을 지원해요.
</td>
<td width="60%">
<img src="./docs/assets/vertical-horizontal-tabs-and-splits.png" alt="세로 탭과 분할 패널" width="100%" />
</td>
</tr>
<tr>
<td width="40%" valign="middle">
<h3>SSH</h3>
<code>cmux ssh user@remote</code>로 원격 머신용 워크스페이스를 생성해요. 브라우저 패널은 원격 네트워크를 통해 라우팅되어 localhost가 그대로 작동해요. 원격 세션에 이미지를 드래그하면 scp로 업로드돼요.
</td>
<td width="60%">
<img src="./docs/assets/ssh.png" alt="cmux SSH" width="100%" />
</td>
</tr>
<tr>
<td width="40%" valign="middle">
<h3>Claude Code Teams</h3>
<code>cmux claude-teams</code>로 Claude Code의 팀원 모드를 한 명령어로 실행해요. 팀원은 네이티브 분할로 생성되며 사이드바에 메타데이터와 알림이 표시돼요. tmux가 필요 없어요.
</td>
<td width="60%">
<img src="./docs/assets/claude-code-teams.png" alt="Claude Code Teams" width="100%" />
</td>
</tr>
</table>

- **브라우저 가져오기** — Chrome, Firefox, Arc 및 20개 이상의 브라우저에서 쿠키, 방문 기록, 세션을 가져와서 브라우저 패널이 로그인된 상태로 시작돼요
- **커스텀 명령어** — [`cmux.json`](https://cmux.com/docs/custom-commands)에서 프로젝트별 액션을 정의하고 명령 팔레트에서 실행할 수 있어요
- **스크립팅** — CLI와 socket API로 워크스페이스 생성, 패널 분할, 키 입력 전송, 브라우저 자동화가 가능해요
- **네이티브 macOS 앱** — Electron이 아닌 Swift와 AppKit으로 만들었어요. 빠르게 실행되고 메모리도 적게 써요.
- **Ghostty 호환** — 기존 `~/.config/ghostty/config`에서 테마, 글꼴, 색상 설정을 그대로 읽어와요
- **GPU 가속** — libghostty 기반이라 렌더링이 부드러워요

## 설치하기

### DMG (권장)

<a href="https://github.com/manaflow-ai/cmux/releases/latest/download/cmux-macos.dmg">
  <img src="./docs/assets/macos-badge.png" alt="macOS용 cmux 다운로드" width="180" />
</a>

`.dmg` 파일을 열고 cmux를 응용 프로그램 폴더로 드래그하면 돼요. Sparkle을 통해 자동 업데이트되니 한 번만 다운로드하면 돼요.

### Homebrew

```bash
brew tap manaflow-ai/cmux
brew install --cask cmux
```

나중에 업데이트하려면 아래 명령어를 실행해주세요:

```bash
brew upgrade --cask cmux
```

처음 실행할 때 macOS에서 개발자 확인 팝업이 뜰 수 있어요. **열기**를 클릭하면 돼요.

## 왜 cmux를 만들었나요?

저는 Claude Code와 Codex 세션을 여러 개 동시에 돌려요. 예전에는 Ghostty에서 분할 패널을 여러 개 열어놓고, 에이전트가 입력을 기다릴 때 macOS 기본 알림에 의존했어요. 그런데 Claude Code 알림은 항상 "Claude is waiting for your input"이라는 아무 맥락 없이 똑같은 메시지뿐이었고, 탭이 많아지면 제목조차 읽을 수가 없었어요.

여러 코딩 오케스트레이터를 써봤는데, 대부분 Electron/Tauri 앱이라 성능이 별로였어요. GUI 오케스트레이터는 특정 워크플로우에 갇히게 돼서 터미널이 더 낫다고 생각했고요. 그래서 Swift/AppKit으로 네이티브 macOS 앱인 cmux를 직접 만들었어요. 터미널 렌더링에는 libghostty를 쓰고, 기존 Ghostty 설정에서 테마, 글꼴, 색상을 그대로 가져와요.

핵심은 사이드바와 알림 시스템이에요. 사이드바에는 각 워크스페이스의 git 브랜치, 연결된 PR 상태/번호, 작업 디렉토리, 수신 포트, 최근 알림 텍스트를 보여주는 세로 탭이 있어요. 알림 시스템은 터미널 시퀀스(OSC 9/99/777)를 감지하고, Claude Code나 OpenCode 같은 에이전트 훅에 연결할 수 있는 CLI(`cmux notify`)를 제공해요. 에이전트가 대기 중이면 해당 패널에 파란색 링이 뜨고 사이드바 탭이 강조되니까, 여러 패널과 탭 중에서 어디서 입력을 기다리는지 바로 알 수 있어요. ⌘⇧U를 누르면 가장 최근 읽지 않은 알림으로 이동해요.

내장 브라우저는 [agent-browser](https://github.com/vercel-labs/agent-browser)에서 포팅한 스크립팅 API를 제공해요. 에이전트가 접근성 트리 스냅샷을 가져오고, 요소를 참조·클릭하고, 양식을 채우고, JS를 실행할 수 있어요. 터미널 옆에 브라우저 패널을 띄워서 Claude Code가 개발 서버와 직접 상호작용하게 할 수 있어요.

CLI와 socket API로 모든 걸 자동화할 수 있어요 — 워크스페이스/탭 생성, 패널 분할, 키 입력 전송, 브라우저에서 URL 열기까지요.

## The Zen of cmux

cmux는 개발자가 도구를 어떻게 사용해야 하는지 규정하지 않아요. 터미널과 브라우저에 CLI가 있고, 나머지는 여러분의 몫이에요.

cmux는 솔루션이 아니라 프리미티브예요. 터미널, 브라우저, 알림, 워크스페이스, 분할, 탭, 그리고 이 모든 것을 제어하는 CLI를 제공해요. cmux는 코딩 에이전트를 특정 방식으로 사용하도록 강요하지 않아요. 프리미티브로 무엇을 만들지는 여러분에게 달려 있어요.

최고의 개발자들은 항상 자신만의 도구를 만들어왔어요. 에이전트와 함께 일하는 최적의 방법은 아직 아무도 찾지 못했고, 폐쇄적인 제품을 만드는 팀들도 마찬가지예요. 자신의 코드베이스에 가장 가까운 개발자가 먼저 답을 찾을 거예요.

100만 명의 개발자에게 조합 가능한 프리미티브를 주면, 어떤 프로덕트 팀이 위에서 설계하는 것보다 빠르게 가장 효율적인 워크플로우를 함께 찾아낼 거예요.

## 문서

cmux 설정 방법에 대한 자세한 내용은 [문서를 확인해주세요](https://cmux.com/docs/getting-started?utm_source=readme).

## 키보드 단축키

### 워크스페이스

| 단축키 | 동작 |
|----------|--------|
| ⌘ N | 새 워크스페이스 |
| ⌘ 1–8 | 워크스페이스 1–8로 이동 |
| ⌘ 9 | 마지막 워크스페이스로 이동 |
| ⌃ ⌘ ] | 다음 워크스페이스 |
| ⌃ ⌘ [ | 이전 워크스페이스 |
| ⌘ ⇧ W | 워크스페이스 닫기 |
| ⌘ ⇧ R | 워크스페이스 이름 변경 |
| ⌘ B | 사이드바 토글 |

### 서피스

| 단축키 | 동작 |
|----------|--------|
| ⌘ T | 새 서피스 |
| ⌘ ⇧ ] | 다음 서피스 |
| ⌘ ⇧ [ | 이전 서피스 |
| ⌃ Tab | 다음 서피스 |
| ⌃ ⇧ Tab | 이전 서피스 |
| ⌃ 1–8 | 서피스 1–8로 이동 |
| ⌃ 9 | 마지막 서피스로 이동 |
| ⌘ W | 서피스 닫기 |

### 분할 패널

| 단축키 | 동작 |
|----------|--------|
| ⌘ D | 오른쪽으로 분할 |
| ⌘ ⇧ D | 아래로 분할 |
| ⌥ ⌘ ← → ↑ ↓ | 방향키로 패널 포커스 이동 |
| ⌘ ⇧ H | 현재 패널 깜빡임 |

### 브라우저

브라우저 개발자 도구 단축키는 Safari 기본값을 따르며, `설정 → 키보드 단축키`에서 변경할 수 있어요.

| 단축키 | 동작 |
|----------|--------|
| ⌘ ⇧ L | 분할 패널로 브라우저 열기 |
| ⌘ L | 주소창 포커스 |
| ⌘ [ | 뒤로 |
| ⌘ ] | 앞으로 |
| ⌘ R | 페이지 새로고침 |
| ⌥ ⌘ I | 개발자 도구 열기 (Safari 기본값) |
| ⌥ ⌘ C | JavaScript 콘솔 표시 (Safari 기본값) |

### 알림

| 단축키 | 동작 |
|----------|--------|
| ⌘ I | 알림 패널 표시 |
| ⌘ ⇧ U | 최근 읽지 않은 알림으로 이동 |

### 찾기

| 단축키 | 동작 |
|----------|--------|
| ⌘ F | 찾기 |
| ⌘ G / ⌘ ⇧ G | 다음 찾기 / 이전 찾기 |
| ⌘ ⇧ F | 찾기 바 숨기기 |
| ⌘ E | 선택한 텍스트로 찾기 |

### 터미널

| 단축키 | 동작 |
|----------|--------|
| ⌘ K | 스크롤백 지우기 |
| ⌘ C | 복사 (선택 시) |
| ⌘ V | 붙여넣기 |
| ⌘ + / ⌘ - | 글꼴 크기 확대 / 축소 |
| ⌘ 0 | 글꼴 크기 초기화 |

### 창

| 단축키 | 동작 |
|----------|--------|
| ⌘ ⇧ N | 새 창 |
| ⌘ , | 설정 |
| ⌘ ⇧ , | 설정 다시 불러오기 |
| ⌘ Q | 종료 |

## 나이틀리 빌드

[cmux NIGHTLY 다운로드](https://github.com/manaflow-ai/cmux/releases/download/nightly/cmux-nightly-macos.dmg)

cmux NIGHTLY는 자체 번들 ID를 가진 별도의 앱이라 안정 버전과 함께 실행할 수 있어요. 최신 `main` 커밋에서 자동으로 빌드되고, 자체 Sparkle 피드를 통해 자동 업데이트돼요.

## 세션 복원 (현재 동작)

재실행 시 cmux는 현재 앱 레이아웃과 메타데이터만 복원해요:
- 창/워크스페이스/패널 레이아웃
- 작업 디렉토리
- 터미널 스크롤백 (최선 노력)
- 브라우저 URL 및 탐색 기록

cmux는 터미널 앱 내부의 라이브 프로세스 상태를 복원하지 **않아요**. 예를 들어 활성 Claude Code/tmux/vim 세션은 재시작 후 아직 복원되지 않아요.

## Star History

<a href="https://star-history.com/#manaflow-ai/cmux&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=manaflow-ai/cmux&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=manaflow-ai/cmux&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=manaflow-ai/cmux&type=Date" width="600" />
 </picture>
</a>

## 기여하기

참여 방법:

- X에서 팔로우해주세요: [@manaflowai](https://x.com/manaflowai), [@lawrencecchen](https://x.com/lawrencecchen), [@austinywang](https://x.com/austinywang)
- [Discord](https://discord.gg/xsgFEVrWCZ)에서 대화에 참여해주세요
- [GitHub Issues](https://github.com/manaflow-ai/cmux/issues)와 [토론](https://github.com/manaflow-ai/cmux/discussions)에 참여해주세요
- cmux로 무엇을 만들고 있는지 알려주세요

## 커뮤니티

- [Discord](https://discord.gg/xsgFEVrWCZ)
- [GitHub](https://github.com/manaflow-ai/cmux)
- [X / Twitter](https://twitter.com/manaflowai)
- [YouTube](https://www.youtube.com/channel/UCAa89_j-TWkrXfk9A3CbASw)
- [LinkedIn](https://www.linkedin.com/company/manaflow-ai/)
- [Reddit](https://www.reddit.com/r/cmux/)

## Founder's Edition

cmux는 무료이고 오픈 소스이며, 앞으로도 그럴 거예요. 개발을 지원하고 다음에 나올 기능에 먼저 접근하고 싶다면:

**[Founder's Edition 구매하기](https://buy.stripe.com/3cI00j2Ld0it5OU33r5EY0q)**

- **기능 요청/버그 수정 우선 처리**
- **얼리 액세스: 모든 워크스페이스, 탭, 패널의 컨텍스트를 제공하는 cmux AI**
- **얼리 액세스: 데스크톱과 휴대폰 간 터미널을 동기화하는 iOS 앱**
- **얼리 액세스: 클라우드 VM**
- **얼리 액세스: 음성 모드**
- **저의 개인 iMessage/WhatsApp**

## 라이선스

cmux는 [GPL-3.0-or-later](LICENSE) 하에 오픈 소스예요.

GPL을 준수할 수 없는 조직을 위해 상용 라이선스도 제공돼요. 자세한 내용은 [founders@manaflow.com](mailto:founders@manaflow.com)으로 문의해주세요.
