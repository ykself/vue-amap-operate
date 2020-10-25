# 写在开头
本文参考了https://elemefe.github.io/vue-amap/#/

# 版本信息:package.json
{
  "name": "vueamap",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build"
  },
  "dependencies": {
    "core-js": "^3.6.5",
    "sass": "^1.27.0",
    "sass-loader": "^10.0.4",
    "vue": "^2.6.11",
    "vue-amap": "^0.5.10",
    "vue-router": "^3.2.0",
    "vuex": "^3.4.0"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "~4.5.0",
    "@vue/cli-plugin-router": "~4.5.0",
    "@vue/cli-plugin-vuex": "~4.5.0",
    "@vue/cli-service": "~4.5.0",
    "vue-template-compiler": "^2.6.11"
  }
}

# 使用步骤
1、新建文件夹

2、将项目文件下载并全部放入到文件夹中

3、安装node.js   npm    yarn

4、在项目文件夹下使用cmd打开终端

5、输入npm run serve运行项目

# 注意：
当前程序运行的内容是在地图上显示多个点，并在点击标注点的时候出现文本提示框。如果需要其他功能请修改./components/map.vue文件。功能代码在./components/template.vue文件和./components/test.vue中。其中test.vue文件实现的功能就是点击标注点的时候出现文本提示框，template.vue文件实现的功能则是使用vue-amap实现地图经纬度显示、具体地址显示、卫星图、路网路况、在鼠标按下的地方添加标注点和添加多个标注点。

# 最后提示：
本人在CSDN上写的关于VUE-AMAP的两篇博客可能会让你受到一点启发，链接如下：
https://blog.csdn.net/qq_41674526/article/details/109229911  和  https://blog.csdn.net/qq_41674526/article/details/109271009
