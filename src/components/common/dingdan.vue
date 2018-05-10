<template>
	<div>
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
	</div>
</template>

<script type="text/javascript">
	export default{
		name : "dingdan",
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
		methods:{
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
		}

	}
</script>