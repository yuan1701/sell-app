# sell

> 一个外卖App

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build


###  hbuilder 打包成手机APP
# 1.修改config/index.js的build路径
    build: {
        assetsPublicPath: './',
    }
# 2.打包文件, 生成dist文件
    npm run build
# 3.打开hbuilder软件,打开/dist目录,右键将文件转化为APP模式，会出现一个mainifest.json文件，双击编辑app的名字，图标等信息

# 4.点击发行，发行为原生安装包

# 点击打包成apk文件，安装在手机上就可以用了
