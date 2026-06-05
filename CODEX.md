# CODEX.md

## 프로젝트 기준

- 이 프로젝트는 화투를 기반으로 한 스토리형 싱글플레이 덱빌딩 로그라이크 게임이다.
- 1차 출시 목표는 Windows PC / Steam이다. Mac은 개발과 테스트 보조 환경으로 본다.
- Unreal Engine은 새 프로젝트 기준 5.6을 우선 검토한다. 5.1은 특정 강의, 플러그인, 에셋 호환성이 필요할 때만 선택한다.
- 개발 환경은 MacBook Pro M3 Pro 18GB와 Desktop RTX 4060 Ti 16GB / Ryzen 5 7500F / RAM 32GB를 기준으로 한다.
- MacBook에서는 C++ 규칙, 테스트, 문서, Git 작업을 우선 처리한다.
- Desktop에서는 Unreal Editor, Blueprint, 애니메이션, UI, 패키징, PIE 테스트를 우선 처리한다.

## 작업 전 읽을 문서

- 항상 이 파일을 먼저 읽는다.
- 게임 기획, 스토리, 모드, 보스, 카드 컨셉과 관련된 작업 전에는 `docs/GAME_DESIGN.md`도 읽는다.
- Unreal 프로젝트 구조나 C++ 구현 작업 전에는 기존 폴더 구조와 README를 먼저 확인한다.

## 기술 방향

- 핵심 게임 규칙은 `RulesCore`라는 순수 C++ 레이어로 분리한다.
- `RulesCore`는 Unreal Editor 없이 빌드하고 테스트할 수 있어야 한다.
- Unreal C++는 `RulesCore`를 Blueprint와 에디터에 연결하는 얇은 어댑터 역할을 한다.
- Blueprint는 애니메이션, UI, 사운드, VFX, 화면 연출을 담당한다.
- 카드와 밸런스 데이터는 Unreal `DataAsset`만 고집하지 않고 CSV/JSON도 함께 검토한다.
- 순수 C++ 테스트 프레임워크는 Catch2를 우선 추천한다.

## RulesCore 원칙

`RulesCore`에는 다음처럼 게임 판단에 필요한 로직을 둔다.

- 카드 사용 가능 여부
- 비용, 피해량, 방어도 계산
- 드로우, 버림, 소멸, 셔플 처리
- 턴 시작과 턴 종료
- 버프, 디버프, 상태 효과
- 적 행동, 전투 승패, 보상 처리

`RulesCore`에서는 Unreal 타입을 피한다.

- 피할 것: `UObject`, `AActor`, `UCLASS`, `UPROPERTY`, `TArray`, `FString`, `FName`, `UE_LOG`
- 선호할 것: `std::vector`, `std::string`, `std::optional`, `enum class`, 일반 `struct`/`class`, `int32_t`

## Codex 작업 규칙

- 사용자가 만든 변경사항을 허락 없이 되돌리지 않는다.
- 관련 없는 리팩터링을 섞지 않는다.
- 파일을 수정하기 전에 기존 구조를 먼저 확인한다.
- 검색은 가능하면 `rg`를 사용한다.
- 수동 편집은 `apply_patch`를 사용한다.
- 규칙 변경에는 가능하면 테스트를 추가한다.
- Unreal Editor 실행이 필요한 작업은 왜 필요한지 먼저 설명한다.
- `RulesCore`에 Unreal 의존성을 넣지 않는다.
