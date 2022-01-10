# 학습
# dataset : 학습에 사용할 이미지 + result.json(라벨링 값) 파일이 있는 디렉토리
# model : 사용할 파리미터 가중치 (coco 선택 시 프로젝트 root 폴더의 mask_rcnn_coco.h5 사용, last 선택 시 프로젝트root/logs/*coco* 마지막 폴더에서 시작)

# case 1 최초 학습 or 기존 학습 완료된 데이터 기준 추가 학습 시
python windroneus.py train --dataset=/home/windroneus/RCNN/Mask_RCNN/samples/coco/project-1 --model=coco

# case 2 학습 진행 중 abort 됬을 때 이어서 학습 
python windroneus.py train --dataset=/home/windroneus/RCNN/Mask_RCNN/samples/coco/project-1 --model=last



# 예제
# 첫번째 라벨 스튜디오 프로젝트 파일로 학습 진행
python windroneus.py train --dataset=/home/windroneus/RCNN/Mask_RCNN/samples/coco/project-1 --model=coco

# 학습 완료 후 완료된 가중치 파일을 프로젝트root 로 복사 
cd /home/windroneus/RCNN/Mask_RCNN
./copy_last_weight_to_root.sh


결과
(py356) windroneus@windroneus-X570-AORUS-ELITE:~/RCNN/Mask_RCNN$ ls -la
합계 750524
drwxrwxr-x 12 windroneus windroneus      4096  1월 10 10:18 .
drwxrwxr-x  6 windroneus windroneus      4096  1월  5 13:17 ..
drwxrwxr-x  8 windroneus windroneus      4096  1월  7 13:28 .git
-rw-rw-r--  1 windroneus windroneus       569  1월  5 10:52 .gitignore
drwxrwxr-x  6 windroneus windroneus      4096  1월  5 10:57 .venv
-rw-rw-r--  1 windroneus windroneus      1095  1월  5 10:52 LICENSE
-rw-rw-r--  1 windroneus windroneus        58  1월  5 10:52 MANIFEST.in
-rw-rw-r--  1 windroneus windroneus     13771  1월  5 10:52 README.md
drwxrwxr-x  2 windroneus windroneus      4096  1월  5 10:52 assets
drwxrwxr-x  4 windroneus windroneus      4096  1월  5 11:45 build
-rwxrwxr-x  1 windroneus windroneus       551  1월 10 09:45 copy_last_weight_to_root.sh
-rw-rw-r--  1 windroneus windroneus      7843  1월 10 10:07 detect.py
drwxrwxr-x  2 windroneus windroneus      4096  1월  5 11:45 dist
drwxrwxr-x  6 windroneus windroneus      4096  1월  7 15:46 images
drwxrwxr-x 14 windroneus windroneus      4096  1월 10 10:12 logs
drwxrwxr-x  2 windroneus windroneus      4096  1월  7 14:57 mask_rcnn.egg-info
-rw-rw-r--  1 windroneus windroneus 255856928  1월  5 14:47 mask_rcnn_balloon.h5
-rw-rw-r--  1 windroneus windroneus 256268824  1월 10 10:18 mask_rcnn_coco.h5
-rw-rw-r--  1 windroneus windroneus 256268824  1월 10 10:18 mask_rcnn_coco.h5_backup_20220110101837
drwxrwxr-x  3 windroneus windroneus      4096  1월 10 09:19 mrcnn
-rw-rw-r--  1 windroneus windroneus       120  1월  5 11:41 requirements.txt
drwxrwxr-x  9 windroneus windroneus      4096  1월 10 10:07 samples
-rw-rw-r--  1 windroneus windroneus        99  1월  5 10:52 setup.cfg
-rw-rw-r--  1 windroneus windroneus      2518  1월  5 10:52 setup.py
-rw-rw-r--  1 windroneus windroneus     22113  1월 10 09:59 windroneus.py
