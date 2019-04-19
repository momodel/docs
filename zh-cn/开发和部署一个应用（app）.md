# 开发和部署一个应用（app）

## 1. 简介
应用(app)一般由模块组成, 它能够满足普通用户的直接使用需求，例如航班延误预测应用, 图片风格迁移应用, 或者我们即将部署的兰花分类应用等。
你可以在训练营中的[平台功能教程](http://www.momodel.cn:8899/classroom/class?id=5c5696cd1afd9458d456bf54&type=doc)，选择`开发和部署一个应用(APP)`，来按照教程中的`.ipynb`文件指引边学边做，通过调用别人已经部署的模块(Module)，开发和部署一个应用。

## 2. 新建一个 Notebook
你新建的任何应用Notebook都默认有下面这个cell代码，这部分代码是系统自动生成的，主要作用是引入一些默认的包、Client和其他一些控制文件。
![](https://ws3.sinaimg.cn/large/006tNbRwly1fxwyz1al2oj31fe0s6ag3.jpg)

## 3. 调用他人发布公开的模块
在 Mo 的 Notebook 中，我们可以轻松的在我们自己的代码中，插入别人已经编写好的模块 (Module)。
插入过程按以下步骤操作：
1. 首先选择想要插入模块代码的 Cell (在这里, 请选择下方 '请点击此处，插入module' 字样的 Cell)
2. 打开左侧的 Modules Tab， 搜索 `iris`, 选择 `iris_classifier`
3. 在中间区域打开的 Tab 中， 浏览 Modules 详情, 选择对应的版本
5. 点击 `Insert Code` 按钮， 插入 Module (插入代码时, 会有一个上方会有一个 `Importing...` 的提示框， 表示正在导入模型到项目中)
6. 插入完成即可点击跳回 `*.ipynb` 的链接， 回到此 Notebook
7. 用 Notebook 界面顶上的运行按钮 <img src='https://ws4.sinaimg.cn/large/006tNbRwly1fwpcsg4aq2j301g01a744.jpg' width='30px'>， 或者 `Shift+Enter`， 即可运行插入的 Module

<img src='https://ws3.sinaimg.cn/large/006tNbRwly1fy2m612yt9g30sg0e5npe.gif' width=100% height=100%>

如果你得到了下图中的结果， 那么恭喜你， 成功用模块进行了一次预测
<img src='https://ws4.sinaimg.cn/large/006tNbRwly1fwq0gaefc3j30rs0640tf.jpg' width='80%'>

## 4. 二次训练他人发布公开的模型模块
如果引入的模块是可以训练的，那么我们看看如何对引入的模型模块进行二次训练.

1. 选择想要插入模块代码的 Cell 
2. 先引入我们在本教程中需要使用到的数据集，前面的教程里我们已经学会了[如何使用数据集](https://momodel.github.io/docs/#/zh-cn/%E8%BE%B9%E5%AD%A6%E8%BE%B9%E5%81%9A?id=%E5%9C%A8notebook%E4%B8%AD%E4%BD%BF%E7%94%A8%E6%95%B0%E6%8D%AE%E9%9B%86),我们搜索并插入 star-face 这个数据集. 然后` !unzip ./datasets/luxu1220-star-face/star-face.zip `来解压这个数据集的压缩文件,我们得到一个新的文件夹名为 star-face

![](https://ws2.sinaimg.cn/large/006tNc79ly1fz9n6thdhxj30x60a8mz1.jpg)

3. 然后, 打开左侧的 Modules Tab，搜索 `new_face_feature`, 选择 `new_face_feature`
4. 在中间区域打开的 Tab 中， 浏览 Modules 详情， 注意查看各个参数的说明

5. 在 Train 部分点击 插入代码 按钮， 插入此模块 (插入代码时, 会有一个上方会有一个 Importing... 的提示框， 表示正在导入模型到项目中)
6. 插入完成即可点击跳回 *.ipynb 的链接， 回到此 Notebook
7. 更改训练模块的输入参数，定义 `conf={'model_save_path': 'my_model.h5', 'epochs': 2, 'log_dir': './', 'data_path': './star-face', 'weight_save_path': 'my_weight.h5'}`
8. 然后使用 shift+enter 快捷键或者点击上面的运行按钮运行刚才插入的几个cell

![](https://ws3.sinaimg.cn/large/006tNc79ly1fz9n5ddk25j31za0ncwlr.jpg)

9. 训练完成后，在左侧可以看到模型和权重文件已经被保存下来了，然后我们就可以在预测部分使用它们了.

当然, 我们这里只是一个示例，模型没有很大， 训练的 epoch 也很小，效果提升不明显. 如果你使用的模型模块是基于深度神经网络的，并且网络结构很大，那么训练时间会比较长，这时候可以创建 job 并使用 GPU 来加速训练过程，详见[这里](https://momodel.github.io/docs/#/zh-cn/%E8%BE%B9%E5%AD%A6%E8%BE%B9%E5%81%9A?id=%E5%9C%A8gpucpu%E8%B5%84%E6%BA%90%E4%B8%8A%E8%AE%AD%E7%BB%83%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E6%A8%A1%E5%9E%8B).

## 5. 部署应用
我们的功能代码在 Notebook 调试完成后就可以点击左侧的 Deploy Tab 栏，按照指引部署应用。

1. 开始部署

在 Notebook 中点击左侧栏中的 `Deploy` 按钮，进入部署页面。

<img src='https://ws3.sinaimg.cn/large/006tKfTcgy1g0jlpfvcykj30ii0wmq61.jpg' width=40% height=40%>

2. 插入handle函数

handle函数是应用函数的的主函数，也是把输入和输出参数对应起来的接口函数，是部署之后其他人调用你的服务的处理方法。选中 Cell 代码的地方，点击第一步的 Insert “按钮”插入handle函数。然后根据前面调试的功能代码和定义的输入输出参数，整理 handle 函数。请按照该函数中的注释说明规范填写参数结构，这样系统就能自动提取输入输出参数，生成配置文件。

<img src='https://ws4.sinaimg.cn/large/006tKfTcgy1g0jlpe1mn5j30ii0wmdix.jpg' width=40% height=40%>

3. 准备部署文件

整理好 handle 函数之后，点击第二步的 start 按钮，开始准备部署时需要的文件。
<img src='https://ws3.sinaimg.cn/large/006tKfTcgy1g0jm8rzfofj30ia0wi0vu.jpg' width=40% height=40%>

以下为操作过程，首先选择需要部署的代码，handle 函数和新建 Notebook 生成的代码必须选择。然后预览生成的代码，如果有误可以点击上一步重新选择，接下来定义输入输出参数，这里定义四个输入参数，一个输出参数。最后生成 YML 配置文件，系统会根据之前定义的handle函数自动识别参数。通过这个步骤我们生成部署时需要的 Python 脚本和 YML 配置文件。
当然，你可以对生成的 `handler.py` 和 `app_spec.yml` 文件进行进一步的编辑。

![](https://ws4.sinaimg.cn/large/006tKfTcgy1g0lyrjvdieg30sg0isnpf.gif)


4. 部署应用项目

完成所有以上步骤后，点击第3步的 `部署` 按钮进行项目部署。

<img src='https://ws1.sinaimg.cn/large/006tKfTcgy1g0jmau96r9j30i80x0n0k.jpg' width=40% height=40%>

在部署的时候，系统会自动对 `handler.py` 文件和 `app_spec.yml`  配置文件进行基本的格式检查，检查通过后才能进行部署，你可以选择需要发布的文件或勾选发布开发版本或正式版本（开发版本部署后只有所有者可以使用，正式版项目为公开的，所有人可见），然后点击 `OK`，进行部署。

<img src='https://ws1.sinaimg.cn/large/006tKfTcgy1g0jskge0awj31cu0oyjwb.jpg' width=100% height=100%>

在短暂的等待后, 回到应用的详情页面, 在右上角会有部署成功的通知。恭喜你！你已经完成了你的第一个APP的部署。

## 6. 运行已部署的应用
然后你就可以找到这个项目，在网页中输入参数，运行得到输出结果，如果发现问题，可再回到项目中进行调试。
<img src='https://ws2.sinaimg.cn/large/006tNbRwly1fy2t303ftjj31cp0u0whf.jpg' width=80% height=80%>
