# News Summarization Project

## 1. 프로젝트 개요

뉴스 기사 본문을 입력받아 짧은 headline 형태의 요약문을 생성하는 텍스트 요약 프로젝트입니다. 본 프로젝트에서는 Attention 기반 seq2seq 모델을 활용한 추상적 요약과 Summa 패키지를 활용한 추출적 요약을 함께 수행하고, 두 방식의 결과를 비교하였습니다.

## 2. 사용 데이터

* 데이터셋: `news_summary_more.csv`
* 주요 컬럼

  * `text`: 뉴스 기사 본문
  * `headlines`: 정답 요약문
* 전체 샘플 수: 98,401개
* 중복/결측 제거 후 샘플 수: 98,360개
* 길이 필터링 후 샘플 수: 98,357개

## 3. 전처리 과정

다음과 같은 전처리를 수행했습니다.

* 소문자화
* HTML 태그 제거
* 괄호 안 문자열 제거
* 특수문자 제거
* 축약어 정규화
* 본문 불용어 제거
* 빈 샘플 제거
* SOS/EOS 토큰 추가
* 정수 인코딩
* 패딩

본문 `text`에는 불용어 제거를 적용했고, 정답 요약문 `headlines`는 자연스러운 문장 생성을 위해 불용어를 제거하지 않았습니다.

## 4. 데이터 구성

* `text_max_len`: 60
* `summary_max_len`: 15
* 훈련 데이터: 78,686개
* 테스트 데이터: 19,671개
* 본문 단어장 크기: 10,000
* 요약문 단어장 크기: 5,000

최종 텐서 크기는 다음과 같습니다.

```text
encoder_input_train:  [78686, 60]
decoder_input_train:  [78686, 15]
decoder_target_train: [78686, 15]

encoder_input_test:   [19671, 60]
decoder_input_test:   [19671, 15]
decoder_target_test:  [19671, 15]
```

## 5. 모델 구조

최종 모델은 Attention 기반 seq2seq 모델입니다.

* Encoder: LSTM
* Decoder: LSTM
* Attention: Dot-product Attention
* PAD masking 적용
* Optimizer: AdamW
* Loss: CrossEntropyLoss
* Early Stopping 적용
* Gradient Clipping 적용

개선 모델에서는 LSTM layer 수를 2층으로 설정하고, dropout을 0.2로 조정했습니다. 또한 Attention 계산 시 PAD 토큰을 참조하지 않도록 masking을 적용했습니다.

## 6. 학습 결과

* Epochs: 50
* Batch size: 256
* Learning rate: 0.0005
* Patience: 5
* Best Val Loss: 3.9139
* Best Val Epoch: 25
* Early Stopping: epoch 30

Validation loss는 epoch 25까지 감소했으며, 이후 더 이상 개선되지 않아 epoch 30에서 Early Stopping이 작동했습니다.

## 7. 결과 분석

추상적 요약은 원문에 없는 새로운 요약문을 생성할 수 있다는 장점이 있습니다. 그러나 이번 실습에서는 `<UNK>` 토큰과 문법적으로 부자연스러운 표현이 일부 나타났습니다. 이는 제한된 vocabulary 크기, greedy decoding 방식, LSTM 기반 seq2seq 구조의 한계 때문으로 해석됩니다.

추출적 요약은 Summa 패키지를 사용하여 수행했습니다. 원문 문장을 그대로 선택하기 때문에 문법적으로 안정적이고 핵심 단어 보존 측면에서 장점이 있었습니다. 다만 headline처럼 짧고 압축적인 표현을 새롭게 생성하지는 못했습니다.

## 8. 회고 및 개선 방향

이번 프로젝트를 통해 텍스트 요약의 두 가지 접근 방식인 추출적 요약과 추상적 요약의 차이를 확인했습니다. Attention 기반 seq2seq 모델을 직접 구현하면서 Encoder, Decoder, Attention, SOS/EOS 토큰, 정수 인코딩, 패딩, Early Stopping의 역할을 이해할 수 있었습니다.

향후 개선 방향은 다음과 같습니다.

* vocabulary 크기 조정
* beam search 적용
* pre-trained embedding 사용
* Transformer 기반 모델 적용
* 더 긴 epoch와 다양한 하이퍼파라미터 실험
