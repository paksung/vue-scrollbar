<template>
	<div class="scrollWrapper" ref="scroll" @mousewheel="scrollIt($event)">
		<slot></slot>
		<div class="scrollbar" :style="{height:scrollH+'px',top:top+'px',width:width+'px',borderRadius:radius+'px'}" v-draggable="{updateTop:updateTop}" v-if="contentH > wrapperH"></div>
	</div>
</template>

<script>
	/*
		一个带自制滚动条的容器
	*/
	export default {
		data: ()=>{
			return {
				wrapperH:0,		//容器高度
				contentH:0,		//所包裹的内容高度
				top: 0,			//滚动条的top
			}
		},
		props: {//width:滚动条的宽度	radius:滚动条的borderradius		speed:滚动的速度
			width: {
				default: 6
			},
			radius: {
				default: 3
			},
			speed: {
				default: 10
			}
		},
		mounted(){//获取容器和内容的高度
			this.wrapperH = this.$refs.scroll.clientHeight;
			this.contentH = this.$refs.scroll.childNodes[0].clientHeight;
		},
		updated(){//更新容器和内容的高度
			this.wrapperH = this.$refs.scroll.clientHeight;
			this.contentH = this.$refs.scroll.childNodes[0].clientHeight;
		},
		methods: {
			//滚动。。。
			scrollIt: function(e){
				if(this.contentH > this.wrapperH){
					if(e.deltaY === 100){
						this.updateTop(this.top + this.speed);
					}else if(e.deltaY === -100){
						this.updateTop(this.top - this.speed);
					}
					this.$refs.scroll.childNodes[0].style.marginTop = this.marginT + 'px';
					e.preventDefault();
				}
			},
			//限制top的值
			updateTop: function(v){
				this.top = v;
				if(this.top <= 0){
					this.top = 0;
				}else if(this.top >= this.wrapperH - this.scrollH){
					this.top = this.wrapperH - this.scrollH;
				}
			}
		},
		computed: {
			//滚动条的高度
			scrollH: function(){
				return this.wrapperH / this.contentH * this.wrapperH;
			},
			//内容的margin-top，根据滚动的top换算而来
			marginT: function(){
				return -this.top / this.wrapperH * this.contentH;
			}
		},
		directives: {
			//拖动滚动条
			draggable: function(el, binding){
				let start, top;
				el.onmousedown = function(e){
					start = e.clientY;
						top = parseFloat(this.style.top);
					document.body.onmousemove = function(e){
						let end = e.clientY,
						newtop = top + end - start;
						binding.value.updateTop(newtop);
					};
				}
				
				document.body.onmouseup = function(e){
					document.body.onmousemove = null;
				}
			}
		}
	}
</script>

<style>
	.scrollWrapper{
		overflow: hidden;
		position: relative;
	}
	.scrollbar{
		position: absolute;
		right: 0;
		background-color: rgba(210,210,210,0.5);
		transition: top 0.3s, opacity 0.5s;
		opacity: 0;
		user-select: none;
		cursor: pointer;
	}
	.scrollWrapper:hover .scrollbar{
		opacity: 1;
	}
</style>