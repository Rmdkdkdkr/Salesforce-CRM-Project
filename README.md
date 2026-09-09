# RM Assistant CRM — 근본증권

> 증권사(근본증권) RM(자산관리 상담역)의 상담 준비부터 사후관리까지를 지원하는 Salesforce 기반 CRM

Lead(잠재고객) 발굴부터 Opportunity(영업기회) 전환, 상품 추천, 제안서 생성, 미팅 준비, 만기 관리, 성과(KPI) 추적까지 RM의 업무 흐름 전체를 Salesforce 위에서 지원하는 팀 프로젝트입니다. 반복 업무를 자동화하고, 생성형 AI(Agentforce)로 미팅·이메일 초안을 만들어 상담 준비 시간을 줄이는 것을 목표로 했습니다.

![RM Assistant 홈 대시보드](docs/images/home-dashboard.png)

## ✨ 주요 기능

| 영역 | 기능 | 관련 구현 |
|---|---|---|
| **Lead 관리** | 미팅 완료를 조건으로 한 Lead 전환, 전환 시 후속 Task 자동 생성 | `LeadConvertService`, `LeadEventConvertHandler`, Flow, Trigger |
| **상품 추천** | 고객 정보 기반 금융상품 추천 엔진 | `LeadRecommendationEngine`, `LeadProductRecommendationService` |
| **제안서 생성** | 고객 맞춤 제안서 PDF 자동 생성 | `LeadProposalController`, `LeadProposalPdfController` |
| **미팅 준비** | 상담 전 브리핑 자료 및 이메일 초안 자동 작성 (생성형 AI) | `MeetingPrepBriefingProvider`, `Meeting_Prep_Email_Draft` 프롬프트 템플릿 |
| **만기 관리** | 상품 만기 도래 고객 자동 알림 (배치) | `MaturityAlertBatch`, `MaturityAlertService` |
| **성과 관리** | RM 개인/영업기회 KPI 대시보드, 목표 대비 실적 추적 | `MyKpiController`, `OpportunityKpiController`, `KPI_Target` Custom Metadata |
| **영업기회 레이더** | 유망 고객 발굴 및 AI 이메일 초안 생성 | `OpportunityRadarController`, `Opportunity_Radar_Email_Draft` 프롬프트 템플릿 |

### 📸 주요 화면

**성과(KPI) 대시보드** — 영업기회 단계별 현황과 목표 대비 실적
![KPI 대시보드](docs/images/kpi-dashboard.png)

**미팅 준비 브리핑 (생성형 AI)** — 추천상품·재무·최근동향·미팅포인트를 상담 전 자동 정리
![AI 미팅 브리핑](docs/images/meeting-briefing.png)

**확정 조건서 PDF 자동 생성** — 상품·수수료 조건을 문서로 출력
![확정 조건서 PDF](docs/images/proposal-pdf.png)

<details>
<summary>🔎 화면 더 보기 (미팅 노트 · 영업기회 레이더)</summary>

**미팅 노트 — PII 마스킹 후 AI 상품 추천**
메모 원문의 개인정보를 마스킹한 뒤, 마스킹본만 Agentforce에 전달합니다.
![미팅 노트 마스킹](docs/images/meeting-note-masking.png)

**영업기회 레이더** — 화이트스페이스 기준 유망 고객 발굴 및 AI 이메일 초안
![영업기회 레이더](docs/images/opportunity-radar.png)

</details>

## 🏗 아키텍처 & 기술 스택

### 아키텍처 / 업무 흐름도

![아키텍처 및 RM 업무 흐름도](docs/images/architecture.png)

### 기술 스택

- **Apex** — Service / Controller / Handler / Action / Batch 로 책임을 분리한 계층 구조. 주요 로직마다 테스트 클래스를 작성해 커버리지를 확보했습니다.
- **Lightning Web Components (LWC)** — 홈 대시보드, KPI 카드, 상품추천, 제안서 생성기, 미팅 노트패드 등 16개 컴포넌트
- **Flow** — Lead 전환 전 미팅 필수 검증, 전환 후 Task 생성, 미팅 준비 이메일 초안 등 선언적 자동화
- **Apex Trigger** — Lead 전환 이벤트 처리
- **Einstein Generative AI (Prompt Templates)** — 미팅/영업기회 이메일 초안 자동 생성
- **Custom Metadata / Permission Set** — 분기별 KPI 목표 관리, RM 접근 권한 분리

| 구분 | 내용 |
|---|---|
| Platform | Salesforce (API v65.0) |
| Backend | Apex (32 classes, incl. tests), Trigger, Batch |
| Frontend | Lightning Web Components (16), Flexipage, Visualforce (PDF) |
| Automation | Flow, Generative AI Prompt Templates |
| Config | Custom Metadata Type, Permission Sets, Custom Tabs |

## 👤 담당 역할 (팀 리드)

- **데이터 모델링** — Lead / Opportunity 오브젝트 및 필드 설계
- **대시보드** — 홈 대시보드 및 KPI 대시보드 설계·구현
- **보안 설계** — Permission Set 기반 RM 접근 권한 분리 (RM Assistant / Opportunity Radar)
- **Agentforce 기능** — 생성형 AI 프롬프트 템플릿을 활용한 미팅·이메일 초안 자동 생성
- **작업 규칙 설계** — Flow·Trigger 기반 업무 자동화 규칙 설계
- 팀 리드로서 일정 관리, 역할 분배, 산출물 통합 담당

![데이터 모델 (ERD)](docs/images/erd.png)

## 📦 프로젝트 정보

- **팀**: team3 (팀 프로젝트)
- **성격**: Salesforce CRM 구축 최종 프로젝트

## 🚀 배포 방법

```bash
# Salesforce CLI 필요
sf org login web -a myOrg
sf project deploy start -d force-app -o myOrg
```

---

*이 저장소는 학습/포트폴리오 목적의 프로젝트입니다.*
