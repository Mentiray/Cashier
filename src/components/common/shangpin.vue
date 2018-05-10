<template>
	<div>
		<el-col :span='17'>
			<div class="often-goods">
				<div class="title">常用商品</div>
				<div class="often-goods-list">
					<ul v-for="goods in oftenGoods">
						<li @click="addOrderList(goods)">
							<span>{{goods.goodsName}}</span>
							<span class="goods-price">￥{{goods.price}}元</span>
						</li>
					</ul>
				</div>
			</div>
			<el-tabs>
				<el-tab-pane label="汉堡">
					<div>
						<ul class="cookList" v-for = "goods in type0Goods">
							<li @click="addOrderList(goods)">
								<span class="foodImg"><img :src="goods.goodsImg"></span>
								<span class="foodName">{{goods.goodsName}}</span>
								<span class="foodPrice">￥{{goods.price}}元</span>
							</li>
						</ul>
					</div>
				</el-tab-pane>
				<el-tab-pane label="小食">
					<ul class="cookList" v-for="goods in type1Goods">
						<li @click="addOrderList(goods)">
							<span class="foodImg"><img :src="goods.goodsImg"></span>
							<span class="foodName">{{goods.goodsName}}</span>									
							<span class="foodPrice">￥{{goods.price}}元</span>
						</li>
					</ul>
				</el-tab-pane>
				<el-tab-pane label="饮品">
					<ul class="cookList" v-for="goods in type2Goods">
						<li @click="addOrderList(goods)">
							<span class="foodImg"><img :src="goods.goodsImg"></span>
							<span class="foodName">{{goods.goodName}}</span>				
							<span class="foodPrice">￥{{goods.price}}元</span>
						</li>
					</ul>
				</el-tab-pane>
			</el-tabs>
		</el-col>
	</div>
</template>

<script type="text/javascript">
	import axios from 'axios'	
	export default{
		name: "shangpin",
		data(){
			return{
				tableData:[],
				oftenGoods:[],
				type0Goods: [],
				type1Goods: [],
				type2Goods: [],
				totalMoney: 0,
				totalCount: 0
			}
		},
		created:function(){
			axios.get("http://jspang.com/DemoApi/oftenGoods.php")
			.then(response=>{
				console.log(response);
				this.oftenGoods = response.data;
			})
			.catch(error=>{
				console.log(error);
				alert("未知网络错误");
			}),
			axios.get("http://jspang.com/DemoApi/typeGoods.php")
			.then(response=>{
				this.type0Goods = response.data[0];
				this.type1Goods = response.data[1];
				this.type2Goods = response.data[2];

			})
			.catch(error=>{
				console.log(error);
				alert("未知网络错误");
			})
		},
		mounted:function(){
			var orderHeight = document.body.clientHeight;
			console.log(orderHeight);
			document.getElementById("pos-order").style.height = orderHeight+"px";
		}

	}
</script>