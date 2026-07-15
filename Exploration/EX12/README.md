# Transformer 기반 한국어 챗봇

## 프로젝트 개요

한국어 질문-답변 데이터를 전처리하고 MeCab 형태소 분석을 적용한 뒤,
Word2Vec 기반 Lexical Substitution으로 데이터를 증강하였다.

증강된 데이터를 이용해 Transformer Encoder-Decoder 챗봇을 구현하였다.

## 주요 결과

| 항목 | 결과 |
|---|---:|
| 원본 데이터 | 11,823개 |
| 중복 제거 후 | 11,750개 |
| 최종 증강 데이터 | 32,819개 |
| 결측값 | 0개 |

## 주요 과정

1. 데이터 중복 및 결측값 확인
2. 문장 정제
3. MeCab 형태소 분석
4. Word2Vec 기반 데이터 증강
5. Train/Validation 그룹 분리
6. Transformer 모델 구현
7. 모델 학습 및 회고
