# 6.2 项目训练结构介绍

### 学习目标

- 目标
  - 无
- 应用
  - 无

## 6.2.1 项目目录结构

![](../images/完整训练目录结构.png)

* ckpt:分为预训练与微调模型
* datasets:放训练原始数据以及存储数据、读取数据代码以及模型priorbox
* servingmodel:模型部署使用的模型位置
* export_serving_model:导出TFserving指定模型类型
* train_ssd:训练模型代码逻辑