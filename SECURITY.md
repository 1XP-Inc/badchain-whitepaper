# Security Policy

## 보고 범위 (Scope)

이 저장소는 **백서(설계 문서)** 만 포함합니다. 실행 가능한 코드가 없으므로 전통적 의미의 보안 취약점(CVE 등) 대상은 아닙니다. 다만 다음은 신고를 환영합니다:

- 백서의 **보안 모델·게임 이론 헛점** (예: 공격 시나리오, 인센티브 왜곡)
- **암호학적 가정 오류** (ZK·해시·서명 관련 설명의 부정확함)
- **토큰 이코노미 취약점** (가치 흐름의 비대칭, 자금 세탁 우회 경로)
- **거버넌스 공격 벡터** (vote takeover, multisig 우회 등)
- **법적·규제 리스크** 발견 (특정 관할에서의 잠재적 문제)

> 코드 관련 보안 신고는 코드 저장소 [`1XP-Inc/badchain`](https://github.com/1XP-Inc/badchain) 의 SECURITY 정책 참조 (해당 저장소가 운영되기 시작하면).

## 신고 방법

### 민감한 사항 — 비공개 채널 권장
[**GitHub Security Advisory로 비공개 신고**](../../security/advisories/new) — 권장 경로. 신고자와 maintainer 사이의 비공개 채널을 자동 생성합니다.

### 공개 가능한 사항
일반 [Issue](../../issues)로 — `security` 라벨 권장.

### 신고 시 포함하면 좋은 정보
- 영향받는 백서의 섹션·줄
- 시나리오 (공격 흐름 또는 모순 상황)
- 잠재적 영향 (financial, reputational, technical)
- 가능한 완화책 또는 수정 제안 (선택)

## 응답 시간 (목표)

| 단계 | 목표 시간 |
|---|---|
| 접수 확인 | 7일 이내 |
| 초기 평가 | 14일 이내 |
| 해결·공시 일정 합의 | 사안별 |
| 책임 공개(disclosure) | 합의된 일정 또는 90일 |

## 책임 공개 (Responsible Disclosure)

심각한 이슈는 **수정·완화 방안이 마련된 후** 공개적으로 인정됩니다. 신고자는 (원하는 경우) 공시에 credit됩니다.

## 안전한 변경 프로세스

- `main` branch는 보호됩니다 (PR 리뷰 필수, force-push·삭제 불가)
- 보안 관련 수정 PR은 최우선 리뷰
- LICENSE·CODE_OF_CONDUCT·SECURITY 변경은 maintainer 합의 필요
