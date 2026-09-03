# 원본 소스 이관 계획

## 1. 목적과 현재 상태

목표는 `C:\hoi\hearts_of_korea`의 런타임 콘텐츠를
`C:\hoi\Hearts of Korea the greatest`로 가져온 뒤, 그 복사본에서만 복원과
최강 프리셋 개발을 하는 것이다.

**이 문서 작성 단계에서는 실제 파일 복사를 수행하지 않았다.**

조사 기준 원본에는 `.git`을 제외한 검색 대상 1,027개 파일과 추적된 `.gitignore`가
있다. 그중 런타임 트리는 1,007개 파일, 175,080,505바이트다. 주요 런타임 디렉터리는
`common`, `events`, `gfx`, `history`, `interface`, `localisation`, `map`, `music`,
`portraits`, `sound`다. 수량과 크기는 실제 이관 직전에 다시 산출한다.

문서화 전 대상 기준선은 `main`의
`a34163ac9319d090fddc8ac164781bb64edc9a50`이고 clean이었다. 당시 대상에는 18개
파일이 있었다. 상대 경로 충돌은 8개, 원본에만 있는 파일은 1,019개, 대상에만 있는
파일은 10개였다.

## 2. 절대 원칙

- 원본 경로는 읽기 전용으로 취급한다.
- 게임 설치 폴더와 Workshop 다운로드 폴더는 수정하지 않는다.
- 대상 프로젝트에 이미 있는 사용자 파일을 무조건 덮어쓰지 않는다.
- `remote_file_id`는 어느 원본/포트 값도 계승하지 않는다.
- 지도 파일은 서로 결합된 묶음으로 다루며 부분 복사하지 않는다.
- 원본 이관, 호환성 복원, 최강 강화는 각각 별도 변경 단위로 관리한다.
- 복사 전·후 파일 목록, 크기, 해시 또는 Git 상태로 누락과 예기치 않은 변경을 확인한다.

## 3. 출처 기록

이관 기록에 최소 다음 값을 남긴다.

| 필드 | 기록 값 |
|---|---|
| 원본 절대 경로 | `C:\hoi\hearts_of_korea` |
| 원본 브랜치 | `main` |
| 원본 커밋 | `887930f6e88c80568d62dab9cfbe1ba8a498a252` |
| 원본 조사 시 상태 | clean |
| 문서화 전 대상 브랜치/커밋 | `main` / `a34163ac9319d090fddc8ac164781bb64edc9a50` |
| 원본 지원 표기 | `1.19.*` |
| 원본 의존성 | `Korean Language` |
| 역사적 원작 ID | `2898629778` — 출처 전용 |
| 조사한 소스 포트 ID | `3793992662` — 출처 전용 |
| 현재 애드온 ID | `2902859532` — 과거 애드온 출처 전용 |
| 새 계승판 ID | 첫 신규 게시 전까지 미할당 |

실제 이관일에 원본 커밋과 상태가 달라졌다면 위 표를 덮어쓰지 말고 새 기준선으로
변경 이유와 함께 기록한다.

## 4. 사전 점검

### 4.1 환경 확정

실제 파일 작업 전에 기록한다.

- 목표 HOI4 정확한 버전, 빌드, 체크섬
- 활성 DLC
- 런처 플레이셋과 모드 로드 순서
- `Korean Language`의 정확한 항목과 버전
- 런처가 사용할 로컬 개발 모드의 실제 경로
- 대상 프로젝트의 Git 브랜치/커밋/working tree

대상 프로젝트가 아직 Git 저장소가 아니라면, Git 초기화와 최초 기준선 커밋은 사용자의
명시적 요청을 받은 별도 작업으로 한다.

### 4.2 두 트리 목록화

- 상대 경로별 파일 목록
- 파일 크기와 해시
- 원본에만 있음 / 대상에만 있음 / 같은 경로·같은 내용 / 같은 경로·다른 내용
- 텍스트/바이너리 구분
- 인코딩과 BOM이 중요한 localisation 목록

이 결과를 이관 보고서에 저장하고 복사 허용 목록의 입력으로 사용한다.

## 5. 복사 허용·제외 정책

### 우선 이관 후보

다음 런타임 디렉터리는 원본 기능에 필요할 가능성이 높다. 단, 각 폴더의 전역 정의와
충돌을 먼저 감사한다.

```text
common/
events/
gfx/
history/
interface/
localisation/
map/
music/
portraits/
sound/
```

### 자동 복사 금지

```text
.git/
.gitignore
.github/
.idea/
.vscode/
logs/
crashes/
saves/
caches/
AGENTS.md
README.md
docs/
개발자 개인 설정 및 자격 증명
```

다음은 내용 검토 후 별도로 작성한다.

- `descriptor.mod`
- 런처 `.mod` 파일
- `remote_file_id`가 있는 모든 메타데이터
- Workshop 설명/미리보기 메타데이터
- 루트 `README`와 크레딧의 내용 병합

썸네일과 홍보 이미지는 바이너리 비교 후 계승판 출처/브랜딩 정책에 맞게 선택한다.

원본의 자체 `docs`에는 1.19.2 작업 당시의 유용한 근거가 있지만, 당시 사건 기록과
절대 경로도 함께 들어 있다. 이를 계승판의 현재 상태 문서로 그대로 복사하지 않는다.
필요하면 `docs/source-reference` 같은 명확한 출처 폴더에 원문 스냅샷을 보관하거나,
새 문서에서 출처와 조사 시점을 표시해 요약한다.

## 6. 전체 충돌표

| 상대 경로 | 처리 |
|---|---|
| `AGENTS.md` | 대상 지침 보존; 자동 복사 금지 |
| `README.md` | 계승판 README 보존; 원본 문서는 참고 자료로만 사용 |
| `thumbnail.png` | 내용이 다름; 계승판 브랜딩 보존 |
| `thumbnail_full.png` | 내용이 다름; 계승판 브랜딩 보존 |
| `descriptor.mod` | 어느 쪽도 그대로 사용하지 않고 계승판용으로 합성 |
| `common/characters/KOR.txt` | 원본 1.19 파일을 base로 3-way 의미 병합 |
| `common/ideas/korea.txt` | 원본 1.19 파일을 base로 강화 delta만 재적용 |
| `history/countries/KOR - Korea.txt` | 원본 1.19 파일을 base로 초기화 구조 재설계 |

`/MIR`, 삭제 동기화, 대상 루트 전체 덮어쓰기 방식은 사용하지 않는다.

### 대상 전용 보존 manifest

문서화 전 대상에만 있던 다음 10개 파일은 원본 복사 과정에서 삭제하거나 잃어서는 안
된다. 다만 “파일 보존”은 “현 구현을 검증 없이 최종 채택”한다는 뜻이 아니다.

```text
common/buildings/07_buildings_HoK.txt
common/country_leader/00_KOR_Greatest_traits.txt
common/defines/1001_defines.lua
events/hok_rules_events.txt
localisation/english/buildings_HoK_l_english.yml
localisation/english/hok_Greatest_l_english.yml
localisation/english/korea_the_greatest_ideas_l_english.yml
localisation/korean/buildings_HoK_l_korean.yml
localisation/korean/hok_Greatest_l_korean.yml
localisation/korean/korea_the_greatest_ideas_l_korean.yml
```

원본 localisation 36개와 대상 localisation 6개는 조사 시 모두 UTF-8 BOM이었다.
이관과 병합 과정에서 이를 유지한다.

## 7. 확정 코드 충돌 처리

### `common/characters/KOR.txt`

1. 원본 1.19 인물 정의 73개를 기준으로 둔다.
2. 대상 애드온의 한국 지도자/장군/과학자 강화 의도를 항목별로 추출한다.
3. 같은 character ID를 두 번 정의하지 않는다.
4. 역할, 특성, 초상화, 모집/은퇴/사망 조건을 함께 추적한다.
5. 과학자 특성과 네 연구 분야 배치 가능 여부를 별도 확인한다.
6. 대상에는 66개 정의와 장군 29명에 대한 대규모 능력치 강화가 있다. 능력치 의도는
   보존하되 원본에만 있는 최신 7명을 잃지 않는다.
7. `KOR_syngman_rhee`를 canonical ID로 유지하고 대상의 대문자 변형
   `KOR_Syngman_Rhee`를 새 ID처럼 남기지 않는다.

원본에만 있는 것으로 확인된 7개 ID:

```text
KOR_jiro_minami
KOR_choe_hyon
KOR_kim_il_sung
KOR_lyuh_woon_hyung
KOR_pak_hon_yong
KOR_yi_kang
KOR_yi_un
```

### `common/ideas/korea.txt`

1. 원본 아이디어 ID와 trait 구조를 기준으로 둔다.
2. 대상 애드온에 있는 아이디어 수치 변경 3개와 제조사 trait 치환 16개를 delta 목록으로
   만든다.
3. 대상 애드온 modifier의 키 대소문자와 1.19 유효성을 검사한다.
4. 강화 아이디어는 복원 아이디어와 분리된 ID로 옮긴다.
5. `operation_cost`, `operation_outcome` 등은 공식 1.19 표기와 단위를 사용한다.
6. 한 아이디어에 연구·경제·군사·첩보를 전부 몰아넣지 않는다.

### `history/countries/KOR - Korea.txt`

1. 원본의 커스텀 지도 수도 `525`, 정치, OOB, 인물, 시작 기술을 기준으로 둔다.
2. 현재 애드온 파일을 통째로 덮어씌우지 않는다.
3. 대상의 수도 `1017`과 대문자 캐릭터 ID는 되살리지 않는다.
4. 대상의 하루 뒤 `HoK_Greatest.0` 이벤트 의도는 원본의 게임 규칙/on-startup과
   중복되지 않는 단일 초기화 구조로 옮긴다.
5. 최강 보너스는 history 파일이 아니라 선택형 초기화 효과로 이동한다.
6. 이렇게 해야 비활성 프리셋이 원본 시작 상태를 유지한다.

## 8. 전역 스냅샷 감사

원본의 다음 범위는 최신 바닐라 또는 DLC 데이터베이스를 광범위하게 덮을 수 있다.

- 범용 `on_actions`
- MIO 데이터베이스
- 범용 조언가/특성
- 북마크
- 이름 데이터베이스
- 일본 및 기타 국가 역사/이벤트
- `replace_path`가 걸린 디렉터리

각 파일을 다음 셋 중 하나로 분류한다.

1. **필수 이관**: 한국 콘텐츠가 직접 참조하며 대체 수단이 없음
2. **축소 이관**: 한국 전용 정의만 새 namespace/파일로 추출 가능
3. **제외**: 오래된 바닐라 스냅샷이며 한국 기능에 필요하지 않음

판정에는 대상 버전 바닐라 파일, 참조 검색, 로드 순서, 실제 로그가 필요하다. 오류가
없어 보인다는 이유만으로 전역 파일을 유지하지 않는다.

특히 자동 허용 목록에서 제외하고 대상 버전 바닐라를 base로 다시 대조할 파일군:

```text
common/countries/colors.txt
common/countries/cosmetic.txt
common/names/00_names.txt
common/decisions/JAP.txt
common/decisions/KOR.txt
common/scripted_triggers/JAP_scripted_triggers.txt
common/national_focus/china_shared_TSR.txt
events/WTT_Japan.txt
events/SEA_Japan.txt
common/peace_conference/ai_peace/USA.txt
common/peace_conference/ai_peace/SOV.txt
common/on_actions/14_sea_on_actions.txt
common/bookmarks/the_gathering_storm.txt
history/countries/JAP - Japan.txt
history/units/JAP_1936*.txt
common/military_industrial_organization/organizations/00_generic_organization.txt
common/on_actions/04_mtg_on_actions.txt
common/scripted_effects/SP_scripted_effects.txt
common/intelligence_agencies/00_intelligence_agencies.txt
common/difficulty_settings/00_difficulty.txt
history/general/generic_advisors.txt
events/ElectionEvents.txt
```

원본 문서가 일부를 1.19.2 target-derived KOR bridge로 설명하므로 단순히 “낡은 파일”로
폐기하지도 않는다. 최종 목표 버전의 같은 경로 파일을 base로 한국용 최소 delta를
재구성한다.

## 9. 지도 이관

커스텀 지도는 다음이 결합된 하나의 고위험 하위 프로젝트다.

- province 정의와 비트맵
- 주와 소유권
- 전략지역
- 철도와 보급
- 인접 관계
- 건물·부대 위치
- 승점
- 초점·이벤트·결정에서 사용하는 주/프로빈스 ID

지도 이관 원칙:

1. 원본 지도 묶음과 모든 참조 ID를 목록화한다.
2. 부분적으로 최신 바닐라 지도를 섞지 않는다.
3. `replace_path`의 실제 로드 영향을 기록한다.
4. 정적 참조 검사 후 새 게임에서 지도를 직접 연다.
5. 한국을 선택하고 unpause하여 보급, 철도, 해안, 시설 위치를 검사한다.
6. 네 연구시설의 최종 위치는 이 검증을 통과한 뒤 확정한다.

현재 확인한 18개 물리적 핵심 파일은 하나의 이관 단위로 취급한다.

```text
map/provinces.bmp
map/definition.csv
map/buildings.txt
map/railways.txt
map/supply_nodes.txt
map/unitstacks.txt
map/strategicregions/186-Korea.txt
history/states/525-South Korea.txt
history/states/527-North Korea.txt
history/states/528-Nagasaki.txt
history/states/1028 - Hamgyong.txt
history/states/1029 - Gangwon.txt
history/states/1030 - Gyeongsang.txt
history/states/1031 - Chungcheong Jeolla.txt
history/states/1082 - Jeolla.txt
history/states/1083 - Hwanghae.txt
history/states/1084 - Jeju.txt
history/states/1085 - Tsushima.txt
```

여기에 country history, OOB, focus, event, decision, on-action, AI, localisation에서의
주/프로빈스 참조 fan-out을 같은 작업 범위로 묶는다.

## 10. 권장 이관 단계

### 단계 0 — 승인과 스냅샷

- 구현 작업 승인 확인
- 원본과 대상의 절대 경로 재확인
- 두 트리 상태와 파일 목록 기록
- 대상의 기존 파일 백업 또는 Git 기준선 확보

### 단계 1 — 별도 스테이징

- 대상 프로젝트 안의 명확한 임시 스테이징 경로에 허용 목록만 복사
- 원본 파일 수·해시와 스테이징 결과 비교
- 제외 대상과 충돌 목록 생성
- 이 단계에서는 활성 모드 루트에 섞지 않음

### 단계 2 — 런타임 트리 이관

- 충돌 없는 한국 전용 텍스트부터 이동
- GFX/음악/사운드 등 바이너리는 해시 기반으로 이동
- localisation의 UTF-8 BOM, 헤더, 키를 보존
- 지도 묶음은 별도 변경 단위로 이동

### 단계 3 — 충돌 의미 병합

- 위 세 핵심 충돌 파일을 1.19 원본 기준으로 병합
- ID와 namespace 전역 검색
- 대상 애드온의 강화는 아직 활성화하지 않고 별도 모듈로 보존

### 단계 4 — 디스크립터 재작성

- 새 계승판 이름과 대상 `supported_version` 기록
- 실제 필요한 의존성만 유지
- 원본/포트/애드온 `remote_file_id` 제거
- 원작과 포트의 출처를 크레딧에 기록
- `supported_version` 변경을 호환성 완료로 취급하지 않음

### 단계 5 — 복원 검증

- 정적 파서/참조/중복 ID/인코딩 검사
- 런처가 정확한 로컬 복사본을 로드하는지 확인
- 기준 플레이셋으로 새 게임 시작
- 한국 지도, 정치, 인물, OOB, 연구, 중점, 이벤트, 결정을 검사
- 로그를 바닐라 및 의존 모드 대조군과 비교

### 단계 6 — 최강 프리셋 추가

- 복원 기준선이 작동한 뒤에만 `GREATEST_MODE_DESIGN.md` 구현
- 연구, 시설/프로젝트, 첩보, 장비/경제를 차례로 추가
- 모듈별 커밋과 검증 기록 유지

## 11. 정적 검증 체크리스트

- [ ] 원본 파일은 수정되지 않음
- [ ] 예기치 않은 대상 파일 삭제·덮어쓰기 없음
- [ ] 원본/대상/스테이징 파일 목록과 해시 비교 완료
- [ ] `.git`, 로그, 세이브, 개인 설정이 이관되지 않음
- [ ] 기존 Workshop ID가 활성 descriptor에 없음
- [ ] 모든 namespace 및 안정 ID 충돌 검색 완료
- [ ] 참조되는 focus/event/decision/idea/character/equipment/technology ID 존재
- [ ] localisation BOM·헤더·키 보존
- [ ] 전역 snapshot과 `replace_path` 감사 완료
- [ ] 지도 ID와 교차 참조 검사 완료
- [ ] 전체 파일 줄바꿈/인코딩 일괄 변환 없음

## 12. 런타임 검증 체크리스트

- [ ] 런처가 대상 프로젝트의 로컬 모드를 로드함
- [ ] 정확한 DLC·의존성·로드 순서를 기록함
- [ ] 메인 메뉴에서 신규 치명 오류 없음
- [ ] 한국으로 새 게임 진입 가능
- [ ] 한 달 이상 진행 시 crash/event spam/error 폭증 없음
- [ ] 지도·보급·철도·해안·승점 정상
- [ ] 시작 지도자·정부·법·아이디어·OOB·연구 정상
- [ ] 중점·이벤트·결정의 핵심 경로 정상
- [ ] 현지화·초상화·아이콘·모델 누락 없음
- [ ] 저장 후 재로드 가능
- [ ] 최강 프리셋 비활성 상태가 복원 기준선과 동일
- [ ] 최강 프리셋 활성 상태가 별도 요구사항을 충족

## 13. 중단 조건

다음 상황에서는 임의로 합치지 않고 조사 결과를 보고한다.

- 런처가 예상과 다른 물리적 복사본을 로드함
- 목표 버전이나 DLC 차이가 기술/프로젝트 ID를 바꿈
- `replace_path` 제거 시 커스텀 지도가 깨지고 유지 시 최신 바닐라 DB가 사라짐
- 지도 ID를 대량 재번호해야 함
- 동일 ID 정의 중 어느 쪽이 의도인지 소스와 로그로 판단할 수 없음
- localisation 의존성의 실제 언어 헤더 계약을 확인할 수 없음
- 원본에 조사 이후 사용자 변경이 생겨 기준 커밋이 달라짐

## 14. 산출물 기록 형식

실제 이관이 끝나면 다음을 보고한다.

- 이관한 원본 커밋과 대상 기준선
- 추가/수정/제외한 파일 수와 목록
- 충돌 파일별 병합 결정
- 전역 snapshot 및 지도 감사 결과
- descriptor와 의존성 상태
- 실행한 정적·런타임 검증과 결과
- 확인하지 못한 DLC, 경로, 저장 호환성, 멀티플레이 위험
- Git 커밋/푸시 여부

커밋이나 푸시는 이관 요청에 자동 포함되지 않으며 각각 별도 승인을 받아야 한다.
