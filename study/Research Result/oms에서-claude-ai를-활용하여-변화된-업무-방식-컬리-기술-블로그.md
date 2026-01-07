---
title: "OMS에서 Claude AI를 활용하여 변화된 업무 방식 - 컬리 기술 블로그"
source: "https://helloworld.kurly.com/blog/oms-claude-ai-workflow/"
created: 2026-01-07T21:43:53.371+09:00
type: article
tags: [reading, summary]
---

# 핵심 요약
- PM 1명과 엔지니어 3명이 12개의 MSA를 운영하며, Claude AI를 통해 16명 규모의 조직과 같은 생산성을 확보함
- AI의 역할을 TPM(전체 설계)과 MSA AI(상세 개발)로 분리하고, 지식(Context)과 행동(Skills)을 체계적으로 관리함
- JSON 형태의 DSL(Domain Specific Language)을 도입하여 토큰 효율성을 높이고 전체 서비스 간 연동 맵을 AI가 파악하게 함
- 클린 아키텍처를 적용하여 AI가 읽어야 할 컨텍스트 노이즈를 최소화하고 분석의 정확도를 극대화함
- MCP(Model Context Protocol)를 통해 Jira, Github, Datadog 등 외부 도구와 연동하여 업무 전반을 자동화함

# 상세 내용
- AI 도입 배경 및 변화
  - MSA 증가로 인한 영향도 파악의 어려움과 정책 누락 문제를 해결하기 위해 도입함
  - 엔지니어는 AI의 Supervisor 역할을 수행하며, AI는 TPM 모드에서 전체 숲을 분석하고 MSA 모드에서 개별 코드를 구현함

- AI Context 설계 및 구조화
  - 지식 기반 Context: 도메인 개요, 데이터 모델, API/Kafka 스펙 등 프로젝트의 정적 지식을 관리함
  - 행동 기반 Skill: 개발 및 배포 워크플로우 등 특정 작업을 수행하는 동적 절차를 정의함
  - CLAUDE.md를 인덱스로 활용하여 AI가 필요한 정보의 위치를 정확히 파악하도록 유도함

- JSON DSL을 활용한 학습
  - 자연어 설명 대신 구조화된 JSON DSL을 사용하여 정보 압축 및 토큰 소모를 3배 이상 절감함
  - API와 메시지 스펙에 외부 호출 정보까지 포함하여 TPM AI가 전체 서비스 간 데이터 플로우를 추적할 수 있게 함
  - AI에게 직접 분석을 시키고 피드백을 통해 문서를 스스로 업데이트하게 만드는 '무한 반복 학습' 과정을 거침

- MCP(Model Context Protocol) 연동
  - Atlassian MCP: Jira 티켓 분석 및 하위 티켓 자동 생성, 진행 상황 공유
  - Github MCP: 테스트 코드 검증, PR 리뷰 작성, 배포 태그 관리 자동화
  - Datadog MCP: 실시간 로그 분석 및 배포 전후 영향도 파악

- 아키텍처와 AI 효율성
  - AI Context 품질은 내부 아키텍처의 역할 분리 품질에 비례함
  - 레이어드 아키텍처는 불필요한 코드(노이즈)를 많이 읽게 하지만, 클린 아키텍처(UseCase 중심)는 필요한 로직만 집중해서 읽게 하여 토큰 효율을 10배 이상 높임
  - CQRS와 클린 아키텍처의 조합이 AI 협업에 가장 최적화된 구조임

# 인사이트 / 할 일
- AI를 단순한 코딩 도구가 아니라 팀의 구성원(역할)으로 정의하고 뇌(Context)를 설계해주는 관점이 중요함
- 문서화는 인간뿐만 아니라 AI와의 협업을 위해서도 필수적이며, 특히 구조화된 데이터(JSON 등)가 AI의 추론 능력을 극대화함
- 클린 아키텍처의 가치가 AI 시대에 '컨텍스트 격리'라는 측면에서 더욱 중요해졌음을 인지함
- 실행할 액션: 현재 운영 중인 서비스의 API 및 도메인 지식을 JSON DSL 형태로 정리하여 AI에게 학습시켜보기
- 실행할 액션: Claude Code의 Skill 기능을 활용하여 반복적인 배포/리뷰 프로세스 자동화 시도하기