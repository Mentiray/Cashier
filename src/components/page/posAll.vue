<template>
	<div class="pos">
		<el-row>
			<el-col :span='7' class="pos-order" id="pos-order">
				<el-tabs>
					<el-tab-pane label="点餐">
						<el-table border :data="tableData">
							<el-table-column prop="goodsName" label="商品名称">
							
							</el-table-column>
							<el-table-column prop="count" label="商品数量">
							
							</el-table-column>
							<el-table-column prop="price" label="商品金额">
							
							</el-table-column>
							<el-table-column prop="goodsOperation" label="商品操作">
								<template scope="scope"> 
									<el-button type="text" size="small" @click="addOrderList(scope.row)">添加</el-button>
									<el-button type="text" size="small" @click="delOnePro(scope.row)">删减</el-button>
									<el-button type="text" size="small" @click="delSinglePro(scope.row)">清空</el-button>
								</template>
							</el-table-column>
						</el-table>
					</el-tab-pane>
					<el-tab-pane label="挂单">
						
					</el-tab-pane>
					<el-tab-pane label="外卖">
						
					</el-tab-pane>
				</el-tabs>	
				<div class="totalInfo">
					<el-row>
						<el-col :span="12">
							数量：{{totalCount}}
						</el-col>
						<el-col :span="12">
							总价：￥{{totalMoney}}元
						</el-col>
					</el-row>
				</div>
				<div class="div-btn">
					
					<el-button type="danger" @click="delAllPro()">清空</el-button>
					<el-button type="success" @click="checkout()">结账</el-button>
				</div>
			</el-col>
			
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
		</el-row>


	</div>
</template>
<script type="text/javascript">
	import axios from 'axios'
	export default{
		name: 'pos',
		data(){
			return{
				tableData:[],
				oftenGoods:[],
				type0Goods: [],
				type1Goods: [],
				type2Goods: [],
				totalMoney: 0,
				totalCount: 0
				// type0Goods:[
				// 	{
				// 		goodsId: 1,
				// 		goodsName: "奥尔良鸡腿堡",
				// 		goodsImg: "../static/img/hanbao.jpg",
				// 		goodsPrice: 15

				// 	},
				// 	{
				// 		goodsId: 2,
				// 		goodsName: "奥尔良鸡腿堡",
				// 		goodsImg: "../static/img/hanbao.jpg",
				// 		goodsPrice: 15

				// 	},
				// 	{
				// 		goodsId: 3,
				// 		goodsName: "奥尔良鸡腿堡",
				// 		goodsImg: "../static/img/hanbao.jpg",
				// 		goodsPrice: 15

				// 	},
				// 	{
				// 		goodsId: 4,
				// 		goodsName: "奥尔良鸡腿堡",
				// 		goodsImg: "../static/img/hanbao.jpg",
				// 		goodsPrice: 15

				// 	},
				// 	{
				// 		goodsId: 5,
				// 		goodsName: "奥尔良鸡腿堡",
				// 		goodsImg: "../static/img/hanbao.jpg",
				// 		goodsPrice: 15

				// 	},
				// 	{
				// 		goodsId: 6,
				// 		goodsName: "奥尔良鸡腿堡",
				// 		goodsImg: "../static/img/hanbao.jpg",
				// 		goodsPrice: 15

				// 	}
				// ]
			}
		},
		methods:{
			//添加商品
			addOrderList(goods){
				this.totalCount = 0;
				this.totalMoney = 0;
				let isHave = false;
				// 判断商品是否存在与订单列表
				for(let i = 0; i < this.tableData.length; i++ ){
					console.log(this.tableData[i].goodsId);
					if (this.tableData[i].goodsId == goods.goodsId) {
						isHave = true;
					}
				}	
				// 更新商品列表
				if(isHave){
					let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
					arr[0].count++;
				}
				else{
					let newGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
					this.tableData.push(newGoods);
				}
				//调用函数需要this.
				this.getCount();
				
			},
			//清空单类商品
			delSinglePro(goods){
				this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
				this.getCount();
			},
			//清空所有商品
			delAllPro(){
				this.tableData = [];
				this.totalMoney = 0;
				this.totalCount = 0;
			},
			//删除一件商品
			delOnePro(goods){
				if (goods.count > 1) {
					goods.count--;
				}else{
					this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId);
				}
				this.getCount();
			},
			//结账
			checkout(){
				if(this.totalCount != 0){
					this.totalMoney = 0;
					this.totalCount = 0;
					this.tableData = [];
					this.$message({
						message:"结账成功！",
						type:"success"
					});
				}else{
					this.$message({
						message:"无商品可以结账",
						type: "error"
					});
				}
			},
			//可以复用，所以提出来
			getCount(){
				this.totalMoney = 0;
				this.totalCount = 0;
				if(this.tableData){
					//计算总价
					this.tableData.forEach((element)=>{
						this.totalCount += element.count;
						this.totalMoney = this.totalMoney+(element.price*element.count);
					})
				}				
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
<style type="text/css">
	.pos-order{
		text-align: center;
	}
	.div-btn{
		margin-top: 10px;
	}
	.often-goods-list ul li{
		list-style: none;
		float: left;
		border: 1px solid #F9FAFC;
		padding: 10px;
		margin: 10px;
		background-color: #FFFFFF;
	}
	.often-goods{
		overflow: hidden;
	}
	.title{
		height: 20px;
		border-bottom: 1px solid #d3dce6;
		background-color: #F9FAFC;
		padding: 10px;
	}
	.goods-price{
		color: #58B7FF;
	}

	.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auto;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
 
   }
   .cookList li span{
       
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;
 
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   img{
   	width: 100%;
   }
   .totalInfo{
   	cursor: pointer;
   	padding: 10px 0;
   	background: #fff;
   }
</style>