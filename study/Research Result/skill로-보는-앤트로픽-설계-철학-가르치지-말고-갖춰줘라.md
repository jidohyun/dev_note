---
title: "Skill로 보는 앤트로픽 설계 철학: 가르치지 말고 갖춰줘라"
source: "https://www.linkedin.com/posts/%EB%B2%94%EA%B7%BC-%EC%86%A1-226479119_skill%EB%A1%9C-%EB%B3%B4%EB%8A%94-%EC%95%A4%ED%8A%B8%EB%A1%9C%ED%94%BD-%EC%84%A4%EA%B3%84-%EC%B2%A0%ED%95%99-%EA%B0%80%EB%A5%B4%EC%B9%98%EC%A7%80-%EB%A7%90%EA%B3%A0-%EA%B0%96%EC%B6%B0%EC%A4%98%EB%9D%BC-%EC%9A%94%EC%A6%98-activity-7414607486030274560-4W5h?utm_source=share&utm_medium=member_ios&rcm=ACoAAFSgEwQBnP3WveBlvrNPq-0YtFea0WPJGIk"
created: 2026-01-07T21:37:47.005+09:00
type: article
tags: [reading, summary]
---

# 핵심 요약
- 앤트로픽(Anthropic)의 Claude Skill은 에이전트에게 지식을 직접 주입하는 대신, 필요할 때 꺼내 쓸 수 있는 '도구'를 제공하는 방식이다.
- 에이전트에게 기술을 부여하는 방식은 프롬프트, 커맨드, 커스텀 규칙을 거쳐 '스킬(Skill)'로 진화하고 있다.
- 앤트로픽의 핵심 설계 철학은 "가르치지 말고 갖춰줘라(Don't teach, equip)"이며, 이는 특정 워크플로우를 강요하지 않는 유연함을 지향한다.
- AI에게 최고의 도구를 쥐어준 뒤 스스로 일하게 만드는 전략이 최근 앤트로픽의 강력한 성장 동력이 되고 있다.

# 상세 내용
- 에이전트에게 기술을 주는 4가지 방법
  - 프롬프트(Prompt): 매번 말로 자세히 설명하고 지시하는 단계.
  - 커맨드(Command): 반복되는 지시를 프리셋으로 저장하여 효율을 높이는 단계.
  - 커스텀 규칙(Custom Rules): 시작부터 특정 규칙(예: CLAUDE.md)을 읽고 업무에 임하게 하는 방식.
  - 스킬(Skill): 컨텍스트 용량의 한계를 극복하기 위해, 기술이 담긴 '책'을 만들어 두고 상황에 맞춰 꺼내 쓰게 하는 방식. 에이전트는 스킬의 표지만 기억하면 되므로 효율적이다.

- 앤트로픽의 설계 철학: "가르치지 말고 갖춰줘라"
  - Intentionally unopinionated: 의도적으로 특정 의견이나 방식을 고집하지 않음.
  - Without forcing specific workflows: 특정 작업 흐름을 강요하지 않음.
  - Give Claude more tools, then gets out of the way: 최고의 도구를 제공한 후, AI가 능력을 발휘할 수 있도록 비켜줌.

- 시장의 흐름과 전망
  - MCP(Model Context Protocol)와 Skill은 이러한 철학의 산물이다.
  - AI를 전문적으로 교육시키려 하기보다, AI가 스스로 판단하고 활용할 수 있는 최적의 환경과 도구를 구축해 주는 것이 핵심이다.

# 인사이트 / 할 일
- AI 활용 전략의 변화: 프롬프트 엔지니어링을 통해 AI를 '가르치려' 애쓰기보다, AI가 참조할 수 있는 양질의 데이터와 도구(Skill)를 어떻게 '갖춰줄 것인가'에 집중해야 한다.
- 업무 철학의 확장: 비단 AI뿐만 아니라 팀 빌딩이나 협업에서도 상대방에게 세세한 방식을 강요하기보다, 최선의 결과가 나올 수 있는 환경과 도구를 제공하고 있는지 자문해 볼 필요가 있다.
- 실행 액션: 현재 사용 중인 Claude 워크플로우에 MCP나 Skill을 도입하여 컨텍스트 효율을 높일 수 있는 영역이 있는지 검토하기.