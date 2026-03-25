---
created: 2026-03-25
tags: [디자인시스템, 게임, Circle-Void, 사이드프로젝트]
status: Implementation-Ready
version: 1.0
authors: 디자이너-A (컨셉), 디자이너-B (UI 시스템)
---

# Circle Void — Design System


> 핵심 비주얼 메타포: "오래된 사찰의 창호지 문이 우주를 향해 찢겨 열리는 순간"

---

## 목차

1. 디자인 레퍼런스
2. 컬러 팔레트
3. 타이포그래피
4. 찢어짐 비주얼 시스템 (핵심)
5. 붓 획 렌더링 시스템
6. 애니메이션 시퀀스
7. 파티클 시스템
8. Canvas 렌더링 레이어 스택
9. UI 컴포넌트 (FRD)
10. 사운드 디자인
11. 반응형 레이아웃
12. 접근성
13. 퍼포먼스 버짓
14. CSS Custom Properties (전체 토큰)
15. 경쟁사 포지셔닝
16. 플래그 & 이슈
17. 핸드오프 노트

---

## 1. 디자인 레퍼런스

- 한지 페이퍼 아트: https://www.pinterest.com/ideas/hanji-paper/929093881805/
- 한국 한지 98가지 아이디어: https://www.pinterest.com/georgeirrgang/hanji-korean-paper/
- 수묵화 드로잉: https://www.pinterest.com/joyschultz/sumi-e-ink-drawings/
- 수묵화 페인팅 보드: https://www.pinterest.com/artmartstl/sumi-e-painting/
- 포탈 VFX 게임 이펙트: https://www.pinterest.com/beebe1538/portal-vfx/
- 인디 게임 에스테틱: https://www.pinterest.com/ideas/indie-game-aesthetic/906397849525/
- 포탈 컨셉 아트: https://www.pinterest.com/ideas/portal-concept-art-game/940199982896/

---

## 1.5 화면 흐름도 (Screen Flow)

### 전체 플로우
```
앱 실행 → 스플래시(1.5s) → 메인 메뉴
                              ├── Precision Mode → 레벨 선택(병풍) → 인게임 → 결과 화면 → 레벨 선택
                              ├── Puzzle Mode → 월드 선택 → 레벨 선택 → 인게임 → 결과 화면 → 다음 레벨
                              ├── Zen Mode → 바로 인게임 (HUD 없음)
                              └── 설정
```

### 메인 메뉴 화면
- 배경: 한지 텍스처 풀스크린
- 타이틀: "Circle Void" — `--cv-font-display`, `--cv-text-4xl`
- 3개 모드 카드(F-006) 세로 배치 (모바일) / 가로 배치 (데스크톱)
- 하단: 설정 아이콘(도장 스타일, 48px)

### 결과 화면 (Level Complete)
- 배경: 인게임 캔버스 블러 처리 (backdrop-filter: blur(8px))
- 중앙: 등급 배지(F-003) 스탬프 애니메이션
- 등급 아래: 점수 (F-002 스타일), 정확도 %
- 하단 버튼 3개: 재시도 / 다음 레벨 / 레벨 선택으로
- 전환: `--cv-duration-slow` (800ms) fade-in

### 일시정지 오버레이
- 트리거: ESC 키 / 화면 상단 일시정지 버튼
- 배경: 반투명 한지 오버레이 (`--cv-bg-paper`, opacity 0.85)
- 중앙: "일시정지" 먹 문자 + 버튼 3개 (이어하기 / 재시도 / 나가기)
- 인게임 캔버스: 정지 (rAF 중단)

### 설정 화면
- 족자 스크롤 메타포 (세로 스크롤)
- 항목: 사운드 볼륨, 다크 모드 토글, 축소 모션, 언어 선택
- 각 항목: 먹 프레임 카드 스타일 (F-009 재사용)

---

## 2. 컬러 팔레트

### 라이트 모드 — "한지 아침" (Hanji Morning)
> 분위기: 따뜻한 양피지, 오래된 작업실, 새벽 이전의 고요함

| 토큰 | 이름 | HEX | 용도 |
|---|---|---|---|
| `--cv-bg-paper` | 한지 따뜻한 베이스 | `#F2EBD9` | 메인 게임 캔버스 배경 |
| `--cv-bg-paper-dark` | 한지 그림자 면 | `#E8DCBF` | 카드 그림자, 깊이 레이어 |
| `--cv-bg-paper-light` | 한지 빛받은 면 | `#FAF6EC` | 하이라이트 면 |
| `--cv-bg-paper-fiber` | 한지 섬유 라인 | `#D9CEAA` | 배경 텍스처 디테일 |
| `--cv-ink-dark` | 농묵 | `#1A1209` | 주 획, 두꺼운 붓 중심 |
| `--cv-ink-mid` | 중묵 | `#3D2B1A` | 보조 획, 아웃라인 |
| `--cv-ink-light` | 담묵 | `#7A5C3F` | 건조한 붓 가장자리 |
| `--cv-ink-wash` | 묵세 | `#A89070` | 배경 번짐, 주변 텍스처 |
| `--cv-ink-ghost` | 잔묵 | `#BBA98A` | 흐릿한 보조 요소, 고스트 가이드 |
| `--cv-ink-red` | 인주 | `#8B1A1A` | 도장(인장) 잉크 |
| `--cv-ink-gold` | 금박 | `#C9922B` | S등급 금 도장 |
| `--cv-void-core` | 보이드 코어 | `#03020A` | 완전한 허공 중심 |
| `--cv-void-deep` | 심우주 | `#050D1F` | 깊은 보이드 내부 |
| `--cv-void-mid` | 중간 우주 | `#0A1A3A` | 내부 가장자리, 약한 글로우 |
| `--cv-void-nebula-a` | 퍼플 성운 | `#1A0A2E` | 성운 힌트 A |
| `--cv-void-nebula-b` | 틸 성운 | `#0A2218` | 성운 힌트 B |
| `--cv-void-edge` | 코발트 엣지 | `#2A1F7A` | 종이를 만나는 보이드 가장자리 |
| `--cv-void-glow` | 보이드 글로우 | `#4A7FFF` | 가장자리 코로나 발광 |
| `--cv-void-star` | 별점 | `#E8F0FF` | 보이드 내부 별 포인트 |
| `--cv-star-gold` | 별먼지 골드 | `#C8A94A` | 별 파티클, 정확도 반짝임 |
| `--cv-star-cool` | 쿨 별빛 | `#A8C4E8` | 유성 꼬리 색상 |
| `--cv-semantic-jade` | 비취 성공 | `#2A5C3A` | 성공 상태, S랭크 |
| `--cv-semantic-vermillion` | 주홍 경고 | `#C84B2A` | 경고, C랭크 |
| `--cv-semantic-crimson` | 진홍 오류 | `#8B1A1A` | 오류 상태, D랭크 |
| `--cv-semantic-amber` | 호박 중립 | `#C88A2A` | B랭크, 일반 정보 |

### 다크 모드 — "먹에 물들다" (Ink Immersion)

**스토리 기반 전환** — 시스템 설정이 아닌 게임 세계관에 의해 트리거된다.

**컨셉: "먹에 물들다"**
게임 진행에 따라 먹(墨)이 한지를 서서히 물들이며 세계가 어두워진다. 단순한 색상 토글이 아니라, 먹물이 종이에 스며드는 과정을 통해 자연스럽게 전환.

**전환 트리거**:
- **Puzzle 모드**: 특정 챕터/레벨에서 서사적으로 먹이 번짐 → 어두운 세계 진입
- **Zen 모드**: 장시간 플레이 시 점진적 먹 번짐 (빠른 호흡 → 느린 호흡 → 명상 → 먹 세계)
- **Precision 모드**: 라이트 고정 (순수 한지 위 연습)

**전환 애니메이션** (2000ms):
1. 캔버스 중앙에서 먹물 방울 떨어짐 (0ms)
2. 동심원으로 먹 번짐 확산 — radial gradient transition (0-1200ms)
3. 한지 텍스처가 먹 젖은 종이 텍스처로 blend (800-1600ms)
4. UI 토큰 서서히 전환 — CSS transition 1600ms ease (400-2000ms)

> 분위기: 옻칠 위의 먹, 자정의 서예, 작업 후의 스튜디오

| 토큰 | 라이트 (한지 아침) | 다크 (먹의 밤) |
|------|---------------------|----------------|
| `--cv-bg-paper` | `#F2EBD9` | `#0A0806` |
| `--cv-ink-dark` | `#1A1209` | `#EDE8DC` |
| `--cv-ink-mid` | `#3D2E1A` | `#C8BFA8` |
| `--cv-ink-light` | `#6B5B3E` | `#A89880` |
| `--cv-void-edge` | — | `#3A2FB8` (더 선명한 코발트) |
| `--cv-void-glow` | `#4A7FFF` | `#6050D0` (바이올릿 글로우) |

### 다크 모드 전체 토큰 오버라이드

```css
[data-theme="dark"] {
  /* 스토리 기반 전환 — prefers-color-scheme 미사용 */
  --cv-bg-paper:       #0A0806;
  --cv-bg-paper-dark:  #050403;
  --cv-bg-paper-light: #1A1610;
  --cv-bg-paper-fiber: #2A2418;
  --cv-ink-dark:       #EDE8DC;
  --cv-ink-mid:        #C8BFA8;
  --cv-ink-light:      #A89880;
  --cv-ink-ghost:      #6A5C48;
  --cv-void-edge:      #3A2FB8;
  --cv-void-glow:      #6050D0;
  --cv-hud-bg:         rgba(237,232,220, 0.10);
  --cv-hud-border:     rgba(26,18,9, 0.20);
  --cv-hud-text:       #EDE8DC;
  --cv-hud-text-dim:   #A89880;
  --cv-grade-s:        #D4A832;
  --cv-tear-shadow:    rgba(237,232,220, 0.30);
}
```

트리거: **스토리/맥락 기반** — `prefers-color-scheme` 미사용. JS에서 `document.documentElement.dataset.theme = 'dark'` 설정 시 전환. Puzzle 모드 서사 진행 또는 Zen 모드 장시간 플레이에 의해 자동 트리거.

### HUD & 시스템 컬러

| 토큰 | HEX | 용도 |
|---|---|---|
| `--cv-hud-bg` | `rgba(26,18,9,0.70)` | HUD 배경 |
| `--cv-hud-border` | `rgba(242,235,217,0.15)` | HUD 보더 |
| `--cv-hud-text` | `#F2EBD9` | HUD 텍스트 |
| `--cv-hud-text-dim` | `#BBA98A` | HUD 보조 텍스트 |
| `--cv-focus-ring` | `#4A7FFF` | 키보드 포커스 링 |
| `--cv-particle-tear` | `#E0D4B8` | 종이 섬유 파티클 |
| `--cv-particle-cosmic` | `#4A7FFF` | 보이드 파티클 |
| `--cv-particle-ink` | `#1A1209` | 잉크 스플래터 |

### 등급별 컬러

| 등급 | 토큰 | HEX | 비주얼 |
|---|---|---|---|
| S | `--cv-grade-s` | `#C9922B` | 금 도장 + 발광 |
| A | `--cv-grade-a` | `#8B1A1A` | 붉은 도장 |
| B | `--cv-grade-b` | `#1A1209` | 진한 먹 도장 |
| C | `--cv-grade-c` | `#7A5C3F` | 옅은 먹 도장 |
| D | `--cv-grade-d` | `#BBA98A` | 번진/깨진 도장 |

---

## 3. 타이포그래피

### 폰트 스택

| 용도 | 폰트 | 근거 |
|---|---|---|
| 제목 KO | **Noto Serif KR** | 붓 영향을 받은 세리프, 서예적 굵기 변화 |
| 제목 EN | **Cormorant Garamond** | 오래된 인쇄물 우아함 + 잉크; 세밀한 헤어라인 대비 |
| 본문/UI | **Pretendard** (KO) / **Inter** (EN) | 중립적, 가독성, 게임 HUD 최적화 |
| 점수/숫자 | **JetBrains Mono** | 정확도 퍼센트, 타이머, 점수 카운터 |

### 타입 스케일 (Major Third 1.25 비율)

| 토큰 | 크기 | 줄간격 | 용도 |
|---|---|---|---|
| `--cv-text-4xl` | 48.8px (3.052rem) | 1.1 | 타이틀 스크린 |
| `--cv-text-3xl` | 39px (2.441rem) | 1.15 | 점수 표시 |
| `--cv-text-2xl` | 31.25px (1.953rem) | 1.2 | 등급 배지 |
| `--cv-text-xl` | 25px (1.563rem) | 1.3 | 헤딩 |
| `--cv-text-lg` | 20px (1.25rem) | 1.4 | HUD 주요 레이블 |
| `--cv-text-md` | 16px (1rem) | 1.5 | 본문, 설명 |
| `--cv-text-sm` | 12.8px (0.8rem) | 1.5 | 라벨 |
| `--cv-text-xs` | 10.24px (0.64rem) | 1.5 | 파인 프린트 |

### 자간 & 굵기

| 토큰 | 값 |
|---|---|
| `--cv-tracking-tight` | -0.02em |
| `--cv-tracking-normal` | 0em |
| `--cv-tracking-wide` | 0.05em |
| `--cv-tracking-widest` | 0.15em (점수 숫자) |
| `--cv-weight-light` | 300 |
| `--cv-weight-regular` | 400 |
| `--cv-weight-medium` | 500 |
| `--cv-weight-bold` | 700 |

---

## 4. 찢어짐 비주얼 시스템 (핵심)

> 이 섹션이 Circle Void 디자인의 핵심입니다. 모든 다른 결정은 이 찢어짐을 돋보이게 하기 위해 존재합니다.

### 4.1 정확도별 찢어짐 스펙트럼

| 정확도 | 찢어짐 유형 | baseFreq | Octaves | Displace | Fiber Count | 비주얼 특성 |
|---|---|---|---|---|---|---|
| 95–100% | 완벽한 찢어짐 | 0.012 | 2 | 4px | 4-8 | 헤어라인처럼 매끄러움. 살짝 실크 같은 컬. 거의 안 보이는 섬유질. |
| 80–94% | 깔끔한 찢어짐 | 0.028 | 3 | 12px | 8-16 | 가장자리에 종이 텍스처 보임. 2–4px 섬유 가장자리. 부드러운 컬. |
| 60–79% | 허용 찢어짐 | 0.055 | 4 | 24px | 16-28 | 눈에 띄는 불규칙성. 5–10px 들쭉날쭉. 가끔 섬유 가닥 늘어짐. |
| 40–59% | 거친 찢어짐 | 0.090 | 5 | 42px | 28-48 | 10–18px 스파이크. 다수의 섬유 가닥. 4–8px 종이 청크 늘어짐. |
| 0–39% | 너덜너덜 찢어짐 | 0.090 | 5 | 42px+ | 28-48+ | 20–40px 불규칙 스파이크. 종이 청크 다수. 보이드를 가로지르는 종이 다리. |

### 등급 ↔ 정확도 매핑

| 정확도 범위 | 찢어짐 등급 | 게임 등급 | 토큰 이름 |
|---|---|---|---|
| 95–100% | 완벽한 찢어짐 (clean) | S | `--cv-grade-s` |
| 80–94% | 깔끔한 찢어짐 (gentle) | A | `--cv-grade-a` |
| 60–79% | 허용 찢어짐 (rough) | B | `--cv-grade-b` |
| 40–59% | 거친 찢어짐 (jagged) | C | `--cv-grade-c` |
| 0–39% | 너덜너덜 찢어짐 (ragged) | D | `--cv-grade-d` |

> 0-39% 찢어짐의 토큰 이름: `ragged`. CSS에서 `--cv-tear-freq-ragged`, `--cv-tear-displace-ragged` 등으로 사용.

### 4.2 찢어짐 엣지 해부도 (5레이어 구조)

뒤에서 앞으로 쌓이는 5개의 SVG/Canvas 레이어:

```
레이어 5 (최상단): 섬유 가닥 — 개별 SVG 경로, 0.5–1.5px 너비, 3–15px 길이
                   색상: --cv-ink-light, 불투명도 0.4–0.7, 가장자리에서 무작위 방사 오프셋

레이어 4: 종이 청크 플랩 — 두께 착시를 주는 닫힌 경로
          상단 면: --cv-bg-paper, 하단 면: --cv-bg-paper-dark
          종이 두께: 한지 ~0.3mm = 화면에서 2–4px로 표현

레이어 3: 컬 그림자 — 가장자리에서 안쪽으로 블렌딩되는 방사형 그라디언트 마스크
          20–40px 깊이, 색상: rgba(26,18,8, 0.5)
          하단 그림자 더 강하게 (중력 + 위에서 오는 빛)

레이어 2: 보이드 엣지 글로우 — 내부 가장자리에서 4–8px 바깥쪽 발광
          색상: --cv-void-glow, 블러: 6px, 불투명도: 0.6–0.9

레이어 1 (최하단): 보이드 내부 채우기 — 방사형 우주 그라디언트
```

### 4.3 종이-보이드 전환 그라디언트 (6단계)

```
정지점 1 (내부 가장자리에서 0px):   #D9CBAA  불투명도 1.0  (종이 표면)
정지점 2 (3px 안쪽):               #C4A87A  불투명도 0.9  (따뜻한 앰버 — 종이 섬유)
정지점 3 (8px 안쪽):               #5A3A2A  불투명도 0.85 (숯 — 우주 경계에서 종이 그을림)
정지점 4 (15px 안쪽):              #1A0A30  불투명도 0.9  (보이드 퍼플의 첫 힌트)
정지점 5 (30px 안쪽):              #0B0720  불투명도 1.0  (심우주 확립)
정지점 6 (60px 안쪽→중심):         #03020A  불투명도 1.0  (절대적 허공)
```

> **"그을린 가장자리"(정지점 3)가 핵심입니다.** 종이가 단순히 찢겨진 것이 아니라 현실을 만지는 힘에 의해 그을리고 있음을 암시합니다. 이 4px이 전체 디자인에서 가장 중요한 결정입니다.

### 4.4 SVG 필터 체인 (찢어짐 생성 알고리즘)

원형 클립 경계에 적용. 체인 순서 중요:

```
1. feTurbulence
   → type: "turbulence"
   → baseFrequency: [정확도 테이블 참조]
   → numOctaves: [정확도 테이블 참조]
   → seed: 원 타임스탬프에서 파생 (재현성)
   → result: "noiseMap"

2. feDisplacementMap
   → in: "SourceGraphic"
   → in2: "noiseMap"
   → scale: [displacement 값]
   → xChannelSelector: "R"
   → yChannelSelector: "G"
   → result: "tornEdge"

3. feGaussianBlur (픽셀 엣지 미세 소프트닝)
   → in: "tornEdge"
   → stdDeviation: 0.4(clean) | 0.6(gentle) | 0.8(rough) | 1.0(jagged)
   → result: "softEdge"

4. feComposite
   → operator: "in"
   → in: "softEdge"
   → in2: "SourceGraphic"
```

### 4.5 종이 두께 렌더링

```
종이 단면 = 찢어진 경로 안쪽으로 오프셋
  OFFSET: 2px(clean) → 8px(jagged)
  FILL: --cv-bg-paper-dark → --cv-bg-paper-fiber 선형 그라디언트
  OPACITY: 0.9
  Z-order: 보이드 아래, 종이 위
```

### 4.6 컬 그림자 파라미터

```
feDropShadow (찢어진 종이 가장자리 → 종이 표면 위 투영):
  dx:        2(clean) → 5(jagged)
  dy:        3(clean) → 7(jagged)   — 중력 방향
  stdDeviation: 4(clean) → 10(jagged)
  flood-color: --cv-tear-shadow rgba(26,18,9, 0.45)
  flood-opacity: 0.45

컬 하이라이트 (빛을 받는 찢어진 종이 내부 가장자리):
  너비: 1px(clean) → 3px(jagged)
  색상: --cv-tear-curl-highlight rgba(250,246,236, 0.80)
  블렌드 모드: screen
```

### 4.7 섬유 가닥 알고리즘 (Canvas 2D)

```javascript
for (let i = 0; i < fiberCount; i++) {
  // 1. 찢어진 엣지 경로에서 점 P 샘플링 (arc-length 파라미터화)
  // 2. 점 P에서 외부 노멀 N 구하기
  // 3. fiberLength = random(4px, 28px) × roughnessMultiplier(0.3~1.0)
  // 4. fiberAngle = N.angle + random(-30deg, +30deg)
  // 5. 베지에 곡선 그리기:
  //    start:  P
  //    cp1:    P + N × fiberLength × 0.4 + random lateral
  //    cp2:    P + N × fiberLength × 0.8 + random lateral
  //    end:    P + direction(fiberAngle) × fiberLength
  // 6. strokeStyle: random([--cv-tear-fiber-a, --cv-tear-fiber-b])
  // 7. lineWidth: random(0.5, 1.5)
  // 8. globalAlpha: random(0.4, 0.85)
}
```

---

## 5. 붓 획 렌더링 시스템

### 드로잉 물리

| 파라미터 | 공식 | 결과 |
|---|---|---|
| 획 너비 | `baseWidth × (1 - velocity × 0.4)` | 느림 = 두꺼움, 빠름 = 얇음 |
| 최소 너비 | 2px | 절대 사라지지 않음 |
| 최대 너비 | 28px | 완전 정지 드래그 |
| 불투명도 | `0.75 + (1 - velocity × 0.3)` | 느림 = 불투명, 빠름 = 반투명 |

### 입력별 획 너비 범위

| 입력 | 범위 |
|---|---|
| 마우스 | 1.5px → 4px |
| 터치 | 2.5px → 6px (속도로 압력 시뮬레이션) |
| 스타일러스 (Pointer API) | 0.5px → 8px (실제 압력) |

### 젖음-건조 잉크 변화

```
젖은 잉크 (획 선두):    색상 --cv-ink-dark, 불투명도 0.95, 날카로운 가장자리
중간 건조 (획 중간부):  색상 --cv-ink-mid, 불투명도 0.88, 0.8px 블러
건조 잉크 (2초+ 경과):  색상 --cv-ink-light, 불투명도 0.80, 1.5px 블러 + 종이 텍스처 배경음
```

### 원 닫힘 감지

**원스트로크 원칙 (One-Stroke Rule)**: 마우스/터치 릴리스(pointerup) = 원 완성. 중간에 손을 떼면 그 시점의 형태로 확정된다.

| 파라미터 | 값 | 설명 |
|----------|-----|------|
| 입력 방식 | 원스트로크 | pointerdown → pointermove → pointerup |
| 완성 판정 | pointerup 이벤트 | 릴리스 = 확정, 재연결 없음 |
| 닫힘 보정 | 끝점-시작점 < 15% 반지름 | 자동 스냅 연결 |
| 불완전 닫힘 | 15-40% 반지름 | 먹 흩뿌림 강제 연결, 정확도 -20% 페널티 |
| 실패 | > 40% 반지름 | 먹 스며듦 fade-out (600ms), 즉시 재시도 가능 |
| 멀티 서클 | **불가** | 화면에 활성 보이드는 항상 1개만 존재 |
| 재시도 | 즉시 | 이전 원 fade-out 후 새 드로잉 허용 |

> **설계 근거**: 붓으로 원을 그리는 행위는 "한 호흡"이다. 복잡한 재연결/프롬프트 없이, 손을 떼면 끝. 동양 서예의 일획(一劃) 정신.

---

## 6. 애니메이션 시퀀스 — 드로잉에서 보이드 열림까지

| 페이즈 | 시작 (ms) | 지속 (ms) | 이징 | 설명 |
|---|---|---|---|---|
| 잉크 방울 | 0 | 150 | ease-out | 원 완성 시 잉크 번짐 |
| 원 스냅 보정 | T+0 | 180 | ease-in-out | 그린 원을 정원에 가깝게 보정 |
| 잉크 번짐 펄스 | T+0 | 200 | ease-out | 잉크 방사형 퍼짐 |
| **예상 호흡** | **T+200** | **300** | **ease-in-out** | **완전한 침묵. 종이가 무언가를 알고 있다는 느낌.** |
| 첫 균열 | T+500 | 200 | ease-out | 잉크 라인을 따라 미세 균열 시작 |
| 찢어짐 전파 | T+700 | 500–1100 | (정확도 의존) | 원을 따라 찢어짐 퍼짐. 정확할수록 빠름 |
| 종이 컬백 | T+1200 | 400 | cubic-bezier(0.34,1.56,0.64,1) | 종이가 뒤로 말려 보이드 드러남 |
| 우주 페이드인 | T+1600 | 800 | ease-in | 별들이 개별적으로 "팝"하며 등장 (0–800ms 랜덤 지연) |
| 보이드 안정화 | T+2400 | 지속 | — | 별먼지 궤도 시작, 보이드 완성 |
| **전체 시퀀스** | — | **2400ms** | — | — |

> **페이즈 3 (예상 비트)가 가장 중요합니다.** 300ms의 완전한 침묵. 종이 질감 대비 +10% 밝기 스윙. 원에서 1px 헤어라인 동심 리플 확장. 종이가 쥐고 있는 숨 — 찢어짐이 자동이 아니라 *선택된* 것처럼 느껴지게 합니다.

---

## 6.5 실패 및 복구 흐름

### 원 미닫힘 (Circle Not Closed)

**원스트로크 원칙에 따른 처리**: 마우스/터치를 떼는 순간 원이 확정. "미닫힘"은 시작점과 끝점의 거리가 먼 경우.

| 시작-끝점 거리 | 처리 | 비주얼 |
|---------------|------|--------|
| < 15% 반지름 | 정상 닫힘 | 깨끗한 연결, 정확도 계산 진행 |
| 15-40% | 불완전 닫힘 | 먹 흩뿌림 이펙트로 강제 연결, 정확도 -20% 페널티 |
| > 40% | 실패 | 먹 스며듦 fade-out (600ms), 즉시 재시도 가능 |

재연결/대기 시간 없음. 손을 떼면 끝. 재시도 횟수 제한 없음.

### 매우 낮은 정확도 (0-39%)
- 찢어짐은 발생하나 "너덜너덜" 등급
- 보이드 기능은 작동하되 물체가 걸리거나 튕김
- Puzzle 모드: 클리어 불가능할 수 있음 → 재시도 유도

### 재시도 (Retry)
- 트리거: 결과 화면 "재시도" 버튼 / 일시정지 메뉴 "재시도"
- 동작: 현재 보이드 역재생(400ms, 종이 다시 닫힘) → 캔버스 리셋
- 이징: `--cv-ease-tear` 역방향

### 실행취소 (Undo) — Puzzle 모드 전용
- 트리거: HUD "되돌리기" 버튼 / Ctrl+Z
- 동작: 마지막 보이드가 역재생으로 닫힘 (400ms)
- 제한: 최대 3회 / 레벨

### 레벨 나가기
- 트리거: 일시정지 메뉴 "나가기"
- 확인 다이얼로그: "진행 상황이 저장되지 않습니다" (먹 프레임 다이얼로그)
- 전환: 캔버스 한지 오버레이 페이드(800ms) → 레벨 선택

---

## 7. 파티클 시스템

### 잉크 방울 파티클 (드로잉 페이즈)
- 트리거: 터치다운 + 속도 스파이크
- 개수: 터치다운 시 4–8개, 속도 스파이크당 1–3개
- 크기: 2–6px 지름, 약간 찌그러진 원
- 색상: `--cv-ink-dark`, 불투명도 0.4–0.7
- 속도: 60–180px/s 방사형 외측, 무작위 각도
- 잔재: 1.5–3px 잉크 얼룩으로 영구 남음

### 종이 섬유 파티클 (찢어짐 페이즈)
- 트리거: 찢어짐 전파 (페이즈 5)
- 개수: 호 100도당 8–20개
- 형태: 3–12px 얇은 선분, 0.5–1px 너비
- 색상: `--cv-bg-paper` + `--cv-bg-paper-dark` 혼합
- 중력 영향: Y 가속도 80px/s²
- 수명: 600–900ms, 마지막 40%에서 선형 페이드

### 별먼지 궤도 파티클 (보이드 엣지, 지속)
- 개수: 보이드 지름에 따라 12–30개
- 궤도: 타원형, 보이드 반지름 + 8–16px 반장축, 약간 기울어짐
- 속도: 0.8–2.0도/프레임, 케플러식 (균일하지 않음)
- 색상: `--cv-star-gold` 60%, `--cv-star-cool` 30%, `--cv-void-star` 10%

### 물체 흡수 파티클 (보이드 통과 시)
- 경계 통과 시 잉크 분사 + 섬유 + 우주 전환 이펙트

---

## 8. Canvas 렌더링 레이어 스택

| 레이어 | Z-Index | Canvas ID | 설명 | 업데이트 빈도 |
|---|---|---|---|---|
| 0 — Cosmic BG | 0 | `#cv-layer-cosmic` | 별 필드 + 성운 그라디언트 | 보이드 열림 후 정적. 별 반짝임 2fps |
| 1 — Paper | 10 | `#cv-layer-paper` | 한지 텍스처 타일 | 레벨 로드당 정적 |
| 2 — Tear Shadow | 20 | `#cv-layer-tear-shadow` | 찢어짐 외부 드롭 섀도우 | 찢어짐 시 리드로, 정착 후 캐시 |
| 3 — Tear Fiber | 30 | `#cv-layer-tear-fiber` | 개별 섬유 가닥 경로 | 찢어짐 중 애니메이션, 이후 정적 |
| 4 — Ink Stroke | 40 | `#cv-layer-ink` | 플레이어 실시간 붓 획 | 드로잉 중 프레임당. 닫힘 후 고정 |
| 5 — Particle | 50 | `#cv-layer-particle` | 찢어짐 잔해 + 우주 파티클 | 활성 시 프레임당. count=0이면 클리어 |
| 6 — HUD | 100 | `#cv-hud` (DOM) | 점수, 정확도, 등급 — DOM 오버레이 | 이벤트 기반 |

**합성 노트:**
- 레이어 0–5: `globalCompositeOperation: 'source-over'` (예외: Cosmic BG는 보이드 원형 `destination-in` 마스크, Tear Fiber는 `multiply` 블렌드)
- HUD(레이어 6): `position: fixed` DOM 오버레이 — 포커스 관리 + 스크린 리더 접근성
- 정적 레이어는 `OffscreenCanvas`에 캐시 후 blit (리드로 비용 0)

---

## 9. UI 컴포넌트 (FRD)

### [F-001] 정확도 미터 (엔소 인디케이터)

| 속성 | 값 |
|---|---|
| 메타포 | 엔소 원이 브러시로 채워짐 — 정확도 빌드에 따라 시계방향 |
| 크기 | 48px(모바일) / 64px(데스크톱) |
| 위치 | 우상단, 가장자리에서 20px |
| 트랙 | `--cv-ink-ghost` 20% 불투명도 원 |
| 채우기 | `--cv-ink-dark` → 95%+에서 `--cv-ink-gold` 전환 |
| 중앙 라벨 | 실시간 %, `--cv-font-mono`, `--cv-text-xs` |
| 접근성 | `role="progressbar"`, `aria-valuenow/min/max` |

### [F-002] 점수 표시

| 속성 | 값 |
|---|---|
| 폰트 | `--cv-font-score`, `--cv-text-3xl` |
| 위치 | 좌상단(데스크톱) / HUD 바 중앙(모바일) |
| 배경 | `--cv-hud-bg`, 8px radius 필, 12px 수평 패딩 |
| 애니메이션 | 숫자 붓 획 좌→우 등장, 각 자릿수 80ms 스태거 |
| 자간 | `--cv-tracking-widest` (0.15em) |
| 접근성 | `aria-live="polite"`, `aria-label="Score: {value}"` |

### [F-003] 등급 배지 (도장/인장 스타일)

| 등급 | 비주얼 | 파티클 이펙트 |
|---|---|---|
| S | 원형 도장, 이중 링, 금색 + 발광 | 금빛 파티클 버스트 + 글로우 |
| A | 원형 도장, 단일 링, 붉은색 | 붉은 잉크 스플래시 |
| B | 사각 도장, 진한 먹, 솔리드 | 잉크 번짐만 |
| C | 사각 도장, 옅은 먹, 불투명도 0.7 | 없음 |
| D | 사각 도장, SVG 필터 왜곡, 번짐 | 없음 |

- 크기: 96px(모바일) / 128px(데스크톱)
- 스탬프 애니메이션: 스케일 1.4→1.0, `--cv-ease-seal`, `--cv-duration-seal` (600ms)
- 회전: 랜덤 ±8deg (손으로 찍는 느낌)
- 접근성: `role="img"`, `aria-label="Grade: {letter}"`

### [F-004] 레벨 선택 (병풍/족자 메타포)

| 속성 | 값 |
|---|---|
| 메타포 | 병풍(屛風) — 접이식 스크린 패널 나란히 |
| 레이아웃 | 수평 스크롤 패널 행, `scroll-snap-type: x mandatory` |
| 패널 크기 | 160×240px(모바일) / 200×300px(데스크톱) |
| 잠긴 레벨 | CSS 3D rotateY 60deg 접힘 + 먹 점 봉인 |
| 완료 표시 | 우하단에 주홍 도장 마크 |
| 접근성 | `role="list"` + `role="listitem"` + 키보드 화살표 nav |

### [F-005] Zen 모드 UI

| 속성 | 값 |
|---|---|
| 활성 시 | 모든 HUD 800ms에 걸쳐 opacity: 0 |
| 보이는 요소 | 캔버스만 + 우하단 파티클 카운트 (--cv-text-xs, opacity 0.4) |
| 엣지 호흡 | 비네트 펄스: box-shadow inset 0→20px opacity 0→0.15 |
| 호흡 주기 | 3000ms, ease-in-out, infinite |

### [F-006] 모드 선택 카드

| 모드 | 워터마크 한자 | 설명 |
|---|---|---|
| Precision | 精 | 정확도 도전 |
| Puzzle | 謎 | 스토리 기반 퍼즐 |
| Zen | 禪 | 자유 모드 ASMR |

### [F-007] 진행 바
- 붓 획 채우기 바, 테이퍼드 탑
- 타이머 색상 전환 (여유→긴장)

### [F-008] 보이드 내부 (7레이어 우주 공간)
- 성운 + 별장 + 유성 + 패럴랙스 레이어드 우주 공간

### [F-009] HUD 패널 컨테이너
- 서리 종이 오버레이, 헤어라인 테두리, 배경 블러

### [F-010] 레벨 완료 모달

| 속성 | 값 |
|---|---|
| 배경 | 인게임 캔버스 위 `backdrop-filter: blur(8px)` + `--cv-bg-paper` opacity 0.85 오버레이 |
| 레이아웃 | 등급 배지(F-003) 중앙 → 점수+정확도 → 버튼 3개 세로 배치 |
| 등장 | `--cv-duration-slow` (800ms) fade-in, 등급은 별도 `--cv-delay-seal` 후 스탬프 |
| 버튼 | 재시도 / 다음 레벨 / 레벨 선택 (HUD 버튼 스타일, 가로 48px 높이) |
| 접근성 | `role="dialog"`, `aria-modal="true"`, 포커스 트랩, ESC로 닫기 |

### [F-011] HUD 버튼 세트

| 버튼 | 아이콘 | 위치 | 키보드 단축키 |
|---|---|---|---|
| 일시정지 | 두 개의 세로 먹선 (∥) | 우상단 (정확도 미터 아래) | ESC |
| 되돌리기 (Puzzle 전용) | 역방향 엔소 화살표 | 좌하단 | Ctrl+Z |
| Zen 토글 | 禪 문자 | 우하단 | Z |

공통 스타일: 48×48px, `--cv-hud-bg` 배경, `--cv-hud-text` 아이콘, `--cv-radius-lg` (8px)

### [F-012] 빈 상태 & 로딩

| 상태 | 비주얼 |
|---|---|
| 캔버스 초기 (Idle) | 중앙: "여기에 원을 그리세요" (`--cv-ink-ghost`, `--cv-text-lg`), 아래에 작은 손가락 드로잉 아이콘 |
| 레벨 로딩 | 한지 배경 + 중앙에 회전하는 불완전한 엔소 원 (`--cv-ink-mid`, 2초 루프) |
| 첫 사용자 | 3단계 온보딩: ① "손가락으로 원을 그려보세요" ② "종이가 찢어집니다!" ③ "보이드로 퍼즐을 풀어보세요" — 각 단계 먹 그림 일러스트 |

---

## 9.5 인터랙션 상태 매트릭스

### 드로잉 캔버스
| 상태 | 비주얼 | 트리거 |
|---|---|---|
| Idle | 한지 배경, 중앙에 옅은 가이드 텍스트 "여기에 원을 그리세요" (`--cv-ink-ghost`, `--cv-text-lg`) | 레벨 시작 시 |
| Hover (데스크톱) | 커서: 붓 모양 커스텀 커서 (ink drop SVG, 24px) | 마우스 캔버스 진입 |
| Drawing | 실시간 붓 획 렌더링 + 잉크 파티클 | 터치/클릭 시작 |
| Circle Closing | 고스트 가이드 원 표시 (`--cv-ink-ghost`, 20%) | 끝점이 시작점 24px 이내 |
| Tearing (입력 차단) | 캔버스 포인터 이벤트 비활성, 찢어짐 애니메이션 2400ms | 원 닫힘 완료 |
| Error (미닫힘) | 잉크 획이 2초에 걸쳐 `--cv-ink-ghost`로 페이드 → 사라짐 | 원 닫히지 않고 터치 종료 |
| Loading | 스켈레톤: 한지 텍스처 + 중앙 먹 엔소 로딩 애니메이션 (회전하는 불완전한 원) | 레벨 로딩 중 |

### 레벨 선택 패널 (F-004)
| 상태 | 비주얼 | 트리거 |
|---|---|---|
| Default | 종이 패널, 레벨명, 난이도, 최고 점수 | — |
| Hover | 패널 살짝 확대 (scale 1.03), `--cv-shadow-md` 추가 | 마우스오버 |
| Active/Pressed | scale 0.97, `--cv-shadow-sm` | 클릭/탭 중 |
| Selected | 하단에 `--cv-ink-dark` 2px 밑줄, 150ms 애니메이션 | 선택 시 |
| Locked | CSS 3D rotateY(60deg), 50% 불투명 오버레이 + 먹 자물쇠 아이콘 | 미잠금 해제 |
| Completed | 우하단 주홍 도장(24px), 최고 등급 배지 소형(16px) | 클리어 완료 |

### HUD 버튼 (공통)
| 상태 | 비주얼 |
|---|---|
| Default | 먹 프레임 아이콘, `--cv-hud-text`, 48px 터치 타겟 |
| Hover | 프레임 굵기 2px→3px, `--cv-ink-dark` 강조 |
| Active | scale 0.92, 프레임 채우기 `--cv-ink-ghost` 20% |
| Disabled | `--cv-hud-text-dim`, opacity 0.4 |
| Focus | `--cv-focus-ring` 3px 아웃라인 |

### Zen 모드 토글
| 상태 | 비주얼 | 위치 |
|---|---|---|
| Off (기본) | 먹 프레임 禪 문자, 48px | 우하단 HUD 영역 |
| On | 禪 문자 페이드아웃(800ms), 모든 HUD 페이드아웃 | — |
| 해제 시 | 캔버스 가장자리 탭 → HUD 800ms 페이드인 | — |

---

## 10. 사운드 디자인 비주얼 컨텍스트

| 비주얼 이벤트 | 사운드 이벤트 | 특성 |
|---|---|---|
| 터치다운 잉크 방울 | 종이 위 붓 털 | 부드럽고 질감 있는, 건조 |
| 실시간 획 드로잉 | 한지 위 잉크 붓 | ASMR: 조용하고 섬유질 |
| 원 완성 | 잉크 풀링 클릭 | 단일 탭, 공명 |
| 예상 비트 침묵 | 근침묵, 룸 톤 | 부재를 통한 긴장감 |
| 종이 균열 | 종이 섬유 스트레스 | 바삭하고 날카로운 마이크로 균열 |
| 찢어짐 전파 | 종이 찢어짐 — 한지 특유 | 연속적인 섬유질 찢어짐 |
| 우주 출현 | 먼 공명 톤 | 깊고, 넓고, 우주적 |
| 별 파티클 팝 | 높은 마이크로 톤, 랜덤 피치 | 작은 종 또는 크리스탈 |
| 보이드 안정화 | 우주 앰비언트 루프 | 무한하고 호흡 가능한 |

**Zen 모드 특별 규칙:** 모든 기계식 클릭 사운드 제거. 종이 텍스처와 붓 소리가 주역. 우주 내부: 20–30Hz 서브베이스 존재, 거의 들리지 않게.

---

## 11. 반응형 레이아웃

### 브레이크포인트

| 이름 | 최소 너비 | 컨텍스트 |
|---|---|---|
| `mobile-sm` | 320px | 소형 폰 |
| `mobile` | 390px | iPhone 14 기준 |
| `tablet` | 768px | 태블릿, 폴더블 |
| `desktop` | 1024px | 랩톱 |
| `wide` | 1440px | 대형 디스플레이 |

### 캔버스 사이징

```
모바일 (< 768px):
  width: 100vw, height: 100dvh (fallback: 100vh)
  세로 모드 우선, 가로 모드 지원 (HUD 축소)

데스크톱 (>= 1024px):
  max-width: 1440px, height: 100vh
  16:9 비율 유지, 레터박스 (--cv-bg-paper-dark)

픽셀 비율:
  Low tier: 1x | Mid tier: min(dpr, 1.5) | High tier: min(dpr, 2)
  3x 화면이라도 최대 2x — 퍼포먼스 > 미세 디테일
```

### HUD 포지셔닝

```
모바일 (세로): 하단 72px 수평 바, env(safe-area-inset-bottom) 적용
모바일 (가로): 정확도 우상단(48px), 점수 상단 중앙
데스크톱: 정확도 우상단 20px, 점수 좌상단 20px, 등급 화면 중앙
```

### 터치 입력 보정

```
최소 터치 타겟: 44×44px (WCAG 2.5.5)
속도 스무딩: Catmull-Rom 스플라인, 룩어헤드 3포인트
최소 포인트 거리: 2px (서브픽셀 지터 제거)
팬 리젝션: 접촉 면적 > 200px² → 팬으로 판정
```

### Safe Area

```css
.cv-hud-top    { padding-top:    max(var(--cv-space-4), env(safe-area-inset-top)); }
.cv-hud-bottom { padding-bottom: max(var(--cv-space-4), env(safe-area-inset-bottom)); }
.cv-hud-left   { padding-left:   max(var(--cv-space-4), env(safe-area-inset-left)); }
.cv-hud-right  { padding-right:  max(var(--cv-space-4), env(safe-area-inset-right)); }
```

---

## 12. 접근성

### 대비율 (WCAG 2.1 AA)

| 조합 | 비율 | 필요 | 상태 |
|---|---|---|---|
| HUD 텍스트 / HUD 배경 | ~8.2:1 | 4.5:1 | PASS |
| 먹 / 종이 | ~14.8:1 | 4.5:1 | PASS |
| 보이드 글로우 / 보이드 딥 | ~5.7:1 | 3:1 | PASS |
| S등급 금 / 종이 | ~2.9:1 | 3:1 | **FLAG** |
| 보조 HUD 텍스트 | ~4.6:1 | 4.5:1 | PASS (근소) |

### 축소 모션

```css
@media (prefers-reduced-motion: reduce) {
  .cv-tear         { transition: none; }        /* 찢어짐 → 즉시 최종 상태 */
  #cv-layer-particle { display: none; }          /* 파티클 비활성 */
  .cv-grade-badge  { animation: cv-fade-in 300ms ease; } /* 불투명도만 */
  .cv-score-digit  { animation: none; }          /* 즉시 표시 */
  .cv-zen-breathe  { animation: none; }          /* 호흡 끔 */
}
```

### 색맹 대응
- 등급은 항상 **문자**(S/A/B/C/D)로 전달 — 색상은 보조
- S(원형)/A(타원)/B(사각)/C(사각 옅게)/D(사각 왜곡) — 형태로 구분
- 종이 vs 보이드: 밝기 대비 L≈90 vs L≈3 — 모든 색맹 유형에서 통과

### 스크린 리더 & ARIA

```
aria-live="assertive" 알림:
  - "Drawing started"
  - "Circle closed — {accuracy}% accuracy"
  - "Void opened"
  - "Level complete — Score: {score}, Grade: {grade}"

랜드마크: <main> 게임 캔버스 | <aside> HUD | <dialog> 등급 공개 | <nav> 레벨 선택
포커스: 레벨 완료 시 등급 다이얼로그로 이동, 닫히면 마지막 활성 패널로 복귀
```

### 인게임 키보드 단축키

| 키 | 동작 |
|---|---|
| ESC | 일시정지 / 다이얼로그 닫기 |
| Z | Zen 모드 토글 |
| R | 재시도 (결과 화면 / 일시정지 메뉴에서) |
| Ctrl+Z | 되돌리기 (Puzzle 모드, 최대 3회) |
| Tab | HUD 요소 간 포커스 이동 |
| Enter/Space | 선택/확인 |
| ←/→ | 레벨 선택 패널 탐색 |

### 포커스 순서 (Tab Order)

**인게임:** 캔버스 → 일시정지 버튼 → 정확도 미터 → 점수 → (Puzzle) 되돌리기 → Zen 토글
**레벨 선택:** 뒤로가기 → 패널 1 → 패널 2 → ... → 패널 N
**결과 화면:** 등급 배지 → 재시도 → 다음 레벨 → 레벨 선택

### 입력 방식 범위

| 입력 방식 | 지원 여부 | 근거 |
|-----------|-----------|------|
| 마우스/트랙패드 | ✅ 완전 지원 | 데스크톱 주요 입력 |
| 터치 | ✅ 완전 지원 | 모바일/태블릿 주요 입력 |
| 스타일러스/펜 | ✅ 완전 지원 | 압력 감지 활용 |
| 키보드 | ⚠️ 메뉴 탐색만 | 원 드로잉은 포인터 전용 |
| 음성 | ❌ 미지원 | 드로잉 게임 특성상 불가 |
| 스위치 | ❌ 미지원 | 연속 경로 입력 필수 |

> **설계 근거**: Circle Void는 "원을 그리는 행위" 자체가 핵심 메커닉인 드로잉 게임. 키보드/음성으로 원 그리기를 대체하면 게임의 본질이 훼손된다. 메뉴 탐색, 일시정지 등 비드로잉 UI는 키보드 접근성 완전 지원.

---

## 13. 퍼포먼스 버짓

### 디바이스 티어

| 티어 | 프로필 | 감지 기준 |
|---|---|---|
| Low | 구형 Android, iPhone A12 미만 | `hardwareConcurrency <= 4 && dpr < 2` |
| Mid | iPhone 12, 2022+ 중급 Android | 기본값 |
| High | iPhone 14+, 2023+ 플래그십 | `hardwareConcurrency >= 8 && dpr >= 2` |

### 파티클 한도

| 티어 | 찢어짐 버스트 | 앰비언트 | 합계 |
|---|---|---|---|
| Low | 24 | 0 | 24 |
| Mid | 64 | 16 | 80 |
| High | 128 | 32 | 160 |

### Draw Call 목표: **프레임당 최대 10회** (정적 상태)

| 레이어 | 전략 | Draw Call |
|---|---|---|
| Cosmic BG | 1회 그리기, OffscreenCanvas 캐시 | 1 (blit) |
| Paper | 정적 텍스처 blit | 1 |
| Tear Shadow | 정착 후 캐시 | 1 |
| Tear Fiber | 단일 beginPath/stroke 패스 | 1-3 |
| Ink Stroke | 증분 append (전체 리드로 금지) | 1 |
| Particles | 스프라이트 시트 배치 | 1-2 |

### 텍스처 아틀라스

| 아틀라스 | 크기 | 포맷 |
|---|---|---|
| `atlas-paper.webp` | 512×512 | WebP q85, 한지 타일 (시밀리스) |
| `atlas-cosmic.webp` | 1024×1024 | WebP q80, 성운+별 필드 |
| `atlas-seals.webp` | 256×256 | WebP q90, S/A/B/C/D 도장 5종 |
| `atlas-particles.webp` | 128×128 | WebP q85, 섬유/별/잉크 3종 |

총 텍스처 예산: **< 500KB** (압축 후 합산)

---

## 14. CSS Custom Properties (전체 토큰)

```css
:root {
  /* Colors — Paper */
  --cv-bg-paper:            #F2EBD9;
  --cv-bg-paper-dark:       #E8DCBF;
  --cv-bg-paper-light:      #FAF6EC;
  --cv-bg-paper-fiber:      #D9CEAA;

  /* Colors — Ink */
  --cv-ink-dark:            #1A1209;
  --cv-ink-mid:             #3D2B1A;
  --cv-ink-light:           #7A5C3F;
  --cv-ink-wash:            #A89070;
  --cv-ink-ghost:           #BBA98A;
  --cv-ink-red:             #8B1A1A;
  --cv-ink-gold:            #C9922B;

  /* Colors — Void */
  --cv-void-core:           #03020A;
  --cv-void-deep:           #050D1F;
  --cv-void-mid:            #0A1A3A;
  --cv-void-edge:           #2A1F7A;
  --cv-void-nebula-a:       #1A0A2E;
  --cv-void-nebula-b:       #0A2218;
  --cv-void-star:           #E8F0FF;
  --cv-void-glow:           #4A7FFF;

  /* Colors — Tear Edge */
  --cv-tear-fiber-a:        #C8B896;
  --cv-tear-fiber-b:        #E0D4B8;
  --cv-tear-shadow:         rgba(26, 18, 9, 0.45);
  --cv-tear-curl-highlight: rgba(250, 246, 236, 0.80);

  /* Colors — Grade Seal */
  --cv-grade-s:             #C9922B;
  --cv-grade-a:             #8B1A1A;
  --cv-grade-b:             #1A1209;
  --cv-grade-c:             #7A5C3F;
  --cv-grade-d:             #BBA98A;

  /* Colors — HUD */
  --cv-hud-bg:              rgba(26, 18, 9, 0.70);
  --cv-hud-border:          rgba(242, 235, 217, 0.15);
  --cv-hud-text:            #F2EBD9;
  --cv-hud-text-dim:        #BBA98A;

  /* Colors — System */
  --cv-focus-ring:          #4A7FFF;
  --cv-error:               #8B1A1A;
  --cv-success:             #2A5C3A;

  /* Spacing — 4px grid */
  --cv-space-1: 4px;   --cv-space-2: 8px;   --cv-space-3: 12px;
  --cv-space-4: 16px;  --cv-space-5: 20px;  --cv-space-6: 24px;
  --cv-space-8: 32px;  --cv-space-10: 40px; --cv-space-12: 48px;
  --cv-space-16: 64px; --cv-space-20: 80px; --cv-space-24: 96px;

  /* Typography */
  --cv-font-display:  'Noto Serif KR', 'Source Han Serif', serif;
  --cv-font-body:     'Pretendard', 'Noto Sans KR', 'Source Han Sans', sans-serif;
  --cv-font-mono:     'JetBrains Mono', 'Courier New', monospace;
  --cv-text-xs: 0.64rem;  --cv-text-sm: 0.80rem;  --cv-text-md: 1rem;
  --cv-text-lg: 1.25rem;  --cv-text-xl: 1.563rem; --cv-text-2xl: 1.953rem;
  --cv-text-3xl: 2.441rem; --cv-text-4xl: 3.052rem;
  --cv-weight-light: 300; --cv-weight-regular: 400;
  --cv-weight-medium: 500; --cv-weight-bold: 700;
  --cv-leading-tight: 1.15; --cv-leading-normal: 1.5; --cv-leading-loose: 1.8;
  --cv-tracking-tight: -0.02em; --cv-tracking-normal: 0em;
  --cv-tracking-wide: 0.05em; --cv-tracking-widest: 0.15em;

  /* Border Radius */
  --cv-radius-none: 0; --cv-radius-sm: 2px; --cv-radius-md: 4px;
  --cv-radius-lg: 8px; --cv-radius-xl: 16px; --cv-radius-full: 9999px;

  /* Shadows */
  --cv-shadow-sm:    0 1px 3px rgba(26,18,9,0.30);
  --cv-shadow-md:    0 4px 12px rgba(26,18,9,0.40);
  --cv-shadow-lg:    0 8px 24px rgba(26,18,9,0.50);
  --cv-shadow-void:  0 0 40px rgba(74,127,255,0.25), inset 0 0 60px rgba(2,4,8,0.90);
  --cv-shadow-tear:  2px 3px 8px rgba(26,18,9,0.45), -1px -1px 4px rgba(26,18,9,0.20);
  --cv-shadow-seal:  1px 2px 0px rgba(26,18,9,0.60);
  --cv-glow-void:    0 0 20px rgba(74,127,255,0.40);
  --cv-glow-gold:    0 0 16px rgba(201,146,43,0.60);

  /* Motion — Easing */
  --cv-ease-brush:   cubic-bezier(0.25, 0.46, 0.45, 0.94);
  --cv-ease-tear:    cubic-bezier(0.55, 0.00, 0.45, 1.00);
  --cv-ease-float:   cubic-bezier(0.45, 0.05, 0.55, 0.95);
  --cv-ease-seal:    cubic-bezier(0.34, 1.56, 0.64, 1.00);
  --cv-ease-spring:  cubic-bezier(0.175, 0.885, 0.320, 1.275);

  /* Motion — Duration */
  --cv-duration-instant: 50ms;  --cv-duration-fast: 150ms;
  --cv-duration-normal: 300ms;  --cv-duration-tear: 420ms;
  --cv-duration-seal: 600ms;    --cv-duration-slow: 800ms;
  --cv-duration-breathe: 3000ms; --cv-duration-particle: 1200ms;

  /* Motion — Delay */
  --cv-delay-fiber: 80ms; --cv-delay-cosmic: 120ms; --cv-delay-seal: 500ms;

  /* Tear Edge */
  --cv-tear-freq-clean: 0.012;  --cv-tear-freq-gentle: 0.028;
  --cv-tear-freq-rough: 0.055;  --cv-tear-freq-jagged: 0.090;
  --cv-tear-freq-ragged: 0.090;
  --cv-tear-octaves-clean: 2;   --cv-tear-octaves-gentle: 3;
  --cv-tear-octaves-rough: 4;   --cv-tear-octaves-jagged: 5;
  --cv-tear-octaves-ragged: 5;
  --cv-tear-displace-clean: 4px;  --cv-tear-displace-gentle: 12px;
  --cv-tear-displace-rough: 24px; --cv-tear-displace-jagged: 42px;
  --cv-tear-displace-ragged: 42px;
  --cv-tear-thickness-min: 2px;   --cv-tear-thickness-max: 8px;
  --cv-fiber-count-base: 12;
  --cv-fiber-length-min: 4px;     --cv-fiber-length-max: 28px;

  /* Z-Index */
  --cv-z-cosmic: 0;   --cv-z-paper: 10;  --cv-z-tear-shadow: 20;
  --cv-z-tear-fiber: 30; --cv-z-ink: 40; --cv-z-particle: 50;
  --cv-z-hud: 100;    --cv-z-modal: 200; --cv-z-tooltip: 300;

  /* Layout */
  --cv-canvas-max-w: 1440px;
  --cv-touch-min: 44px;
  --cv-touch-ideal: 48px;
}
```

### SVG 필터 ID (HTML에 숨김 `<svg>` 임베드)

```
#cv-filter-tear-clean    — roughness: clean
#cv-filter-tear-gentle   — roughness: gentle
#cv-filter-tear-rough    — roughness: rough
#cv-filter-tear-jagged   — roughness: jagged
#cv-filter-paper-texture — 종이 마이크로 디테일
#cv-filter-seal-stamp    — 등급 도장 왜곡
```

---

## 15. 경쟁사 포지셔닝

| 측면 | Perfect Circle (네온) | Zen Circle | **Circle Void** |
|---|---|---|---|
| 색상 분위기 | 전기적, 네온, 디지털 | 미니멀 흰/검정 | 따뜻한 종이 + 깊은 우주 |
| 메타포 | 레이저 정밀도 | 마음챙김 | 현실의 물리적 찢어짐 |
| 텍스처 | 플랫 / 유리 | 플랫 / 매트 | 풍부함: 종이 결 + 잉크 + 우주 |
| 고유한 비주얼 | 다크 위의 네온 원 | 볼 것 없음 | **찢어짐이 설계된 오브제** |

> **우리의 비주얼 간극:** 어떤 원 그리기 게임도 원의 파괴를 미적 중심으로 삼지 않았습니다. 보이드는 부재가 아닙니다 — 화면에서 가장 상세하고 설계되고 아름다운 것입니다. 경쟁사는 원을 만듭니다. **우리는 열림(Opening)을 만듭니다.**

---

## 16. 플래그 & 이슈

| # | 이슈 | 심각도 | 해결 방향 |
|---|---|---|---|
| FLAG-001 | S등급 금 `#C9922B` 대비율 2.9:1 (WCAG 3:1 미달) | HIGH | 도장 주변 `1px solid rgba(26,18,9,0.6)` 보더 추가 또는 텍스트 요소만 `#8B6520`으로 어둡게 |
| FLAG-002 | Noto Serif KR 풀 웨이트 10MB+ | HIGH | 한글+라틴+숫자만 서브셋, 웨이트당 <300KB 목표 |
| FLAG-003 | 데스크톱 레벨 선택 가로 스크롤 | MED | 좌우 화살표 컨트롤(44px) 추가 필수, 스크롤 제스처만 의존 금지 |
| FLAG-004 | feTurbulence seed 재현성 | MED | seed를 게임 상태에 저장하여 리플레이/공유 시 동일 찢어짐 보장 |
| FLAG-005 | Zen 모드 고스트 가이드 | LOW | `zenMode === true`일 때 가이드 표시 안 함 |
| FLAG-006 | Low 티어 앰비언트 파티클 0개 | MED | CSS gradient mask rotate 애니메이션으로 제로 JS 비용 폴백 |
| FLAG-007 | `100dvh` 호환성 | HIGH | `height: 100vh; height: 100dvh;` 캐스케이드 폴백 |

---

## 17. 핸드오프 노트

### 폰트 로딩
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preload" href="/fonts/NotoSerifKR-Bold.woff2" as="font" crossorigin>
<link rel="preload" href="/fonts/JetBrainsMono-Regular.woff2" as="font" crossorigin>
```
셀프 호스트 필수. 런타임 Google Fonts fetch 금지 — 게임 시작 지연 불가.

### CSS 아키텍처
```
tokens.css       — :root 커스텀 프로퍼티 (이 문서)
reset.css        — 최소 리셋
layout.css       — 캔버스 사이징, HUD 포지셔닝, safe area
components.css   — HUD, 등급 배지, 레벨 선택 DOM 컴포넌트
animations.css   — 키프레임, 트랜지션 유틸리티
a11y.css         — 축소 모션 오버라이드, 포커스 스타일
```

### Canvas 아키텍처
```
GameCanvas class
  ├── CosmicLayer     (OffscreenCanvas, 정적)
  ├── PaperLayer      (OffscreenCanvas, 정적)
  ├── TearShadowLayer (OffscreenCanvas, 찢어짐 후 캐시)
  ├── TearFiberLayer  (OffscreenCanvas, 정착 후 캐시)
  ├── InkLayer        (실시간 Canvas, 증분 그리기)
  └── ParticleLayer   (실시간 Canvas, 프레임당 전체 클리어/리드로)

메인 루프:
  rAF → ParticleLayer.update() → 전체 레이어 디스플레이 캔버스에 합성
  InkLayer: 활성 스트로크 중에만 리드로
  정적 레이어: OffscreenCanvas에서 blit (리드로 비용 0)
```

---

*Circle Void Design System v1.0 | 2026-03-25*
*컨셉: 디자이너-A | UI 시스템: 디자이너-B*
*기능 스펙 및 사용자 플로우는 기획자 참조*
*관련 문서: [[Circle Void 기획서]]*

---

## 18. 퍼즐 모드 레벨 설계 (별도 문서)

퍼즐 유형, 난이도 곡선, 레벨별 메커닉 소개 순서는 **별도 레벨 디자인 문서**에서 관리한다.

본 DESIGN.md에서 정의하는 범위:
- ✅ 퍼즐 모드의 **비주얼 시스템** (찢어짐, 보이드, HUD)
- ✅ 퍼즐 모드의 **인터랙션 규칙** (원스트로크, 실행취소 3회)
- ✅ 퍼즐 모드의 **UI 컴포넌트** (레벨 선택 병풍, 결과 모달)
- ❌ 개별 퍼즐 유형/메커닉 → `Circle Void — Level Design.md`
- ❌ 난이도 곡선/진행 시스템 → `Circle Void — Level Design.md`
- ❌ 스토리/챕터 구조 → `Circle Void — Level Design.md`

> 레벨 디자인은 디자인 시스템과 독립적으로 이터레이션되어야 하므로 분리.
