<template>
  <div class='header'>
      <div class="content-wrapper">
        <div class="avater">
          <img width="64" height="64" :src="sellers.avatar">
        </div>
        <div class="content">
          <div class="title">
              <span class="brand"></span>
              <span class="name">{{sellers.name}} </span>
          </div>
          <div class="description">
            {{sellers.description}}/{{sellers.deliveryTime}}分钟送达
          </div>
          <div v-if="sellers.supports" class="supports">
              <span class="icon" :class = 'classMap[sellers.supports[0].type]'></span>
              <span class="text">{{sellers.supports[0].description}}</span>
          </div>
        </div>
        <div v-if="sellers.supports" class="support-count" @click = "showDetail()">
          <span class="count">{{sellers.supports.length}}
          个
          </span>
          <i class="icon-keyboard_arrow_right">></i>
        </div>
      </div>
      <div class="bulletin-wrapper" @click = "showDetail()">
        <span class="bulletin-title">

        </span>
        <span class="bulletin-text">
            {{sellers.bulletin}}
      </span>
        <i class="icon-keyboard_arrow_right">></i>

      </div>
      <div class="background">
        <img :src="sellers.avatar" width="100%" height="100%">
      </div>
      <div class="detail" v-show="detailShow" transition="fade">
        <div class="detail-wrapper clearfix">
            <div class="detail-main">
              <h1 class="name">{{sellers.name}}</h1>
              <div class="star-wrapper">
                <star :size="48" :score='sellers.score'></star>
              </div>
              <div class="title">
                <div class="line"></div>
                <div class="text">优惠信息</div>
                <div class="line"></div>
              </div>
              <ul v-if="sellers.supports" class="supports sup">
                <li class="support-item" v-for="item in sellers.supports">
                  <span class="icon" :class="classMap[sellers.supports[$index].type]">

                  </span>
                  <span class="text">{{sellers.supports[$index].description}}</span>
                </li>
              </ul>
              <div class="title">
                <div class="line"></div>
                <div class="text">商家公告</div>
                <div class="line"></div>
              </div>
              <div class="bulletin">
                <p class="content">{{sellers.bulletin}}</p>

              </div>
            </div>
        </div>
        <div class="detail-close" @click = 'hidden()'>
              关
      </div>
      </div>

  </div>
</template>
<script type='text/ecmascript-6'>
  import star from '../star/star.vue'
    export default{
      props : {
            sellers : {
                type : Object
            }
      },
      data () {
          return {
            detailShow : false
          }
      },
      methods : {
        showDetail () {
            this.detailShow = true
        },
        hidden () {
          this.detailShow = false
        }
      },
      created () {
          this.classMap = ['decrease', 'discount', 'invoice', 'special', 'guarantee']
      },
      components : {
          star
      }
    }
</script>
<style>
  .header{
    position: relative;
    color: #fff;
    background: rgba(7,17,27,0.5);
    overflow: hidden;

  }
  .content-wrapper{
    padding: 24px 12px 18px 24px;
    color: white;
    font-size: 0px;
    position: relative;
  }
  .avater{
    display: inline-block;
    vertical-align: top;
  }
  img{
    border-radius: 2px;
  }
  .content{
    display: inline-block;
    font-size: 14px;
    margin-left: 14px;
  }
  .title{
    margin: 2px 0 8px 0;
  }
  .brand{
    display: inline-block;
    vertical-align: top;
    width: 30px;
    height: 18px;
    background-image: url("brand@2x.png");
    background-size: 30px 18px;
    background-repeat: no-repeat;
  }
  .name{
    margin-left: 6px;
    font-size: 16px;
    line-height: 18px;
    font-weight: bold;

  }
  .description{
    margin-bottom: 10px;
    line-height: 12px;
    font-size: 12px;
  }
  .supports .icon{
    display: inline-block;
    width: 12px;
    height: 12px;
    margin-right: 4px;
    background-size: 12px 12px;
    background-repeat: no-repeat;

  }
.decrease{
  background: url("decrease_1@2x.png");

}
  .discount{
    background: url("discount_1@2x.png");

  }
  .invoice{
    background: url("invoice_1@2x.png");

  }
  .special{
    background: url("special_1@2x.png");

  }
  .guarantee{
    background: url("guarantee_2@2x.png");

  }
  .text{
    line-height: 12px;
    font-size: 12px;
    vertical-align: top;

  }
  .support-count{
    position: absolute;
    right: 12px;
    bottom: 18px;
    padding: 0 8px;
    height: 24px;
    line-height: 24px;
    border-radius: 14px;
    background: rgba(0,0,0,0.2);
    text-align: center;
    color: white;
  }
  .count{
    font-size: 10px;

  }
  .icon-keyboard_arrow_right{
    font-size: 10px;
  }
  .bulletin-wrapper{
    height: 28px;
    color: white;
    line-height: 28px;
    padding:0 22px 0 12px ;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    font-size: 1px;
    position: relative;
    background: rgba(7,17,27,0.2);
  }
  .bulletin-title{
    display: inline-block;
    width: 22px;
    height: 12px;
    background-image: url("bulletin@2x.png");
    background-size: 22px 12px;
    background-repeat: no-repeat;
    vertical-align: top;
    margin-top: 8px;
  }
  .bulletin-text{
    margin: 0 4px;
    font-size: 10px;
    font-weight: 200;
    vertical-align: top;

  }
  .bulletin-wrapper .icon-keyboard_arrow_right{
    position: absolute;
    font-size: 10px;
    right: 12px;
    top: 0;

  }
  .background{
    position: absolute;
    top:0;
    left: 0%;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
  }
  .detail{
    position: fixed;
    top:0;
    z-index: 100;
    width: 100%;
    height: 100%;
    overflow: auto;
    transition: all 1s;
    -webkit-backdrop-filter: ;
  }
  .detail.fade-transition{
    opacity: 1;
    background: rgba(7,17,27,0.8);

  }
  .detail.fade-enter,.detail.fade-leave{
    opacity: 0;
    background: rgba(7,17,27,0);

  }
  .clearfix{
    display: inline-block;
  }
  .clearfix:after{
    display: block;
    content: ".";
    height: 0;
    line-height: 0;
    clear: both;
    visibility: hidden;
  }
  .detail-wrapper {
    min-height: 100%;
    width: 100%;
  }
  .detail-main{
    margin-top: 64px;
    padding-bottom: 64px;
  }
  .detail-close{
    position: relative;
    width: 32px;
    height: 32px;
    margin: -64px auto 0 auto;
    clear: both;
    font-size: 32px;
  }
  .detail-main .name{
    line-height: 16px ;
    text-align: center;
    font-size: 16px;
    font-weight: 700;
  }
  .star-wrapper{
    margin-top: 18px;
    padding: 2px 0;
    text-align: center;
  }
  .detail-main .title{
    display: flex;
    width: 80%;
    margin: 28px auto 24px auto;

  }
  .detail-main .title .line{
    flex: 1;
    position: relative;
    top: -6px;
    border-bottom: 1px solid rgba(255,255,255,0.2);
  }

  .detail-main .title .text{
    padding: 0 12px;
    font-size: 14px;
    font-weight: 700;
  }
  .supports{
    /*width: 80%;*/
    margin: 0 auto;
  }
  .supports .support-item {
    padding: 0 12px;
    margin-bottom: 12px;
    font-size: 0px;
    vertical-align: top;

  }
  .supports .support-item:last-child{
    margin-bottom: 0px;
  }
  .supports .support-item .icon{
    display: inline-block;
    width: 16px;
    height: 16px;
    margin-right: 6px;
    background-size: 16px 16px;
    background-repeat: no-repeat;
  }
  .supports .support-item .text{
    line-height: 16px;
    font-size: 12px;

  }
  .bulletin{
    width: 80%;
    margin: 0 auto;

  }
  .bulletin .content{
    padding: 0;
    line-height: 24px;
    font-size: 12px;
  }
  .sup{
    width: 80%;
  }
</style>
