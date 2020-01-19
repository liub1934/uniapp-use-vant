总结下使用uni-app开发微信小程序过程中引入及使用Vant Weapp的方法及问题。
 
## 下载Vant Weapp
 
下载有2种方法：
 
方法1：克隆Vant Weapp的仓库，运行下面的命令将仓库克隆到本地
 
```shell
git clone https://github.com/youzan/vant-weapp.git
```
 
将`dist`目录复制到uniapp项目的`wxcomponents`目录中，`wxcomponents`和`components`同级，如果没有`wxcomponents`，自己建一个就行，
修改`dist`文件夹名称为`vant`。
 
方法2：通过`npm`安装Vant Weapp，如果项目中存在`package.json`，执行下方的命令安装Vant Weapp，如果没有，则在项目根目录使用命令`npm init`，一路回车即可，会自动生成package.json。
 
```shell
npm i vant-weapp -S --production
```
 
安装完成后在项目根目录`node_modules`中找到`@vant`，同上找到`dist`目录,复制到`wxcomponents`中并改名为`vant`
 
个人更倾向与方法二，因为几个星期或者几个月后估计就不知道具体用的什么版本了。
 
## 引入Vant Weapp
 
找到`pages.json`，在`globalStyle`或者具体`page`的`style`中引入Vant的组件，
如果需要全局使用该组件，可以在`globalStyle`中`usingComponents`中全局引入
 
```javascript
"usingComponents": {
  "van-button": "/wxcomponents/vant/button/index",
  "van-grid": "/wxcomponents/vant/grid/index",
  "van-grid-item": "/wxcomponents/vant/grid-item/index",
  "van-index-bar": "/wxcomponents/vant/index-bar/index",
  "van-index-anchor": "/wxcomponents/vant/index-anchor/index"
}
```
 
![image](https://image.liubing.me/2020/01/17/6cd551a3f245d.png)
 
如果只需要单独在个别页面使用，可在具体页面的`style`中配置`usingComponents`
 
```
{
  "path": "pages/test",
  "style": {
    "navigationBarTitleText": "测试",
    "usingComponents": {
      "van-button": "/wxcomponents/vant/button/index"
    }
  }
}
```
 
## 内置样式
 
Vant 中默认包含了一些常用样式，可以直接通过 className 的方式使用，如`van-ellipsis`，各种边框的class`van-hairline--top` 。 
官网介绍的引入方式如下： 
 
```shell
# 在 app.wxss 中引入内置样式
@import "path/to/@vant/weapp/dist/common/index.wxss";
```
 
在uni-app中是不存在`app.wxss`，只有经过编译后才会生成`app.wxss`，自己试了下，发现可以在`App.vue`中的`style`中引入可以正常使用。
 
```
<style>
/*每个页面公共css */
@import "/wxcomponents/vant/common/index.wxss";
</style>
```

## 样式覆盖

有时候Vant的样式不满足现在的需求，需要对其做些简单的调整，我们不太可能直接去修改别人的源CSS，根据官网提供的几种方案，整理了以下方法供大家参考：

```html
<van-button type="primary" block class="custom-button">自定义样式覆盖按钮</van-button>
```

通过定义个`class`直接进行样式覆盖

```css
<style lang="scss">
.custom-button {
  .van-button {
    background-color: blue;
    border-radius: 10px;
  }
}
</style>
```

如果你的`style`样式中存在`scoped`，我们可以利用`vue`中的语法，加个`/deep/`进行样式覆盖，如下所示：

```css
<style lang="scss" scoped>
/deep/ .custom-button {
  .van-button {
    background-color: blue;
    border-radius: 10px;
  }
}
</style>
```

## 定制主题

如果你对Vant的颜色样式不满意，可以通过官方提供的方法进行主题定制。  
官方的介绍：

小程序基于 [Shadow DOM](https://developers.google.com/web/fundamentals/web-components/shadowdom?hl=zh-cn) 来实现自定义组件，所以 Vant Weapp 使用与之配套的 [Css 变量](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_custom_properties) 来实现定制主题。链接中的内容可以帮助你对这两个概念有基本的认识，避免许多不必要的困扰。  
Css变量 的兼容性要求可以在 [这里](https://caniuse.com/#feat=css-variables) 查看。对于不支持 Css变量 的设备，定制主题将不会生效，不过不必担心，默认样式仍会生效。

### 全局定制

为了目录更清晰，我们可以在项目根目录建立`assets\styles`文件夹，用于存放和样式相关的代码。  
`assets\styles`中新建2个文件：`index.scss`和定制主题的`vant-theme.scss`文件，  
`index.scss`引入`vant-theme.scss`

```css
@import "./vant-theme.scss";
```

然后在`main.js`中引入`index.scss`

```javascript
import Vue from 'vue'
import App from './App'
import './assets/styles/index.scss' // 引入index.scss
// 其他省略
```

最后我们就可以在`vant-theme.scss`中根据官方所说的进行主题定制了：  
以下是官方配置文件中变量，完整内容请参考[配置文件](https://github.com/youzan/vant-weapp/blob/dev/packages/common/style/var.less)

```css
// Component Colors
@text-color: #323233;
@border-color: #ebedf0;
@active-color: #f2f3f5;
@background-color: #f8f8f8;
@background-color-light: #fafafa;
```

最后在`vant-theme.scss`根据配置文件的内容进行需要的变量定制，如下所示，也可以使用`uni.scss`中的变量：

```css
page {
  --button-info-background-color: $uni-text-color;
  --button-info-border-color: $uni-text-color;
}
```

### 局部定制

可能我们会对某个页面的某个组件进行单独的定制，官方提供2中方法：  
方法1：通过设置class，单独设置设置变量

```html
<van-button type="info" block class="my-button">class局部定制主题的信息按钮</van-button>
```

```css
.my-button {
  --button-info-background-color: grey;
  --button-info-border-color: grey;
}
```

方法2：通过style属性来动态设置变量

```html
<van-button type="info" block :style="buttonStyle">style局部定制主题的信息按钮</van-button>
```

```javascript
data() {
  return {
    buttonStyle: `
      --button-info-background-color: pink;
      --button-info-border-color: pink;
    `
  }
}
```

## 结语

以上是我用uni-app开发微信小程序使用Vant Weapp的一些经验总结，希望可以帮到有需要的人，如有不懂的地方可以评论回复，一般看到都会及时回复。