# Vue3核心技术学习



## 学习资料：

b站视频📺：

- [【入门篇】从零开始学习Vue3核心技术知识](https://www.bilibili.com/video/BV1cT411P7to/?spm_id_from=333.788&vd_source=f25f3aff6cb51f0344e3819804d8f007)



## 学习笔记

### 01.创建应用和插值表达式

借助 script 标签直接通过 CDN 来使用 Vue

创建应用：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>01</title>
    <script src="https://unpkg.com/vue@3"></script>
  </head>
  <body>
    <div id="app"></div>

    <script>
      // createApp({})可选参数是一个对象
      // 创建应用后，还要告诉vue：新的应用实例需要挂载到DOM的哪个位置
      // 注意：应用实例知识挂载到DOM的一个标签上
      Vue.createApp({}).mount("#app");
    </script>
  </body>
</html>
```

vue运行成功：

![image-20221207163723356](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221207163723356.png)





插值表达式：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>01</title>
    <script src="https://unpkg.com/vue@3"></script>
  </head>
  <body>
    <div id="app">
      <!-- 插值表达式引用data函数返回的对象里的数据 -->
      <h1>{{title}}</h1>
    </div>

    <script>
      // createApp({})可选参数是一个对象
      // 创建应用后，还要告诉vue：新的应用实例需要挂载到DOM的哪个位置
      // 注意：应用实例知识挂载到DOM的一个标签上
      Vue.createApp({
        data() {
          // 返回一个对象
          return {
            // 对象里存放数据
            title: "零食清单",
          };
        },
      }).mount("#app");
    </script>
  </body>
</html>
```

> data(){} data是函数不是对象。

运行结果：

![image-20221207164430485](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221207164430485.png)



### 02.v-for,v-bind,v-model

02.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>01</title>
    <script src="https://unpkg.com/vue@3"></script>
  </head>
  <body>
    <div id="app">
      <h1>{{ title }}</h1>
    </div>

    <script>
      Vue.createApp({
        data() {
          return {
            title: "零食清单",
            foods: [
              {
                id: 1,
                name: "原味鱿鱼丝",
                image: "./images/原味鱿鱼丝.png",
                purchased: false,
              },
              {
                id: 2,
                name: "辣味鱿鱼丝",
                image: "./images/辣味鱿鱼丝.png",
                purchased: false,
              },
              {
                id: 3,
                name: "炭烧味鱿鱼丝",
                image: "./images/炭烧味鱿鱼丝.png",
                purchased: false,
              },
            ],
          };
        },
      }).mount("#app");
    </script>
  </body>
</html>
```

现在想要展示这些数据，需要`ul`标签包裹多个`<li></li>`。

vue不需要这么麻烦，只需要加上`v-for`,

`<li v-for="item in items">{{item}}</li>`

- items是data里的属性名



插值表达式只是把数据以字符的形式反馈到DOM，所以输入的结果依旧是字符。

![image-20221208150734067](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208150734067.png)

`v-bind`直接读取Vue里的数据名

![image-20221208151055951](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208151055951.png)





![image-20221208151915504](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208151915504.png)

v-model 双向绑定：

![image-20221208152129863](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208152129863.png)

![image-20221208152156983](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208152156983.png)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>02</title>
    <link rel="stylesheet" href="./style.css" />
    <script src="https://unpkg.com/vue@3"></script>
  </head>
  <body>
    <div id="app">
      <h1>{{ title }}</h1>
      <ul>
        <li v-for="food in foods">
          <img v-bind:src="food.image" />
          <span>{{food.name}}</span>
          <input type="checkbox" v-model="food.purchased" />
          <span>{{food.purchased}}</span>
        </li>
      </ul>
    </div>

    <script>
      Vue.createApp({
        data() {
          return {
            title: "零食清单",
            foods: [
              {
                id: 1,
                name: "原味鱿鱼丝",
                image: "./images/原味鱿鱼丝.png",
                purchased: false,
              },
              {
                id: 2,
                name: "辣味鱿鱼丝",
                image: "./images/辣味鱿鱼丝.png",
                purchased: false,
              },
              {
                id: 3,
                name: "炭烧味鱿鱼丝",
                image: "./images/炭烧味鱿鱼丝.png",
                purchased: false,
              },
            ],
          };
        },
      }).mount("#app");
    </script>
  </body>
</html>
```



### 03.key,v-show,computed

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02</title>
    <link rel="stylesheet" href="./style.css">
    <script src="https://unpkg.com/vue@3"></script>
</head>
<body>
    <div id="app">
        <section>
            <h2>已购零食</h2>
            <ul>
                <li v-for="food in foods">
                    <img v-bind:src="food.image">
                    <span>{{ food.name }}</span>
                    <input type="checkbox" v-model="food.purchased">
                </li>
            </ul>
        </section>
        
        <section>
            <h2>未购零食</h2>
            <ul>
                <li v-for="food in foods">
                    <img v-bind:src="food.image">
                    <span>{{ food.name }}</span>
                    <input type="checkbox" v-model="food.purchased">
                </li>
            </ul>
        </section>
        
    </div>

    <script>
        Vue.createApp({
            data() {
                return {
                    foods: [
                        { id: 1, name: '原味鱿鱼丝', image: './images/原味鱿鱼丝.png', purchased: false },
                        { id: 2, name: '辣味鱿鱼丝', image: './images/辣味鱿鱼丝.png', purchased: false },
                        { id: 3, name: '炭烧味鱿鱼丝', image: './images/炭烧味鱿鱼丝.png', purchased: false }
                    ]
                }
            }
        }).mount('#app');
    </script>
</body>
</html>
```



![image-20221208152433125](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208152433125.png)

现在已购零食不应该显示，使用数组的filter进行过滤：

```html
      <section>
        <h2>已购零食</h2>
        <ul>
          <li v-for="food in foods.filter(item => item.purchased)">
            <img v-bind:src="food.image" />
            <span>{{ food.name }}</span>
            <input type="checkbox" v-model="food.purchased" />
          </li>
        </ul>
      </section>
```

![image-20221208152720434](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208152720434.png)

![image-20221208152734243](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208152734243.png)

但是如果零食已经出现在已购零食，那么不应该出现在未够零食。

```html
      <section>
        <h2>未购零食</h2>
        <ul>
          <li v-for="food in foods.filter(item => !item.purchased)">
            <img v-bind:src="food.image" />
            <span>{{ food.name }}</span>
            <input type="checkbox" v-model="food.purchased" />
          </li>
        </ul>
      </section>
```

出现bug:

![image-20221208153141020](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208153141020.png)





index是数组的序列号，Vue默认绑定 `:key="index"`，这个`key`是Vue的一个特殊属性，简单来说，Vue就是利用这个key属性来跟踪和更新元素，从而提高性能。

这个key必须唯一值。

![image-20221208153110984](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208153110984.png)



```html
      <section>
        <h2>已购零食</h2>
        <ul>
          <li
            v-for="food in foods.filter(item => item.purchased)"
            v-bind:key="food.id"
          >
            <img v-bind:src="food.image" />
            <span>{{ food.name }}</span>
            <input type="checkbox" v-model="food.purchased" />
          </li>
        </ul>
      </section>

      <section>
        <h2>未购零食</h2>
        <ul>
          <li
            v-for="food in foods.filter(item => !item.purchased)"
            :key="food.id"
          >
            <img v-bind:src="food.image" />
            <span>{{ food.name }}</span>
            <input type="checkbox" v-model="food.purchased" />
          </li>
        </ul>
      </section>
```

`:key`是`v-bind:key`的缩写。



![image-20221208153646844](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208153646844.png)

当没有勾选的时候，已购零食不显示体验更好。

可以用`v-show`来控制。v-show其实是CSS的display来控制的。

```html
    <div id="app">
      <section v-show="foods.filter(item => item.purchased).length">
        <h2>已购零食</h2>
        <ul>
          <li
            v-for="food in foods.filter(item => item.purchased)"
            v-bind:key="food.id"
          >
            <img v-bind:src="food.image" />
            <span>{{ food.name }}</span>
            <input type="checkbox" v-model="food.purchased" />
          </li>
        </ul>
      </section>

      <section v-show="foods.filter(item => !item.purchased).length">
        <h2>未购零食</h2>
        <ul>
          <li
            v-for="food in foods.filter(item => !item.purchased)"
            :key="food.id"
          >
            <img v-bind:src="food.image" />
            <span>{{ food.name }}</span>
            <input type="checkbox" v-model="food.purchased" />
          </li>
        </ul>
      </section>
    </div>
```



![image-20221208154123015](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208154123015.png)



代码重复过多：

![image-20221208154250837](https://codermartinn.oss-cn-guangzhou.aliyuncs.com/img/image-20221208154250837.png)



可以利用到Vue的`computed`计算属性，computed是createApp对象参数里的一个属性。

V.createApp({data(){},computed})

computed:{},computed值接受的是一个对象，在这个对象里可以设置函数。



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>03</title>
    <link rel="stylesheet" href="./style.css" />
    <script src="https://unpkg.com/vue@3"></script>
  </head>
  <body>
    <div id="app">
      <section v-show="beforeBuy.length">
        <h2>未购零食</h2>
        <ul>
          <li v-for="food in beforeBuy" :key="food.id">
            <img v-bind:src="food.image" />
            <span>{{ food.name }}</span>
            <input type="checkbox" v-model="food.purchased" />
          </li>
        </ul>
      </section>

      <section v-show="afterBuy.length">
        <h2>已购零食</h2>
        <ul>
          <li v-for="food in afterBuy" v-bind:key="food.id">
            <img v-bind:src="food.image" />
            <span>{{ food.name }}</span>
            <input type="checkbox" v-model="food.purchased" />
          </li>
        </ul>
      </section>
    </div>

    <script>
      Vue.createApp({
        data() {
          return {
            foods: [
              {
                id: 1,
                name: "原味鱿鱼丝",
                image: "./images/原味鱿鱼丝.png",
                purchased: false,
              },
              {
                id: 2,
                name: "辣味鱿鱼丝",
                image: "./images/辣味鱿鱼丝.png",
                purchased: false,
              },
              {
                id: 3,
                name: "炭烧味鱿鱼丝",
                image: "./images/炭烧味鱿鱼丝.png",
                purchased: false,
              },
            ],
          };
        },
        computed: {
          beforeBuy() {
            return this.foods.filter((item) => !item.purchased);
          },
          afterBuy() {
            return this.foods.filter((item) => item.purchased);
          },
        },
      }).mount("#app");
    </script>
  </body>
</html>
```



### 04.拆组件和引用组件

