# 在 GPU 或 CPU 资源上训练机器学习模型
你可以在训练营中的[平台功能教程](http://www.momodel.cn:8899/classroom/class?id=5c5696cd1afd9458d456bf54&type=doc)，选择`在 GPU 或 CPU 资源上训练机器学习模型`，来按照教程中的`.ipynb`文件指引边学边做，通过调用别人已经部署的模块 (Module)，开发和部署一个应用。

## 1. 简介
在 Mo 平台上你可以通过两种方式训练你的模型：在 Notebook 中直接运行、通过建立 Job 在 GPU/CPU 后台运行。前者在网页上长时间运行很容易因为各种外部因素而中断，适合短时间小模型的调试训练。后者则通过后台建立 Job 任务运行，而且可以选择 GPU 加速，适合长时间大模型的训练。

本次教程采用卷积神经网络对手写数据集 ([MNIST](http://wiki.jikexueyuan.com/project/tensorflow-zh/tutorials/mnist_beginners.html))进行分类，指引大家学习如何在 CPU/GPU 资源上训练机器学习模型，本教程采用卷积神经网络进行分类，相较于全连接神经网络具有更好的特征提取能力，能够更好地保存图片的二维特征，在手写数据集上的准确率也会有很大提升。你可以在新建应用项目时选择[从模版创建该教程项目](https://momodel.github.io/docs/#/zh-cn/%E5%B9%B3%E5%8F%B0%E6%95%99%E7%A8%8B?id=%E5%88%9B%E5%BB%BA%E6%95%99%E7%A8%8Bcreate-project)，按照指引完成相应的操作。

## 2. 在 Notebook 中调试训练
首先我们运行下面的 cell 代码进行必要模块的导入, 以及参数的定义, 我们在这里将每个 epoch 训练的的 batch_size 定为 128

```Python
# -*- coding: utf-8 -*-

'''Trains a simple convnet on the MNIST dataset.

Gets to 99.25% test accuracy after 12 epochs
(there is still a lot of margin for parameter tuning).
16 seconds per epoch on a GPU.
'''

from __future__ import print_function
import numpy as np
np.random.seed(1337)  # for reproducibility

from keras.datasets import mnist
from keras.models import Sequential
from keras.layers import Dense, Dropout, Activation, Flatten
from keras.layers import Convolution2D, MaxPooling2D
from keras.utils import np_utils
# Keras的底层库使用Theano或TensorFlow
from keras import backend as K

batch_size = 128
nb_classes = 10
nb_epoch = 12

# input image dimensions
img_rows, img_cols = 28, 28
# number of convolutional filters to use
nb_filters = 32
# size of pooling area for max pooling
pool_size = (2, 2)
# convolution kernel size
kernel_size = (3, 3)
```

然后我们导入数据集以及做一些数据预处理

```Python
path = 'keras_mnist_data/mnist.npz'
f = np.load(path)
X_train, y_train = f['x_train'], f['y_train']
X_test, y_test = f['x_test'], f['y_test']
f.close()

if K.image_data_format() == 'channels_first':
    X_train = X_train.reshape(X_train.shape[0], 1, img_rows, img_cols)
    X_test = X_test.reshape(X_test.shape[0], 1, img_rows, img_cols)
    input_shape = (1, img_rows, img_cols)
else:
    X_train = X_train.reshape(X_train.shape[0], img_rows, img_cols, 1)
    X_test = X_test.reshape(X_test.shape[0], img_rows, img_cols, 1)
    input_shape = (img_rows, img_cols, 1)

X_train = X_train.astype('float32')
X_test = X_test.astype('float32')
X_train /= 255
X_test /= 255
print('X_train shape:', X_train.shape)
print(X_train.shape[0], 'train samples')
print(X_test.shape[0], 'test samples')

# convert class vectors to binary class matrices
Y_train = np_utils.to_categorical(y_train, nb_classes)
Y_test = np_utils.to_categorical(y_test, nb_classes)
```

然后使用 Keras 的 Sequential 定义两层卷积网络模型

```Python
model = Sequential()

# 卷积层
# 二维卷积层对二维输入进行滑动窗卷积
# keras.layers.convolutional.Convolution2D(nb_filter, nb_row, nb_col, init='glorot_uniform', activation='linear', weights=None, border_mode='valid', subsample=(1, 1), dim_ordering='th', W_regularizer=None, b_regularizer=None, activity_regularizer=None, W_constraint=None, b_constraint=None, bias=True)

# nb_filter：卷积核的数目,（即输出的维度）
# nb_row：卷积核的行数
# nb_col：卷积核的列数
# border_mode：边界模式，为“valid”，“same”或“full”，full需要以theano为后端
model.add(Convolution2D(nb_filters, kernel_size[0], kernel_size[1],
                        border_mode='valid',
                        input_shape=input_shape))
model.add(Activation('relu'))
model.add(Convolution2D(nb_filters, kernel_size[0], kernel_size[1]))
model.add(Activation('relu'))

# keras.layers.convolutional.MaxPooling2D(pool_size=(2, 2), strides=None, border_mode='valid', dim_ordering='th')
# 空域信号施加最大值池化
model.add(MaxPooling2D(pool_size=pool_size))
model.add(Dropout(0.25))

# Flatten层用来将输入“压平”，即把多维的输入一维化，常用在从卷积层到全连接层的过渡。Flatten不影响batch的大小。
model.add(Flatten())
model.add(Dense(128))
model.add(Activation('relu'))
model.add(Dropout(0.5))
model.add(Dense(nb_classes))
model.add(Activation('softmax'))

model.compile(loss='categorical_crossentropy',
              optimizer='adadelta',
              metrics=['accuracy'])

```

接下来运行下面的 cell 代码进行训练

```Python
model.fit(X_train, Y_train, batch_size=batch_size, nb_epoch=nb_epoch,
          verbose=1, validation_data=(X_test, Y_test))
score = model.evaluate(X_test, Y_test, verbose=0)
print('Test score:', score[0])
print('Test accuracy:', score[1])
```

*如果觉得训练时间太长, 可以直接点击 Notebook 顶部的<img src='http://imgbed.momodel.cn/stop.png' width='30px'>按钮停止程序的运行, 然后到下一小节, 把以上代码转换为py类型的文件，通过创建 Job 任务的方式训练模型。*

最后保存训练好的模型

```Python
model.save('results/my_model.h5')
```

*PS: 这里有个重要的地方, 我们需要将模型保存到 `results/` 文件夹下, 因为这个文件夹是 Job 与 Notebook 的共享文件夹, Job 中的训练结果只有保存到 `results/` 下才能被 Notebook 读取到。*

## 3. 导出代码为 Python 文件
由于加入了深层卷积网络, 此次训练过程可能会比较长, 我们不推荐在 Notebook 中进行长时间训练, 最好的方法是通过创建一个 GPU Job 后台训练模型。 
Notebook 中的代码是在 *.ipynb 文件下的，为之后创建 Job 和部署做准备，点击 <img src='http://imgbed.momodel.cn/5cc1a287e3067ceb154f0e43.jpg' width=3% height=3%> 将其转为 `.py` 格式的标准 python 代码。然后整理你的代码，完成测试后，即可进行下一步的操作。  

*如果你是从模版中创建的项目，我们已经为你准备好了一份整理好的 `How_Train_Model.py` 文件, 你可以从左侧 'Files' 文件目录中双击查看, 并直接进行下一步。*  

## 4. 创建 GPU Job 训练模型
点击 Python 编辑器上方的 <img src='http://imgbed.momodel.cn/row.png' width=3% height=3%>创建 Job 任务， 选择 `GPU 机器`创建 Job，我们可以选择为 Job 输入一个容易辨识名字，当然也可以选择不输入，系统会默认生成。您也可以创建 `Notebook 控制台` 或 `CPU 机器` 形式的 Job ，这需要根据您训练的模型特点选择。

<img src='http://imgbed.momodel.cn/GPU.gif' width=80% height=50%>

## 5. 查看 Job 运行进程
成功创建 Job 后, 可以在左侧 JOBS 栏中查看运行状态和运行日志，或者您也可以在项目详情页的`任务`栏中查看任务, Job 运行成功或失败您都可以收到通知消息。当然您也可以通过终止按钮终止训练过程。

如果您的训练状态显示为沙漏等待图标 (queuing)，说明当前 GPU 资源被占用正在排队等待。如果显示为运行中 (running) 说明 Job 正在运行。如果显示为运行失败 (failed) 则说明 Job 因为运行错误而中断，请检查您的代码。在训练过程中和训练完成后您都可以点击`查看日志`查看运行日志。以下展示了 Job 运行过程中的三种状态。

<img src='http://imgbed.momodel.cn/prepare.png' width=30% height=30%>
<img src='http://imgbed.momodel.cn/running.png' width=30% height=30%>
<img src='http://imgbed.momodel.cn/complete.png' width=30% height=30%>

Job 运行成功后系统会弹出以下弹框提示 Job 运行完成，恭喜您已经学会了如何在 GPU 或 CPU 资源上训练您的第一个任务～

<img src='http://imgbed.momodel.cn/success.png' width=50% height=50%>


## 6. 训练指标的可视化

 我们会解析你的 Job 的输出行并自动将它们转换为训练指标,并进行可视化。
 
 你需要按照如下格式,一行一行的输出你的训练指标.
 
 ```python
 print('{"metric": "<choose_metric_name>", "value": <int_or_float>}')
 ```
 
 举例来说,你可以这样来记录 accuracy 的变化
 
```python
 print('{"metric": "accuracy", "value": 0.985}')
 # {"metric": "accuracy", "value": 0.95}
```

或者这样来记录 loss 的变化

```python
print('{{"metric": "loss", "value": {}}}'.format(loss))
# {"metric": "loss", "value": 2.2233}
```

实际上你的 value 值可以是任意的格式,但是我们目前的可视化只支持整型和浮点型数字。

我们默认使用输出的时间来作为 X-axis, 但是你也可以使用 step
或者 epoch 关键字.注意 step 和 epoch 必须是整型数值。

```python
print('{"metric": "<metric_name>", "value": <int_or_float>, "step": <int>}')
```

```python
print('{"metric": "<metric_name>", "value": <int_or_float>, "epoch": <int>}')
```

然后,可以在项目详情页的任务部分,查看可视化结果。
![](http://imgbed.momodel.cn/5cc1a288e3067ceb154f0e46.jpg)
