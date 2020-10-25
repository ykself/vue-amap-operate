# 在VUE中导入高德地图模块并操作使用

## 1、创建VUE项目

​	前提：已经配置好了环境，安装了node.js  npm  yarn

​	1、安装vue-cli  全局安装！！！需要vue为3.0以上版本

​	2、新建文件夹存放工程，不要使用vue-amap命名，否则之后生成时会出错

​	3、在cmd中键入vue ui 命令以新建项目，按照指导一步一步进行新建。

​	4、安装完成之后在cmd内键入npm run dev看是否成功

## 2、在vue项目中导入高德地图组件

​	前提：第一小节的VUE项目已经创建完成并测试通过

​	1、安装vue-amap：在项目文件夹下的cmd中键入npm install vue-amap --save

​	2、在高德地图上申请一个KEY，此处本人用的KEY为：7cd24a352582bb2983a4cce99b1915d8

​	3、将项目文件夹用VSCode打开

​	4、添加代码，导入地图组件  

​		**/src/main.js**

```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import VueAMap from 'vue-amap'


Vue.use(VueAMap);
VueAMap.initAMapApiLoader({
  key: '7cd24a352582bb2983a4cce99b1915d8',
  plugin: ['AMap.Autocomplete', 'AMap.PlaceSearch', 'AMap.Scale', 'AMap.OverView', 'AMap.ToolBar', 'AMap.MapType', 'AMap.PolyEditor', 'AMap.CircleEditor'],
  // 默认高德 sdk 版本为 1.4.4
  v: '1.4.4'
});
Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')

```

​		**/src/App.vue**    或者    **其他任一.vue文件中**

​	**注意**：如果在其他的.vue文件中添加时请删除代码中原来就在App.vue文件中的内容。

​				使用的具体步骤可以参考第四章：**4、在App.vue中调用其他.vue文件**

```vue
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>

    <div class="amap-wrapper">
      <el-amap
        class="amap-box"
        vid="amap-vue"
        :zoom="zoom"
        :center="center"
      ></el-amap>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      zoom: 11,
      center: [112.5862, 37.8268],
    };
  },
};
</script>

<style>
.amap-wrapper {
  width: 800px;
  height: 800px;
  float: right;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

```

5、在文件夹目录的cmd终端中输入npm run dev即可观看网页效果，如果提示找不到dev，则可以使用npm run看可以用其他的指令打开，比如npm run serve

## 3、修改地图尺寸

在刚刚添加的.vue文件的<style></style>中具有以下一段代码，可修改此代码以修改地图大小。

```vue
.amap-wrapper {
  width: 800px;
  height: 800px;
  float: right;
}
```

其中width为地图的宽度，height为地图发高度，float表示地图靠左边还是靠右边

width可以修改为100%以使地图的宽度等于页面的宽度，但是height参数不能修改为100%或者其他百分值，否则会无法显示地图，经测试在height为975px时是近似等于页面大小，width也可以修改其值为 px。

​	

## 	4、在App.vue中调用其他.vue文件

添加组件可以参考下面的链接：https://www.jb51.net/article/156127.htm

1、创建map.vue文件存放地图程序

2、给子组件map.vue命名一个全局id

```vue
export default {
  name: "mmap"
}
```

3、返回App.vue组件。先引入组件，之后再渲染界面。

```vue
<script>
    import mmap from "./components/map"
    export default {
        name:'App',
        components: {
            mmap
        }
    }
</script>
```



4、此时就可以在App.vue文件中调用map.vue文件了。

```vue
<mmap></mmap>
```

5、最终的代码：

**/components/map.vue**

```vue
<template>
    <div class="amap-wrapper">
      <el-amap
        class="amap-box"
        vid="amap-vue"
        :zoom="zoom"
        :center="center"
      ></el-amap>
    </div>
</template>


  <script>

export default {
  name: "mmap",
  data() {
    return {
      zoom: 11,
      center: [112.5862, 37.8268],
    };
  },
};
</script>

  <style>
.amap-wrapper {
  width: 100%;
  height: 800px;
  float: right;
}
</style>


```

**App.vue**

```vue
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>

    <mmap></mmap>
  </div>
</template>


<script>
    import mmap from "./components/map"
    export default {
        name:'App',
        components: {
            mmap
        }
    }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

```

## 5、点击地图显示经纬度和具体地址

参考链接：https://elemefe.github.io/vue-amap/#/zh-cn/examples/base/amap

1、添加Geocoder以使用坐标转换服务

**main.js**

```javascript
window.VueAMap.initAMapApiLoader({
  key: 'YOUR_CODE',
  plugin: [... 'Geocoder']
});
```

2、修改map.vue中的代码以增加该功能

**map.vue**

```vue
<template>
  <div class="amap-page-container">
    <el-amap
      vid="amapDemo"
      :center="center"
      :zoom="zoom"
      :events="events"
      class="amap-demo"
    >
    </el-amap>

    <div class="toolbar">
      position: [{{ lng }}, {{ lat }}] address: {{ address }}
    </div>
  </div>
</template>

  <style>
.amap-page-container {
  width: 100%;
  height: 500px;

}
</style>

  <script>

export default {
  name : 'mmap',
  data: function () {
    let self = this;

    return {
      zoom: 12,
      center: [121.59996, 31.197646],
      address : '',
      // amapManager,
      events: {
        init(o) {
          let marker = new AMap.Marker({
            position: [121.59996, 31.197646],
          });

          marker.setMap(o);
        },
        click(e) {
          let { lng, lat} = e.lnglat;
          self.lng = lng;
          self.lat = lat;

          //这里是通过高德地图 SDK 完成的
          var geocoder = new AMap.Geocoder({
            radius: 1000,
            extensions: 'all'

          });
          var position_xy=[lng, lat];            
          console.log(position_xy);
          geocoder.getAddress(position_xy, function(status, result){
            console.log(result)
            if (status === 'complete' && result.info === 'OK') {
              
              if (result && result.regeocode) {
                self.address = result.regeocode.formattedAddress;
                self.$nextTick();
              }
            }
          });
        }
      },
      lng :0,
      lat :0
    };
  }
};
</script>
```

3、踩过的坑

​	3.1 如果Geocoder不行可以试试AMap.Geocoder

​	3.2 代码中有一段控制台输出result   -----   console.log(position_xy);	按F12之后看控制台的输出参数，如果显示的是INVALID_USER_KEY则检查main.js中的KEY是否是正确的，因为该提示是告诉你**开发者发起请求时，传入的key不正确或者过期** 

4、效果展示

![image-20201023202349609](C:\Users\SANSAN\AppData\Roaming\Typora\typora-user-images\image-20201023202349609.png)

## 6、添加卫星图和路网路况

参考链接：https://elemefe.github.io/vue-amap/#/zh-cn/base/amap

1、在map.vue的<template></template>中添加组件amapManager和plugin。其中plugin是为了导入卫星图和路网，amapManager为地图管理对象

2、直接放代码

**map.vue**

```vue
<template>
  <div class="amap-page-container">
    <el-amap
      ref="map"
      vid="amapDemo"
      :center="center"
      :zoom="zoom"
      :events="events"
      :amap-manager="amapManager"
      :plugin="plugin"
      class="amap-demo"
    >
    </el-amap>

    <div class="toolbar">
      position: [{{ lng }}, {{ lat }}] address: {{ address }}
    </div>
  </div>
</template>

  <style>
.amap-page-container {
  width: 100%;
  height: 500px;

}
</style>

  <script>
// NPM 方式
import {AMapManager} from "vue-amap";
import VueAMap from "vue-amap";

let amapManager = new VueAMap.AMapManager();

export default {
  name : 'mmap',
  data: function () {
    let self = this;

    return {
      //*******************地图和组件初始化设置**********************//
      amapManager,
      zoom: 12,
      center: [121.59996, 31.197646],

      markers: [],
      address : '',
      
      
      // amapManager,
      events: {
        init(o){
          o.getCity((result) => {
            //result 为地图初始化时所在位置的行政区域信息
            console.log(result);
          });
        },

        //*******************鼠标点击产生确定地址**********************//
        click(e) {
          let { lng, lat} = e.lnglat;
          self.lng = lng;
          self.lat = lat;
          //这里是通过高德地图 SDK 完成的
          var geocoder = new AMap.Geocoder({
            radius: 1000,
            extensions: 'all'

          });
          var position_xy=[lng, lat];            
          console.log(position_xy);

          geocoder.getAddress(position_xy, function(status, result){
            //console.log(result)
            if (status === 'complete' && result.info === 'OK') {
              
              if (result && result.regeocode) {
                self.address = result.regeocode.formattedAddress;
                self.$nextTick();
              }
            }
          });
        },
        //鼠标点到地图上时触发事件
        // mousedown(){
        //   var a = "hahaha";
        //   console.log (a);
        // }
      
      },
      lng :0,
      lat :0,
      //*******************卫星+路网+路况**********************//
      plugin: [
        "ToolBar",
        {
          pName: "MapType",
          defaultType: 0,
          events: {
            init(o) {
              //console.log(o);
            },
          },
        },
      ]
    };
  }

};
</script>
```

## 7、在鼠标按下的地方添加标注点

参考链接https://elemefe.github.io/vue-amap/#/zh-cn/coverings/marker

**设计目标：显示地图，在地图中按一下就可以在鼠标点击处添加标志点，在鼠标点击另外的地方时原先的标注点消失，即一直保持地图上只有一个标注点。**

1、在第五节点击地图显示经纬度和具体地址已经获取了鼠标按下时该点的经纬度和具体地址，只是没有显示该点。

2、如果需要显示点需要将点的数据写入到markers里面，添加的的格式如下：

```vue
          let marker = {
            position: [lng, lat],
          };
```

3、将该点使用javascript的push方法存入到markers中，代码如下：

```vue
self.markers.push(marker);
```

4、因为每次点击都需要将上一个的标注点删除，故在添加标注点之前需要判断makers数组的长度是否为0，不为0则减一，即删除前一个标注点。

```vue
          //*****************在鼠标点击处添加标注点*******************//
          if (self.markers.length);
          self.markers.splice(self.markers.length - 1, 1);
```

5、放代码：

**map.vue**

```vue
<template>
  <div class="amap-page-container">
    <el-amap
      vid="amapDemo"
      :center="center"
      :zoom="zoom"
      :events="events"
      :amap-manager="amapManager"
      :plugin="plugin"
      class="amap-demo"
    >
      <!-- 遍历显示 -->
      <el-amap-marker
        v-for="marker in markers"
        :position="marker.position"
        :events="marker.events"
      ></el-amap-marker>
    </el-amap>

    <div class="toolbar">
      position: [{{ lng }}, {{ lat }}] address: {{ address }}
    </div>
  </div>
</template>

  <style>
.amap-page-container {
  width: 100%;
  height: 500px;
}
</style>

  <script>
// NPM 方式
import { AMapManager } from "vue-amap";
import VueAMap from "vue-amap";
//import AMap from 'AMap'

let amapManager = new VueAMap.AMapManager();

export default {
  name: "mmap",
  data: function () {
    let self = this;

    return {
      //*******************地图和组件初始化设置**********************//
      amapManager,
      zoom: 12,
      center: [121.59996, 31.197646],
        
      address: "",
      count: 1,
      markers: [],
      events: {
        init(o) {
          o.getCity((result) => {
            //result 为地图初始化时所在位置的行政区域信息
            console.log(result);
          });
        },

        //*******************鼠标点击产生确定地址**********************//
        click(e) {
          let { lng, lat } = e.lnglat;
          self.lng = lng;
          self.lat = lat;
          //这里是通过高德地图 SDK 完成的
          var geocoder = new AMap.Geocoder({
            radius: 1000,
            extensions: "all",
          });
          var position_xy = [lng, lat];
          console.log(position_xy);

          //*****************在鼠标点击处添加标注点*******************//
          if (self.markers.length);
          self.markers.splice(self.markers.length - 1, 1);
          let marker = {
            position: [lng, lat],
          };
          self.markers.push(marker);

          geocoder.getAddress(position_xy, function (status, result) {
            //console.log(result)
            if (status === "complete" && result.info === "OK") {
              if (result && result.regeocode) {
                self.address = result.regeocode.formattedAddress;
                console.log(self.address);
                self.$nextTick();
              }
            }
          });
        },
      },

      lng: 0,
      lat: 0,
      //*******************卫星+路网+路况**********************//
      plugin: [
        "ToolBar",
        {
          pName: "MapType",
          defaultType: 0,
          events: {
            init(o) {
              //console.log(o);
            },
          },
        },
      ],
    };
  },

  //*******************在地图上添加点**********************//
  methods: {
  },
};
</script>
```



## 8、在地图上显示多个标注点

**设计目标：创建多个点坐标，并将这些点显示在地图上**

1、使用规定格式创建多个点坐标对象，代码如下：

```vue
      data_position : [
        // 只能使用position
        {position: [121.59996, 31.197646]},
        {position: [121.69996, 31.197646]},
        {position: [121.79996, 31.197646]},
        {position: [121.89996, 31.197646]},
        {position: [121.99996, 31.197646]},
      ],
```

2、将数据全部存入到markers内，使用push只能将最后一个数据放入到markers中，故添加for循环将数据全部推入到markers中，代码如下：

```vue
      for (let x of this.data_position)
      {
        this.markers.push(x);
      }
      console.log(this.markers);
```

3、添加按钮优化效果，并添加移除点按钮，先判断markers是否为空，不为空则每次按一个移除一个点，代码如下：

```vue
      if (!this.markers.length) return;
      this.markers.splice(this.markers.length - 1, 1);
```

4、全部代码如下：

**map.vue**

```vue
<template>
  <div class="amap-page-container">
    <el-amap
      vid="amapDemo"
      :center="center"
      :zoom="zoom"
      :events="events"
      :amap-manager="amapManager"
      :plugin="plugin"
      class="amap-demo"
    >
      <!-- 遍历显示 -->
      <el-amap-marker
        v-for="marker in markers"
        :position="marker.position"
        :events="marker.events"
      ></el-amap-marker>
    </el-amap>

    <div class="toolbar">
      position: [{{ lng }}, {{ lat }}] address: {{ address }}
      <button type="button" name="button" v-on:click="addMarker">
        add markers
      </button>
      <button type="button" name="button" v-on:click="removeMarker">
        remove markers
      </button>
    </div>
  </div>
</template>

  <style>
.amap-page-container {
  width: 100%;
  height: 500px;
}
</style>

  <script>
// NPM 方式
import { AMapManager } from "vue-amap";
import VueAMap from "vue-amap";
//import AMap from 'AMap'

let amapManager = new VueAMap.AMapManager();

export default {
  name: "mmap",
  data: function () {
    let self = this;

    return {
      //*******************地图和组件初始化设置**********************//
      amapManager,
      zoom: 12,
      center: [121.59996, 31.197646],
      data_position : [
        // 只能使用position
        {position: [121.59996, 31.197646]},
        {position: [121.69996, 31.197646]},
        {position: [121.79996, 31.197646]},
        {position: [121.89996, 31.197646]},
        {position: [121.99996, 31.197646]},
      ],

      address: "",
      markers: [],
      events: {
        init(o) {
          o.getCity((result) => {
            //result 为地图初始化时所在位置的行政区域信息
            console.log(result);
          });
        },

        //*******************鼠标点击产生确定地址**********************//
        click(e) {
          let { lng, lat } = e.lnglat;
          self.lng = lng;
          self.lat = lat;
          //这里是通过高德地图 SDK 完成的
          var geocoder = new AMap.Geocoder({
            radius: 1000,
            extensions: "all",
          });
          var position_xy = [lng, lat];
          console.log(position_xy);

          //*****************在鼠标点击处添加标注点*******************//
          if (self.markers.length);
          self.markers.splice(self.markers.length - 1, 1);
          let marker = {
            position: [lng, lat],
          };
          self.markers.push(marker);

          geocoder.getAddress(position_xy, function (status, result) {
            //console.log(result)
            if (status === "complete" && result.info === "OK") {
              if (result && result.regeocode) {
                self.address = result.regeocode.formattedAddress;
                console.log(self.address);
                self.$nextTick();
              }
            }
          });
        },

        // //鼠标在点标记上按下时触发事件
        // mousedown() {
        //   var a = "hahaha";
        //   console.log(a);
        // },
      },

      lng: 0,
      lat: 0,
      //*******************卫星+路网+路况**********************//
      plugin: [
        "ToolBar",
        {
          pName: "MapType",
          defaultType: 0,
          events: {
            init(o) {
              //console.log(o);
            },
          },
        },
      ],
    };
  },

  //*******************在地图上添加点**********************//
  methods: {
    addMarker() {
      // console.log(this.data_position);
      for (let x of this.data_position)
      {
        this.markers.push(x);
      }
      console.log(this.markers);
    },
    removeMarker() {
      if (!this.markers.length) return;
      this.markers.splice(this.markers.length - 1, 1);
    },
  },
};
</script>
```



## 9、给点添加文本信息

参考链接https://www.jianshu.com/p/1e90f255f962

前提：安装sass和sass-loader

具体步骤：在终端中输入npm install sass和npm install sass-loader

**设计目标：在鼠标点击到标注点上时会出现文本信息，并出现链接，可以用来跳转等**

代码：

**map.vue**

```vue
<template>
  <div class="amap-page-container">
    <div class="toolbar">当前坐标: [{{ lng }}, {{ lat }}]</div>
    <el-amap
      vid="amapDemo"
      :center="center"
      :zoom="zoom"
      class="amap-demo"
      :events="events"
      pitch-enable="false"
      :plugin="plugin"
    >
      <el-amap-marker
        v-for="(marker, index) in markers"
        :key="index"
        :events="marker.events"
        :position="marker.position"
      />
      <el-amap-info-window
        v-if="window"
        :position="window.position"
        :visible="window.visible"
        :content="window.content"
        :offset="window.offset"
        :close-when-click-map="true"
        :is-custom="true"
      >
        <div id="info-window">
          <p>{{ window.address }}</p>
          <div class="detail" @click="checkDetail">查看详情</div>
        </div>
      </el-amap-info-window>
    </el-amap>
  </div>
</template>

<script>
// NPM 方式
import { AMapManager } from "vue-amap";
import VueAMap from "vue-amap";
//import AMap from 'AMap'

let amapManager = new VueAMap.AMapManager();

export default {
  name: "mmap",
  data: function () {
    let self = this;
    return {
      amapManager,
      data_position: [
        // 只能使用position
        {
          //必须使用字符串形式，数组形式无法使用
          position: "121.59996, 31.197646",
          address: "first position",
        },
        {
          position: "121.69996, 31.197646",
          address: "second position",
        },
        {
          position: "121.79996, 31.197646",
          address: "third position",
        },
        {
          position: "121.89996, 31.197646",
          address: "forth position",
        },
        {
          position: "121.99996, 31.197646",
          address: "fifth position",
        },
      ],
      zoom: 10,
      center: [121.599197, 31.205379],
      expandZoomRange: true,
      markers: [],
      windows: [],
      window: "",
      address: "",
      events: {
        init(o) {
          o.getCity((result) => {
            //result 为地图初始化时所在位置的行政区域信息
            console.log(result);
          });
        },
        click(e) {
          const { lng, lat } = e.lnglat;
          self.lng = lng;
          self.lat = lat;
        },
      },
      lng: 0,
      lat: 0,
      plugin: [
        "ToolBar",
        {
          pName: "MapType",
          defaultType: 0,
          events: {
            init(o) {
              //console.log(o);
            },
          },
        },
      ],
    };
  },
  mounted() {
    this.point();
  },
  methods: {
    point() {
      const markers = [];
      const windows = [];
      const that = this;
      this.data_position.forEach((item, index) => {
        markers.push({
          position: item.position.split(","),
          // icon:item.url, //不设置默认蓝色水滴
          events: {
            mousedown() {
              // 方法：鼠标移动到点标记上，显示相应窗体
              that.windows.forEach((window) => {
                window.visible = false; // 关闭窗体
              });
              that.window = that.windows[index];
              that.$nextTick(() => {
                that.window.visible = true;
              });
            },
          },
        });
        windows.push({
          position: item.position.split(","),
          isCustom: true,
          offset: [115, 55], // 窗体偏移
          showShadow: false,
          visible: false, // 初始是否显示
          address: item.address,
        });
      });
      //  加点
      this.markers = markers;
      // 加弹窗
      this.windows = windows;
    },
    checkDetail() {
      // //可以直接将链接放在此函数内
      var a = "hahaha";
      console.log(a);
      alert("点击了查看详情");
    },
  },
};
</script>

<style lang="scss" scoped>
.amap-demo {
  height: 450px;
}
.amap-page-container {
  position: relative;
}
#info-window {
  width: 211px;
  height: 146px;
  margin-left: 30px;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 4px;
  position: relative;
  overflow: hidden;
  .detail {
    width: 100%;
    height: 24px;
    color: #fff;
    background-color: #1a73e8;
    position: absolute;
    bottom: 0;
    font-size: 12px;
    line-height: 24px;
    text-align: center;
    cursor: pointer;
  }
}
</style>
```



