# 5.3 SSD网络接口介绍

### 学习目标

- 目标
  - 无
- 应用
  - 应用keras SSD进行物体检测案例

### 5.3.1 keras SSD结构

* SSD300网络结构

网络输入

```python
input_tensor = input_tensor = Input(shape=input_shape)
```

网络输出

```python
net['predictions'] = merge([net['mbox_loc'],
                               net['mbox_conf'],
                               net['mbox_priorbox']],
                               mode='concat', concat_axis=2,
                               name='predictions')
    print(net['mbox_loc'], net['mbox_conf'], net['mbox_priorbox'])
    model = Model(net['input'], net['predictions'])
```

* ssd_layers.py：网络层工具

```python
class PriorBox(Layer):
	# 对于给定的sizes和aspect ratios.生成prior boxes
```

* ssd_utils.py：SSD网络编解码工具以及NMS工具

SSD网络输出结果解码：

```python
def detection_out(self, predictions, background_label_id=0, keep_top_k=200,
                      confidence_threshold=0.01):
    # Do non maximum suppression (nms) on prediction results.
```

