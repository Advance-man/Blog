# npm

node的包管理器，用于node依赖插件的管理；

查看npm版本：npm -v

更新npm至最新版本：npm install -g npm

### 安装依赖包：

1. 全局安装：使用管理员账户安装 npm install -g xxxx;
2. 开发环境依赖的安装："npm install -save-dev xxx '',并写入package.json的”devDependencies”中;
3. 项目的依赖安装：“npm install -save xxxx”,并写入package.json的”dependencies”中;

### 删除依赖包：

1. 【npm uninstall xxx】删除xxx依赖
2. 【npm uninstall xxx -g】删除全局依赖xxx

### 一次性卸载package.json中的所有依赖

```shell
npm uninstall `ls -1 node_modules | tr '/\n' ' '`
```

# cnpm

因为npm的插件是从国外服务器下载，下载可能会受网络影响造成下载失败，以及下载速度过慢，所以淘宝团队为了国内开发者的利益在国内搭建了一个npm的镜像服务器。

cnpm官网说：“这是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。”

### 安装cnpm，输入以下命令：

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

安装依赖包时可以使用cnpm，其他本地的操作场景可以使用npm；