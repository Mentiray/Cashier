<template>
	<div class="pos">
		<el-row>
			<dingdan></dingdan>
			<shangpin></shangpin>	
		</el-row>
	</div>
</template>
<script type="text/javascript">
	
	import dingdan from '@/components/common/dingdan'
	import shangpin from '@/components/common/shangpin'
	export default{
		name: 'pos',
		
		components:{
			dingdan:dingdan,
			shangpin:shangpin
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