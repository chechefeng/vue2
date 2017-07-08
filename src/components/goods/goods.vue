<template>
  <div class="goods">
    <div class="menu-wrapper" v-el:menu-wrapper>
      <ul>
        <li v-for="item in goods" class="menu-item" :class="{'current':currentIndex === $index}" @click="selectMenue($index,$event)">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" v-el:foods-wrapper>
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img width="57" height="57" :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="sellCount">月售{{food.sellCount}}</span>
                  <span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food='food'>
                  </cartcontrol>

                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shop-cart :select-foods='selectFoods' :delivery-price='seller.deliveryPrice' :min-price='seller.minPrice'></shop-cart>

  </div>

</template>
<script type="text/ecmascript-6">
  import BScoll from 'better-scroll'
  import shopCart from '../shopcart/shopcart.vue'
  import cartcontrol from '../cartcontrol/cartcontrol.vue'
      export default {
        props : {
              seller : {
                  type : Object
              }
        },
        data () {
            return {
                goods : [],
                listHeight : [],
                scrollY : 0
            }
        },
        computed : {
            currentIndex () {
                for (var i = 0; i< this.listHeight.length; i++) {
                  let heigth1 = this.listHeight[i]
                  let height2 = this.listHeight[i+1]
                  if (!height2||(this.scrollY >= heigth1 && this.scrollY < height2)) {
                      return i
                  }
                }
                return 0
            },
            selectFoods () {
              let foods = []
              this.goods.forEach((good) => {
                good.foods.forEach((food) => {
                  if (food.count) {
                    foods.push(food)
                  }
                })
              })
              return foods
            }
        },
        created () {
          this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']

          this.$http.get('/api/goods').then((response) => {
            response = response.body.data
            this.goods = response
            this.$nextTick(() => {
              this.initScoll()
              this.calculateHeight()
            })

          })
        },
        methods : {
            selectMenue (index, event) {

              if (!event._constructed) {
                return
              }
              let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook')
              let el = foodList[index]
              this.foodScoll.scrollToElement(el, 300)

            },
            initScoll () {
                this.meunScoll = new BScoll(this.$els.menuWrapper, {
                    click : true
                })
                this.foodScoll = new BScoll(this.$els.foodsWrapper, {
                  click : true,
                  probeType : 3
                })
                this.foodScoll.on('scroll', (pos) => {
                     // console.log(pos)
                    this.scrollY =Math.abs(Math.round(pos.y))
                })
            },
          calculateHeight () {
            let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook')
            let height = 0
            this.listHeight.push(height)
            for (var i = 0; i < foodList.length; i++) {
                let item = foodList[i]
                height += item.clientHeight
                this.listHeight.push(height)

            }
            // console.log(foodList[0].clientHeight)
          },
          _drop (target) {
            
          }

        },
        components : {
          shopCart,
          cartcontrol
        },
        events : {
          'cart.add' (target) {
            console.log(target)
             this._drop(target)
          }
        }
      }
</script>
<style>
  .goods{
    display: flex;
    position: absolute;
    top: 174px;
    bottom: 46px;
    width: 100%;
    overflow: hidden;
  }
  .goods .menu-wrapper{
    flex: 0 0 80px;
    width: 80px;
    background: #f3f5f7;
  }
  .menu-item{
    display: table;
    height: 54px;
    width: 56px;
    line-height: 14px;
    padding: 0 12px;
  }
  .menu-item.current{
    position: relative;
    margin-top: -1px;
    z-index: 10;
    background: #fff;
    font-weight: 700;
  }
  .goods .menu-wrapper .current .text:after{
    display: none;
  }

  .menu-item .icon{
    display: inline-block;
    width: 12px;
    height: 12px;
    margin-right: 2px;
    background-size: 12px 12px;
    background-repeat: no-repeat;

  }
  .decrease{
    background: url("decrease_3@2x.png");

  }
  .discount{
    background: url("discount_3@2x.png");

  }
  .invoice{
    background: url("invoice_3@2x.png");

  }
  .special{
    background: url("special_3@2x.png");

  }
  .guarantee{
    background: url("guarantee_3@2x.png");

  }
  .menu-item .text{
    display: table-cell;
    width: 56px;
    vertical-align: middle;
    font-size: 12px;
  }
  .goods .menu-wrapper .menu-item .text:after{
    display: block;
    width: 100%;
    border-bottom: 1px solid rgba(7,17,27,0.1);
    position: relative;
    left: 0;
    bottom: -15px;
    content: '';
  }

  .goods .menu-wrapper .current .text:after{
    display: none;
  }

  .goods .foods-wrapper{
    flex: 1;

  }
  .goods .foods-wrapper .title{
    height: 26px;
    font-size: 12px;
    color: rgb(147,153,159);
    line-height: 26px;
    background: #f3f5f7;
    border-left: 2px solid #d9dde1;
    padding-left: 14px;
    margin: 0px;
  }
  .goods .foods-wrapper .food-item{
    display: flex;
    margin: 18px;
    padding-bottom: 18px;
    position: relative;
  }
  .goods .foods-wrapper .food-item:last-child{
    margin: 18px;
    padding-bottom: 0px;
  }
  .goods .foods-wrapper .food-item:last-child:after{
    display: none;
  }
  .goods .foods-wrapper .food-item::after{
    display: block;
    width: 100%;
    height: 0;
    border-bottom: 1px solid rgba(7,17,27,0.1);
    position: absolute;
    left: 0;
    bottom: 0px;
    content: '';
  }
  .food-list .icon{
    flex: 0 0 57px;
    margin-right: 10px;
  }


  .food-list .content{
    flex: 1;
    margin-left: 0;
    font-size: 0px;
  }
  .food-list .content .name{
    font-size: 14px;
    color: rgb(7,17,27);
    line-height: 14px;
    margin: 2px 0 8px 0;
  }
  .food-list .content .desc , .extra{
    font-size: 10px;
    color: rgb(147,153,159);

    margin-bottom: 8px;
  }
  .food-list .content .extra{
    line-height: 10px;
  }
  .food-list .content .desc{
    line-height: 16px;

  }
  .food-list .content .extra .sellCount , .rating{
    display: inline-block;
    margin-right: 12px;
  }
  .food-list .content .now{
    font-size: 14px;
    color: rgb(240,20,20);
    font-weight: 700;
    line-height: 24px;
    margin-right: 8px;

  }
  .food-list .content .old{
    font-size: 10px;
    color: rgb(147,153,159);
    text-decoration: line-through;
  }
  .cartcontrol-wrapper{
    position: absolute;
    right: 0;
    bottom: 12px
  }
</style>
