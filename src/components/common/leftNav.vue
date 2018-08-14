<template>
	<div class="left-nav">
		<ul>
			<li>
	      <i class="icon iconfont icon-shouyin"></i>
				<div>收银</div>
			</li>
			<li>
	      <i class="icon iconfont icon-gouwuchedianpusvg"></i>
				<div>店铺</div>
			</li>
			<li>
      	<i class="icon iconfont icon-shangpin"></i>
				<div>商品</div>
			</li>
			<li>
	      <i class="icon iconfont icon-huiyuan"></i>
				<div>会员</div>
			</li>
			<li>
	      <i class="icon iconfont icon-xianshiqianggou"></i>
				<div>抢购</div>
			</li>
			<li>
	      <i class="icon iconfont icon-tongji3"></i>
				<div>统计</div>
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	name: 'leftNav',
	data() {
		return {

		}
	}
}
</script>

<style scoped>
	.left-nav{
		float: left;
		width: 5%;
		height: 100%;
		background-color: #1D8ce0;
		font-size: 12px;
		color: #fff;
	}
	.iconfont{
		font-size: 24px;
	}
	.left-nav ul{
		padding: 0;
		margin: 0;
	}
	.left-nav li{
		list-style: none;
		padding: 10px;
		border-bottom: 1px solid #20a0ff;
		text-align: center;
	}
</style>