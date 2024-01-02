# 把模型转换为 TensorFlow Lite 格式

你可以在训练营中的[平台功能教程](http://www.momodel.cn:8899/classroom/class?id=5c5696cd1afd9458d456bf54&type=doc)，选择`把模型转换为 TensorFlow Lite 格式`，来按照教程中的`.ipynb`文件指引边学边做，学习如何利用 TensorBoard 可视化评估模型。

## 1. 简介

TensorFlow Lite 是 TensorFlow 针对移动和嵌入式设备的轻量级解决方案。它赋予了这些设备在终端本地运行机器学习模型的能力，从而不再需要向云端服务器发送数据。这样一来，不但节省了网络流量、减少了时间开销，而且还充分帮助用户保护自己的隐私和敏感信息。本部分将指引你把训练好的机器学习模型转换为手机或嵌入式设备可以使用的 TensorFlow Lite 格式。

## 2. 准备训练好的模型

当转换为 TensorFlow Lite 格式时，需要将模型和权重都保存在同一个文件中。如果你在 Tensorflow 框架下构建并训练你的模型，你需要将模型保存为 SavedModel 格式。SavedModel 是一种独立于语言且可恢复的序列化格式，使较高级别的系统和工具可以创建、使用和转换 TensorFlow 模型。创建 SavedModel 的最简单方法是使用 ```tf.saved_model.simple_save``` 函数。更多关于SavedModel 的信息请见[官方文档](https://www.tensorflow.org/programmers_guide/saved_model)。

如果你在 Tensorflow Keras 框架下构建并训练你的模型，你可以使用 ```tf.keras.models.save_model``` 函数将模型和权重都保存在同一个模型文件中，这里我们通过构建一个keras简单模型并且保存训练好的模型，得到```.h5```后缀的模型文件。

```Python
import numpy as np
import tensorflow as tf

# Generate tf.keras model.
model = tf.keras.models.Sequential()
model.add(tf.keras.layers.Dense(2, input_shape=(3,)))
model.add(tf.keras.layers.RepeatVector(3))
model.add(tf.keras.layers.TimeDistributed(tf.keras.layers.Dense(3)))
model.compile(loss=tf.keras.losses.MSE,
              optimizer=tf.keras.optimizers.RMSprop(lr=0.0001),
              metrics=[tf.keras.metrics.categorical_accuracy],
              sample_weight_mode='temporal')

x = np.random.random((1, 3))
y = np.random.random((1, 3, 3))
model.train_on_batch(x, y)
model.predict(x)
```

<img src='http://imgbed.momodel.cn/5cc1a28ae3067ceb154f0e4b.jpg' width=90% height=90%>

然后通过以下代码保存模型，会在当前文件目录中生成 'keras_model.h5' 文件。

```Python
# Save tf.keras model in HDF5 format.
keras_file = "keras_model.h5"
tf.keras.models.save_model(model, keras_file)
```

## 3. 将模型转换为 TensorFlow Lite 格式

选择刚刚保存的`.h5`后缀的模型文件，然后右键选择 ```Convert To TensorLite``` 就可以得到转换之后的 TensorFlow Lite 模型文件。

<img src='https://imgbed.momodel.cn/zhuancunlite.png' width=50% height=50%>

## 4. 下载转换后的模型并将其嵌入你的本地程序

模型转换完成后，在左侧的文件列表中便可以看到以转换后的以 ```.tflite``` 为后缀的文件。
右键此文件，并点击下载，即可将其下载到你的本地电脑中。然后你可以将其嵌入到你的 Android app、iOS app 或 Raspberry Pi中。更多信息请参阅[TensorFlow官方文档](https://www.tensorflow.org/lite/devguide)
