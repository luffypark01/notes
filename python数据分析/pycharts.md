# [【宝藏级】PyEcharts 超详细的使用指南](https://blog.csdn.net/m0_67391120/article/details/123374736)

2022-03-09 12:52:22



### Python[可视化](https://so.csdn.net/so/search?q=可视化&spm=1001.2101.3001.7020)神器-pyecharts手册


# pyecharts简介

[Echarts](https://echarts.apache.org/zh/index.html)是一个由百度开源的数据可视化，凭借着良好的交互性，精巧的图表设计，得到了众多开发者的认可。而`Python` 是一门富有表达力的语言，很适合用于数据处理。当数据分析遇上数据可视化时，`pyecharts`诞生了。`Echarts`是用`JS`来写的，而我们使用`pyecharts`则可以使用`Python`来调用里面的`API`。

## 优点：

1. 简洁的 API 设计，使用如丝滑般流畅，支持链式调用
2. 囊括了 30+ 种常见图表，应有尽有
3. 支持主流 Notebook 环境，Jupyter Notebook 和 JupyterLab
4. 可轻松集成至 Flask，Django 等主流 Web 框架
5. 高度灵活的配置项，可轻松搭配出精美的图表
6. 详细的文档和示例，帮助开发者更快的上手项目
7. 多达 400+ 地图文件以及原生的百度地图，为地理数据可视化提供强有力的支持

## 安装：

1. 在普通的`python`环境中：`pip install pyecharts`。

2. 在

   ```
   anaconda
   ```

   中：

   - 先打开`anaconda prompt`。
   - 输入`pip install pyecharts`进行安装。

首先打开命令行(win+r)，输入：

```cmake
pip install pyecharts
```

由于墙的原因，下载时会出现断线和速度过慢的问题导致下载失败，所以建议通过清华镜像来进行下载：

```awk
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pyecharts
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a463387adb3e4d319cbcbed3c6e376e7.png)
出现上方的信息，即代表下载成功，我们可以来进行下一步的实验了！

**使用实例**

**注意**：就是python2.x和python3.x的编码问题，在python3.x中你可以把它看做默认是unicode编码，但在python2.x中并不是默认的，原因就在它的bytes对象定义的混乱，而pycharts是使用unicode编码来处理字符串和文件的，所以当你使用的是python2.x时，请务必在上方插入此代码：

```angelscript
from future import unicode_literals
```

> 可以忽略因为目前大都是**python3**

## 官方文档：

1. 官方文档（中文）：`https://pyecharts.org/#/zh-cn/intro`。
2. 官方github：`https://github.com/pyecharts/pyecharts`。

# pyecharts快速开始

`pyecharts`中可以绘制的图有很多，这里我们先来总体的了解一下他的使用风格，和调用的方式。有宏观的理解后，再具体学习具体图形的绘制。

## 在`Notebook`中创建一个条形图：

```stylus
from pyecharts.charts import Bar

bar = Bar()
bar.add_xaxis(["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"])
bar.add_yaxis("商家A", [5, 20, 36, 10, 75, 90])
bar.render_notebook()
```

## 链式调用：

有些程序员喜欢链式调用，或者链式调用在某些情况下可以让代码更加简洁。`pyecharts`中所有的方法都支持链式调用。比如以上条形图的代码可以改成：

```stylus
from pyecharts.charts import Bar
bar = (
    Bar()
    .add_xaxis(["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"])
    .add_yaxis("商家A", [5, 20, 36, 10, 75, 90])
)
bar.render_notebook()
```

## 配置选项：

`pyecharts`中包括图的标题，颜色主题等，都是通过选项`Options`配置的。

比如：

```reasonml
from pyecharts.charts import Bar
from pyecharts import options as opts
from pyecharts.globals import ThemeType

bar = (
    # 使用了InitOpts来初始化图的主题
    Bar(init_opts=opts.InitOpts(theme=ThemeType.LIGHT))
    .add_xaxis(["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"])
    .add_yaxis("商家A", [5, 20, 36, 10, 75, 90])
    .add_yaxis("商家B", [10, 21, 30, 15, 80, 92])
    # 使用TitleOpts来初始化了标题和子标题
    .set_global_opts(title_opts=opts.TitleOpts(title="Pyecharts练习",subtitle="柱状图"))
)
bar.render_notebook()
```

所有的这些都是有相应的`opts`来配置。

# 全局配置项

我们来看下全局配置项有哪些。在学习具体的配置项之前，先来看下`pyecharts`生成的图由哪几个部分组成。
![在这里插入图片描述](https://img-blog.csdnimg.cn/071f3433c76740d68d4ea3e64ef7336f.png)

针对以上每个部分，都有相应的配置项来进行配置。所有的配置类，都是放到`pyecharts.options`中。

## `AnimationOpts`：画图动画配置项

可以配置画图的动画，比如是否开启动画，动画持续时间，动画缓动效果等。

具体参数参考：`https://pyecharts.org/#/zh-cn/global_options?id=animationopts%ef%bc%9aecharts-%e7%94%bb%e5%9b%be%e5%8a%a8%e7%94%bb%e9%85%8d%e7%bd%ae%e9%a1%b9`

## `InitOpts`：初始化配置项

可以配置诸如图像宽度，高度，图表主题，背景颜色等。示例代码如下：

```stylus
from pyecharts.charts import Bar
from pyecharts import options as opts
from pyecharts.globals import ThemeType
from faker import Faker

c = (
        Bar(
            init_opts=opts.InitOpts(
                width="500px",
                height="400px",
                theme=ThemeType.LIGHT,
                bg_color="skyblue"
            )
        )
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
    )
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/c32a059bcdef48ccac08a386dcccb111.png)

具体参考：`https://pyecharts.org/#/zh-cn/global_options?id=initopts%ef%bc%9a%e5%88%9d%e5%a7%8b%e5%8c%96%e9%85%8d%e7%bd%ae%e9%a1%b9`。

## `ToolBoxFeatureOpts`和`ToolboxOpts`：工具箱配置项

可以配置图片右上角的工具箱。
具体参考：`https://pyecharts.org/#/zh-cn/global_options?id=toolboxfeatureopts%ef%bc%9a%e5%b7%a5%e5%85%b7%e7%ae%b1%e5%b7%a5%e5%85%b7%e9%85%8d%e7%bd%ae%e9%a1%b9`。

## `TitleOpts`：标题配置项

配置图的标题和子标题等信息。示例代码如下：

```stylus
c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
        .set_global_opts(title_opts=opts.TitleOpts(
            title="销售表",
            pos_right="0",
            pos_bottom="2px",
            title_textstyle_opts=opts.TextStyleOpts(**{"color":"#333","font_size":12})
        ))
    )
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/c946f6a9c0e740a187d4e08e7c57f806.png)

具体参考：`https://pyecharts.org/#/zh-cn/global_options?id=titleopts%ef%bc%9a%e6%a0%87%e9%a2%98%e9%85%8d%e7%bd%ae%e9%a1%b9`。

## `DataZoomOpts`：区域缩放配置项

图的底部的缩放配置项目。比如是否展示缩放，缩放过程中是否需要实时更新图等。

具体参考：`https://pyecharts.org/#/zh-cn/global_options?id=datazoomopts%ef%bc%9a%e5%8c%ba%e5%9f%9f%e7%bc%a9%e6%94%be%e9%85%8d%e7%bd%ae%e9%a1%b9`。

## `LegendOpts`：图例配置项

配置图例。示例代码如下：

```stylus
c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
        .set_global_opts(
            legend_opts=opts.LegendOpts(selected_mode="mutiple",orient="vertical",pos_right="30px")
        )
    )
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7bff4561454d4ab69ebfacdf3ba2dda7.png)

更多请参考：`https://pyecharts.org/#/zh-cn/global_options?id=legendopts%ef%bc%9a%e5%9b%be%e4%be%8b%e9%85%8d%e7%bd%ae%e9%a1%b9`。

## `VisualMapOpts`：视觉映射配置项

示例代码如下：

```stylus
c = (
    Scatter()
    .add_xaxis(Faker.choose())
    .add_yaxis("商品",[(x,y) for x,y in zip(Faker.values(),Faker.values())])
    .set_global_opts(
        visualmap_opts = opts.VisualMapOpts(type_="size",range_text=['大','小'])
    )
)
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/9790ed896cb9401ab37bed2cd0d295fa.png)

更多请参考：`https://pyecharts.org/#/zh-cn/global_options?id=visualmapopts%ef%bc%9a%e8%a7%86%e8%a7%89%e6%98%a0%e5%b0%84%e9%85%8d%e7%bd%ae%e9%a1%b9`。

## `TooltipOpts`：提示框配置项

提示框的配置项。示例代码如下：

```stylus
c = (
    Scatter()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A",Faker.values())
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Scatter-多维度数据"),
        tooltip_opts=opts.TooltipOpts(
            formatter=JsCode(
                "function (params) {return params.value}"
            )
        )
    )
)
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/8f9902b4830344b7b0139029383d6644.png)

更多请参考：`https://pyecharts.org/#/zh-cn/global_options?id=tooltipopts%ef%bc%9a%e6%8f%90%e7%a4%ba%e6%a1%86%e9%85%8d%e7%bd%ae%e9%a1%b9`。

## `AxisLineOpts/AxisTickOpts/AxisPointerOpts/AxisOpts`: 坐标轴轴线/刻度/指示器/坐标轴配置项。示例代码如下：

```routeros
c = (
        Bar()
        .add_xaxis(
            [
                "名字很长的X轴标签1",
                "名字很长的X轴标签2",
                "名字很长的X轴标签3",
                "名字很长的X轴标签4",
                "名字很长的X轴标签5",
                "名字很长的X轴标签6",
            ]
        )
        .add_yaxis("商家A", [10, 20, 30, 40, 50, 40])
        .add_yaxis("商家B", [20, 10, 40, 30, 40, 50])
        .set_global_opts(
            xaxis_opts=opts.AxisOpts(
                name="商家名称",
                axislabel_opts=opts.LabelOpts(rotate=-15),
                axisline_opts = opts.AxisLineOpts(symbol="arrow",linestyle_opts=opts.LineStyleOpts(width=2)),
                axistick_opts = opts.AxisTickOpts(is_inside=True,length=20),
                axispointer_opts = opts.AxisPointerOpts(is_show=True,type_="line")
            )
        )
    )
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6e3e572fcc5d4020894752226b70a860.png)

更多请参考：`https://pyecharts.org/#/zh-cn/global_options?id=axislineopts-%e5%9d%90%e6%a0%87%e8%bd%b4%e8%bd%b4%e7%ba%bf%e9%85%8d%e7%bd%ae%e9%a1%b9`。

## `SingleAxisOpts`：单轴配置项

更多请参考：`https://pyecharts.org/#/zh-cn/global_options?id=singleaxisopts%ef%bc%9a%e5%8d%95%e8%bd%b4%e9%85%8d%e7%bd%ae%e9%a1%b9`。

# 条形图

## 横向条形图

横向条形图只要调用`reversal_axis()`即可。

```reasonml
c = (
    Bar()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values())
    .add_yaxis("商家B", Faker.values())
    .reversal_axis()
    .set_series_opts(label_opts=opts.LabelOpts(position="right"))
    .set_global_opts(title_opts=opts.TitleOpts(title="Bar-翻转 XY 轴"))
)
```

效果图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/cdfaadf5e6fd46618797e0cbbad57bf1.png)

## 堆叠条形图：

堆叠条形图只要在添加`y`轴的函数`add_yaxis`上添加`stack`参数即可。示例代码如下：

```reasonml
c = (
    Bar()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values(), stack="stack1")
    .add_yaxis("商家B", Faker.values(), stack="stack1")
    .set_series_opts(label_opts=opts.LabelOpts(is_show=False))
    .set_global_opts(title_opts=opts.TitleOpts(title="Bar-堆叠数据（全部）"))
)
```

效果图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/dcd7c64b56e4482796ad0aaf09af3223.png)

## 设置条形图的间距：

条形图得间距设置有两种。第一种是设置`category_gap`参数，这个参数是`x`轴每个分类的间距，第二个是`gap`，这个是统一分类下多根柱子间的间距。示例代码如下：

```reasonml
c = (
    Bar()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values(), category_gap="80%")
    .set_global_opts(title_opts=opts.TitleOpts(title="Bar-单系列柱间距离"))
)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/40fdd14da9ea40a48775f53678c6feab.png)

```stylus
c = (
    Bar()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values(), gap="0%")
    .add_yaxis("商家B", Faker.values(), gap="0%")
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Bar-不同系列柱间距离"),
    )
)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/41671b8baee64a77aef9d6dbe80c6d3f.png)

## 带有网格的条形图：

网格图，是在`x`轴和`y`轴上，都绘制横线，形成的网格。可以在`opts.AxisOpts`中通过设置`splitline_opts`实现。

```stylus
c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .set_global_opts(
            title_opts=opts.TitleOpts(title="Scatter-显示分割线"),
            xaxis_opts=opts.AxisOpts(splitline_opts=opts.SplitLineOpts(is_show=True)),
            yaxis_opts=opts.AxisOpts(splitline_opts=opts.SplitLineOpts(is_show=True)),
        )
    )
c.render_notebook()
```

**柱状图-Bar**

```stylus
//导入柱状图-Bar
from pyecharts import Bar
//设置行名
columns = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
//设置数据
data1 = [2.0, 4.9, 7.0, 23.2, 25.6, 76.7, 135.6, 162.2, 32.6, 20.0, 6.4, 3.3]
data2 = [2.6, 5.9, 9.0, 26.4, 28.7, 70.7, 175.6, 182.2, 48.7, 18.8, 6.0, 2.3]
//设置柱状图的主标题与副标题
bar = Bar("柱状图", "一年的降水量与蒸发量")
//添加柱状图的数据及配置项
bar.add("降水量", columns, data1, mark_line=["average"], mark_point=["max", "min"])
bar.add("蒸发量", columns, data2, mark_line=["average"], mark_point=["max", "min"])
//生成本地文件（默认为.html文件）
bar.render()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/c8b0f5105b0f44cda5bfdd4338689402.png)
简单的几行代码就可以将数据进行非常好看的可视化，而且还是动态的，在这里还是要安利一下**jupyter**，**pyecharts**在v0.1.9.2版本开始，在**jupyter**上直接调用实例（例如上方直接调用bar）就可以将图表直接表示出来，非常方便。

# 饼图

```stylus
//导入饼图Pie
from pyecharts import Pie
//设置主标题与副标题，标题设置居中，设置宽度为900
pie = Pie("饼状图", "一年的降水量与蒸发量",title_pos='center',width=900)
//加入数据，设置坐标位置为【25，50】，上方的colums选项取消显示
pie.add("降水量", columns, data1 ,center=[25,50],is_legend_show=False)
//加入数据，设置坐标位置为【75，50】，上方的colums选项取消显示，显示label标签
pie.add("蒸发量", columns, data2 ,center=[75,50],is_legend_show=False,is_label_show=True)
//保存图表
pie.render()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e78de9388db843cea7ee8423721df73d.png)

# 箱线图

箱线图使用`pyecharts.charts.Boxplot`来实现。

## 基本使用：

```prolog
v1 = [
    [850, 740, 900, 1070, 930, 850, 950, 980, 980, 880]
    + [1000, 980, 930, 650, 760, 810, 1000, 1000, 960, 960],
    [960, 940, 960, 940, 880, 800, 850, 880, 900]
    + [840, 830, 790, 810, 880, 880, 830, 800, 790, 760, 800],
]
v2 = [
    [890, 810, 810, 820, 800, 770, 760, 740, 750, 760]
    + [910, 920, 890, 860, 880, 720, 840, 850, 850, 780],
    [890, 840, 780, 810, 760, 810, 790, 810, 820, 850, 870]
    + [870, 810, 740, 810, 940, 950, 800, 810, 870],
]
c = Boxplot()
c.add_xaxis(["expr1", "expr2"]).add_yaxis("A", c.prepare_data(v1)).add_yaxis(
    "B", c.prepare_data(v2)
).set_global_opts(title_opts=opts.TitleOpts(title="BoxPlot-基本示例"))

c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/9a7609d7f12a4d2497a9f071db531c66.png) **箱体图-Boxplot**

```stylus
//导入箱型图Boxplot
from pyecharts import Boxplot 
boxplot = Boxplot("箱形图", "一年的降水量与蒸发量")
x_axis = ['降水量','蒸发量']
y_axis = [data1,data2]
//prepare_data方法可以将数据转为嵌套的 [min, Q1, median (or Q2), Q3, max]
yaxis = boxplot.prepare_data(y_axis)       
boxplot.add("天气统计", x_axis, _yaxis)
boxplot.render()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/96b4169503ee41448b2aff574c7adeb7.png)

# 折线图

```pgsql
from pyecharts import Line

line = Line("折线图","一年的降水量与蒸发量")
//is_label_show是设置上方数据是否显示
line.add("降水量", columns, data1, is_label_show=True)
line.add("蒸发量", columns, data2, is_label_show=True)
line.render()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/583aca10343f4d519c4927412206cbd2.png)

# 散点图

散点图用的是`pyecharts.charts.Scatter`来实现的。

## 基本使用：

```reasonml
c = (
    Scatter()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values())
    .set_global_opts(title_opts=opts.TitleOpts(title="Scatter-基本示例"))
)
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/d56d5a211bc04ebcae7e183a4fcd714d.png)

## 多维散点图：

```routeros
c = (
    Scatter()
    .add_xaxis(Faker.choose())
    .add_yaxis(
        "商家A",
        [list(z) for z in zip(Faker.values(), Faker.choose())],
        label_opts=opts.LabelOpts(
            formatter=JsCode(
                "function(params){return params.value[1] +' : '+ params.value[2];}"
            )
        ),
    )
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Scatter-多维度数据"),
        tooltip_opts=opts.TooltipOpts(
            formatter=JsCode(
                "function (params) {return params.name + ' : ' + params.value[2];}"
            )
        ),
        visualmap_opts=opts.VisualMapOpts(
            type_="color", max_=150, min_=20, dimension=1
        ),
    )
)
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b39ed21e804e438bbd51c7bab04193e6.png)

# 雷达图

```lua
from pyecharts import Radar
radar = Radar("雷达图", "一年的降水量与蒸发量")
//由于雷达图传入的数据得为多维数据，所以这里需要做一下处理
radar_data1 = [[2.0, 4.9, 7.0, 23.2, 25.6, 76.7, 135.6, 162.2, 32.6, 20.0, 6.4, 3.3]]
radar_data2 = [[2.6, 5.9, 9.0, 26.4, 28.7, 70.7, 175.6, 182.2, 48.7, 18.8, 6.0, 2.3]]
//设置column的最大值，为了雷达图更为直观，这里的月份最大值设置有所不同
schema = [ 
    ("Jan", 5), ("Feb",10), ("Mar", 10),
    ("Apr", 50), ("May", 50), ("Jun", 200),
    ("Jul", 200), ("Aug", 200), ("Sep", 50),
    ("Oct", 50), ("Nov", 10), ("Dec", 5)
]
//传入坐标
radar.config(schema)
radar.add("降水量",radar_data1)
//一般默认为同一种颜色，这里为了便于区分，需要设置item的颜色
radar.add("蒸发量",radar_data2,item_color="#1C86EE")
radar.render()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/95f2015bdf5b46d0922f13d199557840.png)

# 地理图

## 中国地图：

```stylus
c = (
    Map()
    .add("商家A", [list(z) for z in zip(Faker.provinces, Faker.values())], "china")
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Map-VisualMap（连续型）"),
        visualmap_opts=opts.VisualMapOpts(max_=200),
    )
)
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/0c3df7fb114d47c7a05de940c3c8360a.png)

## 中国局部地图：

```stylus
c = (
    Map()
    .add("商家A", [list(z) for z in zip(Faker.guangdong_city, Faker.values())], "广东")
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Map-广东地图"),
        visualmap_opts=opts.VisualMapOpts(),
    )
)
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b63a042fbc4e4c42b4d0dff749acf05f.png)

## 世界地图：

```stylus
c = (
    Map()
    .add("商家A", [list(z) for z in zip(Faker.country, Faker.values())], "world")
    .set_series_opts(label_opts=opts.LabelOpts(is_show=False))
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Map-世界地图"),
        visualmap_opts=opts.VisualMapOpts(max_=200),
    )
)
c.render_notebook()
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/8811d1c687c64d2a9f9249983680d2ad.png)