# BadChain — Whitepaper

> **Decentralized On-Chain Forensics for a Cleaner Crypto Economy**
> 탈중앙 온체인 체인 어낼리시스 블록체인
>
> v0.1 · 2026-05-15

---

## 목차

1. [Abstract](#abstract--초록)
2. [서론](#1-서론--introduction)
3. [핵심 개념](#2-핵심-개념--core-concepts)
4. [아키텍처](#3-아키텍처--architecture)
5. [토크노믹스](#4-토크노믹스--tokenomics)
6. [활용 사례](#5-활용-사례--use-cases)
7. [보안과 게임 이론](#6-보안과-게임-이론--security--game-theory)
8. [거버넌스](#7-거버넌스--governance)
9. [로드맵](#8-로드맵--roadmap)
10. [결론](#9-결론--conclusion)
11. [부록](#부록--appendix)

---

## Abstract / 초록

**BadChain**은 크립토 해킹·탈취 자금의 흐름을 *탈중앙적으로* 추적하고, 그 결과를 외부 서비스(거래소·DeFi·지갑)가 자유롭게 조회할 수 있는 위험도 지수 **Bad Point**로 산출하는 **Cosmos SDK 기반 소버린 블록체인**이다.

기존 NFT 컬렉션 **Bad Kids**가 블록체인의 수호자(*Balidator*)로 기능하며, **PoBK(Proof of Bad Kids)** 합의로 운영된다. **바운티 헌터** 누구나 자금 흐름의 한 hop을 제출하고 검증 통과 시 보상받는 개방형 구조로 — 중앙화된 체인어낼리시스 서비스(Chainalysis 등)가 제공하지 못하는 *투명성·검증 가능성·참여 가능성*을 결합한다.

---

## 1. 서론 / Introduction

### 1.1 문제 정의

2022–2026년 사이 크립토 생태계는 **수십억 달러 규모**의 해킹·탈취 사고를 누적해 왔다:

| 사건 | 피해액 | 주요 행위자 |
|---|---|---|
| Ronin Bridge (2022) | $620M | Lazarus Group |
| Harmony Horizon (2022) | $100M | Lazarus Group |
| Nomad Bridge (2022) | $190M | Mixed actors |
| Heco Bridge (2024) | $100M | Lazarus Group |
| 2025 $282M 사례 등 | 수십억+ | Various |

이에 대한 기존 대응은 두 갈래로 갈라져 있다:

1. **중앙화 체인어낼리시스 (Chainalysis, Elliptic, TRM Labs)** — 정확하지만 *폐쇄적·고비용·검증 불가능한 블랙박스*
2. **개별 독립 추적자 (ZachXBT 등)** — 투명하고 효과적이지만 *지속 가능한 경제 인센티브 부재*

### 1.2 BadChain의 비전

BadChain은 두 접근의 **장점만 결합**한다:

- **개방성** — 누구나 자금 흐름의 한 조각(엣지)을 제출하고 검증 통과 시 보상
- **검증 가능성** — 모든 추적 결과가 온체인 기록, 결정론적 재검증 가능
- **경제 지속성** — 사건 풀 deposit이 추적 노동의 시장가를 자연 형성
- **신뢰 분산** — PoBK 합의로 단일 주체가 결과를 좌우할 수 없음

핵심 통찰: *해커가 자금을 쓰지 못하게 만드는 블록체인*이 아니라, **해커의 자금 흐름을 모두가 검증 가능하게 만드는 블록체인**.

---

## 2. 핵심 개념 / Core Concepts

### 2.1 Bad Kids NFT — 살아있는 수호자

**Bad Kids**는 기존 9,999개 NFT 컬렉션(Stargaze 발 / Cosmos Hub 마이그레이션 중)으로, BadChain으로 **전면 브릿지**되어 합의의 정체성 단위가 된다. 각 NFT는 trait/rarity 기반의 가변 **Bad Power**(보팅파워)를 가진다.

원래 비전 — *"Bad Kids는 블록체인의 수호자"* — 가 메타포가 아니라 *문자 그대로* 구현된다.

> **출처·권리 관계.** **Bad Kids NFT 컬렉션은 본 프로젝트의 발행자(1XP Inc.)가 발행한 컬렉션이 아니다.** Stargaze 생태계에서 외부 주체에 의해 발행된 9,999개 컬렉션이다. 각 NFT는 *민팅한 보유자(holder)의 자산*이며, NFT의 사용·이전 권한은 그 NFT를 보유한 주체에게 귀속된다. BadChain은 보유자가 자신의 NFT를 PoBK 합의의 *검증인(Balidator) 정체성 단위*로 활용할 수 있도록 **브릿지 인프라**를 제공할 뿐 — 별도의 발행자 측 합의를 전제하지 않으며, 브릿지는 NFT 보유자 본인의 의사로만 발생한다. 이 백서는 BadChain의 기술 설계를 다루며, Bad Kids 컬렉션 자체에 대한 어떠한 권리 주장도 하지 않는다.

### 2.2 Balidator — *validator*의 v → *bad*의 b

1개 이상의 Bad Kids NFT를 등록한 검증인. 각 Balidator는 **AI agent 사이드카** 하나를 운영하며 외부 체인(Ethereum, BSC 등)의 RPC를 통해 추적 증거를 검증한다.

비유: 경찰서 하나당 agent 하나.

### 2.3 Bad Kids 스쿼드 — "경찰서" 단위

한 Balidator에 위임된 Bad Kids NFT 집합. 스쿼드의 보팅파워 = 스쿼드 NFT들의 Bad Power 합산. 위임은 *Bad → NFT → Balidator* 2단계.

### 2.4 Bounty Hunter — 추적자

**Bad 잔고가 있는 누구나.** `MsgSubmitEdges`로 자금 흐름의 한 hop(또는 분기·합류된 서브그래프)을 제출하고, 검증 통과 시 사건 풀에서 **bounty + bond 반환**을 보상받는다. 등록 절차 불필요 — *진입 장벽 0*.

ZachXBT 같은 독립 추적자 모델의 **프로토콜 레벨 일반화**.

### 2.5 Bad vs Bad Point — 구분

- **Bad (네이티브 코인)**: 사건 풀 deposit, 헌터 bond, 거버넌스 토큰. 10% 인플레이션.
- **Bad Point**: 체인의 **최종 산출물**. 주소의 위험도 지수. 외부 서비스가 조회하는 유일한 값. *코인 아님*.

---

## 3. 아키텍처 / Architecture

### 3.1 Flow DAG — 자금 흐름의 그래프 모델

자금 추적은 **방향 비순환 그래프(DAG)**로 모델링된다:

- **노드** = 주소
- **엣지** = 자금이 한 번 이동한 hop

```
                    seed (해킹 TX)
                          │
                          ▼
                       0xHacker1
                       /        \
                      ▼          ▼
                 0xWallet2   0xWallet3
                    │           │
                    ▼           ▼
              [Tornado Cash]  Exchange
                    │           
                    ▼ (HEURISTIC 엣지)
              0xMixedOutput
```

분할(splitter), 합류(consolidation), 브릿지 hop이 모두 DAG의 엣지로 통일된다.

### 3.2 Typed Edges — 단일 스키마, 3-tier 신뢰도

모든 엣지는 `linkType`을 가진다. 믹서(Tornado Cash) 추적 현실에 기반한 3-tier 모델:

| linkType | 의미 | 비중 (믹서 출금 기준) | 밸리데이터 검증 |
|---|---|---|---|
| `DETERMINISTIC_ONCHAIN` | 일반 hop, 또는 믹서의 온체인 실수(주소 재사용·직접 전송) | 일반 hop + 믹서 ~5–13% | **완전 검증** |
| `DETERMINISTIC_ZKPROOF` | note 보유자가 자발 제출한 Merkle+ZK 증명 | opt-in | **완전 검증** |
| `HEURISTIC` | 타이밍·gas 지문·금액 매칭·FIFO·지갑 지문 등 상관관계 집합 | 믹서 ~85–95% | 상관관계 재확인 + 임계값 |

각 엣지는 `confidence`(LOW/MEDIUM/HIGH 이산 등급)와 `evidence`(검증 데이터)를 함께 보유한다.

> **현실 근거**: 학술 연구에 따르면 Tornado Cash 출금의 약 **21–35%**가 입금과 연결 가능하며, 그중 5–13%는 결정론적으로 입증 가능하다. BadChain의 3-tier 모델은 이 현실을 그대로 반영한다.

### 3.3 사건 풀 (Case Pool)

하나의 해킹 사건 단위. 누구나 `MsgCreateCasePool`로 생성하며:
- 1개 이상의 **seed TX**(해킹 TX) 또는 **root address 집합** 등록
- 초기 **Bad deposit** 예치 (bounty 재원)

사건 풀은 *하나의 canonical DAG*를 가지며, 여러 헌터가 엣지를 제안·자동 merge한다. 한 엣지가 여러 풀의 범위에 속하면 각 풀에서 bounty 수령 가능.

### 3.4 AI Agent 사이드카

Balidator당 하나. 다음 두 종류 검증을 수행:

**결정론적 검증** (모든 엣지):
1. TX 존재성 + confirmed 여부
2. 주소 연속성 (이전 hop 수신자 = 다음 hop 송신자)
3. 금액 연속성 (inflow ≈ outflow, 허용 오차 내)
4. 잔액 snapshot 검증 (haircut 비율 계산용)

**휴리스틱 검증** (HEURISTIC 엣지):
- 상관관계 집합이 온체인에 실재하는지 확인
- 프로토콜 임계값 (≥ N개 유형 일치 등) 적용

**핵심 원칙**: AI agent의 *발견*은 비결정론적, *검증*은 결정론적. AI가 어떤 입금↔출금이 연결됐을지 추론하되, 그 결과의 검증은 "온체인 사실 + 프로토콜 파라미터"라는 결정론적 함수.

판정만 데몬에 전달, 데몬이 밸리데이터 키로 서명·브로드캐스트 (**모델 α**). 사이드카 프로세스에 키 없음 → 공격 표면 최소화.

### 3.5 Attestation — 분산 검증 흐름

```
┌─────────────────────────────────────────────────────────┐
│  1. 헌터: MsgSubmitEdges + bond 제출                    │
│  ───────────────────────────────────────────────────    │
│  2. CometBFT 블록 포함 → 엣지들 'pending_attestation'   │
│  ───────────────────────────────────────────────────    │
│  3. 각 사이드카: 외부 RPC 검증 → MsgSubmitAttestation   │
│  ───────────────────────────────────────────────────    │
│  4. EndBlocker: 체인 커버 밸리데이터 Bad Power 2/3+     │
│     동의 시 → 엣지 confirmed                            │
│  ───────────────────────────────────────────────────    │
│  5. 분배: 헌터 bond 반환 + bounty + 스쿼드 분배         │
└─────────────────────────────────────────────────────────┘
```

핵심 설계:
- **TX = 자연 batch** — 별도 batch 객체 없음. submission TX 자체가 attestation 단위 (git commit 비유)
- **체인별 독립 attestation** — 한 submission TX의 멀티체인 엣지는 체인별로 독립 attestation, 각 체인 커버 밸리데이터가 검증
- **정족수 = 체인 커버 밸리데이터의 2/3+** — 전체 셋이 아님 (옵션 B + 옵션 C)
- **참여 인센티브** — 2/3 통과한 블록의 attestation 참여 밸리데이터만 보상 → 커버리지 확대 자연 유도

### 3.6 Bad Point — 위험도 지수

각 confirmed 엣지가 *순방향* 주소(seed에서 도달 가능)에 Bad Point를 부여한다.

**공식**:
```
BadPoint(addr) = Σ over valid paths from seed to addr:
                   amount × hopDecay(h) × timeDecay(t) × Π(confidence)
```

| 요소 | 함수 형태 | 의미 |
|---|---|---|
| `amount` | seed txid 파싱 + 오라클 정규화 (USD 등) | 탈취 자금 규모 |
| `hopDecay(h)` | 지수: `λ^h` (λ는 거버넌스) | seed에서 멀수록 감쇠 |
| `timeDecay(t)` | 선형 + floor (각 엣지 TX 시각 기준) | 시간 지나도 최소치까지만 감소 |
| `confidence` | LOW/MED/HIGH 이산 등급 (linkType별 매핑) | 불확실성 누적 곱 |

**핵심 특성**:
- **사건 간 누적**: 한 주소가 사건 A·B에서 각각 누적 Bad Point 받음
- **영구 유지**: 사건 풀 폐기되어도 Bad Point는 유지 (시간 감쇠는 floor까지만)
- **화이트리스트 제외**: 거래소·DEX·브릿지 컨트랙트는 배정 제외
- **블랙리스트 표시**: 믹서는 별도 표시
- **heuristic 플래그**: HEURISTIC 기반 분량은 별도 플래그로 노출 — *확신도 과대주장 안 함*

### 3.7 Taint Attribution — Proportional (Haircut)

Mixed-fund 주소(tainted + clean 섞임)에서 outflow 발생 시:

> **비례 분산**: 주소 전체 비율대로 모든 outflow에 distributiona

예: 100 tainted + 900 clean = 1000 ETH 보유 주소가 500/300/200 ETH 3건 송금 →
- 각각 50/30/20 ETH가 tainted (모두 10% 비율)

Chainalysis 등 체인어낼리시스 업계 표준. 결정론적(블록 내 TX 순서 무관). FIFO·poison pill 모두 채택 안 함.

**Conservation 검증** (보존):
1. **주소별**: 각 주소에서 tainted outflow ≤ tainted inflow (증폭 불가)
2. **글로벌**: DAG 전체 tainted 합 ≤ 사건 풀 seed 총 피해 금액

---

## 4. 토크노믹스 / Tokenomics

### 4.1 Bad 토큰 발행 및 분배

- **초기 발행량**: Bad Kids NFT 수량 비례 (~9,999 NFT 기준 + 거버넌스 조정)
- **인플레이션**: 10%/년 (밸리데이터·위임자 보상)
- **초기 50% 에어드랍**: **Atom & AtomOne 위임자** 대상 (거래소 주소 포함)

### 4.2 가치 흐름

```
┌──── 사건 풀 deposit (사용자/피해자) ────┐
│                                          │
│   attestation 확정 시 분배:               │
│   ├─ 증거 제출자 bounty (헌터)           │
│   └─ 검증 스쿼드 분배:                   │
│      • 밸리데이터 커미션                  │
│      • Bad Kids NFT 위임자 (Bad Power)   │
│      • Bad 위임자 (Bad Power)            │
└──────────────────────────────────────────┘

┌──── 블록 보상 (10% 인플레이션) ─────────┐
│   Cosmos SDK 표준 위임 모델              │
│   → 밸리데이터 + 위임자 분배              │
└──────────────────────────────────────────┘

┌──── 추적 숨김 Burn ──────────────────────┐
│   Bad Address 소유자가 Bad Point 제거시  │
│   → 탈취 금액 비례 Bad 영구 소각          │
│   → 시그널링 + 경제적 벌금                │
└──────────────────────────────────────────┘
```

> **중요**: 추적 비용 측면의 *Burn은 없음*. 사건 풀 deposit은 전액 헌터/스쿼드에게 분배. Burn은 오직 "추적 숨김" 기능에서만 발생.

### 4.3 헌터 경제 — 시장 기반 추적 우선순위

| 요소 | 규칙 |
|---|---|
| 진입 자격 | Bad 잔고만 있으면 누구나 (등록 불요) |
| Bond 구조 | `Σ(bondRate[edgeType])` — DET 저렴/0, HEU 고가 |
| 보상 | 사건 풀 bounty + bond 전액 반환 |
| 실패 시 | bond slash (일부는 dispute 제기자에게) |

**bounty ∝ deposit** 메커니즘 — 큰 해킹(큰 deposit)에 추적자가 자연히 몰리고, deposit 적은 spam 풀은 bounty ~0이라 자연 무력화된다. *하드 임계값 없이* 경제적 자연 억제.

### 4.4 Bad Power와 거버넌스

- Bad Power는 **NFT의 trait/rarity 기반 가변** + 위임된 Bad 스테이킹으로 부스트
- Bad는 **NFT에만** 스테이킹 (밸리데이터 직접 아님). NFT가 파워 단위
- 미위임 NFT도 Bad Power 보유 가능 (위임 전까지 잠재 파워)
- 파워 집중 상한 없음 (표준 DPoS — 단순성 우선)

---

## 5. 활용 사례 / Use Cases

### 5.1 거래소 (CEX / DEX)
- **입금 스크리닝**: 입금 주소의 Bad Point 조회 → 임계값 초과 시 동결·재검증
- **화이트리스트 자기 등록**: 거래소가 자기 핫월렛 주소를 화이트리스트에 신청 (bond + 이의제기 기간)
- **컴플라이언스 보고**: 거래소가 적극 참여할수록 평판 ↑

### 5.2 DeFi 프로토콜
- **LP·스왑 전 카운터파티 스크리닝** — 오염 자금 유입 차단
- **프로토콜 평판 보호** — 해킹 자금이 자기 풀에 흘러들지 않도록

### 5.3 지갑 서비스
- **송금 전 수신 주소 경고** — Bad Point 높으면 사용자에게 위험 알림
- **자기 주소 모니터링** — 사용자가 자기 주소가 잘못 flagged 되면 ZK 셀프 구제

### 5.4 컴플라이언스 / RegTech
- **AML/KYT** (Know Your Transaction) 인프라
- **규제 보고용 표준 데이터 소스** — 검증 가능한 출처

### 5.5 화이트햇 추적자
- **직업적 바운티 헌터** — 정직한 추적의 경제 지속성
- 기존 ZachXBT 모델의 **프로토콜 레벨 일반화**

### 5.6 AI 추적 agent 개발자
- 외부 AI agent가 MCP로 BadChain에 단서 제공 → 블록 생성 기여 시 Bad 보상
- mixing 추적 기술을 가진 전문 AI agent의 수익 모델

---

## 6. 보안과 게임 이론 / Security & Game Theory

### 6.1 Anti-Griefing — 다층 방어

1. **Bond + Slashing** — 허위 엣지 제출 시 헌터 bond 슬래시 (제출자 단위)
2. **Conservation 검증** — DAG의 inflow ≈ outflow + 사건 풀 seed 총 피해 초과 자동 거부 (구조적)
3. **그룹 연동 attestation** — 같은 src 주소의 분기 outflow는 all-or-nothing 판정 (haircut 일관성)
4. **거버넌스 dispute** — 무고한 주소 정정은 거버넌스 프로포절 + 투표 (저비용 dispute 금지)
5. **ZK 셀프 구제** — 무고한 사용자가 Tornado note로 결백 증명 (HEU 엣지의 dispute window 내)

### 6.2 추적 숨김 — 시그널링 + 벌금

해커가 Bad Point를 제거하려면 *탈취 금액 비례* Bad를 영구 소각해야 한다.

| 효과 | 의미 |
|---|---|
| **벌금** | Burn 비용이 탈취 금액 이상 → 자금 세탁 경제성 없음 |
| **시그널링** | Burn 행위 자체가 영구 온체인 이력 → "돈 내고 숨기려 한 주소" 식별 가능 |

거래소·서비스는 `BadPoint = 0`이지만 `Burn 이력 존재`인 주소를 *위험 신호*로 읽을 수 있다.

### 6.3 믹서 추적 — 3-tier 현실 모델

> **정직한 한계 인지**: 정교한 행위자는 회피 가능. *완벽한* 추적이 아니라, *충분히 비싼 evasion 비용* 부과가 목표.

| Tier | 비중 (믹서 출금) | BadChain 처리 |
|---|---|---|
| 결정론적 (해커 실수) | ~5–13% | 일반 엣지 동급 검증·신뢰 |
| ZK 자기 구제 | opt-in | 풀 컨트랙트 대조 검증 |
| 휴리스틱 | ~85–95% | 상관관계 집합 + 임계값, confidence 가중 |

**역설적 효과**: 불완전한 믹서 추적도 공격자를 *DeFi 체인호핑*으로 밀어내며 — 그건 바로 BadChain의 멀티체인 DAG가 *쫓도록 설계*된 것.

### 6.4 검증 결정론성

핵심 원칙: 검증은 *비정형 추론이 아니라 구조화된 true/false 조회*.

"주소 X에 얼추 비슷한 수량이 있거나, 있었던 흔적이 있는가?" → 진짜 결정론적이려면 3개 프로토콜 파라미터 고정:

1. "얼추 비슷한"의 허용 오차 임계값
2. "있었던 흔적"의 lookback 윈도우 + archive 데이터 접근
3. 검증 기준 블록 높이 고정 (pinning)

→ AI agent가 검증해도 입력 같으면 출력 같음. *"AI 옷을 입은 결정론적 함수."*

---

## 7. 거버넌스 / Governance

- **표준 Cosmos SDK governance** — 파라미터 변경, 화이트리스트 추가, 체인 어댑터 승인
- **Expedited Proposals (신속제안)** — 긴급 화이트리스트 추가 등 빠른 대응. 별도 multisig council 없음 — 탈중앙성 유지
- **거버넌스 투표자 인센티브** — 커뮤니티 풀에서 분배 (투표율 형해화 방지)
- **체인 어댑터 = 스펙** — 새 체인 지원은 컴파일 코드가 아니라 "RPC + 인식 방법" 스펙. 모듈식 어댑터 + 거버넌스 승인으로 확장

---

## 8. 로드맵 / Roadmap

### Phase 1 — MVP (6개월)
- `x/casepool` 모듈: 사건 풀 생성·deposit·canonical DAG
- `x/dag` 모듈: 엣지 제출·attestation·conservation 검증
- `x/badpoint` 모듈: Bad Point 계산·조회 인터페이스
- **단일 체인 (Ethereum) 지원**

### Phase 2 — Bad Kids 통합 (3개월)
- Bad Kids NFT BadChain 브릿지
- PoBK 합의 활성화
- Bad 토큰 인플레이션 분배 시작
- 50% 에어드랍 to Atom·AtomOne 위임자

### Phase 3 — AI Agent 사이드카 (6개월)
- 사이드카 binary 출시
- 체인 어댑터 스펙 시스템 + 거버넌스 승인 흐름
- HEURISTIC 엣지 지원 (믹서 추적 활성화)

### Phase 4 — Multi-chain 확장 (지속)
- BSC, Polygon, Arbitrum 등 EVM 체인 추가
- Bitcoin, Solana 어댑터 (모듈식)
- Cosmos IBC 체인 통합

### Phase 5 — 생태계 (지속)
- 거래소·DeFi 통합 SDK
- 자기 등록 화이트리스트 거버넌스
- AI 추적 agent 마켓플레이스

---

## 9. 결론 / Conclusion

BadChain은 *해커가 자금을 쓰지 못하게 만드는 블록체인*이 아니라 — **해커의 자금 흐름을 모두가 검증 가능하게 만드는 블록체인**이다.

추적의 성공은 **정직한 참여자의 경제적 보상**에서 비롯되고, 그 보상은 탈취 자금 회수에 가치를 둔 시장이 지불한다. 중앙화된 단일 기업의 영업 비밀이 아니라 **공공재로서의 체인 어낼리시스**.

Bad Kids NFT가 단순한 디지털 아트가 아니라 **체인의 살아있는 수호자**가 되는 이 모델은, Cosmos 생태계의 차세대 가치 제안 — **문화적 자산이 인프라가 되는 모델** — 의 한 실증이 될 것이다.

> *"The serpent does not repeat — it evolves."*

---

## 부록 / Appendix

### A. 기술 스펙 요약

| 항목 | 결정 |
|---|---|
| Consensus | PoBK on CometBFT (전체 셋 2/3 블록 합의 + 별도 attestation 모듈) |
| Language | Go (Cosmos SDK) |
| External Chain Support | EVM 시작, 모듈식 어댑터 + 거버넌스 승인으로 확장 |
| State Architecture | 이벤트 기반 Bad Point 캐시 + 시간 감쇠 실시간 적용 |
| Query Interface | 표준 Cosmos ABCI Query (요약/상세 두 엔드포인트, **무료**) |
| Bond/Slashing | 헌터 bond — 엣지 수 × 유형 단가. 검증 실패 시 slash |
| AI Agent | Balidator당 1개 사이드카. 모델 α (데몬이 서명) |

### B. 핵심 결정 요약 (인터뷰 21라운드 결과)

| 영역 | 결정 |
|---|---|
| 신뢰 루트 | 즉시 반영, 사후 정정. 거버넌스 dispute |
| NFT 모델 | Bad Kids 전면 브릿지. 미위임 NFT도 Bad Power |
| 블록 모델 | 혼합 단일 블록. 한 블록에 전체 flow |
| flow 구조 | DAG (분기·합류·믹서 모두 typed edge) |
| Attestation | TX = 자연 batch. 체인별 독립. 정족수 = 체인 커버의 2/3+ |
| 검증 | 결정론적 true/false 조회. 3 파라미터 고정 |
| 믹서 | 3-tier typed edge (DET_ONCHAIN / DET_ZKPROOF / HEURISTIC) |
| Bad Point | `amount × hopDecay × timeDecay × Π(confidence)`. 영구 유지 |
| Taint 배분 | Proportional Haircut + 주소별·글로벌 conservation |
| 헌터 | 허가 불요. bond = 엣지수 × 유형 단가. 보상 = 사건 풀 + bond 반환 |
| 거버넌스 | 표준 + Expedited Proposals. multisig council 없음 |

### C. 참고 자료

- Tornado Cash de-anonymization 학술 연구: arXiv:2510.09433
- Chainalysis methodology
- [Cosmos SDK 문서](https://docs.cosmos.network)
- ZachXBT 사례 분석
- Bad Kids NFT (Stargaze): `stars19jq6mj84cnt9p7sagjxqf8hxtczwc8wlpuwe4sh62w45aheseues57n420`

### D. 사양서 (SEED)

본 백서는 **Ouroboros 방법론**으로 21라운드 Socratic 인터뷰를 거쳐 결정화된 **SEED 명세** (`SEED.md`)에서 도출되었다. 자세한 결정 이력·근거·미해결 항목·권고 기본값은 SEED.md 참조.

---

## License

이 백서는 **[Creative Commons Attribution 4.0 International (CC-BY 4.0)](https://creativecommons.org/licenses/by/4.0/)** 라이센스로 배포된다. 출처를 표시하면 누구나 자유롭게 사용·공유·번역·각색할 수 있다. 라이센스 전문은 본 저장소 [`LICENSE`](./LICENSE) 파일을 참조하라.

**권장 출처 표시:** `BadChain Whitepaper © 2026 1XP Inc., CC-BY 4.0, https://github.com/1XP-Inc/badchain-whitepaper`

---

*BadChain · 2026 · v0.1*
