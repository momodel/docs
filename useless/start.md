# 快速开始


## 寻找App
进入market, 浏览app列表并通过category和tags进行筛选， 或者使用顶部搜索框进行筛选。

## 第一次使用App

We’ll make our first call with the demo algorithm “Hello”. This algorithm takes an input of a string (preferably your name!) and returns a greeting addressed to the input.

Calling the algorithm is as simple as making a curl request. For example, to call the demo/Hello algorithm, simply run a cURL request in your terminal:

```$xslt
curl -X POST -d '"YOUR_USERNAME"' -H 'Content-Type: application/json' -H 'Authorization: Simple YOUR_API_KEY' https://api.algorithmia.com/v1/algo/demo/Hello/
```

You can also use one of the clients to make your call. See below for examples or visit one of the Client Guides for details on how to call algorithms and work with data in your language of choice.

```$xslt
import Algorithmia

input = "YOUR_USERNAME"
client = Algorithmia.client('YOUR_API_KEY')
algo = client.algo('demo/Hello/')
print algo.pipe(input)
```

## 返回的结果
Each algorithm returns a response in JSON. It will include the "result" as well as metadata about the API call you made. The metadata will include the content_type as well as a duration.
```$xslt
curl -X POST -d '"YOUR_USERNAME"' -H 'Content-Type: application/json' -H 'Authorization: Simple API_KEY' https://api.algorithmia.com/v1/algo/demo/Hello/

{ "result": "Hello YOUR_USERNAME",
  "metadata": {
     "content_type": "text",
     "duration": 0.000187722
  }
}
```
he duration is the compute time of the API call into the algorithm. This is the time in seconds between the start of the execution of the algorithm and when it produces a response. Because you are charged on the compute time of the API call, this information will help you optimize your use of the API.

For more information about pricing, check out our Pricing Guide


## 发布需求
<!--
这些是注释文本，不会显示
## 如何寻找可用的module/app/dataset
1. 进入market选择module/app/dataset，输入搜索关键字

## 如何寻找别人提出的app需求
1. 进入request

## 如何创建第一个app

# APP开发
	notebook相关功能介绍
	如何调用别人写好的module、dataset
	如何将做好的项目deploy
	如何用写好的app回答需求


# 发布需求
	在哪里可以发布需求
	发布需求的类型及注意事项
		module
		dataset
# 交流讨论
	如何回答别人的问题

-->
