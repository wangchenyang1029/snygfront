<template>
  <div class="order-confirm">
    <order-header title="订单确认">
      <template v-slot:tip>
        <span>请认真填写收货地址</span>
      </template>
    </order-header>
    <svg
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      style="position: absolute; width: 0px; height: 0px; overflow: hidden;"
    >
      <defs>
        <symbol id="icon-add" viewBox="0 0 31 32">
          <title>add</title>
          <path
            d="M30.745 15.152h-14.382v-14.596c0-0.308-0.243-0.557-0.543-0.557s-0.543 0.249-0.543 0.557v14.596h-14.665c-0.3 0-0.543 0.249-0.543 0.557s0.243 0.557 0.543 0.557h14.665v15.177c0 0.307 0.243 0.557 0.543 0.557s0.543-0.249 0.543-0.557v-15.177h14.382c0.3 0 0.543-0.249 0.543-0.557s-0.243-0.557-0.543-0.557z"
            class="path1"
          ></path>
        </symbol>
        <symbol id="icon-edit" viewBox="0 0 32 32">
          <title>edit</title>
          <path
            d="M28.287 8.51l-4.805-4.806 0.831-0.831c0.472-0.472 1.086-0.777 1.564-0.777 0.248 0 0.452 0.082 0.622 0.253l3.143 3.144c0.539 0.54 0.133 1.529-0.524 2.186l-0.831 0.831zM26.805 9.992l-1.138 1.138-4.805-4.806 1.138-1.138 4.805 4.806zM24.186 12.612l-14.758 14.762-4.805-4.806 14.758-14.762 4.805 4.806zM7.379 28.288l-4.892 1.224 1.223-4.894 3.669 3.67zM31.123 4.011l-3.143-3.144c-0.567-0.567-1.294-0.867-2.103-0.867-1.036 0-2.174 0.52-3.045 1.391l-20.429 20.436c-0.135 0.134-0.23 0.302-0.276 0.487l-2.095 8.385c-0.089 0.355 0.017 0.736 0.276 0.995 0.198 0.198 0.461 0.307 0.741 0.307 0.085 0 0.171-0.010 0.254-0.031l8.381-2.096c0.185-0.047 0.354-0.142 0.487-0.276l20.43-20.436c1.409-1.41 2.042-3.632 0.524-5.15v0z"
            class="path1"
          ></path>
        </symbol>
        <symbol id="icon-del" viewBox="0 0 32 32">
          <title>delete</title>
          <path
            d="M11.355 4.129v-2.065h9.29v2.065h-9.29zM6.194 29.935v-23.742h19.613v23.742h-19.613zM30.968 4.129h-8.258v-3.097c0-0.569-0.463-1.032-1.032-1.032h-11.355c-0.569 0-1.032 0.463-1.032 1.032v3.097h-8.258c-0.569 0-1.032 0.463-1.032 1.032s0.463 1.032 1.032 1.032h3.097v24.774c0 0.569 0.463 1.032 1.032 1.032h21.677c0.569 0 1.032-0.463 1.032-1.032v-24.774h3.097c0.569 0 1.032-0.463 1.032-1.032s-0.463-1.032-1.032-1.032v0z"
            class="path1"
          ></path>
          <path
            d="M10.323 9.806c-0.569 0-1.032 0.463-1.032 1.032v14.452c0 0.569 0.463 1.032 1.032 1.032s1.032-0.463 1.032-1.032v-14.452c0-0.569-0.463-1.032-1.032-1.032z"
            class="path2"
          ></path>
          <path
            d="M16 9.806c-0.569 0-1.032 0.463-1.032 1.032v14.452c0 0.569 0.463 1.032 1.032 1.032s1.032-0.463 1.032-1.032v-14.452c0-0.569-0.463-1.032-1.032-1.032z"
            class="path3"
          ></path>
          <path
            d="M21.677 9.806c-0.569 0-1.032 0.463-1.032 1.032v14.452c0 0.569 0.463 1.032 1.032 1.032s1.032-0.463 1.032-1.032v-14.452c0-0.569-0.463-1.032-1.032-1.032z"
            class="path4"
          ></path>
        </symbol>
      </defs>
    </svg>
    <div class="wrapper">
      <div class="container">
        <div class="order-box">
          <div class="item-address">
            <h2 class="addr-title">收货地址</h2>
            <div class="addr-list clearfix">
              <div
                class="addr-info"
                :class="{ checked: index == checkIndex }"
                @click="checkIndex = index"
                v-for="(item, index) in list"
                :key="index"
              >
                <h2>{{ item.recipient }}</h2>
                <div class="phone">{{ item.phone }}</div>
                <div class="street">
                  {{
                    item.province +
                      ' ' +
                      item.city +
                      ' ' +
                      item.district +
                      ' ' +
                      item.detail
                  }}
                </div>
                <div class="action">
                  <a href="javascript:;" class="fl" @click="delAddress(item)">
                    <svg class="icon icon-del">
                      <use xlink:href="#icon-del"></use>
                    </svg>
                  </a>
                  <a
                    href="javascript:;"
                    class="fr"
                    @click="editAddressModal(item)"
                  >
                    <svg class="icon icon-edit">
                      <use xlink:href="#icon-edit"></use>
                    </svg>
                  </a>
                </div>
              </div>
              <div class="addr-add" @click="openAddressModal">
                <div class="icon-add"></div>
                <div>添加新地址</div>
              </div>
            </div>
          </div>
          <div class="item-good">
            <h2>商品</h2>
            <ul>
              <li v-for="(item, index) in cartList" :key="index">
                <div class="good-name">
                  <img :src="'img/productSingle/' + item.firstProductImage + '.jpg'" alt="" />
                  <span>{{
                 item.name + ' , ' + item.subTitle
                  }}</span>
                </div>
                <div class="good-price">
                  {{ item.promotePrice }}元x{{ item.number }}
                </div>
                <div class="good-total">{{ item.productTotalPrice }}元</div>
              </li>
            </ul>
          </div>
          <div class="item-shipping">
            <h2>配送方式</h2>
            <span>包邮</span>
          </div>
          <div class="item-invoice">
            <h2>发票</h2>
            <a href="javascript:;">电子发票</a>
            <a href="javascript:;">个人</a>
          </div>
          <div class="detail">
            <div class="item">
              <span class="item-name">商品件数：</span>
              <span class="item-val">{{ count }}件</span>
            </div>
            <div class="item">
              <span class="item-name">商品总价：</span>
              <span class="item-val">{{ cartTotalPrice }}元</span>
            </div>
            <div class="item">
              <span class="item-name">优惠活动：</span>
              <span class="item-val">0元</span>
            </div>
            <div class="item">
              <span class="item-name">运费：</span>
              <span class="item-val">0元</span>
            </div>
            <div class="item-total">
              <span class="item-name">应付总额：</span>
              <span class="item-val">{{ cartTotalPrice }}元</span>
            </div>
          </div>
          <div class="btn-group">
            <a href="/#/cart" class="btn btn-default btn-large">返回购物车</a>
            <a href="javascript:;" class="btn btn-large" @click="orderSubmit"
              >去结算</a
            >
          </div>
        </div>
      </div>
    </div>
    <modal
      title="新增确认"
      btnType="1"
      :showModal="showEditModal"
      @cancel="showEditModal = false"
      @submit="submitAddress"
    >
      <template v-slot:body>
        <div class="edit-wrap">
          <div class="item">
            <input
              type="text"
              class="input"
              placeholder="姓名"
              v-model="checkedItem.recipient"
            />
            <input
              type="text"
              class="input"
              placeholder="手机号"
              v-model="checkedItem.phone"
            />
          </div>
          <div class="item">
            <select name="province" v-model="checkedItem.province">
              <option value="湖南">湖南</option>
              <option value="湖北">湖北</option>
              <option value="广东">广东</option>
            </select>
            <select name="city" v-model="checkedItem.city">
              <option value="长沙">长沙</option>
              <option value="株洲">株洲</option>
              <option value="湘潭">湘潭</option>
              <option value="衡阳">衡阳</option>
            </select>
            <select name="district" v-model="checkedItem.district">
              <option value="天心区">天心区</option>
              <option value="雨花区">雨花区</option>
              <option value="岳麓区">岳麓区</option>
              <option value="芙蓉区">芙蓉区</option>
            </select>
          </div>
          <div class="item">
            <textarea
              name="street"
              v-model="checkedItem.detail"
            ></textarea>
          </div>
          <div class="item">
            <input
              type="text"
              class="input"
              placeholder="邮编"
              v-model="checkedItem.post"
            />
          </div>
        </div>
      </template>
    </modal>
    <modal
      title="删除确认"
      btnType="1"
      :showModal="showDelModal"
      @cancel="showDelModal = false"
      @submit="submitAddress"
    >
      <template v-slot:body>
        <p>您确认要删除此地址吗？</p>
      </template>
    </modal>
  </div>
</template>
<script>
import OrderHeader from './../components/OrderHeader'
import Modal from './../components/Modal'
// import { message } from 'ant-design-vue'
// import { message } from 'ant-design-vue'
import Qs from 'qs';
export default {
  name: 'order-confirm',
  data() {
    return {
      list: [], //收货地址列表
      cartList: [], //购物车中需要结算的商品列表
      cartTotalPrice: 0, //商品总金额
      count: 0, //商品结算数量
      checkedItem: {}, //选中的商品对象
      userAction: '', //用户行为 0：新增 1：编辑 2：删除
      showDelModal: false, //是否显示删除弹框
      showEditModal: false, //是否显示新增或者编辑弹框
      checkIndex: 0, //当前收货地址选中索引
    }
  },
  components: {
    OrderHeader,
    Modal,
  },
  mounted() {
        this.getItemList()
    this.getAddressList()

  },
  methods: {
    getAddressList() {
      this.$axios.get('/address/address_list').then((res) => {
        this.list = res

      })
   
    },
     getItemList() {
     let res =  this.$route.params.res || JSON.stringify({'orderItems': [], 'total': 700});
     res = JSON.parse(res)
          // eslint-disable-next-line no-console
    //  console.log("ddd")
      let data_list = []
       let t_result = res.orderItems
       t_result.forEach((item) =>{
        item['productSelected'] = false;
        item['productTotalPrice'] = (item.product.promotePrice * item.number).toFixed(2);
        data_list.push({"id": item.id, "number":item.number, "productTotalPrice": item.productTotalPrice, "firstProductImage": item.product.firstProductImage.id,
        "name":item.product.name, "subTitle":item.product.subTitle, "promotePrice":item.product.promotePrice})
      })
              t_result.forEach((item) =>{
        item['productSelected'] = false;
        item['productTotalPrice'] = (item.product.promotePrice * item.number).toFixed(2);
      })
       t_result.map((item) => {
        this.count += item.number
        })

        this.cartTotalPrice = res.total //商品总金额
        this.cartList = data_list
    },
    // 打开新增地址弹框
    openAddressModal() {
      this.userAction = 0
      this.checkedItem = {}
      this.showEditModal = true
    },
    // 打开新增地址弹框
    editAddressModal(item) {
      this.userAction = 1
      this.checkedItem = item
      this.showEditModal = true
    },
    delAddress(item) {
      this.checkedItem = item
      this.userAction = 2
      this.showDelModal = true
    },
    // 地址删除、编辑、新增功能
    submitAddress() {
      let { checkedItem, userAction } = this
      let method,
        url,
        params = {}
      if (userAction == 0) {
        // eslint-disable-next-line no-extra-semi
        ;(method = 'post'), (url = '/address/add_address')
      } else if (userAction == 1) {
        // eslint-disable-next-line no-extra-semi
        ;(method = 'put'), (url = '/address/add_address')  // 待定
      } else {
        // eslint-disable-next-line no-extra-semi
        ;(method = 'delete'), (url = '/address/delete_address')
      }
      if (userAction == 0 || userAction == 1) {
        let {
          recipient,
          phone,
          province,
          city,
          district,
          detail,
          post
        } = checkedItem
        let errMsg = ''
        if (!recipient) {
          errMsg = '请输入收货人名称'
        } else if (!phone || !/\d{11}/.test(phone)) {
          errMsg = '请输入正确格式的手机号'
        } else if (!province) {
          errMsg = '请选择省份'
        } else if (!city) {
          errMsg = '请选择对应的城市'
        } else if (!detail || !district) {
          errMsg = '请输入收货地址'
        } else if (!/\d{6}/.test(post)) {
          errMsg = '请输入六位邮编'
        }
        if (errMsg) {
          this.$message.error(errMsg)
          return
        }
              params = {
          recipient,
          phone,
          province,
          city,
          district,
          detail,
          post,
        }

  
      }
      else{
      //       params = {
      //  aid: checkedItem.id,
      //   };

        params = {
        params: {
          aid: checkedItem.id,
        },}
     
      }
      this.$axios[method](url, params).then(() => {
        this.closeModal()
        this.getAddressList()
        this.$message.success('操作成功')
      })
    },
    closeModal() {
      this.checkedItem = {}
      this.userAction = ''
      this.showDelModal = false
      this.showEditModal = false
    },
    getCartList() {
      this.$axios.get('/carts').then((res) => {
        let list = res.cartProductVoList //获取购物车中所有商品数据
        this.cartTotalPrice = 500 //商品总金额
        this.cartList = list.filter((item) => item.productSelected)
        this.cartList.map((item) => {
          this.count += item.number
        })
      })
    },
  
    // 订单提交
    orderSubmit() {
      let item = this.list[this.checkIndex]
      if (!item) {
        this.$message.error('请选择一个收货地址')
        return
      }
      // message.info(item.id)
      this.$axios
        .post('/fore_createOrder', Qs.stringify({
        aid:item.id,
     }))
        .then((res) => {
          this.$router.push({
            path: '/order/pay',
            query: {
              orderNo: res.oid,
            },
          })
        })
    },
  },
}
</script>
<style lang="scss">
.order-confirm {
  .wrapper {
    background-color: #f5f5f5;
    padding-top: 30px;
    padding-bottom: 84px;
    .order-box {
      background-color: #ffffff;
      padding-left: 40px;
      padding-bottom: 40px;
      .addr-title {
        font-size: 20px;
        color: #333333;
        font-weight: 200;
        margin-bottom: 21px;
      }
      .item-address {
        padding-top: 38px;
        .addr-list {
          .addr-info,
          .addr-add {
            box-sizing: border-box;
            float: left;
            width: 271px;
            height: 180px;
            border: 1px solid #e5e5e5;
            margin-right: 15px;
            padding: 15px 24px;
            font-size: 14px;
            color: #757575;
          }
          .addr-info {
            cursor: pointer;
            h2 {
              height: 27px;
              font-size: 18px;
              font-weight: 300;
              color: #333;
              margin-bottom: 10px;
            }
            .street {
              height: 50px;
            }
            .action {
              height: 50px;
              line-height: 50px;
              .icon {
                width: 20px;
                height: 20px;
                fill: #666666;
                vertical-align: middle;
                &:hover {
                  fill: #ff6700;
                }
              }
            }
            &.checked {
              border: 1px solid #ff6700;
            }
          }
          .addr-add {
            text-align: center;
            color: #999999;
            cursor: pointer;
            .icon-add {
              width: 30px;
              height: 30px;
              border-radius: 50%;
              background: url('/imgs/icon-add.png') #e0e0e0 no-repeat center;
              background-size: 14px;
              margin: 0 auto;
              margin-top: 45px;
              margin-bottom: 10px;
            }
          }
        }
      }
      .item-good {
        margin-top: 34px;
        border-bottom: 1px solid #e5e5e5;
        padding-bottom: 12px;
        h2 {
          border-bottom: 1px solid #e5e5e5;
          padding-bottom: 5px;
        }
        li {
          display: flex;
          align-items: center;
          height: 40px;
          line-height: 40px;
          margin-top: 10px;
          font-size: 14px;
          color: #333333;
          .good-name {
            flex: 5;
            img {
              width: 30px;
              height: 30px;
              vertical-align: middle;
            }
          }
          .good-price {
            flex: 2;
          }
          .good-total {
            padding-right: 44px;
            color: #ff6600;
          }
        }
      }
      .item-shipping,
      .item-invoice {
        margin-top: 31px;
        line-height: 20px;
        h2 {
          display: inline-block;
          margin-right: 71px;
          font-size: 20px;
          width: 80px;
        }
        span,
        a {
          font-size: 16px;
          color: #ff6700;
          margin-right: 23px;
        }
      }
      .detail {
        padding: 50px 44px 33px 0;
        border-bottom: 1px solid #f5f5f5;
        text-align: right;
        font-size: 16px;
        color: #666666;
        .item-val {
          color: #ff6700;
        }
        .item {
          line-height: 15px;
          margin-bottom: 12px;
        }
        .item-val {
          display: inline-block;
          width: 100px;
        }
        .item-total {
          .item-val {
            font-size: 22px;
          }
        }
      }
      .btn-group {
        margin-top: 37px;
        text-align: right;
      }
    }
  }
  .edit-wrap {
    font-size: 14px;
    .item {
      margin-bottom: 15px;
      .input {
        display: inline-block;
        width: 283px;
        height: 40px;
        line-height: 40px;
        padding-left: 15px;
        border: 1px solid #e5e5e5;
        & + .input {
          margin-left: 14px;
        }
      }
      select {
        height: 40px;
        line-height: 40px;
        border: 1px solid #e5e5e5;
        margin-right: 15px;
      }
      textarea {
        height: 62px;
        width: 100%;
        padding: 13px 15px;
        box-sizing: border-box;
        border: 1px solid #e5e5e5;
      }
    }
  }
}
</style>
