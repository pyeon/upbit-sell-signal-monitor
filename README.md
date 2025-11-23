# 🔴 업비트 매도 신호 모니터링 시스템 v2.0

## 🆕 v2.0 주요 개선사항

### 1️⃣ 10분 급락도 감지 가능!
```
v1.0: 60분봉만 사용 → 10분 급락 놓침
v2.0: 10분봉 + 60분봉 혼합 → 10분 급락도 포착!
```

**실제 예시 (NOM 코인)**
```
14:00 - 10,000원
14:30 - 12,000원 (+20% 급등)
14:40 - 10,500원 (-12.5% 급락)

v1.0: 못 잡음 (60분봉에서는 +5%로만 보임)
v2.0: ✅ 감지! "단기급락 10분전 -12.5%"
```

### 2️⃣ Config에서 모든 설정 조정 가능
- 타임프레임 (어느 정도 빠른 변동을 잡을지)
- 급락 기준 (몇 % 하락을 급락으로 볼지)
- 신호 민감도 (알림 많게 vs 적게)
- 상세한 주석으로 설명

### 3️⃣ 30분마다 자동 실행
```
v1.0: 2시간마다 → 최대 2시간 지연
v2.0: 30분마다 → 최대 30분 지연
```

### 4️⃣ 10개 지표로 확장
```
v1.0: 9개 지표
v2.0: 10개 지표 (단기 급락 감지 추가)
```

---

## 📋 10가지 분석 지표

### 가격 패턴 분석 (4개)
1. ✅ **단기 급락** (10분봉) - NEW!
   - 최근 2시간 내 고점 대비 5% 이상 하락
   - "10분 전 12,000원 → 지금 11,400원 = -5%" 

2. ✅ **중기 고점 대비 하락** (60분봉)
   - 12시간 내 최고가 대비 8% 이상 하락

3. ✅ **급등 후 하락 전환**
   - 6시간 동안 15% 급등 후 1시간에 3% 하락

4. ✅ **고변동성**
   - 최근 1시간 평균 변동률 3% 이상

### 거래량 분석 (2개)
5. ✅ 거래량 3일 연속 감소
6. ✅ 약세 다이버전스 (가격↑ 거래량↓)

### 호가창 분석 (1개)
7. ✅ 매도벽 우세 (비율 1.5 이상)

### 기술적 지표 (3개)
8. ✅ RSI 과매수 (70 이상)
9. ✅ MACD 데드크로스
10. ✅ 볼린저 상단 이탈

---

## 🚀 설치 및 설정

### 1. 저장소 클론
```bash
git clone https://github.com/YOUR_USERNAME/upbit-sell-signal-monitor.git
cd upbit-sell-signal-monitor
```

### 2. 필수 패키지 설치
```bash
pip install pyupbit pandas numpy requests ta openpyxl pytz
```

### 3. 설정 파일 생성
```bash
cp config_v2.example.py config.py
```

`config.py` 파일을 열어 다음 정보 입력:
```python
BOT_TOKEN = "YOUR_BOT_TOKEN"  # 텔레그램 봇 토큰
CHAT_ID = "YOUR_CHAT_ID"      # 텔레그램 채팅방 ID
```

### 4. 설정 커스터마이징 (선택)

#### 📌 급락 감지 속도 조절
```python
# 🔥 초단타 (10분 급락도 즉시 감지, 알림 많음)
QUICK_DROP_LOOKBACK = 6       # 1시간만 봄
QUICK_DROP_THRESHOLD = 3.0    # 3% 급락도 감지

# ⚡ 단타 (30분 급락 감지, 균형) - 권장
QUICK_DROP_LOOKBACK = 12      # 2시간
QUICK_DROP_THRESHOLD = 5.0    # 5% 급락 감지

# 🐢 스윙 (큰 급락만, 알림 적음)
QUICK_DROP_LOOKBACK = 18      # 3시간
QUICK_DROP_THRESHOLD = 8.0    # 8% 급락만 감지
```

#### 📌 신호 민감도 조절
```python
# 공격적 (알림 많음)
SELL_STAGE_REVIEW = 2      # 2개 지표만 있어도 알림
SELL_STAGE_PREPARE = 4
SELL_STAGE_IMMEDIATE = 6

# 균형 (권장)
SELL_STAGE_REVIEW = 3      # 현재 기본값
SELL_STAGE_PREPARE = 5
SELL_STAGE_IMMEDIATE = 7

# 보수적 (확실한 신호만)
SELL_STAGE_REVIEW = 4
SELL_STAGE_PREPARE = 6
SELL_STAGE_IMMEDIATE = 8
```

상세한 설정은 `config_v2.example.py` 파일의 주석을 참고하세요!

---

## 💻 사용 방법

### 로컬 실행
```bash
python upbit_sell_signal_monitor_v2.py
```

### GitHub Actions 자동 실행

#### 1️⃣ GitHub Secrets 설정
- `TELEGRAM_BOT_TOKEN`: 텔레그램 봇 토큰
- `TELEGRAM_CHAT_ID`: 채팅방 ID

#### 2️⃣ 실행 주기 선택

**옵션 A: 30분마다 (권장)**
```yaml
- cron: '*/30 * * * *'
```
- 감지 속도: ⚡⚡⚡⚡⚡ (최대 30분 지연)
- 비용: Public Repo 무료 / Private Repo ~$7/월

**옵션 B: 1시간마다 (무료)**
```yaml
- cron: '0 * * * *'
```
- 감지 속도: ⚡⚡⚡ (최대 1시간 지연)
- 비용: 완전 무료

**옵션 C: 2시간마다 (v1.0)**
```yaml
- cron: '0 */2 * * *'
```
- 감지 속도: ⚡⚡ (최대 2시간 지연)
- 비용: 완전 무료

---

## 💰 GitHub Actions 비용

### Public Repository
```
✅ 완전 무료, 무제한!
어떤 주기로 실행해도 비용 없음
```

### Private Repository
| 실행 주기 | 월 사용 시간 | 비용 |
|----------|-------------|------|
| 2시간마다 | 720분 | ✅ 무료 |
| 1시간마다 | 1,440분 | ✅ 무료 |
| 30분마다 | 2,880분 | ❌ ~$7/월 |

**💡 Tip**: Public Repo로 만들면 30분마다도 완전 무료!

---

## 📊 알림 메시지 예시

### 🔴 즉시매도 신호
```
🔴 [BTC] 즉시매도 ⭐⭐⭐⭐⭐
━━━━━━━━━━━━━━━━━━━━━
💰 현재가: 89,500,000원
🎯 권장행동: 즉시 매도 권장

【 가격 패턴 분석 】
🚨 단기급락: 40분 전 고점(92,000,000원) 대비 -2.7%
📉 12시간 고점(95,000,000원) 대비: -5.8%
📈 6시간 변화: +18.5%
⚠️ 1시간 변화: -4.2%
⚡ 변동성: 3.8% (최근 6개 봉)

【 거래량 분석 】
⚠️ 거래량: 3일 연속 감소 ▶ 상승동력 약화
⚡ 약세 다이버전스:
   └ 가격 +15.2%, 거래량 -18.5%
   └ 가격상승에도 거래량 감소 ▶ 매도 신호

【 기술적 지표 】
✅ RSI: 78.5 → 과매수
✅ MACD: 데드크로스
✅ 볼린저: 상단이탈 (95%)

━━━━━━━━━━━━━━━━━━━━━
🎯 종합판단: 8/10 지표 일치
⏰ 발생시각(KST): 2025-01-15 14:35:20
```

---

## 🎮 실전 시나리오별 설정 가이드

### 시나리오 1: "NOM처럼 10분 급락 절대 못 놓친다"
```python
QUICK_DROP_LOOKBACK = 6       # 1시간만 봄
QUICK_DROP_THRESHOLD = 4.0    # 4% 급락도 잡음
SELL_STAGE_REVIEW = 2         # 2개 지표면 알림
MIN_QUICK_DROP = 3.0          # 3% 이상만 분석
```
**결과**: 알림 많지만 급락 놓치지 않음

### 시나리오 2: "거짓 신호 싫어, 확실한 것만"
```python
QUICK_DROP_LOOKBACK = 18      # 3시간 봄
QUICK_DROP_THRESHOLD = 8.0    # 8% 급락만
SELL_STAGE_IMMEDIATE = 8      # 8개 지표 필요
MIN_QUICK_DROP = 5.0          # 5% 이상만 분석
```
**결과**: 알림 적고 정확도 높음

### 시나리오 3: "균형 잡힌 설정, 실전용" (기본값)
```python
QUICK_DROP_LOOKBACK = 12      # 2시간
QUICK_DROP_THRESHOLD = 5.0    # 5% 급락
SELL_STAGE_REVIEW = 3         # 3/5/7 단계
```
**결과**: 적절한 알림, 중요한 신호 놓치지 않음

---

## 📈 엑셀 리포트

자동으로 `upbit_sell_signals_v2.xlsx` 파일 생성:
- 시간(KST), 코인명, 매도단계
- **단기급락** (NEW!) - 몇 분 전, 몇 % 하락
- 12시간 고점 대비 하락률
- 거래량 분석, 기술적 지표
- 최근 100개 신호 유지

---

## 🆚 v1.0 vs v2.0 비교

| 항목 | v1.0 | v2.0 |
|------|------|------|
| **데이터** | 60분봉 + 일봉 | 10분봉 + 60분봉 + 일봉 |
| **급락 감지** | 1시간 이상 변동만 | 10분 급락도 감지 |
| **지표 개수** | 9개 | 10개 |
| **실행 주기** | 2시간 | 30분 (권장) |
| **최대 지연** | 2시간 | 30분 |
| **설정 조정** | 제한적 | 모든 파라미터 조정 가능 |
| **Config 가이드** | 간단 | 상세한 설명 |

---

## 🔗 관련 시스템

- **매수 신호 시스템**: [upbit-buy-signal-monitor](../upbit-buy-signal-monitor)
  - 동일한 텔레그램 그룹 사용
  - 매수/매도 신호를 함께 모니터링

---

## ⚠️ 주의사항

1. **투자 판단의 참고 자료**일 뿐, 최종 결정은 본인의 책임
2. 30분 주기로 실행하면 최대 30분 지연 가능
3. Private Repo에서 30분 주기는 약간의 비용 발생 (~$7/월)
4. Public Repo는 모든 주기에서 완전 무료
5. Config 설정에 따라 알림 빈도가 크게 달라집니다

---

## 🎯 추천 설정

### 일반 투자자 (균형)
```python
QUICK_DROP_LOOKBACK = 12
QUICK_DROP_THRESHOLD = 5.0
SELL_STAGE_REVIEW = 3
# + GitHub Actions 30분마다
```

### 데이트레이더 (민감)
```python
QUICK_DROP_LOOKBACK = 6
QUICK_DROP_THRESHOLD = 3.0
SELL_STAGE_REVIEW = 2
# + GitHub Actions 30분마다
```

### 장기 투자자 (보수)
```python
QUICK_DROP_LOOKBACK = 18
QUICK_DROP_THRESHOLD = 8.0
SELL_STAGE_IMMEDIATE = 8
# + GitHub Actions 1시간마다
```

---

## 📝 라이선스

MIT License

## 💬 문의 및 개선 제안

이슈나 PR은 언제든 환영합니다!

---

## 🙏 v1.0에서 업그레이드 하기

### 1. 파일 교체
```bash
# 기존 파일 백업
mv upbit_sell_signal_monitor.py upbit_sell_signal_monitor_v1_backup.py

# v2.0 파일 사용
cp upbit_sell_signal_monitor_v2.py upbit_sell_signal_monitor.py
```

### 2. Config 업데이트
```bash
# v2.0 config 복사
cp config_v2.example.py config.py
# 기존 BOT_TOKEN과 CHAT_ID 입력
```

### 3. GitHub Actions 업데이트
```bash
# .github/workflows/sell_signal_monitor.yml 업데이트
# 30분 주기로 변경 (선택)
```

### 4. 테스트 실행
```bash
python upbit_sell_signal_monitor_v2.py
```

완료! 🎉
