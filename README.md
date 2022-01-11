# 관련 소스 파일
```bash
# 학습
~/RCNN/Mask_RCNN/windroneus.py
# detect
~/RCNN/Mask_RCNN/detect.py
```

# 학습 데이터 준비
라벨 스튜디오에서 export 받은 폴더의 result.json 파일 수정 필요


before
```json
"file_name": "images\\1/2955d42c-211018-way2-H40-2-orthophoto.png"
```

after
```json
"file_name": "images/2955d42c-211018-way2-H40-2-orthophoto.png"
```


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


### 학습의 결과로 나온 weight 파일 저장 위치
~/RCNN/Mask_RCNN/logs 폴더 밑에 coco로 시작하는 폴더명 밑에 mask_rcnn_coco_????.h5의 형태로 저장됨
????의 숫자가 높을 수록 마지막 학습된 weight 파일임

```bash
# 드론으로찍은 사진 학습 파일 project-1, project-2
~/RCNN/Mask_RCNNmask_rcnn_coco_drone_pic.h5

```

# Detect
```bash
# 소스 폴더에 있는 모든 이미지내의 나무이미지를 찾아서 타겟 폴더에 생성  (파일 명 규칙은 기존 파일명의 앞에 OUTPUT_를 추가)
# 최신 가중치 파일 사용 
python detect.py --sourcedir images --targetdir output3
# 지정 가중치 파일 사용
python detect.py --sourcedir images --targetdir output3 --model /home/windroneus/RCNN/Mask_RCNN/logs/coco20220107T1754/mask_rcnn_coco_0118.h5
```
결과 파일 예
![image](https://user-images.githubusercontent.com/61860152/148712807-424df4c0-17b6-4407-92e6-57e93301dc4f.png)



# Class Name 변경
라벨 스튜디오 작업내용 기준으로 클래스명 지정 나무의 영문명이 잘못 됬을 경우 detect.py파일에서 아래 부분 수정
```python
# COCO Class names
# Index of the class in the list is its ID. For example, to get ID of
# the teddy bear class, use: class_names.index('teddy bear')
class_names = ['곰솔'
,'구실잣밤나무'
,'굴참나무'
,'기타 상록활엽수'
,'기타 침엽수'
,'기타 활엽수'
,'낙엽송'
,'리기다 소나무'
,'밤나무'
,'백합나무(튤립나무)'
,'삼나무'
,'상수리나무'
,'서어나무'
,'소나무'
,'신갈나무'
,'아까시나무'
,'자작나무'
,'잣나무'
,'침활혼효림'
,'편백']
```

# 폰트 사이즈 변경
```python
# detect.py 222 line  50 숫자를 변경
# Label
font = ImageFont.truetype("/usr/share/fonts/truetype/freefont/FreeMonoBold.ttf",50)
```


# Error
* 학습 도중 Detect를 진행하면 오류 발생 
 -> 학습 종료 후 진행 OR 학습 진행 중인 내용 강제 종료(학습 진행 중인 창에서 CTRL+C)
![image](https://user-images.githubusercontent.com/61860152/148710771-b0bb551c-9bde-4fbc-ac8a-48491d63cd20.png)




# 참고

![image](https://user-images.githubusercontent.com/61860152/148738120-84bf7f64-443d-4ebf-8dca-6936c0c250c6.png)

![image](https://user-images.githubusercontent.com/61860152/148738173-709f5da9-2051-4bdb-b2c1-0921f0063e1e.png)

![image](https://user-images.githubusercontent.com/61860152/148738352-4ef57f6c-7b80-4eaf-97c4-6763d3b6e3ff.png)

![image](https://user-images.githubusercontent.com/61860152/148738380-d9216100-4316-4ea9-86df-c7abc0e38d71.png)



## 윈도우 설치 참고 링크
https://jaeishin.tistory.com/2

```bash
# 리눅스 설치 python 패키지 버전 (윈도우 설치 시 대략 맞출 것)
(py356) windroneus@windroneus-X570-AORUS-ELITE:~/RCNN/Mask_RCNN/logs/coco20220111T0859$ pip list
DEPRECATION: Python 3.5 reached the end of its life on September 13th, 2020. Please upgrade your Python as Python 3.5 is no longer maintained. pip 21.0 will drop support for Python 3.5 in January 2021. pip 21.0 will remove support for this functionality.
Package                       Version
----------------------------- ---------
absl-py                       0.15.0
alabaster                     0.7.12
argon2-cffi                   21.1.0
astor                         0.8.1
attrs                         21.4.0
Babel                         2.9.1
backcall                      0.2.0
bleach                        1.5.0
certifi                       2020.6.20
cffi                          1.15.0
chardet                       4.0.0
cycler                        0.10.0
Cython                        0.29.26
decorator                     5.1.0
defusedxml                    0.7.1
docutils                      0.16
entrypoints                   0.3
gast                          0.2.2
google-pasta                  0.2.0
grpcio                        1.41.1
h5py                          2.10.0
html5lib                      0.9999999
idna                          2.10
imageio                       2.9.0
imagesize                     1.3.0
imgaug                        0.2.6
importlib-metadata            2.1.2
ipykernel                     5.5.6
ipyparallel                   6.3.0
ipython                       7.9.0
ipython-genutils              0.2.0
ipywidgets                    7.6.5
jedi                          0.17.2
Jinja2                        2.11.3
jsonschema                    3.2.0
jupyter-client                6.1.12
jupyter-core                  4.6.3
Keras                         2.0.8
Keras-Applications            1.0.8
Keras-Preprocessing           1.1.2
kiwisolver                    1.1.0
Markdown                      3.2.2
MarkupSafe                    1.1.1
mask-rcnn                     2.1
matplotlib                    3.0.3
mistune                       0.8.4
nbconvert                     5.6.1
nbformat                      5.1.3
networkx                      2.4
nose                          1.3.7
notebook                      6.2.0
numpy                         1.18.5
opencv-python                 4.4.0.42
opt-einsum                    3.3.0
packaging                     20.9
pandocfilters                 1.5.0
parso                         0.7.1
pexpect                       4.8.0
pickleshare                   0.7.5
Pillow                        7.2.0
pip                           20.3.4
prometheus-client             0.12.0
prompt-toolkit                2.0.10
protobuf                      3.19.1
ptyprocess                    0.7.0
pycocotools                   2.0
pycparser                     2.21
Pygments                      2.11.1
pyparsing                     2.4.7
pyrsistent                    0.17.3
python-dateutil               2.8.2
pytz                          2021.3
PyWavelets                    1.1.1
PyYAML                        5.3.1
pyzmq                         20.0.0
qtconsole                     4.7.7
QtPy                          1.9.0
requests                      2.25.1
scikit-image                  0.15.0
scipy                         1.4.1
Send2Trash                    1.8.0
setuptools                    50.3.2
Shapely                       1.7.1
six                           1.16.0
snowballstemmer               2.2.0
Sphinx                        3.5.4
sphinxcontrib-applehelp       1.0.2
sphinxcontrib-devhelp         1.0.2
sphinxcontrib-htmlhelp        1.0.3
sphinxcontrib-jsmath          1.0.1
sphinxcontrib-qthelp          1.0.3
sphinxcontrib-serializinghtml 1.1.5
tensorboard                   1.15.0
tensorflow-estimator          1.15.1
tensorflow-gpu                1.15.0
tensorflow-tensorboard        0.1.8
termcolor                     1.1.0
terminado                     0.8.3
testpath                      0.5.0
tornado                       6.1
traitlets                     4.3.3
urllib3                       1.26.7
wcwidth                       0.2.5
Werkzeug                      1.0.1
wheel                         0.37.0
widgetsnbextension            3.5.2
wrapt                         1.13.3
zipp                          1.2.0
```

