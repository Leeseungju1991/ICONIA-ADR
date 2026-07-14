# ICONIA · 아키텍처 의사결정 기록 (ADR)

> **작성자 / Author — LEE SEUNG JU** · antleorkfl00@naver.com

이 저장소는 **ICONIA**(AI 컴패니언 IoT 인형) 시스템을 설계하며 내린 핵심 결정을,
**“왜 그렇게 했는가”** 를 남기기 위해 기록한 **ADR(Architecture Decision Records)** 모음입니다.

> ADR은 “무엇을 만들었는가”가 아니라 **“어떤 문제 앞에서, 어떤 대안을 두고, 무엇을 감수하고 결정했는가”** 를 남기는 문서입니다.
> 코드는 *결과*만 보여주지만, ADR은 *판단 과정*을 보여줍니다.

---

## 왜 ADR을 쓰는가

- 🧭 **결정의 맥락이 사라지지 않는다.** — 6개월 뒤 “이거 왜 이렇게 했지?”에 문서가 답한다.
- ⚖️ **트레이드오프가 명시된다.** — 얻은 것뿐 아니라 **포기한 것**을 기록한다.
- 🤝 **합의의 근거가 된다.** — “말이 아니라 정의서로 합의한다”는 원칙의 실천.

---

## ICONIA 시스템 개요

ICONIA는 하나의 제품을 **6개 저장소**로 나눈 분산 시스템입니다.

| 레포 | 역할 |
|---|---|
| `ICONIA-HW` | ESP32 기기(인형) 펌웨어 — 절전·OTA·부팅 보안 |
| `ICONIA-SERVER` | 중앙 API · 단일 진실원천(SSOT) |
| `ICONIA-AI` | 페르소나 · 멀티모달 RAG · 외부 LLM 게이트 |
| `ICONIA-APP` | 사용자 모바일 앱 (React Native) |
| `ICONIA-ADMIN` | 운영자 콘솔 (Next.js) |
| `ICONIA-CI` | IaC · 무중단 배포 · 관측성 |

---

## ADR 목록

| # | 제목 | 상태 | 핵심 트레이드오프 |
|---|---|---|---|
| [0001](adr/0001-external-llm-isolation.md) | 외부 LLM 의존을 단일 게이트로 격리한다 | Accepted | 지연·복잡도 ↑ vs 장애 전파 차단 |
| [0002](adr/0002-signed-rpc-and-ordering.md) | 서비스 간 호출을 서명 RPC + 순서 보장으로 계약화한다 | Accepted | 처리량 ↓ vs 위·변조·순서 문제 제거 |
| [0003](adr/0003-token-rotation-auth.md) | 인증을 토큰 회전 + 재사용 감지로 탈취에 강하게 만든다 | Accepted | 토큰 관리 복잡도 ↑ vs 탈취 피해 봉쇄 |
| [0004](adr/0004-zero-downtime-deploy.md) | 배포를 5계층 무중단 파이프라인으로 자동화한다 | Accepted | 파이프라인 구축 비용 vs 배포 사고 제거 |
| [0005](adr/0005-firmware-anti-rollback.md) | 펌웨어를 되돌리기 불가능하게 3중으로 보장한다 | Accepted | 유연성(강제 다운그레이드 불가) vs 공급망 위조 차단 |

> 새 ADR은 [`template.md`](template.md)를 복사해 `adr/000N-제목.md`로 추가합니다.

---

## ADR 상태값

- **Proposed** — 제안됨(검토 중)
- **Accepted** — 채택되어 적용 중
- **Deprecated** — 더 이상 권장하지 않음
- **Superseded by ADR-XXXX** — 다른 결정으로 대체됨

---

## 작성 규칙

1. 하나의 ADR은 **하나의 결정**만 다룬다.
2. 채택된 ADR은 **수정하지 않는다.** 결정이 바뀌면 새 ADR을 만들고 이전 것을 `Superseded`로 표시한다.
3. **Consequences(결과)** 에는 반드시 *부정적 영향/감수한 비용*을 함께 적는다.
