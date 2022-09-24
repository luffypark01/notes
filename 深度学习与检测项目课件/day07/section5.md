# 7.5 TensorFlow Client对接模型服务

### 学习目标

- 目标
  - 无
- 应用
  - 应用TensorFlow Serving Client完成对接模型服务编写以及运行

## 7.5.1Tensorflow Client代码编写对接Web

- main.py当中调用

```python
# 获取用户上传图片
image = request.files.get('image')
if not image:
abort(400)
# 预测标记
result_img = make_prediction(image.read())
data = result_img.read()
result_img.close()
```

### 7.5.1.1 Client端代码

需要用到tensorflow_serving.apis中的两个模块

```python
from tensorflow_serving.apis import prediction_service_pb2_grpc
from tensorflow_serving.apis import predict_pb2
```

##### prediction_service_pb2_grpc

##### predict_pb2

- prediction.py文件当中，定义make_prediction函数进行预测代码逻辑
  - 步骤分析
    - 1、获取读取后台读取的图片
    - 2、图片大小处理，转换数组
    - 3、打开通道channel,构建stub，预测结果
    - 4、predict_pb2进行预测请求创建

#### 7.5.1.2 步骤过程：

* 1、获取读取后台读取的图片,图片大小处理，转换数组

```python
def make_prediction(image):
    """
    """
    def resize_img(image, target_size):
        img = io.BytesIO()
        img.write(image)
        img = Image.open(img).convert("RGB")
        if target_size:
            img = img.resize((target_size[1], target_size[0]))
        return img

    image = resize_img(image, (300, 300))
    image_array = img_to_array(image)

    feature = []
    feature.append(image_array)
    img_tensor = preprocess_input(np.array(feature))
    print(img_tensor.shape)
```

* 2、打开通道channel,构建stub，预测结果，predict_pb2进行预测请求创建

```
 # 打开到tensorflow server的通道
    with grpc.insecure_channel('127.0.0.1:8500') as channel:
        stub = prediction_service_pb2_grpc.PredictionServiceStub(channel)

        # 创建预测请求
        request = predict_pb2.PredictRequest()
        request.model_spec.name = 'commodity'
        request.model_spec.signature_name = signature_constants.DEFAULT_SERVING_SIGNATURE_DEF_KEY
        request.inputs['images'].CopyFrom(tf.contrib.util.make_tensor_proto(img_tensor, shape=[1, 300, 300, 3]))

        # 进行预测
        result = stub.Predict(request)
```

```python
{'concat_3:0': <tf.Tensor 'concat_3:0' shape=(?, 7308, 21) dtype=float32>}
```

* 3、预测结果过滤并且解析，图片标记

```python
with tf.Session() as sess:
            _res = sess.run(tf.convert_to_tensor(result.outputs['concat_3:0']))
        # 3、测试阶段 进行NMS 过滤
        butil = BBoxUtility(9)

        outputs = butil.detection_out(_res)

    return tag_picture(image_array, outputs)
```

* tag_picture的逻辑

```python
import matplotlib.pyplot as plt
import numpy as np
from io import BytesIO

classes_name = ['clothes', 'pants', 'shoes', 'watch', 'phone',
                             'audio', 'computer', 'books']


def tag_picture(img, outputs):
    """
    对图片预测物体进行画图显示
    :param images_data: N个测试图片数据
    :param outputs: 每一个图片的预测结果
    :return:
    """
    # 1、先获取每张图片6列中的结果

    # 通过i获取图片label, location, xmin, ymin, xmax, ymax
    pre_label = outputs[0][:, 0]
    pre_conf = outputs[0][:, 1]
    pre_xmin = outputs[0][:, 2]
    pre_ymin = outputs[0][:, 3]
    pre_xmax = outputs[0][:, 4]
    pre_ymax = outputs[0][:, 5]

    top_indices = [i for i, conf in enumerate(pre_conf) if conf >= 0.3]
    top_conf = pre_conf[top_indices]
    top_label_indices = pre_label[top_indices].tolist()
    top_xmin = pre_xmin[top_indices]
    top_ymin = pre_ymin[top_indices]
    top_xmax = pre_xmax[top_indices]
    top_ymax = pre_ymax[top_indices]

    # print("pre_label:{}, pre_loc:{}, pre_xmin:{}, pre_ymin:{},pre_xmax:{},pre_ymax:{}".
    #       format(tag_label, tag_loc, tag_xmin, tag_ymin, tag_xmax, tag_ymax))

    # 对于每张图片的结果进行标记
    colors = plt.cm.hsv(np.linspace(0, 1, 21)).tolist()
    plt.imshow(img / 255.)
    currentAxis = plt.gca()

    for i in range(top_conf.shape[0]):
        xmin = int(round(top_xmin[i] * img.shape[1]))
        ymin = int(round(top_ymin[i] * img.shape[0]))
        xmax = int(round(top_xmax[i] * img.shape[1]))
        ymax = int(round(top_ymax[i] * img.shape[0]))

        # 获取该图片预测概率，名称，定义显示颜色
        score = top_conf[i]
        label = int(top_label_indices[i])
        label_name = classes_name[label - 1]
        display_txt = '{:0.2f}, {}'.format(score, label_name)
        coords = (xmin, ymin), xmax - xmin + 1, ymax - ymin + 1
        color = colors[label]
        # 显示方框
        currentAxis.add_patch(plt.Rectangle(*coords, fill=False, edgecolor=color, linewidth=2))
        # 左上角显示概率以及名称
        currentAxis.text(xmin, ymin, display_txt, bbox={'facecolor': color, 'alpha': 0.5})

        # plt.show()
    image_io = BytesIO()
    plt.savefig(image_io, format='png')
    image_io.seek(0)
    return image_io
```

完整代码：

```python
import tensorflow as tf
import grpc
from tensorflow_serving.apis import prediction_service_pb2_grpc
from tensorflow_serving.apis import predict_pb2
from tensorflow.python.saved_model import signature_constants

from keras.preprocessing.image import img_to_array
from keras.applications.imagenet_utils import preprocess_input
from utils.ssd_utils import BBoxUtility
from utils.tag_img import tag_picture
import io
from PIL import Image
import numpy as np


def make_prediction(image):
    """
    """
    def resize_img(image, target_size):
        img = io.BytesIO()
        img.write(image)
        img = Image.open(img).convert("RGB")
        if target_size:
            img = img.resize((target_size[1], target_size[0]))
        return img

    image = resize_img(image, (300, 300))
    image_array = img_to_array(image)

    feature = []
    feature.append(image_array)
    img_tensor = preprocess_input(np.array(feature))
    print(img_tensor.shape)

    # 打开到tensorflow server的通道
    with grpc.insecure_channel('127.0.0.1:8500') as channel:
        stub = prediction_service_pb2_grpc.PredictionServiceStub(channel)

        # 创建预测请求
        request = predict_pb2.PredictRequest()
        request.model_spec.name = 'commodity'
        request.model_spec.signature_name = signature_constants.DEFAULT_SERVING_SIGNATURE_DEF_KEY
        request.inputs['images'].CopyFrom(tf.contrib.util.make_tensor_proto(img_tensor, shape=[1, 300, 300, 3]))

        # 进行预测
        result = stub.Predict(request)

        with tf.Session() as sess:
            _res = sess.run(tf.convert_to_tensor(result.outputs['concat_3:0']))
        # 3、测试阶段 进行NMS 过滤
        butil = BBoxUtility(9)

        outputs = butil.detection_out(_res)

    return tag_picture(image_array, outputs)
```

