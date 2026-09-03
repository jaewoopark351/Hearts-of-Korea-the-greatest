# 현재 상태 조사

조사일: 2026-09-03  
조사 범위: 정적 소스와 설치된 HOI4 1.19.2 정의 비교  
런타임 상태: 게임 실행 및 실제 플레이 검증 미실시

## 판정 기준

- **확인됨**: 파일, Git 상태 또는 대상 버전 바닐라 정의에서 직접 확인
- **강하게 뒷받침됨**: 여러 정적 근거가 일치하지만 게임 실행 증거는 없음
- **미확인**: 실제 플레이셋이나 런타임 검증이 필요

## 기준선

| 항목 | 상태 | 근거 |
|---|---|---|
| 원본 소스 | 확인됨 | `C:\hoi\hearts_of_korea` |
| 원본 Git 상태 | 확인됨 | `main`, `887930f6e88c80568d62dab9cfbe1ba8a498a252`, 조사 시 clean |
| 원본 성격 | 확인됨 | 역사적 Workshop 원본 그 자체가 아니라 1.19 대응 계승/수정 소스 |
| 원본 호환 표기 | 확인됨 | `descriptor.mod`의 `supported_version="1.19.*"` |
| 원본 의존성 | 확인됨 | `Korean Language` |
| 원본의 업로드 식별자 | 확인됨 | `3793992662`; 이 프로젝트가 물려받아서는 안 됨 |
| 역사적 Workshop 식별자 | 확인됨 | `2898629778`; 출처와 크레딧에만 사용 |
| 작업 프로젝트 | 확인됨 | `C:\hoi\Hearts of Korea the greatest` |
| 문서화 전 대상 Git 기준선 | 확인됨 | `main`, `a34163ac9319d090fddc8ac164781bb64edc9a50`, 조사 시 clean |
| 현재 대상 프로젝트 성격 | 확인됨 | 완전한 원본이 아니라 구형 소규모 강화 애드온 상태 |
| 로컬 바닐라 | 확인됨 | 설치 경로에서 HOI4 1.19.2.0, 체크섬 `d245` 계열 정의 확인 |
| 실제 목표 빌드·체크섬 | 미확인 | 게임 실행 전 별도 기록 필요 |
| DLC·플레이셋·로드 순서 | 미확인 | 런타임 작업 전 사용자 환경 확인 필요 |

## 기존 한국 시작 연구 상태

`history/countries/KOR - Korea.txt`에서 확인한 시작 상태는 다음과 같다.

- 시작 연구 슬롯은 2개다.
- 보병 장비, 공병·수색·지원 장비, 초기 포병/대공, 열차·트럭·연료 저장 등
  제한된 기초 기술만 지급한다.
- DLC 조건에 따라 초기 전차·함정·항공기 관련 기초 기술을 일부 지급한다.
- 현재 버전의 모든 산업·전자·보병·포병·기갑·항공·해군 기술을 완료하는 구조가 아니다.
- 과거 방식의 교리 기술 항목이 섞여 있어 1.19의 교리 시스템과 별도 대조가 필요하다.

기본 한국과 두 국뽕 초기화 어디에도 지급이 확인되지 않은 대표 핵심 기술:

```text
basic_machine_tools
construction1
concentrated_industry 또는 dispersed_industry
electronic_mechanical_engineering
radio
mechanical_computing
atomic_research
rocket_engines
support_weapons
interwar_artillery
```

강한 국뽕 초기화의 산업 기술은 사실상 `fuel_refining`과 `fuel_silos`뿐이며,
`fuel_silos`는 기본 한국 history에도 있어 중복된다.

판정: **모든 일반 연구 완료 상태가 아님 — 확인됨**.

## 기존 `gookppong` 효과

주요 파일:

- `common/on_actions/gookppong_on_actions.txt`
- `common/on_actions/gookppong_middle_power_on_actions.txt`
- 관련 게임 규칙 `kor_gookppong_status`

정적 조사 결과:

- 강한 프리셋은 연구 슬롯 2개를 추가하고, 중간 프리셋은 1개를 추가한다.
- 일부 산업, 보병, 기갑, 함정, 항공기 기초 기술 및 장비 변형을 지급한다.
- 건물, 부대, 영토·핵심주 등 여러 시작 효과가 한 파일에 함께 묶여 있다.
- 최신 일반 기술 전체 목록을 지급하지 않는다.
- 네 분야 연구시설과 모든 특수 프로젝트를 완성하는 모듈이 아니다.
- 한 번만 실행되어야 할 강화와 지도·부대·연구 변경이 강하게 결합되어 있어,
  그대로 확대하면 중복 실행과 유지보수 위험이 커진다.

판정: **강력하지만 구버전형·부분형 국뽕 프리셋 — 확인됨**.

현지화는 이 프리셋을 “7대 열강급 시작 기술력”으로 설명하지만, 실제 계산상 시작
연구 슬롯은 기본 2개, 중견국 3개, 국뽕 4개다. 후속 중점을 진행하면 대체로 5~6개에
도달하도록 설계된 것으로 보인다. 시작 시 모든 연구가 완료되는 프리셋은 아니다.

### 대상 프로젝트의 독립 시작 혁신 포인트 구현

2026-09-03 구현 전 정적 확인 시점에 대상 프로젝트의 `common/on_actions` 폴더는 존재하지만
`gookppong_on_actions.txt`를 포함한 실제 스크립트 파일은 없다. 원본의 동명 파일은
`C:\hoi\hearts_of_korea`에 남아 있으며 읽기 전용 참고 자료다.

원본 게임 규칙과 독립적으로 한국에 시작 혁신 포인트를 주는 새 on_action을 구현했다.
현재 상태는 **게임 코드 정적 구현 완료, 런타임 미검증**이다.

- 구현 상대 경로: `common/on_actions/hok_greatest_breakthrough_on_actions.txt`
- 실행 지점: 등록된 `on_startup` 콜백
- 수신 스코프: `KOR`
- 효과: `add_breakthrough_points`, `specialization = all`, `value = 30`
- 결과 의도: 육군·해군·공군·원자력에 각각 30점 추가
- 적용 범위: 게임 규칙과 무관한 새 게임의 사람/AI 한국
- 비적용 범위: 타국, 기존 세이브 소급
- 충돌 방지: 원본 `gookppong_on_actions.txt`와 다른 고유 파일명 및 전용 국가 플래그 사용

1.19.2 생성 문서상 효과의 국가 스코프와 `all` 인수는 **확인됨**이다. 그러나 실제 대상
플레이셋에서 시작 직후 30점이 표시되는지는 아직 게임을 실행하지 않았으므로 **미검증**이다.
자세한 구현 계약과 검증 기준은 `GREATEST_MODE_DESIGN.md`의 `GRT-211`에 기록했다.

## 현재 대상 애드온의 강화 방식

대상 프로젝트는 문서화 전 기준선에서 18개 파일뿐인 소규모 애드온이며, 원본 전체가
들어 있지 않다. 확인된 강화 방식은 다음과 같다.

- 국가 history에서 원본과 다른 수도 `1017`을 사용하고, 하루 뒤
  `HoK_Greatest.0` 이벤트를 호출한다.
- 시작 연구 슬롯과 원시 값 `research_speed_factor = 500`을 사용하지만, 연구 속도는
  기술 ID를 실제 완료하는 것과 다르다. 이 값의 표시 단위와 clamp도 검증해야 한다.
- 인물 29명의 능력치를 크게 높이는 변경이 있다.
- 한국 아이디어 수치 3개와 제조사 trait 16개의 강화 의도가 들어 있다.
- `MAX_SHARED_SLOTS = 500` 같은 전역 define과 전 국가에 노출될 가능성이 있는 커스텀
  건물이 있어 한국 한정 강화인지 별도 감사가 필요하다.
- 첩보 modifier 일부가 `Operation_cost`처럼 대문자로 시작한다. 1.19 바닐라에서 확인한
  표기는 `operation_cost`처럼 소문자이므로 실제 등록 여부를 검증해야 한다.

즉, 현재 애드온 파일은 그대로 보존할 완성 구현이 아니라 **강화 의도 delta**로
보존해야 한다. 1.19 원본 정의 위에 유효한 부분만 다시 적용한다.

## 일반 연구, 교리, 연구시설, 특수 프로젝트의 차이

HOI4 1.19에서는 다음 항목을 서로 같은 것으로 취급할 수 없다.

| 계층 | 예시 | 필요한 처리 |
|---|---|---|
| 일반 기술 | 산업, 전자, 보병, 포병, 전차, 함정, 항공기 | 검증된 기술 ID별 `set_technology` |
| 교리 | 육군·해군·공군 교리 | 1.19 교리 체계와 상호 배타 분기별 처리 |
| 연구시설 | 육군·해군·공군·원자력 시설 | 유효한 주/프로빈스에 건설 |
| 돌파구 | 분야별 breakthrough | 분야별 진행도/포인트 지급 |
| 특수 프로젝트 | 원자력·로켓·제트·실험 장비 등 | 프로젝트 ID별 완료 또는 가속 |

바닐라에서 확인된 연구시설 건물 키는 다음과 같다.

- `land_facility`
- `naval_facility`
- `air_facility`
- `nuclear_facility`

각 시설은 위치와 최대 건설 수 등의 제약이 있다. 특히 해군 시설은 해안 조건을
검증해야 한다. 커스텀 한국 지도에서 임의의 주 ID에 네 시설을 넣는 방식은 안전하지
않다.

원본의 한국 시작 주 9개에는 이 네 시설이 하나도 없다. 강한 국뽕 초기화 끝에는
Gotterdammerung 보유 시 육군 연구시설을 주겠다는 주석만 있고 실제 효과 블록은 비어
있다. 원본 전체에서 확인되는 시설 건물은 일본 나가사키의 `naval_facility`뿐이다.

반면 다음 네 분야 한국 과학자는 이미 정의되고 시작 시 모집된다.

- 육군: 김용관
- 해군: 황부길
- 공군: 조경연
- 원자력: 도상록

즉 인력은 있으나 시설과 프로젝트 연결이 빠진 상태다. 기존 인물 ID·특성·초상화를
재사용하는 것이 새 중복 인물을 만드는 것보다 안전하다.

특수 프로젝트에는 `complete_special_project`, 분야별 돌파구에는
`add_breakthrough_progress` 계열의 1.19 예시가 존재한다. 그러나 모든 프로젝트를
무조건 완료하면 DLC 부재, 선행 프로젝트, 상호 배타 보상, 후속 이벤트가 충돌할 수
있다. 그러므로 대상 DLC별 명시적 허용 목록이 필요하다.

판정: **기존 모드는 네 계층을 모두 완성하지 않음 — 확인됨**.

## 첩보가 느린 이유

기존 국뽕 초기화는 정보기관을 만들지 않는다. 정보기관 창설은 특정 정치 경로의
`국군정보사령부` 중점에 묶여 있고, 그 국민정신도 실제 작전 기간을 줄이지 않는다.
기관 업그레이드·정보망·비용·위험·결과 관련 보너스가 있더라도 이것만으로 실제 작전
실행일이 줄어든다고 볼 수 없다.

설치된 1.19.2 바닐라 정의와 생성 문서에서 확인한 관련 modifier 키:

- `agency_upgrade_time`
- `intel_network_gain_factor`
- `intelligence_operation_speed`
- `operation_outcome`
- `operation_cost`
- `operation_risk`
- `decryption_power`
- `decryption_power_factor`
- `crypto_strength`

바닐라 작전 파일은 각 작전에 `days = 35`, `days = 60`, `days = 90` 같은 기본 기간을
직접 둔다. 생성된 modifier 문서에는 정보기관 범위의 `intelligence_operation_speed`가
실제 작전 속도용 키로 등록되어 있다. 바닐라 콘텐츠에서 이 키를 배정한 사례는 찾지
못했으므로 유효한 범위, 수치 방향, 실제 날짜 계산은 런타임에서 검증해야 한다.

대표 바닐라 기본 실행 기간은 민간 침투 90일, 육·해·공군 침투 75일, 협력정부 90일,
기술 탈취 120일, 쿠데타 180일이다. 원본에는 작전/기관 업그레이드/define을 덮는
디렉터리가 없으므로 이 기간을 그대로 상속한다.

따라서 다음 문장은 구분해야 한다.

- 정보망이 빨리 쌓임: 작전 **준비 전 단계** 단축
- 장비·민간공장 비용 감소: 작전 **비용** 감소
- 성공률 증가/위험 감소: 작전 **결과** 개선
- `intelligence_operation_speed` 증가: 작전 **실행 기간 자체** 단축 후보

판정: **원본 모드는 실제 작전 기간을 단축하지 않음 — 확인됨**.  
최강 프리셋은 `intelligence_operation_speed`를 한국 정보기관에 적용하는 방식을 먼저
시험한다. modifier가 예상대로 작동하지 않는 경우에만 한국 전용 작전 정의를 별도 ID로
제공하는 대안을 검토한다. 바닐라 작전 DB 전체를 덮어쓰는 방식은 금지해야 한다.

## 핵 개발 상태

한국 중점에는 핵 분야 과학자 역할/특성과 핵 연구 보너스를 주는 내용이 있지만,
다음과 같은 상태와는 다르다.

- 원자력 연구시설이 이미 건설됨
- 원자력 돌파구가 최대치임
- 핵 관련 일반 기술이 모두 연구됨
- 핵 분야 특수 프로젝트가 모두 완료됨
- 후속 보상과 무기 운용 조건까지 충족됨

판정: **핵 전문가·연구 보너스는 있으나 핵 연구 전체 완료는 아님 — 확인됨**.

## AI 연구 상태

역사 AI 연구 가중치는 보병 50, 보병 기술 15, 포병 8, 지원 6.5에 집중되어 있다.
산업·전자·공군·해군·원자력·특수 프로젝트 우선순위는 확인되지 않았고, 대체 정치
AI 계획에는 별도 연구 블록도 없다.

판정: **AI 한국이 최강 연구 기반을 활용하도록 설계되어 있지 않음 — 확인됨**.

## 이관 시 충돌이 확정된 파일

원본과 현재 프로젝트에 동일한 상대 경로로 존재하는 핵심 텍스트 파일:

- `common/characters/KOR.txt`
- `common/ideas/korea.txt`
- `history/countries/KOR - Korea.txt`

이 세 파일은 한쪽을 그대로 덮어쓰면 현재 애드온 강화나 1.19 원본 정의 중 하나가
사라진다. 이관 시에는 1.19 원본을 기준으로 현재 강화 의도만 ID·스코프·수치를
검증해 의미 단위로 합쳐야 한다.

세 파일 외에도 `AGENTS.md`, `README.md`, `descriptor.mod`, `thumbnail.png`,
`thumbnail_full.png`가 충돌한다. 이 다섯 항목은 계승판의 문서·업로드 정체성·브랜딩을
보존해야 하므로 원본 파일로 자동 덮어쓰지 않는다.

## 이관 전에 별도 감사가 필요한 범위

원본에는 한국 전용 파일만 있는 것이 아니라 다음과 같은 전역 스냅샷 또는 타국
콘텐츠가 포함되어 있다.

- 범용 `on_actions`
- 군수산업조직(MIO) 데이터베이스
- 범용 조언가/인물 정의
- 일본 역사·이벤트
- 북마크와 이름 데이터
- 커스텀 지도·주·전략지역·철도·보급 묶음

이 자료는 원본의 기능에 필요할 수 있지만, 1.19.2 최신 바닐라보다 오래된 전역
정의를 다시 덮어쓸 수도 있다. 파일 존재만 보고 전부 복사해서는 안 된다.

## 정적 근거 색인

다음 경로는 원본 소스 루트 기준이다. 바닐라 경로는 로컬 HOI4 설치 루트 기준이다.

| 확인 내용 | 근거 위치 |
|---|---|
| 기본 한국 연구 슬롯 2 | `history/countries/KOR - Korea.txt:5` |
| 기본 시작 기술 | `history/countries/KOR - Korea.txt:70-150` |
| 중견국 슬롯 +1·기초 기술 | `common/on_actions/gookppong_middle_power_on_actions.txt:11-170` |
| 국뽕 슬롯 +2·부분 기술 | `common/on_actions/gookppong_on_actions.txt:145-473` |
| 비어 있는 육군 시설 주석 | `common/on_actions/gookppong_on_actions.txt:787-789` |
| 네 분야 과학자 | `common/characters/KOR.txt:1480-1529` |
| 과학자 시작 모집 | `history/countries/KOR - Korea.txt:237-241` |
| 국방과학연구소가 시설 대신 국민정신 지급 | `common/national_focus/korea.txt:877-894` |
| 공군·핵 돌파구 일부 | `common/national_focus/korea.txt:955-1007` |
| 정보기관 창설 중점 | `common/national_focus/korea.txt:7412-7447` |
| 역사 AI의 편중된 연구 가중치 | `common/ai_strategy_plans/KOR_historical_strategy_plan.txt:79-84` |
| 바닐라 작전의 `days` 구조 | `common/operations/00_operations.txt:201-245` |
| 기술 탈취 120일 | `common/operations/00_operations.txt:1915-1963` |
| `intelligence_operation_speed` 정의 | `documentation/modifiers_documentation.md:2977-2980` |
| `set_technology` 효과 | `documentation/effects_documentation.md:7805-7817` |
| 특수 프로젝트 완료 효과 | `documentation/effects_documentation.md:2896-2922` |
| 혁신 포인트 직접 지급 효과와 국가 스코프 | `documentation/effects_documentation.md:788-803` |
| 새 게임 시작 콜백 | `common/on_actions/_documentation.md:8` |
| 네 혁신 전문 분야 ID | `common/special_projects/specialization/specializations.txt:2-18` |
| 네 시설 건물 정의 | `common/buildings/00_buildings.txt:126,500,533,556` |

라인 번호는 조사한 소스 커밋과 로컬 1.19.2.0 참조본에 고정된 값이며 파일이 바뀌면
다시 산출한다.

## 아직 입증하지 않은 것

- 실제 런처가 어느 물리적 모드 복사본을 로드하는지
- `Korean Language`와 기타 모드의 실제 로드 순서
- 모든 DLC 조합에서 기술/프로젝트 ID가 유효한지
- 커스텀 한국 지도에서 각 시설의 안전한 건설 위치
- 정보기관 생성 및 업그레이드 효과의 실제 UI 결과
- 한국 전용 작전 복제안이 AI와 멀티플레이 체크섬에 미치는 영향
- 구버전 저장 파일 호환성

이 항목들은 구현 후 런타임 검증 없이는 완료로 판정하면 안 된다.
