<template>
	<div class="pos">
		<div>
			<el-row>
				<!-- 订单栏布局 -->
				<el-col :span='7' class="pos-order" id="order-list">
					<el-tabs>
						<el-tab-pane label="点餐">
							<el-table :data="tableData" border style="width: 100%;">
					      <el-table-column prop="goodsName" label="商品名称"></el-table-column>
					      <el-table-column prop="count" label="数量" width="50px;"></el-table-column>
					      <el-table-column prop="price" label="金额" width="70px;"></el-table-column>
					      <el-table-column label="操作" width="100px;" fixed="right">
									<template slot-scope="scope">
										<el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
										<el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
									</template>
					      </el-table-column>
					    </el-table>
					    <div class="total">
					    	数量：{{totalCount}}   金额：{{totalMoney}}元
					    </div>
						</el-tab-pane>
						<el-tab-pane label="挂单">
							挂单
						</el-tab-pane>
						<el-tab-pane label="外卖">
              外卖
						</el-tab-pane>																		
					</el-tabs>
					
					<div class="btn-box">
						<el-button type="primary">挂单</el-button>
						<el-button type="danger" @click="delAllGoods">删除</el-button>
						<el-button type="success" @click="checkOut">结账</el-button>
					</div>
				</el-col>
				<!-- 常用商品区域布局 -->
				<el-col :span='17'>
					<div class="often-goods">
						<div class="title">常用商品</div>
						<div class="often-goods-list">
							<ul>
								<li v-for="item in oftenGoods" @click="addOrderList(item)">
									<span>{{item.goodsName}}</span>
									<span class="o-price">￥{{item.price}}元</span>
								</li>
							</ul>
						</div>
					</div>

					<div class="goods-type">
					  <el-tabs>
					    <el-tab-pane label="汉堡">
								<ul class="cooklist">
									<li v-for="good in type0Goods" @click="addOrderList(good)">
										<span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
										<span class="foodName">{{good.goodsName}}</span>
										<span class="foodPrice">￥{{good.price}}元</span>
									</li>
								</ul>
					    </el-tab-pane>
					    <el-tab-pane label="小食">
					    	<ul class="cooklist">
									<li v-for="good in type1Goods" @click="addOrderList(good)">
										<span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
										<span class="foodName">{{good.goodsName}}</span>
										<span class="foodPrice">￥{{good.price}}元</span>
									</li>
								</ul>
					    </el-tab-pane>
					    <el-tab-pane label="饮料">
								<ul class="cooklist">
									<li v-for="good in type2Goods" @click="addOrderList(good)">
										<span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
										<span class="foodName">{{good.goodsName}}</span>
										<span class="foodPrice">￥{{good.price}}元</span>
									</li>
								</ul>
					    </el-tab-pane>
					    <el-tab-pane label="套餐">
								<ul class="cooklist">
									<li v-for="good in type3Goods" @click="addOrderList(good)">
										<span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
										<span class="foodName">{{good.goodsName}}</span>
										<span class="foodPrice">￥{{good.price}}元</span>
									</li>
								</ul>
					    </el-tab-pane>
					  </el-tabs>	
					</div>
				</el-col>
			</el-row>
		</div>
	</div>
</template>

<script>
import axios from 'axios'
export default {
	name: 'Pos',
	data () {
		return {
			tableData: [],
      oftenGoods:[],
      typeGoods:[],
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],
      totalMoney: 0,
      totalCount: 0
		}
	},
	created: function() {
		axios.get('http://jspang.com/DemoApi/oftenGoods.php')
		.then(res=>{
			this.oftenGoods = res.data
		})
		.catch(err=>{
			console.log(err)
		})
		axios.get('http://jspang.com/DemoApi/typeGoods.php')
		.then(res=>{
			console.log(res)
			this.type0Goods = res.data[0];
			this.type1Goods = res.data[1];
			this.type2Goods = res.data[2];
			this.type3Goods = res.data[3];
		})
		.catch(err=>{
			console.log(err)
		})
	},
	mounted: function() {
		var orderHeight = document.body.clientHeight;
		document.getElementById('order-list').style.height = orderHeight + 'px';
	},
	methods: {
		// 添加订单列表的方法
		addOrderList(goods) {
			this.totalMoney = 0;
			this.totalCount = 0;
			let isHave = false;
			for (let i = 0; i < this.tableData.length; i++) {
				if (this.tableData[i].goodsId == goods.goodsId) {
					isHave = true; // 存在
				};
			};
			if (isHave) { // 订单中存在此商品
				let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
				arr[0].count++;
			} else {
				let newGoods = {
					goodsId: goods.goodsId,
					goodsName: goods.goodsName,
					price: goods.price,
					count: 1
				}
				this.tableData.push(newGoods)
			}
			this.getAllMoney()
		},
		// 删除单个商品
		delSingleGoods(goods) {
			this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId);
			this.getAllMoney();
		},
		// 模拟结账
		checkOut() {
			if (this.totalCount != 0) {
				this.delAllGoods();
				this.$message({
					message: '结账成功，欢迎下次再来',
					type: 'success'
				})
			} else {
				this.$message.error('老板理解你急切的心情，请先下单')
			}
		},
		// 汇总数量和金额
		getAllMoney() {
			this.totalCount = 0;
			this.totalMoney = 0;
			if (this.tableData) {				
				// 计算汇总金额和数量
				this.tableData.forEach((ele) => {
					this.totalCount+=ele.count;
					this.totalMoney = this.totalMoney + (ele.price*ele.count)
				})
			};
		},
		delAllGoods() {
			this.tableData = [];
			this.totalCount = 0;
			this.totalMoney = 0;
		}
	}
}
</script>

<style>
	.pos-order{
		background-color: #f9fafc;
		border-right: 1px solid #c0ccda;
	}
	.el-tabs__nav-wrap{
		padding: 0 20px;
		background-color: #fff;
	}
	.btn-box{
		margin-top: 10px;
	}
	.often-goods-list{
		padding-bottom: 10px;
		background-color: #f9fafc;
		border-bottom: 1px solid #d3dce6;
		overflow: hidden;
	}
	.title{
		padding: 10px;
		height: 19px;
		border-bottom: 1px solid #d3dce6;
		background-color: #f9fafc;
		text-align: left;
	}
	.often-goods-list ul li{
		list-style: none;
		float: left;
		padding: 10px;
		margin: 5px;
		bottom: 1px solid #e5e9f2;
		background-color: #fff;
		cursor: pointer;
	}
	.o-price{
		color: #58b7ff;
	}

	.cooklist li{
		list-style: none;
		padding: 2px;
		margin: 2px;
		float: left;
		width: 23%;
		height: auto;
		border: 1px solid #e5e9f2;
		background-color: #fff;
		overflow: hidden;
		cursor: pointer;
	}
	.cooklist{
		overflow: hidden;
	}
	.cooklist span{
		display: block;
		float: left;
	}
	.foodImg{
		width: 40%;
	}
	.foodName{
		padding-top: 6px;
		padding-left: 10px;
		font-size: 16px;
		color: brown;
	}
	.foodPrice{
		padding-left: 10px;
		padding-top: 10px;
		font-size: 14px;
	}
	.total{
		padding: 10px;
		background-color: #fff;
		border-bottom: 1px solid #d3dce6;
	}
</style>