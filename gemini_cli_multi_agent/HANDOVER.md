# 🏁 Project Handover & Context Memo

**Last Updated**: 2026-03-13 (Day 2)
**Status**: 🟢 Active Development

---

## 🎯 Current Project Goal
- **Upbit Professional Quant Platform (A-G)**: 구축된 엔진을 계층형 아키텍처로 전환하고, 병렬 최적화 및 자금 관리 레이어 추가.

## 🚦 Active Processes & Resource Locks
- **Running**: `upbit_downloader.py` (PID: 확인 중, `python.exe` 프로세스 점유 중)
- **Locked**: `data/`, `upbit_downloader.py` (수정 금지)
- **Safe Zone**: `test_data/`, `backtester/`, `strategies/` (개발 가능)

## 📋 Active Work Orders (WO Status)
| WO ID | Task Name | Assigned Agent | Status | Next Step |
| :--- | :--- | :---: | :---: | :--- |
| **WO-20260313-02** | Optimization Engine | `system_architect_agent` | **DESIGNING** | 아키텍처 설계안 확정 후 `builder` 전달 |

## 📍 Next Resumption Point (이어서 할 작업)
1. **[Architect]** `main_optimize.py` 및 `optimization/` 모듈의 병렬 처리 구조 설계 완료.
2. **[Builder]** `test_data/`의 BTC/ETH 데이터를 활용하여 병렬 백테스트 프로토타입 구현.
3. **[another_me]** 전체 로드맵(A-G) 중 [E] 데이터 무결성 체크 로직 우선순위 재점검.

## 🧠 Key Decisions (Today's Pivot)
- **Survival Bias**: 백테스트 시 상폐 종목 포함, 실전 시 자동 제외하는 이원화 전략 확정.
- **Modularization**: 7대 계층형 폴더 구조(`docs/system_improvement_roadmap_v2.md` 참조)로의 리팩토링 결정.

---
**another_me의 메모**: "다음 세션 시작 시 `HANDOVER.md`를 먼저 로드하고, `system_architect_agent`를 호출하여 최적화 엔진 설계를 마무리할 것."
