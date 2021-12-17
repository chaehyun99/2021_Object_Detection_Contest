# 2021_Object_Detection_Contest


## What Is YOLO?

YOLO stands for **Y**ou **O**nly **L**ook **O**nce and is an extremely fast object detection framework using a single convolutional network. YOLO is frequently faster than other object detection systems because it looks at the *entire* image at once as opposed to sweeping it pixel-by-pixel.

YOLO does this by breaking an image into a grid, and then each section of the grid is classified and localized (i.e. the objects and structures are established). Then, it predicts where to place bounding boxes. Predicting these bounding boxes is done with regression-based algorithms, as opposed to a classification-based one.

Generally, classification-based algorithms are completed in two steps: first, selecting the **R**egion **O**f **I**nterest (ROI), then applying the **c**onvolutional **n**eural **n**etwork (CNN) to the regions selected to detect object(s).

YOLO's regression algorithm predicts the bounding boxes for the whole image at once, which is what makes it dramatically faster and gives it the clever name to boot.

- **Creating Bounding Boxes With YOLOv5 On An Image**

![https://api.wandb.ai/files/onlineinference/images/projects/461118/4ca26a3d.png](https://api.wandb.ai/files/onlineinference/images/projects/461118/4ca26a3d.png)

- reference
    
    [https://wandb.ai/onlineinference/YOLO/reports/YOLOv5-Object-Detection-on-Windows-Step-By-Step-Tutorial---VmlldzoxMDQwNzk4#what-is-yolo](https://wandb.ai/onlineinference/YOLO/reports/YOLOv5-Object-Detection-on-Windows-Step-By-Step-Tutorial---VmlldzoxMDQwNzk4#what-is-yolo)?
    

## 1. 구성 (주요 파일)

```
|--Object_Detection_Model

|    |--ex_train_dataset
|    |--contest_testdata
|    |--runs 
        |    |--detect 
		|    |--exp'#' 
			|    |--contest_format

        |    |--train

|    |--train.py
|    |--inference.py
|    |--lastv3.pt
```

- `ex_train_dataset` : ‘train.py’ 실행 시 학습 과정을 보여주기 위한 샘플 데이터셋 디렉토리
- `contest_testdata` : 대회에서 제공한 최종 테스트 데이터 디렉토리
- `runs` : ‘train.py’와 ‘inference.py’가 실행된 후 결과 파일이 저장되는 디렉토리
    - detect : ‘inference.py’ 실행결과인 테스트 데이터에 대한 어노테이션파일(xml, txt)과 이미지 파일이 저장됨
        - exp’#’: 실행횟수 만큼  ‘#’(ex. 1,2,3 ...)가 증가함
            - contest_format : 어노테이션파일(xml)이 대회 결과데이터셋 제출 규정에 맞도록 저장됨
- `train`: ‘train.py’의 실행결과인 weight파일(.pt)가 저장됨
- `inference.py` : 테스트 데이터를 입력으로 하여 객체 검출을 함
- `train.py` : 학습 데이터를 입력으로 하여 모델을 학습시킴
- `lastv3.pt` : 모델 학습의 최종결과, weight

---

## 2. 실행 과정

### 1) YOLOv5 개발환경 구성

- 튜토리얼 참고 >  [yolov5-Tutorial](https://wandb.ai/onlineinference/YOLO/reports/YOLOv5-Object-Detection-on-Windows-Step-By-Step-Tutorial---VmlldzoxMDQwNzk4)
    
    > pycharm 가상환경과 같은 환경에서도 개발환경 구성 가능
    > 
    

### 2) 실행 방법

- 디렉토리 `Object_Detection_Model` 의 경로에서 `train.py` 실행 : 모델 생성 재현
    - 실행 시 발생할 수 있는 에러
        - ImportError: DLL load failed while importing _sobol: 이 작업을 완료하기 위한 페이징 파일이 너무 작습니다.
            
            ⇒ 메모리 용량 부족
            
        - RuntimeError: Unable to find a valid cuDNN algorithm to run convolution
            
            ⇒ batch_size 값이 클 경우 (현재는 ‘batch_size = 8’로 작은 값으로 설정되어 있음)
            
- 디렉토리 `Object_Detection_Model` 의 경로에서 `inference.py` 실행 : test 결과 재현

### 3) 실행 결과 저장 경로

- `runs/detect/exp'#'/contest_format`
    - ‘inference.py’ 실행 후 어노테이션파일(xml)이 대회 결과데이터셋 제출 규정에 맞도록 저장됨
- `runs/train/exp'#'/weights`
    - ‘train.py’ 실행 후 학습 결과가 저장됨
