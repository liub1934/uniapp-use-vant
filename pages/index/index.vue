<template>
  <view class="content">
    <view>
      <van-button type="default">默认按钮</van-button>
      <van-button type="primary">主要按钮</van-button>
      <van-button type="info">信息按钮</van-button>
      <van-button type="warning">警告按钮</van-button>
      <van-button type="danger">危险按钮</van-button>
    </view>

		<van-divider contentPosition="center">分割线</van-divider>

		<view>
			<van-button type="info" block>全局定制主题的信息按钮</van-button>
			<van-button type="info" block class="my-button">class局部定制主题的信息按钮</van-button>
			<van-button type="info" block :style="buttonStyle">style局部定制主题的信息按钮</van-button>
		</view>

		<van-divider contentPosition="center">分割线</van-divider>

		<view>
			<van-button type="primary" block class="custom-button">自定义样式覆盖按钮</van-button>
		</view>

		<van-divider contentPosition="center">分割线</van-divider>

		<view>
			<view>内置icon图标</view>
			<van-icon name="chat-o" />
			<van-icon name="https://b.yzcdn.cn/vant/icon-demo-1126.png" />
			<view>iconfont自定义图标</view>
			<van-icon class-prefix="iconfont" name="edit" />
			<van-icon class-prefix="iconfont" name="drag" />
			<van-icon class-prefix="iconfont" name="rectangle" />
		</view>
		<van-divider contentPosition="center">分割线</van-divider>
		<view>
			<view class="van-ellipsis">这是一段宽度限制 250px 的文字，后面的内容会省略这是一段宽度限制 250px 的文字，后面的内容会省略这是一段宽度限制 250px 的文字，后面的内容会省略</view>
			<van-divider contentPosition="center">分割线</van-divider>
			<view class="van-multi-ellipsis--l2">这是一段最多显示两行的文字，后面的内容会省略这是一段最多显示两行的文字，后面的内容会省略这是一段最多显示两行的文字，后面的内容会省略这是一段最多显示两行的文字，后面的内容会省略</view>
			<van-divider contentPosition="center">分割线</van-divider>
			<view class="van-multi-ellipsis--l3">这是一段最多显示三行的文字，后面的内容会省略这是一段最多显示三行的文字，后面的内容会省略这是一段最多显示三行的文字，后面的内容会省略这是一段最多显示三行的文字，后面的内容会省略这是一段最多显示三行的文字，后面的内容会省略这是一段最多显示三行的文字，后面的内容会省略这是一段最多显示三行的文字，后面的内容会省略这是一段最多显示三行的文字，后面的内容会省略</view>
		</view>
		<van-divider contentPosition="center">分割线</van-divider>
		<view style="height:100px;line-height:100px;margin:10px;" class="van-hairline--surround">内置样式边框</view>
  </view>
</template>

<script>
	export default {
		data() {
			return {
				buttonStyle: `
					--button-info-background-color: pink;
					--button-info-border-color: pink;
				`
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
.content {
	.my-button {
		--button-info-background-color: grey;
		--button-info-border-color: grey;
	}

	// 样式覆盖需要加deep
  /deep/ .custom-button {
    .van-button {
			background-color: blue;
			border-radius: 10px;
    }
  }
}
</style>
