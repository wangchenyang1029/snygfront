<template>
  <div class="detail">
    <product-param v-bind:title="product.name"></product-param>
    <div class="wrapper">
      <div class="container clearfix">
        <div class="swiper">
          <a-carousel autoplay>
            <!-- <div><img src="/imgs/detail/phone-1.jpg" alt="" /></div>
            <div><img src="/imgs/detail/phone-2.jpg" alt="" /></div>
            <div><img src="/imgs/detail/phone-3.jpg" alt="" /></div>
            <div><img src="/imgs/detail/phone-4.jpg" alt="" /></div> -->
                 <div v-for="(item, index) in product.productSingleImages" v-bind:key="index">
    <img :src="'img/productSingle/' + item.id + '.jpg'" alt="" /></div>
          </a-carousel>
        </div>
        <div class="content">
          <h2 class="item-title">{{ product.name }}</h2>
          <p class="item-info">
          {{ product.subTitle }}
          </p>
          <div class="delivery"> 促销价： <span class="del">{{ product.originalPrice }}元</span>  </div>
          <div class="item-price">
              {{ product.promotePrice }}元
          </div>
    <!-- <span class="stock_k"> 库存： <span class="">{{ product.stock }}</span></span>  </div>   -->
          <!-- <div class="line"></div> -->
           <div class="stock_k"> 库存： <span class="">{{ product.stock }}</span>  </div>
                  <div class="productNumber">
                <span>数量：</span>
                <span>
                    <span class="productNumberSettingSpan">
                    <input class="productNumberSetting" type="text" v-model="item_num">
                    </span>
                    <span class="arrow">
                        <a class="increaseNumber" v-on:click="add_product_num">
                        <span class="updown">
                                <img src="img/site/increase.png">
                        </span>
                        </a>

                        <span class="updownMiddle"> </span>
                        <a class="decreaseNumber" v-on:click="reduce_product_num">
                        <span class="updown">
                                <img src="img/site/decrease.png">
                        </span>
                        </a>

                    </span>

                件</span>
              
            </div>
          <!-- <div class="item-addr">
            <i class="icon-loc"></i>
            <div class="addr">北京 北京市 朝阳区 安定门街道</div>
          </div> -->
   
          <!-- <div class="item-color">
            <h2>选择颜色</h2>
            <div class="phone checked">
              <span class="color"></span>
              深空灰
            </div>
          </div> -->
          <!-- <div class="item-total">
            <div class="phone-info clearfix">
              <div class="fl">
                {{ product.name }}
                {{ version == 1 ? '6GB+64GB 全网通' : '4GB+64GB 移动4G' }}
                深灰色  X 1
              </div>
              <div class="fr">{{ product.price }}元</div>
            </div>
          
          </div> -->
          <div class="btn-group">
                <a href="javascript:;" class="btn fl btn-single">
                  <div style="width:100%">
                    <div style="width:100%; height: 24px">￥{{ product.originalPrice }} </div>
              <div style="width:100%; height: 24px">单独购买 </div>
              </div>
              
              </a
            >
            <a href="javascript:;" class="btn  fl text-two btn-pindan" @click="addCart"
              >
                             <div style="width:100%">
                    <div style="width:100%; height: 24px">￥{{ product.promotePrice }} </div>
              <div style="width:100%; height: 24px">发起拼单 </div>
              </div>
              </a
            >
          </div>
        </div>
      </div>
    </div>
   <div>
       <div class="product">
    <div class="content">
           <div class="productParamterPart">
            <div class="productParamter">产品参数：</div>
            <div class="productParamterList">
                <span v-for="(pv, i) in pvs"   v-bind:key="i">{{pv.property.name}}: {{pv.value}} </span>
            </div>
            <div style="clear:both"></div>
        </div>
    <div class="productDetailImagesPart">
            <img v-for="(pi, i) in product.productDetailImages" :src="'img/productDetail/'+pi.id+'.jpg'" v-bind:key="i">
        </div>
    </div>
  </div>
   </div>
    <service-bar></service-bar>
  </div>
</template>
<script>
import { getCurrentInstance, onMounted, ref } from 'vue'
import ProductParam from './../components/ProductParam'
import ServiceBar from './../components/ServiceBar'
// import Qs from 'qs';
export default {
  name: 'detail',
  components: {
    ProductParam,
    ServiceBar,
  },
  setup() {
    const { proxy } = getCurrentInstance()
    let id = proxy.$router.currentRoute.value.params.id //获取商品ID
    let item_num = ref(1)
    const version = ref(1) //商品版本切换
    const product = ref({}) //商品信息
    const pvs = ref([]) //商品信息
    const reviews = ref([]) //商品信息
    onMounted(() => {
      getProductInfo()
    })
    const getProductInfo = () => {
      proxy.$axios.get(`/product/${id}`).then((res) => {
                // eslint-disable-next-line no-console
        console.log(res);
        product.value = res.product;
        pvs.value = res.pvs;
        reviews.value=[res.reviews];

    
      })
    }
    const addCart = () => {

       if (sessionStorage.getItem("username") == '' || sessionStorage.getItem("username") == "undefined" || sessionStorage.getItem("username") == null){
        proxy.$router.push('/login')
       }
      proxy.$axios
        .get('/add_cart', {
        params: {
           pid: id,
          num: item_num.value,
        },
      })
        .then(() => {
          let cart_count = proxy.$store.state.cartCount;
          proxy.$store.dispatch('saveCartCount', cart_count + item_num.value)
          proxy.$router.push({path:'/cart',
          params: {
            date:new Date().getTime()
          }})
        })
    } 

    const add_product_num = () =>{
      let temp_value =  Number(item_num.value);
      if (temp_value < product.value.stock){
        temp_value = temp_value + 1;
      }
      item_num.value = temp_value

    }
    
        const reduce_product_num = () =>{
      let temp_value =  Number(item_num.value);
      if (temp_value > 1){
        temp_value = temp_value - 1;
      }
      item_num.value = temp_value

    }

    return {
      id,
      item_num,
      version,
      product,
      getProductInfo,
      addCart,
      pvs,
      reviews,
      add_product_num,
      reduce_product_num
    }
  },
}
</script>
<style lang="scss">
@import './../assets/scss/config.scss';
@import './../assets/scss/mixin.scss';
// .text-two{ 
//   margin: 0;
//   padding: 0;
//   br{
//       margin: 0;
//   }
// }
.product{
  .content{
    div.productParamterPart {
	border: 1px solid #DFDFDF;
	padding: 40px 171px;
}

div.productParamter {
	color: #999999;
	font-weight: bold;
	margin-bottom: 20px;
  
}

div.productDetailImagesPart {
width: 80%;
    margin-left: auto;
    margin-right: auto;
}
div.productParamterList span {
	display: block;
	width: 220px;
	float: left;
	padding: 8px 0px;
	color: #666666;
}
div.productDetailImagesPart img {
	display: block; 
  margin: 20px 0px;
	width: 790px;
  width: 100%;
    margin-left: auto;
    margin-right: auto;
}
  }
}
.btn-single{
 width: 202px;
    height: 57px;
    line-height: 34px;
    font-size: 16px;
    background-color: rgb(244,170,167);
}
.btn-pindan{
width: 202px;
    height: 57px;
     line-height: 34px;
    font-size: 16px;
}
.detail {
  .wrapper {
    .swiper {
      float: left;
      width: 450px;
      height: 450px;
      margin-top: 15px;
      margin-left: 48px;
      border: #999999 solid 1px;
      img {
        width: 100%;
        height: 100%;
      }
    }
    .content {
      float: right;
      width: 584px;
      height: 558px;
      .item-title {
        // font-size: 28px;
        padding-top: 30px;
        padding-bottom: 29px;
        height: 22px;
        color: #666;
        font: 700 16px Arial,"microsoft yahei";
      }
      .item-info {
        font-size: 12px;
        // height: 36px;
        color: #e4393c;
        margin-top: 15px;
      }
      .delivery {
        font-size: 16px;
        color: #666;
        margin-top: 18px;
        margin-bottom: 14px;
        height: 15px;
                .del {
          font-size: 16px;
          color: #999999;
          margin-left: 10px;
          text-decoration: line-through;
          margin-right: 5px;
        }
 
      }
             .stock_k{
     font-size: 14px;
  margin-top: 30px;
  color: #666;
  margin-bottom: 8px;
        }
      .item-price {
        font-size: 20px;
        color: #e4393c;
        height: 46px;
        background-color: #f3f3f3;
        padding-left: 32px;
        padding-top: 3px;
        font-size: 26px;
        margin-top: 20px;

      }
      .line {
        height: 0;
        margin-top: 25px;
        margin-bottom: 28px;
        border-top: 1px solid #e5e5e5;
      }
      .item-addr {
        height: 108px;
        background-color: #fafafa;
        border: 1px solid #e5e5e5;
        box-sizing: border-box;
        padding-top: 31px;
        padding-left: 64px;
        font-size: 14px;
        line-height: 14px;
        position: relative;
        .icon-loc {
          position: absolute;
          top: 27px;
          left: 34px;
          @include bgImg(20px, 20px, '/imgs/detail/icon-loc.png');
        }
        .addr {
          color: #666666;
        }
        .stock {
          margin-top: 15px;
          color: #ff6700;
        }
      }
      .item-version,
      .item-color {
        margin-top: 30px;
        h2 {
          font-size: 18px;
          margin-bottom: 20px;
        }
      }
      .item-version,
      .item-color {
        .phone {
          width: 287px;
          height: 50px;
          line-height: 50px;
          font-size: 16px;
          color: #333333;
          border: 1px solid #e5e5e5;
          box-sizing: border-box;
          text-align: center;
          cursor: pointer;
          span {
            color: #666666;
            margin-left: 15px;
          }
          .color {
            display: inline-block;
            width: 14px;
            height: 14px;
            background-color: #666666;
          }
          &.checked {
            border: 1px solid #ff6600;
            color: #ff6600;
          }
        }
      }
      .item-total {
        height: 108px;
        background: #fafafa;
        padding: 24px 33px 29px 30px;
        font-size: 14px;
        margin-top: 50px;
        margin-bottom: 30px;
        box-sizing: border-box;
        .phone-total {
          font-size: 24px;
          color: #ff6600;
          margin-top: 18px;
        }
      }
    }
  }
  .price-info {
    background-color: #f3f3f3;
    height: 340px;
    h2 {
      font-size: 24px;
      color: #333333;
      padding-top: 38px;
      margin-bottom: 30px;
    }
  }
}
@import './../assets/scss/mixin.scss';
.product {
  .content {
    .item-bg {
      background: url('/imgs/product/product-bg-1.png') no-repeat center;
      height: 718px;
      text-align: center;
      h2 {
        font-size: 80px;
        padding-top: 55px;
      }
      h3 {
        font-size: 24px;
        letter-spacing: 10px;
      }
      p {
        margin-top: 21px;
        margin-bottom: 40px;
        a {
          font-size: 16px;
          color: #333333;
        }
        span {
          margin: 0 15px;
        }
      }
      .price {
        font-size: 30px;
        color: #333333;
        em {
          font-style: normal;
          font-size: 38px;
        }
      }
    }
    .item-bg-2 {
      background: url(/imgs/product/product-bg-2.png) no-repeat center;
      height: 480px;
      background-size: 1226px 397px;
    }
    .item-bg-3 {
      background: url(/imgs/product/product-bg-3.png) no-repeat center;
      height: 638px;
      background-size: cover;
    }
    .ant-item-swiper {
      .ant-carousel {
        margin: 60px;
      }
      margin: 36px auto 52px;
      .desc {
        font-size: 18px;
        color: #333333;
        text-align: center;
      }
      img {
        width: 100%;
      }
    }

    .item-video {
      height: 1044px;
      background-color: #070708;
      margin-bottom: 76px;
      color: #ffffff;
      text-align: center;
      h2 {
        font-size: 60px;
        padding-top: 82px;
        margin-bottom: 47px;
      }
      p {
        font-size: 24px;
        margin-bottom: 58px;
      }
      .video-bg {
        background: url('/imgs/product/gallery-1.png') no-repeat center;
        background-size: cover;
        width: 1226px;
        height: 540px;
        margin: 0 auto 120px;
        cursor: pointer;
      }
      .video-box {
        .overlay {
          @include position(fixed);
          background-color: #333333;
          opacity: 0.4;
          z-index: 10;
        }
        @keyframes slideDown {
          from {
            top: -50%;
            opacity: 0;
          }
          to {
            top: 50%;
            opacity: 1;
          }
        }
        @keyframes slideUp {
          from {
            top: 50%;
            opacity: 1;
          }
          to {
            top: -50%;
            opacity: 0;
          }
        }
        .video {
          position: fixed;
          top: -50%;
          left: 50%;
          transform: translate(-50%, -50%);
          z-index: 10;
          width: 1000px;
          height: 536px;
          opacity: 1;
          &.slideDown {
            animation: slideDown 0.6s linear;
            top: 50%;
          }
          &.slideUp {
            animation: slideUp 0.6s linear;
          }
          .icon-close {
            position: absolute;
            top: 20px;
            right: 20px;
            @include bgImg(20px, 20px, '/imgs/icon-close.png');
            cursor: pointer;
            z-index: 11;
          }
          video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            outline: none;
          }
        }
      }
    }
  }
  button {
    margin-left: 10px;
  }
}
div.productNumber {
	// color: #999999;
  font-size: 14px;
      color: #666;
      margin-bottom: 20px;
      margin-top: 15px;
}

span.productNumberSettingSpan {
	border: 1px solid #999;
	display: inline-block;
    width: 42px;
    height: 27px;
    padding: 3px 4px;
}

input.productNumberSetting {
	border: 0px;
	height: 80%;
	width: 80%;
}

div.productNumber span.glyphicon {
	font-size: 6px;
}

div.productNumber span.arrow {
    display: inline-block;
    width: 15px;
    height: 14px;
    vertical-align:top;
    margin-left: 4px;
}

div.productNumber span.updown img{
    display:inline-block;
    vertical-align:top;
}
div.productNumber span.updown {
    border: 0px solid #999;
    display: block;
    width: 12px;
    height: 10px;
    text-align: center;
    padding-top:4px;
}

div.productNumber span.updownMiddle {
	height: 4px;
	display: block;
}

</style>
