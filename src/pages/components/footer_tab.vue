<template lang="pug">
   div(class="footer_box")
       div(class="footer_msg")
           div(class="left_msg")
               span 云南大理古城苍坪街56号床单厂艺术区C-2-2 
               span 0872-2504186 | 新浪微博：@大理生物多样性保护与研究中心.
           div(class="right_msg")
               span Copyright © 2015-2017 云山保护
               span 技术支持：畅迅科技 滇ICP备17007153号      
</template>
<script>
export default {
    name: 'footer-tab'
}
</script>
<style lang="scss" scoped>
  .footer_box{
    background-color: #46787D;
    width:100%;
    height:90px;
    }
  .footer_msg{
    width:1200px;
    height: 100%;
    margin:0 auto;
    color:white;
    font-size:14px;
    line-height: 24px;
  }
  .left_msg{
    float:left;
    width:677px;
    height:44px;
    margin-top:23px;
   
  }
  .left_msg span{
    display: block;
  }
  .right_msg{
    float:right;
    width:397px;
    height:44px;
    margin-top:23px;
    text-align: right;
  }
  .right_msg span{
    padding-right: 0px;
    display: inline-block;
  }
</style>