# ResNet Ablation Study: ResNet vs PlainNet

## 1. 프로젝트 개요

본 프로젝트는 ResNet의 핵심 구조인 residual connection, 즉 skip connection이 깊은 CNN 모델의 학습 성능에 어떤 영향을 주는지 확인하기 위한 Ablation Study 실습이다.

ResNet 모델과 skip connection을 제거한 PlainNet 모델을 직접 구현하고, 동일한 조건에서 학습하여 validation accuracy와 loss를 비교하였다.

## 2. 실험 목적

- ResNet-34와 ResNet-50 구조를 직접 구현한다.
- skip connection이 제거된 PlainNet-34와 PlainNet-50을 구현한다.
- ResNet과 PlainNet을 동일한 조건에서 학습하여 residual connection의 효과를 비교한다.
- 학습 과정에서 loss 감소 여부와 validation accuracy 차이를 확인한다.

## 3. 구현 모델

| 모델 | 사용 블록 | Stage 반복 수 | Skip Connection |
|---|---|---|---|
| ResNet-34 | BasicBlock | [3, 4, 6, 3] | 사용 |
| PlainNet-34 | BasicBlock | [3, 4, 6, 3] | 미사용 |
| ResNet-50 | Bottleneck | [3, 4, 6, 3] | 사용 |
| PlainNet-50 | Bottleneck | [3, 4, 6, 3] | 미사용 |

## 4. 모델 구조 확인 결과

| 모델 | 최종 출력 | 총 파라미터 수 |
|---|---|---:|
| ResNet-34 | [1, 1000] | 21,797,672 |
| PlainNet-34 | [1, 1000] | 21,797,672 |
| ResNet-50 | [1, 1000] | 25,557,032 |
| PlainNet-50 | [1, 1000] | 25,557,032 |

## 5. 학습 데이터셋

- Dataset: CIFAR-10
- 입력 크기: 224 × 224
- 학습 데이터: CIFAR-10 train subset 5,000장
- 검증 데이터: CIFAR-10 test subset 1,000장
- Batch size: 32
- Epoch: 2
- Loss function: CrossEntropyLoss
- Optimizer: SGD

## 6. Ablation Study 결과

| 모델 | Skip Connection | Epoch 1 Train Loss | Epoch 2 Train Loss | Epoch 1 Val Loss | Epoch 2 Val Loss | Epoch 1 Val Acc | Epoch 2 Val Acc |
|---|---|---:|---:|---:|---:|---:|---:|
| ResNet-34 | 사용 | 1.9213 | 1.5931 | 2.0564 | 2.0490 | 32.10% | 34.20% |
| PlainNet-34 | 미사용 | 2.1978 | 1.9268 | 2.0437 | 1.8709 | 25.40% | 29.30% |

## 7. 결과 해석

두 모델 모두 epoch이 진행되면서 train loss가 감소하였다.  
ResNet-34의 train loss는 1.9213에서 1.5931로 감소하였고, PlainNet-34의 train loss는 2.1978에서 1.9268로 감소하였다.

최종 epoch 기준 validation accuracy는 ResNet-34가 34.20%, PlainNet-34가 29.30%로 나타났다.  
동일한 BasicBlock 반복 구조에서 skip connection을 사용하는 ResNet-34가 skip connection을 제거한 PlainNet-34보다 더 높은 검증 정확도를 보였다.

이를 통해 residual connection이 깊은 CNN 모델의 학습 안정성과 성능 향상에 긍정적인 영향을 줄 수 있음을 확인하였다.

## 8. 한계점

본 실험은 시간과 자원 제약으로 CIFAR-10 전체 데이터셋이 아닌 부분 데이터셋을 사용하였고, epoch도 2로 제한하였다.  
따라서 더 정확한 성능 비교를 위해서는 전체 데이터셋과 더 많은 epoch에서 추가 실험이 필요하다.

## 9. 실행 파일

- `ablation_study_resnet_plainnet.ipynb`
