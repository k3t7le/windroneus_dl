# 학습
### 학습 및 detect를 위한 최초 환경 설정
```bash
# 터미널 접속 및 Mask_RCNN폴더로 이동
cd RCNN/Mask_RCNN
# python 설정 (텐서플로우 등의 버전이 최신 버전 사용이 안되어 아나콘다 사용하여 버전 지정 중)
conda activate py356
```
아래 처럼 맨 앞에 (py356)이 보여야 함
![image](https://user-images.githubusercontent.com/61860152/148709196-6ffe275c-afe3-4187-adf6-7dbf82a8e5b2.png)

이 후 아래 모든 명령어는 /home/windroneus/RCNN/Mask_RCNN에서 실행됨

### 학습 할 때 사용하는 용어 정리
dataset : 학습에 사용할 이미지 + result.json(라벨링 값) 파일이 있는 디렉토리

model : 사용할 파리미터 가중치 (coco 선택 시 프로젝트 root 폴더의 mask_rcnn_coco.h5 사용, last 선택 시 프로젝트root/logs/*coco* 마지막 폴더에서 시작)

### case 1 최초 학습 or 기존 학습 완료된 데이터 기준 추가 학습 시
```bash
python windroneus.py train --dataset=/home/windroneus/RCNN/Mask_RCNN/samples/coco/project-1 --model=coco
```

### case 2 학습 진행 중 abort 됬을 때 이어서 학습 
```bash
python windroneus.py train --dataset=/home/windroneus/RCNN/Mask_RCNN/samples/coco/project-1 --model=last
```



### 예제 (여러개의 라벨 스튜디오 프로젝트 파일 대상으로 연이어 학습 진행 시

```bash
# 첫번째 라벨 스튜디오 프로젝트 파일로 학습 진행
python windroneus.py train --dataset=/home/windroneus/RCNN/Mask_RCNN/samples/coco/project-1 --model=coco
# 학습 완료 후 완료된 가중치 파일을 프로젝트root 로 복사 
./copy_last_weight_to_root.sh
# 두번째 라벨 스튜디오 프로젝트 파일로 학습 진행
python windroneus.py train --dataset=/home/windroneus/RCNN/Mask_RCNN/samples/coco/project-2 --model=coco
./copy_last_weight_to_root.sh
# 세번째 라벨 스튜디오 프로젝트 파일로 학습 진행
python windroneus.py train --dataset=/home/windroneus/RCNN/Mask_RCNN/samples/coco/project-3 --model=coco
```

참고 : ./copy_last_weight_to_root.sh 파일은 logs폴더내에 쌓이는 가중치 파일 중 최신 파일을 Mask_RCNN루트 폴더로 복사 하는 명령
![image](https://user-images.githubusercontent.com/61860152/148709453-334e5950-44f5-4170-b71c-07109baddc3c.png)

# Detect
```bash
# 소스 폴더에 있는 모든 이미지내의 나무이미지를 찾아서 타겟 폴더에 생성  (파일 명 규칙은 기존 파일명의 앞에 OUTPUT_를 추가)
# 최신 가중치 파일 사용 
python detect.py --sourcedir images --targetdir output3
# 지정 가중치 파일 사용
python detect.py --sourcedir images --targetdir output3 --model /home/windroneus/RCNN/Mask_RCNN/logs/coco20220107T1754/mask_rcnn_coco_0118.h5
```


# Error
학습 도중 Detect를 진행하면 오류 발생 
 -> 학습 종료 후 진행 OR 학습 진행 중인 내용 강제 종료(학습 진행 중인 창에서 CTRL+C)
![image](https://user-images.githubusercontent.com/61860152/148710771-b0bb551c-9bde-4fbc-ac8a-48491d63cd20.png)



