# Image Data Augmentation 실습

## 프로젝트 개요
Stanford Dogs Dataset을 활용하여 PyTorch 기반 Image Augmentation을 적용하고, ResNet-50 모델의 성능 변화를 비교한 실습입니다.

## 실습 내용
- Stanford Dogs Dataset 불러오기
- 기본 전처리 적용
- RandomResizedCrop, RandomHorizontalFlip, ColorJitter 적용
- No Augmentation / With Augmentation 성능 비교
- CutMix 구현 및 학습 테스트
- Mixup 구현 및 학습 테스트
- soft label과 soft_cross_entropy 정리

## 주요 결과

| 실험 구분 | Epoch | Train Accuracy | Validation Accuracy |
|---|---:|---:|---:|
| No Augmentation | 3 | 83.01% | 81.87% |
| With Augmentation | 3 | 34.90% | 50.89% |
| CutMix | 1 | 2.12% | 6.61% |
| Mixup | 1 | 2.68% | 9.68% |

## 결과 해석
짧은 epoch에서는 augmentation을 적용한 모델의 성능이 낮게 나타날 수 있습니다. 이는 augmentation이 학습 데이터를 더 다양하고 어렵게 만들기 때문입니다.

CutMix와 Mixup은 이미지뿐 아니라 라벨까지 섞는 심화 augmentation 기법이며, soft label을 처리하기 위해 soft_cross_entropy를 사용했습니다.

정확한 성능 비교를 위해서는 모든 실험을 동일한 epoch 수와 조건에서 반복 수행할 필요가 있습니다.

## 파일 구성
- `pytorch_data_augmentation.ipynb`: 전체 실습 노트북
