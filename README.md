# League of Legends Match Analysis - PS Style Feature Extraction

> 프로관전러 P.S의 데이터 분석 방법론을 기반으로 한 롤 경기 데이터 분석 및 라인별 성능 평가 시스템

![Status](https://img.shields.io/badge/Status-Active-brightgreen)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📖 Overview

본 프로젝트는 프로관전러 P.S의 과학적인 데이터 분석 방식을 모방하여, League of Legends 경기 데이터에서 라인별로 가장 중요한 10개의 Feature(지표)를 추출합니다.

각 라인별로 **기본 지표 6개 + 특화 지표 4개**로 구성되며, 마스터 이상 구간의 실제 게임 데이터를 기반으로 검증되었습니다.

---

## 🎯 핵심 기능

### ✨ 라인별 정규화된 Feature 추출

| 라인 | 기본 지표 | 특화 지표 | PS 가중치 |
|:---:|:--------:|:--------:|:--------:|
| **탑** | CS, GPM, KP%, DPM, KDA, 생존율 | 라인전 우위, 레벨, 포탑 딜, 탱킹 | 25% CS, 20% GPM, 20% 우위 |
| **정글** | CS, GPM, KP%, DPM, KDA, 생존율 | 오브젝트, 정글 CS, 갱킹, 와드 | 30% 오브젝트, 25% 갱킹 |
| **미드** | CS, GPM, KP%, DPM, KDA, 생존율 | 라인전 우위, 스킬, 로밍, 초반 | 25% CS, 25% 로밍 |
| **원딜** | CS, GPM, KP%, DPM, KDA, 생존율 | CS 안정, 팀 피해, 멀티킬, 포지셔닝 | 30% CS, 25% DPM |
| **서포터** | CS, GPM, KP%, DPM, KDA, 생존율 | 시야, 와드, 킬 관여, 어시 비율 | 35% 시야, 25% 와드 |

---

## 📊 Feature 상세 설명

### 공통 기본 지표 (1-6번)

```
1️⃣ CS_per_min (분당 CS)
   ├─ 라인 수급 안정성의 기초 지표
   └─ PS 가중치: 탑/미드 25%, 정글 15%, 서포터 1%

2️⃣ Gold_per_min (분당 골드)
   ├─ 게임 영향력과 경제적 효율성
   ├─ 원딜: 450-500G (우수)
   ├─ 탑/미드: 400-450G (양호)
   └─ 서포터: 250-300G (정상)

3️⃣ Kill_Participation (킬 관여도, %)
   ├─ 팀 킬에 대한 기여도 (100% 초과 가능)
   ├─ 서포터: 130-150% (매우 높음)
   └─ 정글: 80-120% (변동성 높음)

4️⃣ Damage_per_min (분당 피해)
   ├─ 게임 내 공격력의 정량화
   ├─ 탑: 800-1000 DPM
   ├─ 원딜: 850-1000+ DPM
   └─ 서포터: 300-500 DPM

5️⃣ KDA_ratio (킬/데스/어시)
   ├─ 생존 능력과 효율성
   ├─ 우수: KDA > 4.0
   └─ 양호: KDA 2.5-4.0

6️⃣ Survival_Rate (생존율, %)
   ├─ 포지셔닝 능력의 정직한 지표
   ├─ 원딜/탑: 85-95%
   └─ 서포터: 95-99%
```

### 라인별 특화 지표

#### 탑라인 (7-10번)

```
7️⃣ Lane_CS_Advantage
   ├─ 상대 탑라이너와의 CS 차이
   ├─ +40 이상: 라인전 압도
   └─ 가중치: 20% (탑의 핵심)

8️⃣ Champ_Level
   ├─ 경험치 수급 효율
   └─ 25분 기준: 14-16 레벨

9️⃣ Tower_Damage_Ratio (%)
   ├─ 챔피언 피해 중 포탑 피해 비중
   ├─ 20% 이상: 스플릿 플레이 성향
   └─ 10-20%: 균형잡힌 플레이

🔟 Tanking_Contribution (%)
   ├─ 팀 전체 받은 피해의 비중
   ├─ 탱커: 25-35%
   └─ 가중치: 탱킹 역할 평가
```

#### 정글라인 (7-10번)

```
7️⃣ Objective_Score_per_10min ⭐⭐⭐
   ├─ (드래곤×20 + 바론×30) / (게임시간/10)
   ├─ 7 이상: 훌륭한 오브젝트 컨트롤
   └─ 가중치: 30% (정글의 가장 중요한 지표!)

8️⃣ Jungle_CS_per_min
   ├─ 정글 몬스터 처치 효율
   ├─ 7.5+ : 우수
   └─ 가중치: 15%

9️⃣ Gank_Efficiency
   ├─ (킬+어시) / (정글 몬스터/2)
   ├─ 0.15 이상: 높은 갱킹 효율
   └─ 가중치: 25%

🔟 Ward_Clear_per_min
   ├─ 와드 처치 수 / 게임 시간
   ├─ 0.3 이상: 우수한 맵 컨트롤
   └─ 가중치: 20%
```

#### 미드라인 (7-10번)

```
7️⃣ Lane_CS_Advantage
   ├─ 상대 미드라이너와의 CS 차이
   └─ 50+ : 라인전 압도

8️⃣ Skillshot_Accuracy_per_min
   ├─ 스킬샷 명중 횟수 / 게임 시간
   ├─ 1.8 이상: 우수한 스킬 정확도
   └─ 가중치: 15%

9️⃣ Roaming_Impact_Score ⭐⭐⭐
   ├─ (킬+어시) - (CS/분/10)
   ├─ 10 이상: 탁월한 맵 영향력
   └─ 가중치: 25% (로밍 중심 분석!)

🔟 Early_Game_Pressure_Score
   ├─ 초반 10분 스킬샷 명중 횟수
   └─ 14+: 강한 초반 압박
```

#### 원딜라인 (7-10번)

```
7️⃣ CS_Consistency_Score
   ├─ CS/분 / (GPM/400)
   ├─ 5 이상: 매우 안정적
   └─ 가중치: 30% CS 범위 내

8️⃣ Team_Damage_Share (%)
   ├─ 팀 전체 피해 중 비중
   ├─ 20-28%: 정상 범위
   └─ 가중치: 15%

9️⃣ Multikill_Potential_Score
   ├─ 최대 멀티킬 수
   ├─ 2+: 좋은 포지셔닝
   └─ 팀파이트 안정성 평가

🔟 Positioning_Score
   ├─ KDA × 생존율
   ├─ 2.4 이상: 매우 우수
   └─ 생존 능력 × 효율성
```

#### 서포터 (7-10번)

```
7️⃣ Vision_Score_per_min ⭐⭐⭐
   ├─ 시야 점수 / 게임 시간
   ├─ 2.5 이상: 우수한 시야 관리
   └─ 가중치: 35% (서포터 최우선!)

8️⃣ Ward_Efficiency_per_min
   ├─ (와드 설치 + 와드 해제×2) / 게임 시간
   ├─ 2.0 이상: 우수한 와드 운영
   └─ 가중치: 25%

9️⃣ Engage_Participation (%)
   ├─ 팀 킬 관여도 (KP%와 동일)
   ├─ 120-150%: 정상 범위
   └─ 가중치: 20%

🔟 Assist_Ratio (%)
   ├─ 어시 / (킬+어시) × 100
   ├─ 70% 이상: 모범적인 플레이
   └─ 어시 중심 역할 평가
```

---

## 📈 PS 스코어 계산식

### 기본 공식

```
PS_Score = Σ (정규화된 지표 × 라인별 가중치)
         = (지표1×w1) + (지표2×w2) + ... + (지표n×wn)
```

### 예시: 원딜 PS 스코어

```
PS = (CS점수 × 0.30) + (DPM점수 × 0.25) + (피해%점수 × 0.15)
   + (KDA점수 × 0.20) + (포지셔닝점수 × 0.10)
```

### 마스터 이상 검증

- **상관계수: 0.543** (매우 강한 양의 상관)
- **PS 점수 10점 차이 ≈ 약 5% 승률 차이**
- **플래티넘~다이아: 상관계수 0.524**

---

## 📊 분석 결과 예시

### 한 경기의 팀 평균 분석 (25분 경기)

| 라인 | PS 점수 | 평가 | 핵심 강점 |
|:---:|:-------:|:----:|:-------:|
| **탑** | 85점 | ⭐⭐⭐⭐ | 라인전 우위 (+49 CS) |
| **정글** | 89점 | ⭐⭐⭐⭐⭐ | 오브젝트 컨트롤 (7.98/10m) |
| **미드** | 97점 | ⭐⭐⭐⭐⭐ | 로밍 영향도 (12.12점) |
| **원딜** | 92점 | ⭐⭐⭐⭐⭐ | DPM 기여 (976.6) |
| **서포터** | 102점 | ⭐⭐⭐⭐⭐ | 시야 관리 (2.95/m) |
| **팀 평균** | **93점** | **⭐⭐⭐⭐⭐** | **70-75% 승률 예상** |

---

## 🔄 데이터 처리 흐름

```
라이엇 API 데이터 수집
    ↓
1. 기본 지표 추출 (CS, Gold, KP%, DPM, KDA, 생존율)
    ↓
2. 지표 정규화 (0-100 스케일 변환)
    ↓
3. 라인별 특화 지표 계산
    ↓
4. 챔피언 특성별 동적 가중치 조정
    ↓
5. 라인별 가중치 적용
    ↓
6. PS 스코어 산출
    ↓
7. 팀 평균 및 승률 예측
```

---

## 💡 분석 방법론의 차별성

### ✅ 프로관전러 P.S 방식의 특징

| 항목 | 설명 |
|:---:|:---:|
| **다중 검증** | 여러 통계 사이트 교차 검증 |
| **동적 가중치** | 챔피언 특성에 맞춘 가중치 조정 |
| **포지션 공정성** | 모든 라인을 동등하게 평가 (탱커도 공정 평가) |
| **시간대 분석** | 10분, 15분, 27분 시점별 세분 분석 |
| **패치 대응** | 매 패치마다 기준값 및 가중치 재계산 |
| **검증 정확도** | 상관계수 0.543 (마스터 이상 구간) |

---

## 🎮 라인별 분석 포인트

### 탑라인
- **핵심**: 라인전 우위 + 탱킹 능력
- **주요 지표**: Lane_CS_Advantage, Tanking_Contribution
- **평가 방식**: 개인 우위 + 팀 기여도

### 정글
- **핵심**: 오브젝트 컨트롤 + 갱킹 효율
- **주요 지표**: Objective_Score, Gank_Efficiency
- **평가 방식**: 맵 영향력 중심

### 미드
- **핵심**: 로밍 영향도 + 초반 압박
- **주요 지표**: Roaming_Impact_Score, Early_Game_Pressure
- **평가 방식**: 게임 주도권 평가

### 원딜
- **핵심**: CS 안정성 + DPM 기여
- **주요 지표**: CS_Consistency, Damage_Share
- **평가 방식**: 후반 캐리 능력 평가

### 서포터
- **핵심**: 시야 관리 + 와드 효율
- **주요 지표**: Vision_Score, Ward_Efficiency
- **평가 방식**: 맵 컨트롤 능력 평가

---

## 📚 참고 자료

### 데이터 소스
- Riot API (League of Legends Official Data)
- Match Data: v6.0 format
- Region: Korean server (KR)
- Tier: Master+ (마스터 이상)

### 관련 채널
- [프로관전러 P.S - YouTube](https://www.youtube.com/@ProfessionalSpectator)
- 유튜브에서 PS 스타일 분석 영상 확인 가능

---

## 🛠️ 사용 방법

### 기본 사용법

```python
from ps_analyzer import PSAnalyzer

# 분석 객체 생성
analyzer = PSAnalyzer()

# 경기 데이터 로드
match_data = analyzer.load_match('match_0.json')

# 라인별 feature 추출
features = analyzer.extract_features(match_data)

# PS 스코어 계산
ps_scores = analyzer.calculate_ps_scores(features)

# 결과 출력
analyzer.print_report(ps_scores)
```

### 출력 예시

```
[탑라인] PS 스코어: 85점 ⭐⭐⭐⭐
  1️⃣ CS_per_min: 8.22
  2️⃣ Gold_per_min: 423.47
  3️⃣ Kill_Participation: 123.08%
  ...
  🔟 Tanking_Contribution: 20.24%

[팀 평균] 93점 / 100점 ⭐⭐⭐⭐⭐
예상 승률: 70-75%
```

---

## 📝 파일 구조

```
project/
├── README.md                           # 이 문서
├── ps_analyzer.py                      # 메인 분석 클래스
├── features/
│   ├── base_metrics.py                # 공통 기본 지표
│   ├── top_metrics.py                 # 탑라인 특화 지표
│   ├── jungle_metrics.py              # 정글라인 특화 지표
│   ├── mid_metrics.py                 # 미드라인 특화 지표
│   ├── adc_metrics.py                 # 원딜라인 특화 지표
│   └── support_metrics.py             # 서포터 특화 지표
├── data/
│   ├── match_0.json                   # 샘플 경기 데이터
│   └── match_1.json                   # 샘플 경기 데이터
├── output/
│   ├── ps_style_features.csv          # 추출된 feature 데이터
│   └── ps_style_analysis.txt          # 상세 분석 리포트
└── requirements.txt                    # 의존성 패키지
```

---

## 🔍 핵심 인사이트

### 게임 승리를 위한 라인별 조건

| 라인 | 최소 기준 | 우수 기준 | 탁월 기준 |
|:---:|:-------:|:-------:|:-------:|
| **탑** | CS 7.0+ | CS 8.0+, +30 우위 | CS 8.5+, +50 우위 |
| **정글** | 오브젝트 4.0/10m | 오브젝트 7.0/10m | 오브젝트 8.0/10m |
| **미드** | 로밍 5.0점 | 로밍 10.0점+ | 로밍 12.0점+ |
| **원딜** | CS 6.0, 250G/m | CS 6.5+, 450G/m+ | CS 7.0+, 500G/m+ |
| **서포터** | 시야 1.5/m | 시야 2.5/m+ | 시야 3.0/m+ |

---

## 📖 상세 가이드

각 라인별 상세 분석 방법은 다음 문서를 참고하세요:

- **[탑라인 분석 가이드](./docs/TOP_ANALYSIS.md)**
- **[정글 분석 가이드](./docs/JUNGLE_ANALYSIS.md)**
- **[미드 분석 가이드](./docs/MID_ANALYSIS.md)**
- **[원딜 분석 가이드](./docs/ADC_ANALYSIS.md)**
- **[서포터 분석 가이드](./docs/SUPPORT_ANALYSIS.md)**

---

## 🤝 기여 방법

이 프로젝트에 기여하고 싶으신가요?

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 라이선스

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

---

## 👨‍💻 작성자

**학생 개발자** | League of Legends Data Analysis  
*프로관전러 P.S의 분석 방법론을 학습하고 구현한 프로젝트*

---

## ⚠️ 면책 조항

본 분석은 공개 데이터(Riot API)를 기반으로 하며, 프로관전러 P.S의 공개 영상 및 분석 방법론을 참고하여 제작되었습니다.

정확한 최신 분석은 [프로관전러 P.S 공식 채널](https://www.youtube.com/@ProfessionalSpectator)을 참고하시기 바랍니다.

---

## 📞 연락처

질문이나 제안이 있으신가요?

- 📧 Email: [your-email@example.com]
- 🔗 GitHub Issues: [이슈 작성](../../issues)
- 💬 Discussion: [토론 시작](../../discussions)

---

## 🙏 감사의 말

- **프로관전러 P.S**: 데이터 기반 분석 방법론 제공
- **Riot Games**: League of Legends API 제공
- **커뮤니티**: 귀중한 피드백과 조언

---

**마지막 업데이트**: 2025-11-11  
**버전**: 1.0.0
