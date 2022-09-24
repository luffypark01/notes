# 7.2 模型导出

### 学习目标

- 目标
  - 无
- 应用
  - 应用tf.saved_model.simple_save完成模型导出

## 7.2.1 keras 模型进行TensorFlow导出

Tensorflow Serving 使用的模型必须已固定格式导出

```
from nets.ssd_net import SSD300
from keras import backend as K
import tensorflow as tf
import os
```

- 使用tf.saved_model.simple_save工具进行导出
  - tf.saved_model.simple_save(
        session, # 会话
        export_dir, # 导出目录
        inputs, # 模型的输入
        outputs, # 模型的输出
    )

模型的结构以及运行结果

```python
Tensor("Reshape_42:0", shape=(?, 7308, 4), dtype=float32) Tensor("truediv:0", shape=(?, 7308, 9), dtype=float32) Tensor("concat_2:0", shape=(?, 7308, 8), dtype=float32)

{'concat_3:0': <tf.Tensor 'concat_3:0' shape=(?, 7308, 21) dtype=float32>}
```

完整带出代码：

```python
def save_model_for_serving(verion=1, path="./serving_model/commodity/"):
    # 2、导出模型过程
    # 路径+模型名字："./model/commodity/"
    export_path = os.path.join(
        tf.compat.as_bytes(path),
        tf.compat.as_bytes(str(verion)))

    print("正在导出模型到 %s" % export_path)

    # 模型获取
    model = SSD300((300, 300, 3), num_classes=9)
    model.load_weights("./ckpt/fine_tuning/weights.13-5.18.hdf5")

    with K.get_session() as sess:
        tf.saved_model.simple_save(
            sess,
            export_path,
            inputs={'images': model.input},
            outputs={t.name: t for t in model.outputs}
        )


if __name__ == '__main__':
    save_model_for_serving(verion=1, path="./serving_model/commodity/")
```



