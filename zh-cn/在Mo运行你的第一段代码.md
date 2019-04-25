# 在 Mo 运行你的第一段代码

## 1. 在训练营中点击创建项目
你可以在训练营中的[平台功能教程](http://www.momodel.cn:8899/classroom/class?id=5c5696cd1afd9458d456bf54&type=doc)，选择`在 Mo 运行你的第一行代码`，来按照教程中的`.ipynb`文件指引边学边做，熟悉Mo使用方法和开发流程。


<img src='http://imgbed.momodel.cn/5cc1a291e3067ceb154f0e5c.jpg' width=100% height=100%>

## 2. 在Mo运行你的第一行代码

你可以通过新建一个Notebook文件运行以下代码，也可以直接打开工程项目中的提供的Notebook文件直接运行。
```python
print('Hello, MO!')
```
你将看到
```python
Hello, MO!
```

## 3. 在Mo上你可以使用 Tensorflow

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


## 4. 在 Mo 上你可以方便地进行数据可视化
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

## 5. 在 Mo 上你可以方便使用 SKlearn 等机器学习工具包
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
