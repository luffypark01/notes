# 6.3 标注数据读取与存储

### 学习目标

- 目标
  - 无
- 应用
  - 应用XML工具进行标签数据读取以及存储

## 6.3.1 案例：xml读取本地文件存储到pkl

* ElementTree工具使用，解析xml结构
* 保存物体坐标结果以及类别
  * pickle工具导出

### 6.3.1.1 解析结构

* 导入

```python
from xml.etree import ElementTree
```

* **处理XML库**
  - import xml.etree.ElementTree as ET
    - tree = et.parse(filename)：形成树状结构
    - tree.getroot():获取树结构的根部分
    - root.find与findall()进行查询XML每个标签的内容.text

定义解析xml结构类，

```python
class XmlProcess(object):

    def __init__(self, data_path):
        self.path_prefix = data_path
        self.num_classes = 8
        self.data = dict()
```

进行preprocess_xml处理

* 解析基本信息

```python
def preprocess_xml(self):
        # 找到文件名字
        filenames = os.listdir(self.path_prefix)
        for filename in filenames:
            # 1、XML解析根路径
            tree = ElementTree.parse(self.path_prefix + filename)
            root = tree.getroot()
            bounding_boxes = []
            one_hot_classes = []
            size_tree = root.find('size')
            width = float(size_tree.find('width').text)
            height = float(size_tree.find('height').text)
```

* 获取每个对象的坐标

```python
			# 每个图片标记的对象进行坐标获取
            for object_tree in root.findall('object'):
                for bounding_box in object_tree.iter('bndbox'):
                    xmin = float(bounding_box.find('xmin').text)/width
                    ymin = float(bounding_box.find('ymin').text)/height
                    xmax = float(bounding_box.find('xmax').text)/width
                    ymax = float(bounding_box.find('ymax').text)/height
                bounding_box = [xmin, ymin, xmax, ymax]
                bounding_boxes.append(bounding_box)
                class_name = object_tree.find('name').text
                # 将类别进行one_hot编码
                one_hot_class = self.on_hot(class_name)
                one_hot_classes.append(one_hot_class)
```

* 获取图片名字，保存bounding_boxes以及类别one_hot编码信息

```python
            image_name = root.find('filename').text
            bounding_boxes = np.asarray(bounding_boxes)
            one_hot_classes = np.asarray(one_hot_classes)
            # 存储图片标注的结果对应的名字，以及图片的标注数据（4个坐标以及onehot编码）
            image_data = np.hstack((bounding_boxes, one_hot_classes))
            self.data[image_name] = image_data
```

### 6.3.1.2 one_hot编码函数

```python
    def on_hot(self, name):
        one_hot_vector = [0] * self.num_classes
        if name == 'clothes':
            one_hot_vector[0] = 1
        elif name == 'pants':
            one_hot_vector[1] = 1
        elif name == 'shoes':
            one_hot_vector[2] = 1
        elif name == 'watch':
            one_hot_vector[3] = 1
        elif name == 'phone':
            one_hot_vector[4] = 1
        elif name == 'audio':
            one_hot_vector[5] = 1
        elif name == 'computer':
            one_hot_vector[6] = 1
        elif name == 'books':
            one_hot_vector[7] = 1
        else:
            print('unknown label: %s' % name)
        return one_hot_vector
```

使用preprocess进行本地保存到pickle文件

```python
if __name__ == '__main__':
    xp = XmlProcess('/Users/huxinghui/workspace/ml/detection/ssd_detection/ssd/datasets/commodity/Annotations/')
    xp.preprocess_xml()
    pickle.dump(xp.data, open('./commodity_gt.pkl', 'wb'))
```

