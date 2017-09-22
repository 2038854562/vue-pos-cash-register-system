<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span="7" id="order-list" >
            <el-tabs>
              <el-tab-pane label="点餐">
                  <el-table :data="tableData" border style="width: 100%">
                      <el-table-column align="center"  prop="goodsName" label="名称" width="130">
                      </el-table-column>
                      <el-table-column align="center" prop="count" label="数量" width="130">
                      </el-table-column>
                      <el-table-column align="center" prop="price" label="单价" width="130">
                      </el-table-column>          
                      <el-table-column align="center" fixed="right" label="操作" width="140">
                          <template scope="scope">
                              <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                              <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                          </template>
                      </el-table-column>
                  </el-table>
                  <div class="result-div">
                    <span>总数：</span><b class="total-count">{{totalCount}}个</b>
                    <span>总额：</span><b class="total-money">{{totalMoney}}元</b>
                  </div>
                  <div class="btn-wrapper">
                    <span class="wrapper">
                      <el-button type="warning">挂单</el-button>
                      <el-button type="danger" @click="delAllGoods()">删除</el-button>
                      <el-button type="success" @click="checkout">结账</el-button>
                    </span>
                  </div>
              </el-tab-pane>
              <el-tab-pane label="挂单" name="second">挂单</el-tab-pane>
              <el-tab-pane label="外卖" name="third">外卖</el-tab-pane>      
            </el-tabs>
        </el-col>
        <el-col :span="17">
          <div class="common-goods">
            <p class="title">常用商品</p>
            <div class="goods-list">
              <ul class="clearfix">
                <li v-for="good in commonGoods" @click="addOrderList(good)">
                  <span>{{good.goodsName}}</span>
                  <span class="price">￥{{good.price}}</span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goods-category">
            <el-tabs class="tab-div">
                <el-tab-pane label="汉堡">
                  <ul class='cookList clearfix'>
                      <li v-for="good in type0Goods" @click="addOrderList(good)">
                          <p class="foodImg"><img :src="good.goodsImg" width="100%"></p>
                          <p class="good-detail clearfix">
                            <span class="foodName">{{good.goodsName}}</span>
                            <span class="price foodPrice">￥{{good.price}}</span>
                          </p>
                      </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="零食">
                  <ul class='cookList clearfix'>
                      <li v-for="good in type1Goods" @click="addOrderList(good)">
                          <p class="foodImg"><img :src="good.goodsImg" width="100%"></p>
                          <p class="good-detail clearfix">
                            <span class="foodName">{{good.goodsName}}</span>
                            <span class="price foodPrice">￥{{good.price}}</span>
                          </p>
                      </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="饮料">
                  <ul class='cookList clearfix'>
                      <li v-for="good in type2Goods" @click="addOrderList(good)">
                          <p class="foodImg"><img :src="good.goodsImg" width="100%"></p>
                          <p class="good-detail clearfix">
                            <span class="foodName">{{good.goodsName}}</span>
                            <span class="price foodPrice">￥{{good.price}}</span>
                          </p>
                      </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="套餐">
                  <ul class='cookList clearfix'>
                      <li v-for="good in type3Goods" @click="addOrderList(good)">
                          <p class="foodImg"><img :src="good.goodsImg" width="100%"></p>
                          <p class="good-detail clearfix">
                            <span class="foodName">{{good.goodsName}}</span>
                            <span class="price foodPrice">￥{{good.price}}</span>
                          </p>
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
    data(){
      return{
        tableData: [],
        commonGoods:[],
        type0Goods:[],
        type1Goods:[],
        type2Goods:[],
        type3Goods:[],
        totalCount:0,
        totalMoney:0
      }
    },
    created:function(){
      //读取常用商品列表
      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(response=>{
        // console.log(response);
        this.commonGoods = response.data;
      })
      .catch(
        error=>{
          // console.log(error);
          alert('网络错误，不能访问')
      });
       //读取不同类别商品列表
      axios.get('http://jspang.com/DemoApi/typeGoods.php')
      .then(response=>{
        console.log(response);
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      })
      .catch(
        error=>{
          // console.log(error);
          alert('网络错误，不能访问')
      })
    },
    mounted:function(){
      var orderHeight = document.body.clientHeight;
      document.getElementById('order-list').style.height=orderHeight+'px';
    },
    methods:{
      // 添加列表的方法
      addOrderList(good){
        let isHave = false;
        // 判断列表中是否已经存在该商品
      for(let i=0;i<this.tableData.length;i++){
        // console.log(this.tableData[i].goodsId);
        if(this.tableData[i].goodsId == good.goodsId){
            isHave = true;//存在
        }
      }
        // 根据isHave 的判断进行添加操作
      if(isHave){
        // 存在就进行数量的添加
        let arr = this.tableData.filter(o => o.goodsId == good.goodsId);
        arr[0].count++;
      }else{
        // 不存在就添加数组
        let newGood = {goodsId:good.goodsId,goodsName:good.goodsName,price:good.price,count:1};
        this.tableData.push(newGood);
      }
      this.getResult();
      },
      // 统计商品最终的数量和金额
      getResult(){
        this.totalCount = 0;
        this.totalMoney = 0;
        if(this.tableData){
            // 汇总数量和总金额
            this.tableData.forEach((element)=>{
              this.totalCount += element.count;
              this.totalMoney += element.price*element.count;
            })
        };
      },
      // 删除单个商品
      delSingleGoods(good){
        // console.log(good);
        this.tableData = this.tableData.filter(o => o.goodsId != good.goodsId);
        this.getResult();
      },
      // 删除所有商品
      delAllGoods(){
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
      },
      // 结账提示方法
      checkout(){
        if(this.totalCount!=0){
          this.tableData = [];
          this.totalCount = 0;
          this.totalMoney = 0;
          this.$message({
            message:'结账成功!',
            type:'success'
          })
        }else{
          this.$message.error('请至少选择一种商品！');
        }
      }
    }
  }

</script>
<style>
    .clearfix:after{
      content: '';
      display: block;
      clear: both;
      height: 0;
      visibility: hidden;
    } 
    .clearfix { zoom: 1; }
    p{margin: 0;padding: 0}
    #order-list{
      background-color: #fff;
      border-right: 1px solid #E5E9F2;
    }
    .btn-wrapper{
      margin-top: 20px;
    }
    .title{
      height: 41px;
      line-height: 41px;
      border-bottom:1px solid #D3DCE6;
      background-color: #F9FAFC;
      text-align: left;
      padding-left:10px; 
    }
    .goods-list li{
      list-style: none;
      float:left;
      border:1px solid #E5E9F2;
      padding:10px;
      margin:5px;
      background-color:#fff;
      font-size: 14px;
      cursor: pointer;
    }
    .price{
      color:#58B7FF; 
    }
    .cookList li{
       list-style: none;
       width:20%;
       border:1px solid #E5E9F2;
       height: auto;
       overflow: hidden;
       background-color:#fff;
       padding: 5px;
       float:left;
       margin: 2px;
       text-align: center;
       cursor: pointer;
   }
  .cookList .good-detail{
    line-height: 1.8;
  }
  .foodName{
      font-size: 14px;
      color:brown;
      float: left;
      width: 70px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
  }
  .foodPrice{
      float: left;
  }
.el-tabs__nav-scroll{
  background-color: #fff;
}
.result-div{
  height: 40px;
  line-height: 40px;
  border-bottom: 1px solid #dfe6ec;
  font-size: 14px;
}
.total-count,.total-money{
  color: red;
}
</style>
