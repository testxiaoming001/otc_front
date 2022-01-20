<template>
  <div class="my_ad_container">
    <div class="contbox">
        <div class="send-box">
          <div class="formbox send-form">
            <Form
              :label-width="90"
            >
             <FormItem label="广告ID">
               {{detail.ad_id}}
             </FormItem>
              <FormItem label="订单ID">
               {{detail.order_id}}
             </FormItem>
              <FormItem label="btc数量">
               {{detail.btc_number}}
             </FormItem>
              <FormItem label="买家地址">
               {{detail.buyer_address}}
             </FormItem>
                <FormItem label="卖家地址">
               {{detail.seller_address}}
             </FormItem>
                   <FormItem label="买家ID">
               {{detail.buyer_id}}
             </FormItem>
              <FormItem label="订单创建时间">
               {{detail.order_create_time}}
             </FormItem>
              <FormItem label="付款地址">
           {{detail.pay_address_type}}  -  {{detail.pay_address}}
             </FormItem>
              <FormItem label="订单价格">
               {{detail.order_price}}
             </FormItem>
            </Form>
          </div>
        </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      detail: {},
    };
  },
  mounted() {
    // TODO 订单详情
    this.$http
      .post(this.apiHost + "/Usdt_order/order_info", {
        token: localStorage.getItem("TOKEN"),
        order_id: this.$route.query.id,
      })
      .then((resp) => {
        if (resp.data.code == 1) {
          this.detail = resp.data.data;
        } else {
          this.$Message.error(resp.data.msg);
        }
      });
  },
};
</script>
<style>
.my_ad_container .my_ad_container_spin.ivu-spin-fix .ivu-spin-main {
  top: 200px;
}
</style>

<style scoped lang="scss">
.my_ad_container {
  width: 80%;
  float: right;
}
.cankao {
  color: #e24a64;
}
.contbox {
  position: relative;
}
#price {
  font-size: 18px;
  color: #e24a64;
}

.send-box .send-form .msg {
  padding-left: 90px;
  margin-bottom: 10px;
  position: relative;
  top: -4px;
}

.formbox {
  width: 50%;
  padding-top: 30px;
}

.send-box {
  color: #fff;
  padding: 32px;
}

.title-box {
  /*border-left: 1px dashed #ebeff5;*/
  border-bottom: 1px dashed #ccc;
  padding-bottom: 30px;
  text-align: left;
  padding-left: 18px;
}

.title-box .titles {
  font-size: 18px;
  font-weight: normal;
  color: #fff;
  margin-bottom: 15px;
}

.title-box p {
  line-height: 2;
}

.title-box p a {
  color: #f0a70a;
}

.order-table {
  margin-top: 20px;
}

.content-wrap {
  // background: #f5f5f5;
  min-height: 750px;
}

.container {
  margin: 0 auto;
}
</style>
