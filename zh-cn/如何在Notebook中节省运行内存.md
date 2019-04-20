# 如何在 Notebook 中节省运行内存

## 1. 通过指定合适的数据类型减少内存
当我们在读取数据文件时，例如某一列的数据范围肯定在0~255之中，那么我们可以指定为np.uint8类型，如果不手动指定的话默认为np.int64类型，这之间的差距巨大。对于浮点数如果不指定数据类型，默认的精度是float64，但是通常情况下float32，甚至float16的精度就已经足够了。所以在读取数据时指定数据类型可以节省大量内存空间。
例如我们用 pandas 读取一个 csv 文件，分别对比两者的内存占用。

当不指定数据类型时
```python
import pandas as pd
data_1 = pd.read_csv('btc.csv')

# 查看数据类型及内存占用情况
data_1.info()
```
输出信息如下：
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3405857 entries, 0 to 3405856
Data columns (total 8 columns):
Timestamp            int64
Open                 float64
High                 float64
Low                  float64
Close                float64
Volume_(BTC)         float64
Volume_(Currency)    float64
Weighted_Price       float64
dtypes: float64(7), int64(1)
memory usage: 207.9 MB
```
当指定数据类型时
```python
# 指定数据类型
data_2 = pd.read_csv('btc.csv', dtype={'Open':np.float16, 'High':np.float16, 'Low':np.float16, 
                                       'Close':np.float16, 'Volume_(BTC)':np.float16, 
                                       'Volume_(Currency)':np.float16, 'Weighted_Price':np.float16})
data_2.info()
```
输出信息如下：
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3405857 entries, 0 to 3405856
Data columns (total 8 columns):
Timestamp            int64
Open                 float16
High                 float16
Low                  float16
Close                float16
Volume_(BTC)         float16
Volume_(Currency)    float16
Weighted_Price       float16
dtypes: float16(7), int64(1)
memory usage: 90.9 MB
```

从以上对比可以看出，在读取数据时，只通过指定合适的数据类型，就可以减少一倍多的内存空间，是一种简单有效的方法。另外在数据处理的过程中也要及时对数据进行类型转换，减少内存。例如字符串占的内存比数字要大很多，可用数字符号代替字符串。比如我有一个叫 "country" 的列，它的值有'China'， 'USA'， 'Italy'，那么我可以用一个 map 把这三个值分别映射到1，2，3。这样也可以节省内存。


## 2. 通过函数封装减少中间变量内存占用
Python 程序 在 Linux 或者 Mac 中，哪怕是 del 这个对象，Python 依旧不把内存还给系统，自己先占着直到进程销毁。正是由于这样的垃圾回收机制，所以会导致我们在数据处理的过程中产生大量的中间变量，浪费内存资源。一种方式是我们在数据处理过程中尽量减少赋值导致的 COPY, 修改时带上 inplace=True。如果某个中间变量一定要产生，并且我们使用后还需要释放的可以把多步数据处理过程封装到一个函数中，利用函数的内存释放机制避免产生中间变量。特别在使用 Jupyter Notebook 进行数据分析时，避免一步一步的处理方式，多采用函数封装的形式，甚至导入模块的步骤也可以封装到函数中。

以下示例对比在 Jupyter Notebook 中单步执行以下命令和封装成一个函数的内存占用情况：

单步执行时
```python
import psutil
import os
import pandas as pd

# 求解每列的最大最小
df = pd.read_csv('btc.csv')
min_df = df.min()
max_df = df.max()

# 查看内存占用情况
info = psutil.virtual_memory()
print(psutil.Process(os.getpid()).memory_info().rss / 1024 / 1024, 'MB')
```
输出当前使用的内存为 455.03515625 MB

把求解最大最小过程封装成一个函数时
```python
import psutil
import os

# 封装函数
def get_min_max():
    import pandas as pd
    df = pd.read_csv('btc.csv')
    min_df = df.min()
    max_df = df.max()
    return min_df, max_df

# 求解每列的最大最小
min_df, max_df = get_min_max()

# 查看内存占用情况
info = psutil.virtual_memory()
print(psutil.Process(os.getpid()).memory_info().rss / 1024 / 1024, 'MB')
```
输出当前使用的内存为 76.86328125 MB

以上过程中间变量 df 并不是我们所需要的，但在数据处理过程中会占用大量内存，由于 Python 的内存回收机制，在单步执行时即使用 del 命令删除某个中间变量，内存资源也无法释放，所以采用封装函数的方式是一种较好的方式。

## 3. 使用迭代器或生成器读取数据
采用迭代器或生成器的方式读取数据时，并没有真正读取数据，而是在调用 next() ，get_chunk()等函数时才会获取加载真正的数据。这样我们就可以分块读取处理数据，减少一次性加载数据时内存占用过大的问题。

以下示例读取大型 csv 文件时，可以采用迭代器的方式加载，加载数据时基本不占用内存，只有在调用 get_chunk 函数时才会加载指定数量数据。

```python
import pandas as pd
reader = pd.read_csv('btc.csv', iterator=True)

# 通过 get_chunk 函数获取指定数量的数据
df = reader.get_chunk(1000)
```

## 4. 其他减少内存占用的技巧
* 图片数据集读取时只存储图片名称的列表，在需要处理时再加载数据。
* 很多机器学习框架提供了控制内存占用的接口参数，例如，在 TensorFlow 中训练神经网络模型时可以控制 batch_size 的值，从而限制训练过程中的内存占用；在 Keras 中提供 fit_generator 函数，实现逐批次生成数据，按批次训练模型。

## 总结
内存的减少是靠牺牲时间资源或 CPU 资源来换取的，实际使用时要综合权衡。在进行数据分析处理时良好的数据处理思维可以大幅提高我们分析数据的效率，减少不必要的资源浪费。

