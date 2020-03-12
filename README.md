### 存储自己的一些想法

客户端热更实现

    首先需要有版本信息确定当前版本需要更新什么东西，所以需要几个文件来存储需要更新的内容

    1、首先需要一个基础db文件来存储基础版本的所有数据，以此为基准来获取有哪些文件变更。
    2、需要一个版本db文件来确定哪些文件需要被下载。
    3、需要一个version文件来控制是否有新的版本db文件需要下载

    接下来走热更流程
        首先，玩家启动游戏后，会请求一个web地址，获取到version文件后，对比本地的version文件中的version信息，当本地版本号低于服务器请求到的version版本信息后，此时需要请求服务器获取版本db文件。
