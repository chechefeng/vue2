<template >
  <div class="cartcontrol">
    <div class="cart-decrease " v-show="food.count" @click="decreasecart" transition='move'>
      <span class="inner icon-remove_circle_outline">

      </span>
    </div>
    <div class="cart-count" v-show="food.count>0">
        {{food.count}}
    </div>
    <div class="cart-add icon-add_circle" @click="addcart">
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
import Vue from 'vue'
export default{
  props : {
    food : {
      type : Object
    }
  },
  created () {
 },
 methods : {
   addcart (event) {
     if (!event._constructed) {
       return
     }
     console.log(1)
     if (!this.food.count) {
       Vue.set(this.food, 'count', 1)
     } else {
        this.food.count++
     }
     this.$dispatch('cart.add', event.target)
   },
   decreasecart (event) {
     if (!event._constructed) {
       return
     }
     if (this.food.count) {
       this.food.count--
     }
   }
 }
}
</script>
<style>
.cartcontrol{
  font-size: 0;
  color: rgb(0, 160, 220);
}
.cartcontrol .cart-decrease{
  display: inline-block;
  padding: 6px;
  transition: all 1s linear;
}
.cartcontrol .cart-decrease.move-transition{
  opacity: 1;
  transform: translate3d(0,0,0);
  width: 24px
}
.cartcontrol .cart-decrease .inner{
  display: inline-block;
  line-height: 24px;
  font-size: 24px;
  color: rgb(0, 160, 220);
  transition: all 1s linear;
  transform: rotate(0);
}

.cartcontrol .cart-decrease.move-enter{
  opacity: 0;
  transform: translate3d(24px,0,0);
}
.cartcontrol .cart-decrease.move-enter .inner{
  transform: rotate(180deg);
}
.cartcontrol .cart-decrease.move-leave{
  opacity: 0;
  transform: translate3d(24px,0,0);
}
.cartcontrol .cart-count{
  display: inline-block;
  line-height: 24px;
  font-size: 10px;
  vertical-align: top;
  width: 12px;
  padding-top: 6px;
  line-height: 24px;
  text-align: center;
  color: rgb(147, 153, 19);
}
.cartcontrol .cart-add{
  display: inline-block;
  padding: 6px;
  line-height: 24px;
  font-size: 24px;
}
</style>
