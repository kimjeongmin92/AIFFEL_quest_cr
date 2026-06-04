# ResNet Ablation Study: ResNet vs PlainNet

## 1. 프로젝트 개요

본 프로젝트는 ResNet의 핵심 구조인 residual connection, 즉 skip connection이 깊은 CNN 모델의 학습 성능에 어떤 영향을 주는지 확인하기 위한 Ablation Study 실습이다.

ResNet 모델과 skip connection을 제거한 PlainNet 모델을 직접 구현하고, 동일한 조건에서 학습하여 validation accuracy와 loss를 비교하였다.

## 2. 실험 목적

* ResNet-34와 ResNet-50 구조를 직접 구현한다.
* skip connection이 제거된 PlainNet-34와 PlainNet-50을 구현한다.
* ResNet과 PlainNet을 동일한 조건에서 학습하여 residual connection의 효과를 비교한다.
* 학습 과정에서 loss 감소 여부와 validation accuracy 차이를 확인한다.

## 3. 구현 모델

| 모델          | 사용 블록      | Stage 반복 수   | Skip Connection |
| ----------- | ---------- | ------------ | --------------- |
| ResNet-34   | BasicBlock | [3, 4, 6, 3] | 사용              |
| PlainNet-34 | BasicBlock | [3, 4, 6, 3] | 미사용             |
| ResNet-50   | Bottleneck | [3, 4, 6, 3] | 사용              |
| PlainNet-50 | Bottleneck | [3, 4, 6, 3] | 미사용             |

## 4. 모델 구조 확인 결과

| 모델          | 최종 출력     |   총 파라미터 수 |
| ----------- | --------- | ---------: |
| ResNet-34   | [1, 1000] | 21,797,672 |
| PlainNet-34 | [1, 1000] | 21,797,672 |
| ResNet-50   | [1, 1000] | 25,557,032 |
| PlainNet-50 | [1, 1000] | 25,557,032 |

## 5. 학습 데이터셋

* Dataset: CIFAR-10
* 입력 크기: 224 × 224
* 학습 데이터: CIFAR-10 train subset 5,000장
* 검증 데이터: CIFAR-10 test subset 1,000장
* Batch size: 32
* Epoch: 2
* Loss function: CrossEntropyLoss
* Optimizer: SGD

## 6. Ablation Study 결과

본 실험에서는 ResNet-34와 PlainNet-34를 동일한 조건에서 학습시켜 residual connection의 효과를 비교하였다.
두 모델은 모두 BasicBlock을 `[3, 4, 6, 3]`개씩 반복하는 동일한 구조를 가지며, 차이는 skip connection의 사용 여부이다.

### 6-1. 학습 결과 비교

| 모델          | Skip Connection | Epoch 1 Train Loss | Epoch 2 Train Loss | Epoch 1 Val Loss | Epoch 2 Val Loss | Epoch 1 Val Acc | Epoch 2 Val Acc |
| ----------- | --------------- | -----------------: | -----------------: | ---------------: | ---------------: | --------------: | --------------: |
| ResNet-34   | 사용              |             1.9213 |             1.5931 |           2.0564 |           2.0490 |          32.10% |          34.20% |
| PlainNet-34 | 미사용             |             2.1978 |             1.9268 |           2.0437 |           1.8709 |          25.40% |          29.30% |

### 6-2. Loss 변화

ResNet-34의 train loss는 `1.9213 → 1.5931`로 감소하였다.
PlainNet-34의 train loss는 `2.1978 → 1.9268`로 감소하였다.

따라서 두 모델 모두 epoch이 진행되면서 학습 손실이 감소하였고, 이미지 분류 학습이 정상적으로 진행되었음을 확인할 수 있다.

### 6-3. Validation Accuracy 변화

ResNet-34의 validation accuracy는 `32.10% → 34.20%`로 증가하였다.
PlainNet-34의 validation accuracy는 `25.40% → 29.30%`로 증가하였다.

최종 epoch 기준으로 ResNet-34는 `34.20%`, PlainNet-34는 `29.30%`의 validation accuracy를 보였다.
동일한 BasicBlock 반복 구조에서 skip connection을 사용하는 ResNet-34가 skip connection을 제거한 PlainNet-34보다 더 높은 검증 정확도를 나타냈다.

## 7. 결과 해석

본 Ablation Study의 목적은 residual connection이 깊은 CNN 모델의 학습 성능에 어떤 영향을 주는지 확인하는 것이다.

실험 결과, ResNet-34와 PlainNet-34 모두 학습이 진행되면서 train loss가 감소하였다.
이는 두 모델 모두 주어진 CIFAR-10 부분 데이터셋에 대해 정상적으로 학습되었음을 의미한다.

그러나 validation accuracy를 비교했을 때 ResNet-34가 PlainNet-34보다 더 높은 성능을 보였다.
이는 skip connection이 깊은 네트워크에서 gradient 흐름을 도와 학습을 더 안정적으로 만들고, 검증 성능 향상에도 긍정적인 영향을 줄 수 있음을 보여준다.

따라서 본 실험을 통해 ResNet의 핵심 아이디어인 residual connection이 깊은 CNN 모델의 학습 안정성과 성능 개선에 기여할 수 있음을 확인하였다.

## 8. 한계점

본 실험은 시간과 자원 제약으로 CIFAR-10 전체 데이터셋이 아닌 부분 데이터셋을 사용하였다.
또한 epoch 수를 2로 제한했기 때문에, 실험 결과는 모델의 최종 성능이라기보다 구조 차이에 따른 초기 학습 경향을 확인하는 데 의미가 있다.

더 정확한 비교를 위해서는 전체 데이터셋을 사용하고, 더 많은 epoch 동안 학습을 진행할 필요가 있다.
또한 ResNet-50과 PlainNet-50에 대해서도 동일한 학습 비교를 수행하면 residual connection의 효과를 더 깊은 모델에서도 확인할 수 있다.

## 9. 실행 파일

* `ablation_study_resnet_plainnet.ipynb`
