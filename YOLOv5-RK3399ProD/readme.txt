本代码是从项目修改而来，运行的时候不方便就重新
rk3399proD系统: debian10  x86主机系统: ubuntu18.4

本工程实现两个功能：
1.训练yolov5网络
2. pt文件转onnx再转rknn
3.python版本的rknn推理

其中第1&2部分在Ubuntu18.4中进行 第3部分在rk3399pro中进行

#创建conda环境
  conda create -n 3399 python=3.6
  conda activate 3399
  cd YOLOv5-*
  pip install -r myrequirement.txt

#训练模型
  python train.py --weights weights/yolov5s.pt --cfg models/yolov5s.yaml --data data/test.yaml --epochs 1

#此处 yolov5s.pt 从V4.0下载，data中只有一张图片，可以快速验证
  https://github.com/ultralytics/yolov5/releases/download/v4.0/yolov5s.pt

# pt转换onnx
  python models/export.py --rknn_mode --weights runs/train/exp/weights/best.pt
  #此处 --weights后面的参数是刚刚训练生成的.pt文件 之后会在runs/train/exp/weights/下生成best.onnx

# onnx转rknn
  python rknn_convert.py 

# 上板推理 之后都是在rk3399上进行
  cp convert best.rknn rknn_detect/weights
  cd convert
  python rknn_detect_for_yolov5_original.py