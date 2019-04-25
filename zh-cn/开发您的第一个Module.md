# 开发您的第一个Module

------

Module也称为模块，分为model和toolkit两类，model和toolkit最大的区别在于，model是可以在发布后由调用方进行二次训练的模型，toolkit是只能进行预测的模型。

下面我们来一起来创建您的第一个module吧。在workspace内新建模块，填写名称，填写描述，并选择类别为model，点击OK按钮。

![newquest](http://imgbed.momodel.cn/5cc1a2a2e3067ceb154f0e8d.jpg)

点击 Notebook按钮 进入工作间。左侧的file broswer显示了当前工作目录下的文件和文件夹。

![](http://imgbed.momodel.cn/5cc1a2a0e3067ceb154f0e85.jpg)

src文件夹内的checkpoint文件夹用来存放训练好的model，data文件夹用来存放训练用的数据，main.py是主文件，module_spec.yml用于定义模块的输入输出。
工作目录除了src文件夹，在requirements.txt内填写您在notebook内通过 `!pip install xxx` 命令安装的包，在README.md中使用markdown填写您的模型的详细说明。

![](http://imgbed.momodel.cn/5cc1a2a4e3067ceb154f0e92.jpg)

双击 main.py 文件，可以看到，其中的主要的方法有 `train`，`predict`，和 `load_model`。

![](http://imgbed.momodel.cn/5cc1a2a3e3067ceb154f0e90.jpg)

`train`和`predict`是必须完成的，在您部署成功并发布后，其他开发者可以调用`train`来训练模型，调用`predict`来使用模型进行预测。


编写`train`和`predict`方法。

![](http://imgbed.momodel.cn/5cc1a2a4e3067ceb154f0e93.jpg)

![](http://imgbed.momodel.cn/5cc1a29fe3067ceb154f0e84.jpg)

完成`train`和`predict`后，填写`train`和`predict`方法中所需要的输入参数以及`predict`的输出参数到yml文件中。这一方面便于我们对您的代码进行自动检查，另一方面也方便您自己和其他人更方便的使用您的module。

![](http://imgbed.momodel.cn/5cc1a2a3e3067ceb154f0e8e.jpg)

![](http://imgbed.momodel.cn/5cc1a2a0e3067ceb154f0e86.jpg)

代码编写完成后，点击deploy按钮开始进行部署。

![](http://imgbed.momodel.cn/5cc1a2a1e3067ceb154f0e88.jpg)

我们会自动的对您的代码进行一些测试，以确保您的代码能够正常运行。如果显示测试通过，可以开始进行部署了，如果出现了报错提示，建议您检查您的代码。
输入commit信息，选择deploy需要的必备文件，在公开moduel前，最好进行dev版本的测试，所以勾选发布为dev版本，点击确认，此时module已开始进行部署。

![](http://imgbed.momodel.cn/5cc1a2a2e3067ceb154f0e8b.jpg)

稍等片刻，部署成功后会收到系统通知。

![](http://imgbed.momodel.cn/5cc1a2a3e3067ceb154f0e8f.jpg)

待成功部署完成后，新建一个用于测试的app project，新建notebook，
并在右侧的module tab页中点击个人头像选择我的modules，在这里可以看到刚才部署完成的kmeans_cluser，

![](http://imgbed.momodel.cn/5cc1a2a4e3067ceb154f0e91.jpg)

![](http://imgbed.momodel.cn/5cc1a2a0e3067ceb154f0e87.jpg)

点击kmeans cluser进入详情页，这里，您将看到刚才在yml文件中定义的输入输出的参数。

![](http://imgbed.momodel.cn/5cc1a2a1e3067ceb154f0e89.jpg)

将此module引入您的代码，然后进行测试。
点击 Import Train 按钮，将此module引入您的app project，此时，note book 中将自动插入相关代码。`conf` 是此module训练所需的输入参数。 运行代码，并检查结果，可以看到代码能够正常运行。

![](http://imgbed.momodel.cn/5cc1a2a1e3067ceb154f0e8a.jpg)

点击 Import predict 按钮，对module的预测功能进行测试。

![](http://imgbed.momodel.cn/5cc1a2a2e3067ceb154f0e8c.jpg)

如果测试发现问题，返回module project进行修改。然后重新部署。
再次进行测试。测试通过后。就可以选择版本号进行发布了。





