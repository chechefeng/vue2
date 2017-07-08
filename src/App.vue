<template>
  <div id='app'>
    <heads :sellers = 'seller'></heads>
    <div class='tab'>
        <div class="tab-item">
          <a v-link="{path:'/goods'}">商品</a>
        </div>
        <div class="tab-item">
          <a v-link="{path:'/ratings'}">评论</a>
        </div>
        <div class="tab-item">
          <a v-link="{path:'/seller'}">商家</a>
        </div>

    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import heads from './components/header/header.vue'

  export default{
      data () {
        return {
            seller : {}
        }
      },
      components : {
        heads

      },
    created () {
        this.$http.get('/api/seller').then((response) => {
            this.seller = response.body.data

        })
    }
  }
</script>
<style>

  .tab{
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    position: relative;

  }
  .tab:after{
    display: block;
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    border-top: 1px solid rgba(7,17,27,0.1);
    content: '  ';
  }
  .tab-item{
    flex: 1;
    text-align: center;

  }
  a{
    display: block;
    font-size: 14px;
    color: rgb(77,85,93);
  }
  .active{
    color: rgb(240,20,20);
  }

</style>
