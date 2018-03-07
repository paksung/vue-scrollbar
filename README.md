# vue-scrollbar
一个自定义的vue垂直滚动条组件
## 使用方法
### 导入组件

	import ScrollBar from './scrollbar.vue';

### 调用

	//scroll-bar的style设置容器的宽高，属性width:滚动条的宽，radius:滚动条的圆角，speed:滚动的速度
	<scroll-bar style="width:100%;height: 300px" :width="6" :radius="3" :speed="10">
		<template scope="props">
			//滚动区域内的内容，需有一个顶级父元素，style内为组件传回的值,用于滚动内容
			<ul :style="{marginTop:props.mt+'px'}">
				<li v-for="(item, index) in arr">{{item}}</li>
			</ul>
		</template>
	</scroll-bar>
