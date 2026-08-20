# AWS AI-DLC Base for VS Code Codex

VS Code의 Codex 확장에서 AWS AI-DLC(AI-Driven Development Life Cycle)
워크플로우를 사용할 수 있도록 구성한 베이스 프로젝트입니다.

프로젝트 루트의 `AGENTS.md`가 전체 AI-DLC 워크플로우를 정의하고,
`.aidlc-rule-details/`가 각 단계의 상세 규칙을 제공합니다. 사용자는 요구사항을
작성한 뒤 Codex에 시작 명령을 전달하면 됩니다.

AI-DLC는 작업공간과 요청의 복잡도를 분석하여 다음 과정을 필요한 수준으로
진행합니다.

1. **Inception**: 작업공간 감지, 요구사항 분석, 설계 및 작업 계획
2. **Construction**: 상세 설계, 코드 생성, 빌드 및 테스트
3. **Operations**: 향후 배포와 운영 워크플로우를 추가하기 위한 자리표시자

## 프로젝트 구성

| 경로                           | 역할                                  |
| ------------------------------ | ------------------------------------- |
| `AGENTS.md`                    | Codex가 따르는 AI-DLC 핵심 워크플로우 |
| `.aidlc-rule-details/`         | 단계별 상세 실행 규칙                 |
| `requirements/requirements.md` | 신규 프로젝트 요구사항 템플릿         |
| `requirements/constraints.md`  | 제외 범위와 제약사항 템플릿           |
| `dlc-example/`                 | 요구사항 작성 예시                    |

AI-DLC가 시작되면 분석 결과와 단계별 산출물은 `aidlc-docs/`에 생성됩니다.
애플리케이션 소스 코드는 `aidlc-docs/`가 아닌 프로젝트 루트에 생성됩니다.

## 사전 준비

1. VS Code에 [Codex 확장](https://learn.chatgpt.com/docs/codex/ide)을 설치합니다.
2. 이 저장소를 새 프로젝트의 베이스로 사용하거나, 필요한 AI-DLC 파일을 기존
   프로젝트의 루트에 복사합니다.
3. VS Code에서 프로젝트 루트 폴더를 엽니다.

## 신규 프로젝트 시작하기 — Greenfield

Greenfield는 기존 애플리케이션 코드가 없는 상태에서 새 서비스를 만드는
프로세스입니다.

### 1. 요구사항 작성

다음 두 파일의 안내에 따라 내용을 작성합니다.

- [`requirements/requirements.md`](requirements/requirements.md): 프로젝트 개요,
  핵심 기능, MVP 범위와 비기능 요구사항
- [`requirements/constraints.md`](requirements/constraints.md): 구현하지 않을 기능,
  기술·환경·보안·일정상의 제약

처음부터 모든 내용을 확정할 필요는 없습니다. 결정하지 못한 항목은 `미정`으로
남기면 Inception 단계에서 Codex가 필요한 질문을 합니다.

### 2. AI-DLC 시작 명령 입력

VS Code의 Codex 채팅에서 아래 내용을 입력합니다. `[서비스명]`은 실제 서비스
이름과 설명으로 바꿉니다.

```text
[서비스명] 서비스를 구축하고 싶습니다.

다음 파일에서 요구사항과 제약사항을 읽어주세요.

- requirements/requirements.md
- requirements/constraints.md

AGENTS.md의 AI-DLC 워크플로우를 따라 Inception 단계부터 시작해주세요.
각 단계의 산출물을 작성하고, 사용자 확인이 필요한 지점에서는 작업을 멈추고 질문해주세요.
```

Codex는 작업공간을 Greenfield로 판별한 뒤 요구사항 분석부터 진행합니다. 단계별
검토나 승인을 요청하면 내용을 확인하고 답변해야 다음 단계로 진행됩니다.

### 정상적으로 시작되지 않는 경우

일반적인 개발 요청으로 처리되거나 AI-DLC 단계가 시작되지 않았다면 다음과 같이
입력합니다.

```text
그린필드 프로세스로 AI-DLC를 다시 시작해주세요.
```

이 요청은 현재 작업을 AI-DLC 신규 개발 프로세스로 다시 판별하고 Inception부터
진행하라는 의미입니다.

## 기존 프로젝트 기능 변경하기 — Brownfield

Brownfield는 이미 소스 코드가 있는 프로젝트에 기능을 추가하거나 기존 기능을
변경하는 프로세스입니다. Codex는 시작 시 작업공간의 소스 코드와 빌드 파일을
확인해 Brownfield 여부를 판별합니다.

### 1. 변경 요구사항 작성

`requirements/feature-update.md` 파일을 만들고 다음 내용을 중심으로 변경 요구사항을
작성합니다.

- 변경하려는 기능과 목적
- 현재 동작과 문제점
- 추가하거나 수정할 동작
- 완료 조건 또는 인수 기준
- 변경하지 않을 범위와 제약사항

간단한 작성 예시는 다음과 같습니다.

```markdown
# [기능명] 변경 요구사항

## 1. 변경 목적

[이 기능을 추가하거나 변경하는 이유]

## 2. 현재 동작과 문제점

- 현재 동작: [작성]
- 해결할 문제: [작성]

## 3. 변경 요구사항

- [추가 또는 수정할 동작]
- [예외 상황에서 필요한 동작]

## 4. 완료 조건

- [ ] [사용자가 확인할 수 있는 결과]
- [ ] [테스트로 확인할 수 있는 조건]

## 5. 제외 범위와 제약사항

- [이번 변경에서 다루지 않을 내용]
```

### 2. Brownfield AI-DLC 시작 명령 입력

```text
기존 프로젝트에 [기능명] 기능을 추가하거나 변경하고 싶습니다.

requirements/feature-update.md에서 변경 요구사항을 읽고
AGENTS.md의 AI-DLC 워크플로우를 시작해주세요.
```

기존 코드가 감지되면 Brownfield로 시작하며, 필요할 경우 먼저 Reverse
Engineering을 통해 현재 구조를 분석한 뒤 요구사항 분석으로 넘어갑니다.

> **중요:** `feature-update.md`라는 파일 이름만으로 Brownfield가 되는 것은
> 아닙니다. 작업공간에 기존 애플리케이션 소스 코드나 빌드 파일이 있어야 합니다.
> 기존 코드가 없다면 AI-DLC는 Greenfield로 판별합니다.

## 진행 중 참고사항

- 기존 `aidlc-docs/aidlc-state.md`가 있으면 AI-DLC는 새로 시작하지 않고 마지막
  진행 단계부터 이어갈 수 있습니다.
- 질문 파일이 생성되면 각 질문의 `[Answer]:` 위치에 답변을 작성합니다.
- 명시적인 검토나 승인을 요구하는 단계에서는 승인 전까지 다음 단계로 넘어가지
  않습니다.
- `dlc-example/`의 문서는 작성 형식을 이해하기 위한 예시이며, 실제 프로젝트에서는
  `requirements/`의 문서를 사용합니다.

## 참고자료

- [AWS AI-DLC Workflows](https://github.com/awslabs/aidlc-workflows)
- [Codex IDE 확장 공식 문서](https://learn.chatgpt.com/docs/codex/ide)
- [Codex의 AGENTS.md 공식 문서](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
