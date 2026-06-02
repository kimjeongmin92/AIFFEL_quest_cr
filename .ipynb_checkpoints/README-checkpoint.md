# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김정민
- 리뷰어 : 유비


# PRT(Peer Review Template)
- [ ]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 최종 결과물이 코드와 사진 모두 잘 첨부되었고 실행에 에러가 없음
        - [사진](fork_test/AIFFEL_quest_cr/results/ex1.png)
    
- [ ]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - img_blur = cv2.GaussianBlur(img_orig, (51, 51), 0)

      img_blur_rgb = cv2.cvtColor(img_blur, cv2.COLOR_BGR2RGB)

      plt.imshow(img_blur_rgb)
      plt.title("Blurred Image")
      plt.axis("off")
      plt.show()
    - 가우시안 블러를 사용해 피사체의 경계를 구분하는 코드 블럭이기 때문에 중요하고 생각함
    - 해당 코드 블럭에 doc string/annotation이 달려 있지 않음
    - 해당 코드의 기능, 존재 이유, 작동 원리 등을 기술하지 않음
    - 주석은 작성되어 있지 않으나 코드를 설명할 때 어떤 단계를 실행하는 코드인지 인지하고 있었음
        
- [ ]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - 디버깅 관련 문제가 보이진 않지만 히잡을 쓴 고양이 사진을 사용하여 고양이"만" 인식했기에 히잡 쓴 부분이 전부 날아간 것을 인지했음
        
- [ ]  **4. 회고를 잘 작성했나요?**
    - 완성된 코드 프로젝트 결과물에 대해 아쉬운점, 느낀점, 문제점 등이 잘 기록되어 있음
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - [사진](fork_test/AIFFEL_quest_cr/results/ex1.png)
        - [사진](fork_test/AIFFEL_quest_cr/results/ex2.png)
        
- [ ]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였지만 반복되는 패턴으로 코드 중복이 이루어지고 있음
    - 함수화/모듈화 또한 되어 있지 않기에 개선점이 보여짐


# 회고(참고 링크 및 코드 개선)
```
- 리뷰어인 본인도, 코더도 높은 이해도를 가지고 있는 건 아니다 보니 코드 설명과 이해하는 부분에서 서로 어려움을 많이 겪었다.
- plt.figure(figsize=(15, 5))
plt.subplot(1, 3, 1)
plt.imshow(cv2.cvtColor(cat_img, cv2.COLOR_BGR2RGB))
plt.title("Original Cat")
plt.axis("off")
plt.subplot(1, 3, 2)
plt.imshow(cat_mask, cmap="gray")
plt.title("Cat Mask")
plt.axis("off")
plt.subplot(1, 3, 3)
plt.imshow(cv2.cvtColor(cat_chroma_result, cv2.COLOR_BGR2RGB))
plt.title("Cat Chroma Result")
plt.axis("off")
plt.show() 
를 줄인다면 
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

images = [
    (cv2.cvtColor(cat_img, cv2.COLOR_BGR2RGB), "Original Cat"),
    (cat_mask, "Cat Mask"),
    (cv2.cvtColor(cat_chroma_result, cv2.COLOR_BGR2RGB), "Cat Chroma Result"),
]

for ax, (img, title) in zip(axes, images):
    ax.imshow(img, cmap="gray" if img.ndim == 2 else None)
    ax.set_title(title)
    ax.axis("off")

plt.show()
이런 식으로 반복되는 패턴을 없앨 수 있다.
