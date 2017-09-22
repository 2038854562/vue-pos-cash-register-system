<template>
  <div class="left-nav">
      <ul>
          <li>
              <i class="icon iconfont icon-goumai"></i>
              <p>收银</p>
          </li>
           <li>
              <i class="icon iconfont icon-dianpu"></i>
              <p>店铺</p>
          </li>
           <li>
              <i class="icon iconfont icon-hanbao"></i>
              <p>商品</p>
          </li>
           <li>
              <i class="icon iconfont icon-huiyuanqia"></i>
              <p>会员</p>
          </li>
           <li>
              <i class="icon iconfont icon-jiezhang"></i>
              <p>统计</p>
          </li>
      </ul>
  </div>
</template>
<script>
export default {
  name:'leftNav',
  data(){
      return{

      }
  }
}
</script>
<style>
.left-nav{
    color: #fff;
    background-color: #1D8ce0;
    width: 5%;
    height: 100%;
    font-size: 10px;
    float: left;
}
.iconfont{
    font-size: 24px;
}
.left-nav ul{
    padding: 0px;
    margin: 0px;
}
.left-nav li{
    list-style: none;
    text-align: center;
    border-bottom:1px solid #20a0ff;
    padding-top: 10px; 
}
</style>
