# 在 Mo 运行你的第一段代码

## 1. 在训练营中点击平台功能教程
你可以在训练营中的[平台功能教程](http://www.momodel.cn:8899/classroom/class?id=5c5696cd1afd9458d456bf54&type=doc)，选择`在 Mo 运行你的第一行代码`，来按照教程中的`.ipynb`文件指引边学边做，熟悉Mo使用方法和开发流程。


<img src='https://imgbed.momodel.cn/yunxingdaima1.png' width=100% height=100%>

## 2. 在 Mo 运行你的第一行代码

你可以通过新建一个Notebook文件运行以下代码，也可以直接打开工程项目中的提供的Notebook文件直接运行。
```python
print('Hello, MO!')
```
你将看到
```python
Hello, MO!
```

## 3. 你可以使用 Tensorflow

 使用 Mo 可直接在浏览器中执行 TensorFlow 代码。下面的代码示例展示了两个矩阵相加的情况。
<img src='http://imgbed.momodel.cn/5cc1a292e3067ceb154f0e5d.jpg' width=50% height=50%>


```python
import tensorflow as tf
import numpy as np

with tf.Session():
  input1 = tf.constant(np.reshape(np.arange(1.0, 7.0, dtype=np.float32), (2, 3)))
  input2 = tf.constant(1.0, shape=[2, 3])
  output = tf.add(input1, input2)
  result = output.eval()
result
```
你将看到
```python
array([[2., 3., 4.],
       [5., 6., 7.]], dtype=float32)
```


## 4. 你可以方便地进行数据可视化
你可以使用 Python matplotlib 模块很方便的进行数据可视化。

```python
import matplotlib.pyplot as plt
import numpy as np
%matplotlib inline
x = np.arange(15)
y = [x_i + np.random.randn() for x_i in x]
a, b = np.polyfit(x, y, 1)
plt.plot(x, y, 'o', np.arange(15), a*np.arange(15)+b, '-');
```

<img src='http://imgbed.momodel.cn/5cc1a291e3067ceb154f0e5a.jpg' width=50% height=50%>

## 5. 你可以使用 SKlearn 等机器学习工具包
Mo 已安装了机器学习中经常会用到的库，例如 tensorflow，sklearn，pytorch等。
下面的代码展示了在Iris数据集上使用sklearn提供的logistic-regression classifier进行分类。

```python
print(__doc__)


# Code source: Gaël Varoquaux
# Modified for documentation by Jaques Grobler
# License: BSD 3 clause

import numpy as np
import matplotlib.pyplot as plt
from sklearn import linear_model, datasets
%matplotlib inline

# import some data to play with
iris = datasets.load_iris()
X = iris.data[:, :2]  # we only take the first two features.
Y = iris.target

h = .02  # step size in the mesh

logreg = linear_model.LogisticRegression(C=1e5)

# we create an instance of Neighbours Classifier and fit the data.
logreg.fit(X, Y)

# Plot the decision boundary. For that, we will assign a color to each
# point in the mesh [x_min, x_max]x[y_min, y_max].
x_min, x_max = X[:, 0].min() - .5, X[:, 0].max() + .5
y_min, y_max = X[:, 1].min() - .5, X[:, 1].max() + .5
xx, yy = np.meshgrid(np.arange(x_min, x_max, h), np.arange(y_min, y_max, h))
Z = logreg.predict(np.c_[xx.ravel(), yy.ravel()])

# Put the result into a color plot
Z = Z.reshape(xx.shape)
plt.figure(1, figsize=(4, 3))
plt.pcolormesh(xx, yy, Z, cmap=plt.cm.Paired)

# Plot also the training points
plt.scatter(X[:, 0], X[:, 1], c=Y, edgecolors='k', cmap=plt.cm.Paired)
plt.xlabel('Sepal length')
plt.ylabel('Sepal width')

plt.xlim(xx.min(), xx.max())
plt.ylim(yy.min(), yy.max())
plt.xticks(())
plt.yticks(())

plt.show()
```

<img src='http://imgbed.momodel.cn/5cc1a291e3067ceb154f0e5b.jpg' width=50% height=50%>

## 6. 你可以插入或分享代码块
代码块是具有某个特定功能的代码片段。当你在 Notebook 中进行程序开发时，如果想实现某个功能函数，可以先试着搜索一下代码块，可能会找到你想要的代码，不用再重新撰写。代码块设计的用意是为了减少不必要的重复代码编写。
<a name="FFtCI"></a>
### 6.1 代码块插入
在 Notebook 中选择你要插入代码块的 celll 点击左上角的 + 号按钮，打开搜寻代码块界面。

![image.png](https://imgbed.momodel.cn/yunxingdaima2.png)

在代码块查询界面，可以搜索你想要的代码块，查看代码块内容或者插入使用，当然你也可以看到插入和分享的代码块的历史记录，方便再次使用。

![分享代码块.gif](https://cdn.nlark.com/yuque/0/2019/gif/307794/1556435835668-38ea9e8d-270b-4678-a9f6-71c833df1551.gif#align=left&display=inline&height=768&name=%E5%88%86%E4%BA%AB%E4%BB%A3%E7%A0%81%E5%9D%97.gif&originHeight=768&originWidth=1536&size=7719500&status=done&width=1536)

<a name="6aAaw"></a>
### 6.2 代码块分享
如果你觉得某些代码比较好，可以把这些代码放到一个 cell 中点击右上角的分享按钮，分享到平台中，供别人使用。<br />![image.png](https://imgbed.momodel.cn/yunxingdaima3.png)

在分享界面，你需要填写代码块的名字，用来描述这部分代码的功能；也可以填写标签分类信息，便于别人检索使用。<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/307794/1556435948936-bd6fad5c-4066-46f9-a961-f66025e936d4.png#align=left&display=inline&height=432&name=image.png&originHeight=864&originWidth=1514&size=247352&status=done&width=757)

## 7. 你可以邀请好友进行协作
Mo Notebook 开发环境支持多人协作，你可以点击加号，在“添加协作成员”弹窗中邀请好友或同事共同进行项目研发。<br />![image.png](https://imgbed.momodel.cn/yunxingdaima4.png)

邀请成功后，你可以享受以下功能：
<a name="ZQRJL"></a>
### 7.1 即时同步编辑内容并查看对方编辑状态
![](https://cdn.nlark.com/yuque/0/2019/png/307794/1556435061620-6cc579c0-632c-43d3-bba6-9800954f98c1.png#align=left&display=inline&height=321&originHeight=658&originWidth=1530&status=done&width=746)

<a name="hhyWN"></a>
### 7.2 代码块定位与分享
（1）点击发送代码块按钮    
（2）填写消息
![](https://imgbed.momodel.cn/20190716170639.png)
### 7.3 通过私有聊天组进行实时交流
![image.png](https://cdn.nlark.com/yuque/0/2019/png/307794/1556435409360-ce4785ec-0d67-494f-a499-2667b7e6686e.png#align=left&display=inline&height=516&name=image.png&originHeight=1032&originWidth=2364&size=279698&status=done&width=1182)
<a name="7EVfj"></a>

