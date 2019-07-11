# 利用 TensorBoard 可视化评估模型
你可以在训练营中的[平台功能教程](http://www.momodel.cn:8899/classroom/class?id=5c5696cd1afd9458d456bf54&type=doc)，选择`利用TensorBoard可视化评估模型`，来按照教程中的`.ipynb`文件指引边学边做，学习如何利用 TensorBoard 可视化评估模型。

## 1. 简介
我们集成了名为 TensorBoard 的可视化工具，来帮助你理解、调试和优化你的 TensorFlow 机器学习模型。
你可以使用 TensorBoard 来可视化你的机器学习模型的推理图（graph）结构，显示训练过程中准确率和损失函数的变化，绘制关于图形执行的量化指标等等。下面以手写数据集 MNIST 为例，来帮助你在 Mo 平台上更便捷的使用 TensorBoard 可视化工具。更多关于 TensorBoard 的信息请参考[官方文档](https://www.tensorflow.org/programmers_guide/summaries_and_tensorboard) 和 [官方Github开源项目](https://github.com/tensorflow/tensorboard)。你可以在新建应用项目时选择[从模版创建该教程项目](https://momodel.github.io/docs/#/zh-cn/%E5%B9%B3%E5%8F%B0%E6%95%99%E7%A8%8B?id=%E5%88%9B%E5%BB%BA%E6%95%99%E7%A8%8Bcreate-project)，按照指引进行相应的操作。

## 2. 撰写模型代码
导入手写数据集，定义神经网络图结构和训练过程，并在程序代码中添加 TensorBoard 总结指令，具体可以参考 TensorBoard [官方文档](https://www.tensorflow.org/guide/summaries_and_tensorboard?hl=zh-cn)。这里需要注意产生的 logs 文件要保存在 `./results/tb_results`文件夹。

```Python
import tensorflow as tf

# Import MNIST data
from tensorflow.examples.tutorials.mnist import input_data

mnist = input_data.read_data_sets("./data", one_hot=True)
print(mnist)
# Parameters
learning_rate = 0.01
training_epochs = 20
batch_size = 100
display_epoch = 1
# ！Important！
# *您需要将logs文件存储到`/results/tb_results`文件夹下*
logs_path = './results/tb_results/tutorial/'

# tf Graph Input
# mnist data image of shape 28*28=784
x = tf.placeholder(tf.float32, [None, 784], name='InputData')
# 0-9 digits recognition => 10 classes
y = tf.placeholder(tf.float32, [None, 10], name='LabelData')

# Set model weights
W = tf.Variable(tf.zeros([784, 10]), name='Weights')
b = tf.Variable(tf.zeros([10]), name='Bias')

# Construct model and encapsulating all ops into scopes, making
# Tensorboard's Graph visualization more convenient
with tf.name_scope('Model'):
    # Model
    pred = tf.nn.softmax(tf.matmul(x, W) + b) # Softmax
with tf.name_scope('Loss'):
    # Minimize error using cross entropy
    cost = tf.reduce_mean(-tf.reduce_sum(y*tf.log(pred), reduction_indices=1))
with tf.name_scope('SGD'):
    # Gradient Descent
    optimizer = tf.train.GradientDescentOptimizer(learning_rate).minimize(cost)
with tf.name_scope('Accuracy'):
    # Accuracy
    acc = tf.equal(tf.argmax(pred, 1), tf.argmax(y, 1))
    acc = tf.reduce_mean(tf.cast(acc, tf.float32))

# Initialize the variables (i.e. assign their default value)
init = tf.global_variables_initializer()

# Create a summary to monitor cost tensor
tf.summary.scalar("loss", cost)
# Create a summary to monitor accuracy tensor
tf.summary.scalar("accuracy", acc)
# Merge all summaries into a single op
merged_summary_op = tf.summary.merge_all()

# Start training
with tf.Session() as sess:

    # Run the initializer
    sess.run(init)

    # op to write logs to Tensorboard
    summary_writer = tf.summary.FileWriter(logs_path, graph=tf.get_default_graph())

    # Training cycle
    for epoch in range(training_epochs):
        avg_cost = 0.
        total_batch = int(mnist.train.num_examples/batch_size)
        # Loop over all batches
        for i in range(total_batch):
            batch_xs, batch_ys = mnist.train.next_batch(batch_size)
            # Run optimization op (backprop), cost op (to get loss value)
            # and summary nodes
            _, c, summary = sess.run([optimizer, cost, merged_summary_op],
                                     feed_dict={x: batch_xs, y: batch_ys})
            # Write logs at every iteration
            summary_writer.add_summary(summary, epoch * total_batch + i)
            # Compute average loss
            avg_cost += c / total_batch
        # Display logs per epoch step
        if (epoch+1) % display_epoch == 0:
            print("Epoch:", '%04d' % (epoch+1), "cost=", "{:.9f}".format(avg_cost))

    print("Optimization Finished!")

    # Test model
    # Calculate accuracy
    print("Accuracy:", acc.eval({x: mnist.test.images, y: mnist.test.labels}))
```

## 3. 利用 TensorBoard 查看模型结构和训练情况
点击左侧`Running Terminals and Kernels`栏，点击 TensorBoard 图标按钮<img src='http://imgbed.momodel.cn/TensorBoard icon.png' width=5% height=5%>打开 TensorBoard 网页界面。

<img src='http://imgbed.momodel.cn/TensorBoard.png' width=50% height=50%>

查看你的模型 Graph 和训练情况。

<img src='http://imgbed.momodel.cn/Tensorboard.gif' width=90% height=90%>
