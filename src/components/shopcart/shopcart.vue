<template>
  <div class="shopcart">
    <div class="content">
      <div class="contentLeft">
        <div class="logo-wrapper">
          <div class="logo" :class="{'hightlight':totalCount>0}">
            <span class="icon-shopping_cart" :class="{'hightlight':totalCount>0}"></span>
          </div>
          <div class="num" v-show="totalCount>0">
                {{totalCount}}
          </div>
        </div>
        <div class="price">￥{{totalPrice}}</div>
        <div class="desc">另需配送费{{deliveryPrice}}元</div>
      </div>
      <div class="contentRight" :class="payClass" >{{payDesc}}</div>
    </div>
    <div class="ball-container">
      <div v-for="ball in balls" v-show="ball.show" transition='drop' class="ball">
          <div class="inner">

          </div>

      </div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
export default{
  props : {
    selectFoods : {
      type : Array,
      default () {
        return [
          {
            price : 10,
            count : 1
          }
        ]
      }
    },
    deliveryPrice : {
      type : Number,
      dedefault : 0
    },
    minPrice : {
      type : Number,
      dedefault : 0
    }
  },
  data () {
    return {
      balls : [
        {
         show : false
        },
        {
          show : false
        },
        {
          show : false
        },
        {
          show : false
        },
        {
          show : false
        }
      ]
    }
  },
  computed : {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach((food) => {
          total += food.price*food.count
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach((food) => {
          count += food.count
      })
      return count
    },
    payDesc () {
      if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`
      } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice
          return  `还差￥${diff}元起送`
      } else {
        return '去结算'
      }
    },
    payClass () {
      if (this.totalPrice < this.minPrice) {
          return  'not-enouth'
      } else {
        return 'enouth'
      }
    }
  },
  methods : {
    drop (el) {

    }
  }

}
</script>
<style>
.shopcart{
  position: fixed;
  left: 0px;
  bottom: 0px;
  z-index: 50;
  width: 100%;
  height: 48px;
}

.shopcart .content{
  display: flex;
  background: #141d27;
  font-size: 0;
  margin-left: 0;
}
.shopcart .content .contentLeft{
  flex: 1;
}
.shopcart .content .contentLeft .logo-wrapper{
display: inline-block;
position: relative;
top:-10px;
margin: 0 12px;
padding: 6px;
width: 56px;
height: 56px;
box-sizing: border-box;
vertical-align: top;
border-radius: 50%;
background: #141d27;
}
.shopcart .content .contentLeft .logo-wrapper .num{
  position: absolute;
  top: 0;
  right: 0;
  width: 24px;
  height: 16px;
  line-height: 16px;
  text-align: center;
  border-radius: 16px;
  font-size: 9px;
  font-weight: 400;
  color: #fff;
  background: rgb(240, 20, 20);
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);
}
.shopcart .content .contentLeft .logo-wrapper .logo{
  width: 100%;
  height: 100%;
  border-radius: 50%;
  text-align: center;
  background: #2b343c;

}
.shopcart .content .contentLeft .logo-wrapper .logo.hightlight{
  background: rgb(0, 160, 220);

}
.icon-shopping_cart{
  line-height: 48px;
  font-size: 24px;
  color: #80858a;
}
.icon-shopping_cart.hightlight{
  color: white
}
.shopcart .content .contentLeft .price{
  display: inline-block;
  vertical-align: top;
  line-height: 24px;
  margin-top: 12px;
  box-sizing: border-box;
  padding-right: 12px;
  border-right: 1px solid rgba(255, 255, 255, 0.1);
  font-size: 16px;
  font-weight: 700;
  color: white;
}
.shopcart .content .contentLeft .desc{
  display: inline-block;
  vertical-align: top;
  line-height: 24px;
  margin: 12px 0 0 12px;
  color: rgba(255, 255, 255, 0.5);
  font-size: 10px;
}

.shopcart .content .contentRight{
  flex: 0 0 105px;
  width: 105px;
  font-size: 12px;
  color: rgba(255, 255, 255, 0.4);
  font-weight: 700;
  line-height: 48px;
  padding: 0 8px;
  box-sizing: border-box;
  background: #2b333b;
  text-align: center;
}
.shopcart .content .contentRight.not-enouth{
    background: #2b333b;
}
.shopcart .content .contentRight.enouth{
    background: #00b43c;
    color: white
}
.ball-container .ball{
  position: fixed;
  left: 32px;
  bottom: 22px;
  z-index: 200;
}
.ball-container .ball.drop-transition{
  transition: all 0.4
}
.ball-container .ball.drop-transition .inner{
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: rgb(0, 160, 220);
  transition: all 0.4
}
</style>
