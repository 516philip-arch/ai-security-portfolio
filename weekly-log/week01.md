# 1주차 (2026-11-23 ~ 11-29)

**이번 주 목표:** AI 보안 위협의 큰 그림을 잡고, 프롬프트 인젝션을 직접 해 보고, 첫 실습 기록을 공개 저장소에 올린다.

**최소 목표:** write-up 1편 공개. 계획보다 느리면 일요일 TryHackMe를 빼고 PortSwigger 랩은 1개만 한다.

## 한 일

| 요일 | 계획 | 실제 | 결과물 |
|---|---|---|---|
| 월 (1.5h) | OWASP LLM Top 10(2025판) 개요 읽기, 10개 항목을 한 줄씩 내 말로 요약 | 2026-11-19 완료. 책 1장 'AI 앱의 속을 들여다보기'(LLM, 프롬프트, 시스템 프롬프트, RAG, 도구와 에이전트) 원고 검토, 퀴즈 4/5 | [notes/owasp-llm-top10.md](../notes/owasp-llm-top10.md) |
| 화 (1.5h) | 실습 계정 만들기(TryHackMe, PortSwigger, Lakera), 이 저장소 만들기 | 2026-11-19 완료. TryHackMe·PortSwigger 가입. Lakera는 가입 없이 바로 실습 가능 | 저장소 첫 커밋 |
| 수 (1.5h) | Lakera Gandalf / Agent Breaker 첫 단계들. 시도한 프롬프트와 결과 기록 | 2026-11-19 완료. Agent Breaker(Gandalf 후속) Cycling Coach Level 1·2 통과 | 시도 기록은 개인용으로만 보관 |
| 목 (1.5h) | 프롬프트 인젝션 개념 정리: 직접 vs 간접, 왜 막기 어려운지 | 2026-11-19 완료. 책 2장 '말로 하는 해킹, 프롬프트 인젝션'(직접·간접 인젝션, 그림 속 지시, 탈옥, 페이로드 분할, 막기 어려운 이유, 심층 방어) 원고 검토, 퀴즈 3.5/4 | [notes/prompt-injection.md](../notes/prompt-injection.md) |
| 금 | 쉬는 날 (밀린 것이 있을 때만 따라잡기) | | |
| 토 (4h) | PortSwigger 'Web LLM attacks' 읽기, 첫 랩 1~2개 (Burp Suite Community 설치 포함) | ① 2026-11-20 완료 (1.5h). 책 10장 '직접 속여 보기' 앞부분(API, 과도한 위임, 옛 웹 공격 잇기, 메일 전달 규칙, 학습 자료 유출, 막는 법) 원고 검토, 퀴즈 5/5. ② 2026-11-20 완료 (1h). Burp Suite Community 설치, 책 10장 Burp 소개(쪽지·프록시·가로채기·Repeater, 화면이 막아도 쪽지는 고칠 수 있다), 퀴즈 4/4. ③ 2026-11-20 완료 (1.5h). 'Exploiting LLM APIs with excessive agency' 랩 성공(2번째 시도), 채팅이 WebSockets history로 오감을 확인, 책 10장 첫 랩 이야기·맺음 원고. 1-5 칸 끝 | 랩 풀이 초안 |
| 일 (5h) | TryHackMe 무료 입문 룸 1개(1.5h), write-up 1편 완성, 주간 회고, 2주차 계획 조정 | ① 2026-11-20 완료 (1.5h). TryHackMe 'Offensive Security Intro'·'Defensive Security Intro'. 숨겨 둔 관리자 페이지가 잠겨 있지 않아 뚫림 → 숨기기가 아니라 잠그기(접근 통제). ② 2026-11-20 완료 (2h). PortSwigger excessive agency 랩 write-up. ③ 2026-11-20 완료 (1.5h). 주간 확인 문제 9.5/10, 회고, 2주차 조정 | [write-up](../writeups/portswigger-llm-excessive-agency.md), 이 기록, [2주차 기록](week02.md) |

## TryHackMe 배운 점 3줄

1. 'Offensive Security Intro': 가짜 은행 사이트에서 숨겨 둔 페이지를 찾는 도구로 관리자용 페이지를 찾아냈다.
2. 그 페이지는 숨겨져 있었을 뿐 잠겨 있지 않아서, 주소만 알면 누구나 쓸 수 있었다. 숨기기는 막기가 아니다.
3. 막으려면 페이지에 들어올 때마다 로그인과 권한을 확인해야 한다(접근 통제). 'Defensive Security Intro'에서 해 본 수상한 접속 막기는 두 번째 겹일 뿐이다.

## 회고

- 주간 확인 문제 점수: 9.5/10 (그림 주소로 대화를 빼내는 원리에서 반만 맞음)
- 이번 주 실제 공부 시간: 따로 재지 않음 (계획 15시간, 2026-11-19 ~ 11-20 이틀에 진행)
- 가장 많이 배운 것 하나: AI가 속는 것보다, 속은 AI가 쥔 힘이 피해 크기를 정한다. 상담 챗봇에게 계정을 지우는 힘이 있었기 때문에 글 몇 줄로 계정이 지워졌다.
- 막힌 곳과 다음 주에 다시 볼 것: excessive agency 랩은 첫 시도에 실패하고 두 번째에 풀었다. 그림 주소로 대화가 새는 원리(화면이 그림 주소로 알아서 접속하고, 주소 꼬리가 해커 사이트 기록에 남음)는 2주차 HTTP에서 개발자 도구로 직접 본다.
- 공개한 결과물 링크: [PortSwigger excessive agency 랩 write-up](../writeups/portswigger-llm-excessive-agency.md)

## 다음 주에 바꿀 것

- 노트 03~08을 따로 쓰지 않고, 책 2부 '웹과 보안의 기초' 원고(11장부터)로 대신한다.
- TryHackMe 룸(DNS in Detail, HTTP in Detail)은 문제를 보기 전에 답을 먼저 짐작해 보고 푼다. 이번 주 룸은 따라 하기만 해서 쉬웠다.
- 보안 소식(2-4)은 Claude가 후보 2~3개를 고르고, 하나를 골라 읽은 뒤 Claude가 3줄로 정리한다.
- 분량은 그대로 둔다 (2주차 15시간 + 책 3~5장 칸 4.5시간 = 19.5시간, 순서대로).
