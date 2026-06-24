# Korean Transformer Chatbot

## 프로젝트 개요
송영숙님의 한국어 챗봇 데이터를 활용하여 Transformer 기반 한국어 챗봇을 구현하였다.

영어 챗봇 실습에서 사용했던 Transformer Encoder-Decoder 구조를 한국어 질문-답변 데이터에 적용하고, 형태소 분석기 대신 SentencePiece 기반 서브워드 토크나이저를 사용하여 한국어 입력 문장에 대해 한국어 답변을 생성하는 것을 목표로 하였다.

## 진행 과정
1. 한국어 챗봇 데이터 로드
2. 결측치 및 데이터 길이 확인
3. 한국어 전처리 함수 구현
4. SentencePiece 기반 서브워드 토크나이저 학습
5. Dataset / DataLoader 구성
6. Transformer Encoder-Decoder 모델 구현
7. 모델 학습 및 checkpoint 저장
8. 한국어 입력 문장에 대한 답변 생성

## 주요 결과
- 전체 데이터 수: 11,823개
- 입력 데이터: Q
- 정답 데이터: A
- SentencePiece vocabulary size: 8,000
- 모델 구조: Transformer Encoder-Decoder
- 학습 방식: teacher forcing
- 평가 방식: token-level accuracy 확인 및 생성 문장 테스트

## 결과 해석
추가 학습 결과 training loss는 감소하고 token-level accuracy는 상승하였다. 이는 모델이 학습 데이터의 질문-답변 패턴을 학습하고 있음을 보여준다.

다만 별도의 validation set을 구성하지 않았기 때문에 높은 training accuracy가 곧 일반화 성능을 의미하지는 않는다. 또한 일부 입력에 대해서는 학습 데이터에서 자주 등장하는 일반적인 답변이 반복되는 경향이 있었다.

## 한계 및 개선 방향
- train/validation split 적용 필요
- validation loss 기준 best checkpoint 저장 필요
- beam search 또는 sampling decoding 적용 가능
- 더 다양한 한국어 대화 데이터 추가 필요
- label 정보를 활용한 감정 기반 응답 제어 가능

## 회고
이번 실습을 통해 한국어 데이터 전처리, SentencePiece 토크나이징, Transformer Encoder-Decoder 모델 학습, checkpoint 저장, 예측 함수 구현까지 전체 흐름을 경험하였다.

특히 한국어 데이터는 영어 데이터와 달리 한글, 자음/모음, 영어 약어, 숫자, 구두점이 섞여 있기 때문에 한글 정보를 보존하는 전처리가 중요했다. 또한 생성형 챗봇의 token-level accuracy와 실제 답변 품질은 다를 수 있음을 확인하였다.
