---
title: webPack 文件打包与启动服务配置
date: 2019-09-20 23:40:49
---
# webPack 文件打包与启动服务配置

## 1. 打包

```
npm run build						在webpack的项目中打包
```

### 会生成dist文件 内部包含 css/js等static样式 一级index.htmlwenjian

## 2. 配置启动服务

```
npm install express-generator -g
```

### 安装 express-generator 用于加载启动文件

```
express expressDemo
```

### 生成一个webpack加载文件夹

```
cd expressDemo				进入文件目录
npm install
run the app:
     > SET DEBUG=expressdemo:* & npm start			用于启动app 但对于h5的文件可以执行以下方法
     
```

### 将 dist内部所以样式已经js 和index 文件复制到expressdemo项目的pubilc文件夹内

启动localhost:3000服务;

npm start

## 3.注意事项

```
需要注意的的事

如果发现文件路劲不对

1.使用mode：‘history’修改配置：

config里面的index.js找到env: require('./prod.env')模块(新版本build: {),将下面的assetsPublicPath: '/'  改成assetsPublicPath: './'   然后将dist放在服务器上就可以运行了，建议使用这种方法，灵活改动少

2.不使用mode：‘history’

手动添加：dist包不是服务器跟目录，在index.htm里手动给js和css添加dist目录即可/dist/；





```

#### 3.配置文件路径![201808161537544](..\assets\images\info\201808161537544.png)





run build生成的dist中动画未生效

解决方法：在vue项目中找到build文件夹下的vue-loader.conf.js，将extract：isProduction  改为extract：false