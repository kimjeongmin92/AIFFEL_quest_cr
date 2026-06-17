# NSMC Sentiment Analysis with SentencePiece

## 1. 프로젝트 개요

본 프로젝트는 네이버 영화리뷰 감성분석 데이터셋(NSMC)을 활용하여 한국어 감성분석 모델을 구현한 실습이다.

기존 한국어 자연어처리에서는 형태소 분석기 기반 토큰화가 자주 사용되지만, 본 실습에서는 Google의 SentencePiece를 사용하여 subword 기반 토크나이저를 직접 학습하고, 이를 감성분석 모델에 적용하였다.

최종적으로 SentencePiece로 인코딩한 리뷰 데이터를 BiLSTM 모델에 입력하여 긍정/부정 이진 분류를 수행하였다.

---

## 2. 프로젝트 목표

- NSMC 데이터셋을 다운로드하고 전처리한다.
- SentencePiece 모델을 직접 학습한다.
- 학습된 SentencePiece 모델로 리뷰 문장을 subword ID로 변환한다.
- PyTorch Dataset과 DataLoader를 구성한다.
- BiLSTM 감성분석 모델을 학습한다.
- SentencePiece와 Okt의 토큰화 결과를 비교한다.
- 실험 결과와 한계를 정리한다.

---

## 3. 사용 데이터

- 데이터셋: Naver Sentiment Movie Corpus, NSMC
- 파일:
  - `ratings_train.txt`
  - `ratings_test.txt`
- 라벨:
  - `0`: 부정 리뷰
  - `1`: 긍정 리뷰

---

## 4. 주요 실습 과정

### 4.1 데이터 전처리

NSMC 데이터를 불러온 뒤 결측치와 중복 리뷰를 제거하였다.

전처리 과정은 다음과 같다.

1. train/test 데이터 불러오기
2. 결측치 확인 및 제거
3. `document` 컬럼 기준 중복 제거
4. 라벨 분포 확인
5. SentencePiece 학습용 corpus 파일 생성

---

### 4.2 SentencePiece 모델 학습

NSMC train 리뷰 문장을 이용하여 SentencePiece 모델을 직접 학습하였다.

| 항목 | 설정 |
|---|---|
| model_type | unigram |
| vocab_size | 8000 |
| pad_id | 0 |
| unk_id | 1 |
| bos_id | 2 |
| eos_id | 3 |

---

### 4.3 감성분석 모델 학습

SentencePiece로 인코딩된 리뷰 데이터를 BiLSTM 모델에 입력하여 감성분석 모델을 학습하였다.

| 항목 | 설정 |
|---|---|
| Tokenizer | SentencePiece |
| model_type | unigram |
| vocab_size | 8000 |
| MAX_LEN | 80 |
| Model | BiLSTM |
| Epoch | 3 |

---

## 5. 실험 결과

| Epoch | Train Loss | Train Acc | Test Loss | Test Acc |
|---:|---:|---:|---:|---:|
| 1 | 0.6080 | 0.6562 | 0.5620 | 0.7079 |
| 2 | 0.4361 | 0.7990 | 0.4529 | 0.7916 |
| 3 | 0.3301 | 0.8589 | 0.4301 | 0.8065 |

최종 Test Accuracy는 약 **0.8065**로 확인되었다.

---

## 6. SentencePiece와 Okt 비교

SentencePiece와 Okt는 같은 한국어 문장을 서로 다른 방식으로 토큰화하였다.

SentencePiece는 데이터에서 자주 등장하는 subword 조각을 기준으로 문장을 나눈다. 따라서 형태소 분석 결과처럼 사람이 직관적으로 이해하기 어려운 조각이 나올 수 있지만, OOV 문제에 강하고 vocab size를 직접 제어할 수 있다는 장점이 있다.

반면 Okt는 한국어 형태소 분석기이기 때문에 조사, 어미, 명사, 형용사 등을 비교적 직관적인 단위로 나눌 수 있다. 그러나 Java/JVM 환경에 의존하고, 대규모 데이터 처리에서는 속도와 환경 설정 문제가 발생할 수 있다.

---

## 7. Trouble Shooting

### 7.1 Okt 실행 오류

Okt 실행 시 다음 오류가 발생하였다.

```text
JVMNotFoundException: No JVM shared library file (libjvm.so) found.
