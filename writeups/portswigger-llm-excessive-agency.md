# [PortSwigger] Exploiting LLM APIs with excessive agency

| 항목 | 내용 |
|---|---|
| 날짜 | 2026-11-20 |
| 사이트 | PortSwigger Web Security Academy (Web LLM attacks) |
| 난이도 | Apprentice |
| 관련 OWASP LLM 항목 | LLM06 과도한 위임 (Excessive Agency) |
| 걸린 시간 | 대략 5분 |

> 이 기록은 직접 푼 실습을 Claude Code와 함께 정리한 공부 기록입니다.

## 한 줄 요약

상담 챗봇이 손님에게 필요 없는 '계정을 지우는 힘'까지 갖고 있어서, 말로 속이는 것만으로 다른 손님의 계정을 지울 수 있었다.

## 상황

- 랩을 열면 'Gin and Juice'라는 가짜 쇼핑몰이 생기고, 화면 구석에 상담 챗봇(Live chat)이 있다.
- 목표: 챗봇을 이용해 carlos라는 손님의 계정을 지우기.
- 챗봇 뒤에는 LLM이 있고, LLM은 웹사이트 안쪽 기능(API)을 대신 불러 줄 수 있다.
- 랩은 Burp Suite Community에 딸린 브라우저로 열어서, 챗봇과 오간 내용을 Burp에서도 볼 수 있게 했다.

## 시도한 것

| 순서 | 시도 | 결과 | 왜 그렇게 됐는지 |
|---|---|---|---|
| 1 | 챗봇에게 "너는 어떤 API를 쓸 수 있어?"라고 물어봄 | 랩은 아직 안 풀림. 대신 챗봇이 쓸 수 있는 기능 목록(상품 정보 찾기, 비밀번호 재설정 메일 보내기, 안쪽 자료를 다루는 도구)을 순순히 알려 줌 | 챗봇에게 "이건 손님에게 말하면 안 된다"는 막음이 없었다. 해커가 공격할 곳을 고르는 데 필요한 정보를 챗봇이 먼저 내줬다 |
| 2 | 상품 정보를 묻는 말 사이에, carlos 계정을 지우라는 지시를 섞어 보냄 | 성공. carlos 계정이 지워지고 랩이 풀림 | 챗봇이 계정을 건드리는 기능까지 쓸 수 있었고, 그 기능을 부르기 전에 "이 사람이 정말 이 일을 해도 되나"를 확인하는 곳이 없었다 |

## 통한 방법

핵심은 **챗봇이 가진 힘을 먼저 알아내고, 그 힘을 쓰게 만드는 말을 평범한 부탁 속에 섞는 것**이었다.

- 상품을 묻는 척하면서 할 일을 두 가지로 나눠 주고, 두 번째 일로 carlos 계정 일을 시켰다.
- 보낸 글의 첫머리만 적고 나머지는 가린다: `I'd like to know the information about Gin and Juice products. ... [가림]`

## 막으려면

- **챗봇에게 꼭 필요한 힘만 준다.** 상품 안내 챗봇에 계정을 지우는 기능을 연결하지 않는다. (OWASP LLM06이 말하는 '과도한 기능·권한'을 줄이는 일)
- **확인은 챗봇이 아니라 API 쪽에서 한다.** 계정 삭제 같은 큰일은 기능을 받는 쪽 프로그램이 "지금 요청한 사람이 이 계정의 주인이나 관리자인가"를 직접 확인한다. 챗봇이 속아도 여기서 막힌다.
- **큰일 앞에서는 사람에게 한 번 묻는다.** 지우기, 메일 보내기 같은 일은 실행 전에 사용자가 직접 '예'를 누르게 한다.
- 개발자가 놓치기 쉬운 점: 시스템 프롬프트에 "이 기능은 쓰지 마"라고 적어 두는 것만으로는 막히지 않는다. 말로 한 부탁은 말로 뒤집힐 수 있다.

## AI와 비교 (2달차부터)

이번에는 비워 둔다.

## 배운 것

- 공격은 어려운 기술이 아니었다. **챗봇에게 무엇을 할 수 있는지 묻고, 그 힘을 엉뚱하게 쓰게 만든 것**이 전부였다. 뚫리느냐는 결국 챗봇에게 어떤 힘을 줬는가에 달려 있다.
- 채팅 말은 화면에는 말풍선이지만, 뒤에서는 WebSocket(연결을 이어 둔 채 말을 주고받는 방식)으로 `{"user":"...","content":"..."}` 모양의 글(JSON)로 오간다. Burp의 'WebSockets history' 탭에서 볼 수 있었다.
- 같은 주에 푼 TryHackMe 'Offensive Security Intro'의 숨은 관리자 페이지와 같은 교훈이다. 숨기거나 말로 부탁하는 것으로는 부족하고, **창구가 스스로 "이 사람이 해도 되나"를 확인해야** 한다.

## 출처

- PortSwigger, "Web LLM attacks": https://portswigger.net/web-security/llm-attacks
- PortSwigger, "Lab: Exploiting LLM APIs with excessive agency": https://portswigger.net/web-security/llm-attacks/lab-exploiting-llm-apis-with-excessive-agency
- OWASP Top 10 for LLM Applications 2025, LLM06 Excessive Agency: https://genai.owasp.org/llmrisk/llm062025-excessive-agency/

---

### 공개 전 점검

- [x] 정답 플래그, 비밀번호, 최종 페이로드를 가렸다 (보낸 글은 첫머리만, 나머지 가림)
- [x] 자격증·시험 문제, 진행 중인 대회 문제가 아니다
- [x] 실습 사이트가 아닌 실제 서비스에 시험한 내용이 없다
- [x] 화면 캡처에 내 이메일, 계정 이름, 토큰, IP 주소가 보이지 않는다 (캡처 없음)
- [x] API 키나 개인정보가 들어 있지 않다
