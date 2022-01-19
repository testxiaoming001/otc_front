<template>
  <div class="my_ad_container">
    <div class="contbox">
      <Spin v-show="isSpinShow" class="my_ad_container_spin" fix></Spin>
      <div class="">
        <div class="send-box">
          <div class="title-box">
            <h6 class="titles">
              {{ $t("otc.publishad.createad") }}
              <i class="iconfont icon-hongjiantou"></i>
            </h6>
            <p>
              {{ $t("otc.publishad.msg1") }}
              <router-link to="/otc/trade/usdt">{{
                $t("otc.publishad.tradead")
              }}</router-link>
              。
            </p>
            <p>
              {{ $t("otc.publishad.msg2") }}{{ $t("otc.publishad.msg3") }}。
            </p>
            <p>
              {{ $t("otc.publishad.msg4") }}
              <router-link to="/uc/ad">{{
                $t("otc.publishad.myad")
              }}</router-link>
              。
            </p>
          </div>
          <!--  -->
          <div class="formbox send-form">
            <Form
              ref="form"
              :model="form"
              :rules="ruleValidate"
              :label-width="90"
            >
              <FormItem :label="$t('otc.publishad.iwant')" prop="advertiseType">
                <RadioGroup v-model="form.advertiseType">
                  <Radio label="1" :disabled="isId">{{
                    $t("otc.publishad.sellonline")
                  }}</Radio>
                  <Radio label="0" :disabled="isId">{{
                    $t("otc.publishad.buyonline")
                  }}</Radio>
                </RadioGroup>
              </FormItem>
              <FormItem
                :label="wantstyle + $t('otc.publishad.title')"
                prop="title"
              >
                <Input
                  v-model="form.title"
                  :placeholder="$t('otc.publishad.titleplaceholder')"
                ></Input>
              </FormItem>
              <!-- <FormItem :label="$t('otc.publishad.exchangecoin')" prop="coin">
                <Select
                  v-model="form.coin"
                  :disabled="isId"
                  @on-change="changeCoin"
                >
                  <Option
                    v-for="(item, index) in coinList"
                    :value="item.id"
                    :key="index"
                    >{{ item.unit }}</Option
                  >
                </Select>
              </FormItem> -->
              <!-- 国家 -->
              <!-- <FormItem :label="$t('otc.publishad.country')" prop="country">
                <Select v-model="form.country" @on-change="onAreaChange">
                  <Option
                    v-for="(area, index) in areas"
                    :value="area.zhName"
                    :key="index"
                    >{{ area.zhName }}</Option
                  >
                </Select>
              </FormItem> -->
              <!-- 1 -->
              <!-- <FormItem :label="$t('otc.publishad.currency')" prop="rmb">
                <Input v-model="form.rmb" disabled placeholder=""></Input>
              </FormItem> -->
              <!-- <FormItem :label="$t('otc.publishad.openfixedprice')">
                <i-switch v-model="form.fixed" size="large">
                  <span slot="open">{{ $t("otc.publishad.open") }}</span>
                  <span slot="close">{{ $t("otc.publishad.close") }}</span>
                </i-switch>
              </FormItem>
              <p class="msg" v-show="form.fixed">
                {{ $t("otc.publishad.usetip") }}
              </p> -->
              <!-- <FormItem
                :label="$t('otc.publishad.premiseprice')"
                prop="premisePrice"
                v-show="!form.fixed"
                class="ivu-form-item-required"
              >
                <Input
                  v-model="form.premisePrice"
                  :placeholder="$t('otc.publishad.premisepricetip')"
                >
                  <span slot="append">%</span>
                </Input>
              </FormItem>
              <FormItem
                :label="$t('otc.publishad.fixedprice')"
                prop="fixedPrice"
                v-show="form.fixed"
                class="ivu-form-item-required"
              >
                <Input
                  v-model="form.fixedPrice"
                  :placeholder="$t('otc.publishad.fixedpricetip')"
                >
                  <span slot="append">{{ form.rmb }}</span>
                </Input>
              </FormItem> -->
              <!-- 1 -->
              <!-- <p class="msg">
                {{ $t("otc.publishad.marketprice") }}：
                <span class="cankao">{{ cankao }}</span>
              </p>
              <p class="msg" v-show="!form.fixed">
                {{ $t("otc.publishad.marketpricetip") }}{{ wantstyle }}。
              </p>
              <div class="ivu-form-item">
                <label class="ivu-form-item-label" style="width: 90px;">{{
                  $t("otc.publishad.exchangeprice")
                }}</label>
                <div class="ivu-form-item-content" style="margin-left: 90px;">
                  <div class="ivu-input-wrapper ivu-input-type" id="price">
                    {{ price }}&nbsp;CNY/{{ symbol }}
                  </div>
                </div>
              </div>
              <p class="msg">
                {{
                  $t("otc.publishad.formual")
                }}：（Bitstamp+Bitfinex+Coinbase）/ 3 *{{ gongshi.toFixed(4) }}
              </p> -->
              <!-- <FormItem
                :label="wantstyle + $t('otc.publishad.num')"
                prop="number"
              >
                <Input
                  v-model="form.number"
                  :placeholder="
                    $t('otc.publishad.num_text1') +
                      wantstyle +
                      $t('otc.publishad.num_text2')
                  "
                ></Input>
              </FormItem> -->
              <!-- <FormItem
                :label="$t('otc.publishad.exchangeperiod')"
                prop="timeLimit"
              >
                <Input
                  v-model="form.timeLimit"
                  :placeholder="
                    $t('otc.publishad.exchangeperiod_text1') +
                      '(' +
                      wantTime +
                      $t('otc.publishad.minute') +
                      ')'
                  "
                >
                  <span slot="append">{{ $t("otc.publishad.minute") }}</span>
                </Input>
              </FormItem> -->
              <!-- <p class="msg">{{ $t("otc.publishad.tip1") }}</p> -->

              <router-link
                to="/uc/account"
                style="padding-left: 90px;color:#f0ac19;"
                >{{ $t("otc.publishad.tip2") }}</router-link
              >
              <!-- TODO 收款方式 -->
              <FormItem
                :label="$t('otc.index.unpaymode')"
                style="margin-top:10px"
              >
               <CheckboxGroup v-model="form.payMode">
                  <Checkbox :label="v.value" v-for="v in payModeList" :key="v">{{
                   v.label
                  }}</Checkbox>
                </CheckboxGroup>
              </FormItem>
              <!-- TODO -->
              <!-- 付款方式 -->
              <FormItem
                :label="$t('otc.jybz')"
                style="margin-top:10px"
              >
                  <Radio :true-value="'trc20'" :false-value="'trc20'" v-model="form.pay_way">USDT</Radio>
              </FormItem>
              <FormItem
                :label="$t('otc.publishad.usdtprice')"
                prop="price"
                class="ivu-form-item-required"
              >
                <Input
                  v-model="form.price"
                  :placeholder="$t('otc.publishad.usdtprice')"
                >
                  <span slot="append">{{ form.rmb }}</span>
                </Input>
              </FormItem>
              <FormItem :label="$t('otc.publishad.minlimit')" prop="minLimit">
                <Input
                  v-model="form.minLimit"
                  :placeholder="$t('otc.publishad.tip3')"
                >
                  <span slot="append">CNY</span>
                </Input>
              </FormItem>
              <FormItem :label="$t('otc.publishad.maxlimit')" prop="maxLimit">
                <Input
                  v-model="form.maxLimit"
                  :placeholder="$t('otc.publishad.tip4')"
                >
                  <span slot="append">CNY</span>
                </Input>
              </FormItem>
              <FormItem :label="$t('otc.publishad.remark')" prop="remark">
                <Input
                  v-model="form.remark"
                  type="textarea"
                  :autosize="{ minRows: 4, maxRows: 6 }"
                  :placeholder="$t('otc.publishad.tip5')"
                ></Input>
              </FormItem>
              <FormItem :label="$t('otc.publishad.isEnable')" prop="is_able">
                <RadioGroup v-model="form.is_able">
                  <Radio label="1">{{ $t("otc.publishad.enable") }}</Radio>
                  <Radio label="0">{{ $t("otc.publishad.noEnable") }}</Radio>
                </RadioGroup>
              </FormItem>
              <!--
              <FormItem :label="$t('otc.publishad.openautoreply')">
                <i-switch v-model="form.autoReply" size="large">
                  <span slot="open">{{ $t("otc.publishad.open") }}</span>
                  <span slot="close">{{ $t("otc.publishad.close") }}</span>
                </i-switch>
              </FormItem>
              <p class="msg">{{ $t("otc.publishad.msg5") }}</p>
              <FormItem
                :label="$t('otc.publishad.autoreply')"
                prop="autoword"
                v-show="form.autoReply"
              >
                <Input
                  v-model="form.autoword"
                  type="textarea"
                  :autosize="{ minRows: 4, maxRows: 6 }"
                  :placeholder="$t('otc.publishad.autoreplytip')"
                ></Input>
              </FormItem>
              <FormItem :label="$t('otc.publishad.fundpwd')" prop="priceW">
                <Input
                  v-model="form.priceW"
                  :placeholder="$t('otc.publishad.fundpwdtip')"
                  type="password"
                ></Input>
              </FormItem> -->
              <FormItem>
                <Button
                  style="background:#f0a70a;color:#fff;border:1px solid #f0a70a;"
                  long
                  @click="handleSubmit('form')"
                  :disabled="disAllowBtn"
                  >{{ $t("otc.publishad.submit") }}</Button
                >
                <!-- <Button type="ghost" @click="handleReset('form')" style="margin-left: 8px">Reset</Button> -->
              </FormItem>
            </Form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    const numberCheck = (rule, value, callback) => {
      if (value === "" || value == 0) {
        callback(new Error(this.$t("otc.publishad.warning1")));
      } else if (Number.isNaN(Number(value))) {
        callback(new Error(this.$t("otc.publishad.warning1")));
      } else {
        callback();
      }
    };
    const premisePriceCheck = (rule, value, callback) => {
      if (this.form.fixed == false) {
        if (!value || value == 0) {
          return callback(new Error(this.$t("otc.publishad.warning2")));
        } else if (Number.isNaN(Number(value))) {
          callback(new Error(""));
        } else if (value < 0 || value > 20) {
          callback(new Error(this.$t("otc.publishad.warning1")));
        } else {
          callback();
        }
      } else {
        callback();
      }
    };
    const fixedPCheck = (rule, value, callback) => {
      if (this.form.fixed == true) {
        if (value === "") {
          callback(new Error(this.$t("otc.publishad.warning1")));
        } else if (!/^[0-9]+(.[0-9]{2})?/.test(value)) {
          callback(new Error(this.$t("otc.publishad.warning1")));
        } else {
          callback();
        }
      } else {
        callback();
      }
    };
    const maxCheck = (rule, value, callback) => {
      let priceNow;
      // TODO 这里逻辑奇奇怪怪，先屏蔽报错，之后bugfix修复
      try {
        priceNow =
          (this.price || 0 + "").replace(/,/g, "").replace(/[^\d|.]/g, "") - 0;
        priceNow = this.round(
          this.mul(priceNow || 0, this.form.number || 0),
          8
        );
      } catch (error) {
        console.log(error);
      }
      if (!value || value == 0) {
        return callback(new Error(this.$t("otc.publishad.warning3")));
      } else if (!/^[0-9]+(.[0-9]{2})?$/.test(value)) {
        callback(new Error(this.$t("otc.publishad.warning4")));
      } else if (parseFloat(value) < parseFloat(this.form.minLimit)) {
        callback(new Error(this.$t("otc.publishad.warning5")));
      } else if (parseFloat(value) > parseFloat(priceNow)) {
        // callback(
        //   new Error(this.$t("otc.publishad.warning6") + priceNow + "CNY！")
        // );
        callback();
      } else {
        callback();
      }
    };
    const minCheck = (rule, value, callback) => {
      if (!value || value == 0) {
        return callback(new Error(this.$t("otc.publishad.warning7")));
      } else if (!/^\d+$/.test(value)) {
        callback(new Error(this.$t("otc.publishad.warning4")));
      } else if (parseFloat(value) < 100) {
        callback(new Error(this.$t("otc.publishad.warning8")));
      }
      if (
        this.form.maxLimit &&
        this.form.maxLimit != 0 &&
        parseFloat(value) > this.form.maxLimit - 0
      ) {
        // console.log(this.form.maxLimit)
        callback(new Error(this.$t("otc.publishad.warning9")));
      } else {
        callback();
      }
    };
    //    期限
    const timeLimitCheck = (rule, value, callback) => {
      //数字
      if (value === "" || !/^\d+$/.test(value)) {
        callback(new Error(this.$t("otc.publishad.warning1")));
      }
      //出售
      if (this.form.advertiseType == 1) {
        if (value < 15 || value > 120) {
          callback(new Error(this.$t("otc.publishad.warning1")));
        } else {
          callback();
        }
      }
      //买入
      if (this.form.advertiseType == 0) {
        if (value < 10 || value > 30) {
          callback(new Error(this.$t("otc.publishad.warning1")));
        } else {
          callback();
        }
      } else {
        callback();
      }
    };
    return {
      isSpinShow: true,
      isId: false,
      disAllowBtn: false,
      //币种列表
      coinList: [],
      //账户余额
      balance: 100,
      //参考价
      cankao: "",
      price: "",
      symbol: "",
      gongshi: 1,
      wantstyle: this.$t("otc.publishad.sellout"),
      wantTime: "15-120",
      areas: [],
      form: {
        advertiseType: "1",
        coin: "1",
        country: "china",
        rmb: "",
        fixed: false,
        premisePrice: "",
        fixedPrice: "",
        number: "",
        timeLimit: "",
        payMode: [],
        minLimit: "",
        maxLimit: "",
        autoReply: false,
        remark: "",
        priceW: "",
        autoword: "",
        is_able: "1",
        price: "",
        pay_way:'trc20'
      },
      ruleValidate: {
        advertiseType: [{ required: true, trigger: "change" }],
        coin: [
          {
            required: true,
            message: this.$t("otc.publishad.inputtip1"),
            trigger: "change",
          },
        ],
        country: [
          {
            required: true,
            message: this.$t("otc.publishad.inputtip2"),
            trigger: "change",
          },
        ],
        rmb: [
          {
            required: true,
            message: this.$t("otc.publishad.inputtip2"),
            trigger: "change",
          },
        ],
        premisePrice: [
          {
            validator: premisePriceCheck,
            message: this.$t("otc.publishad.inputtip3"),
            trigger: "blur",
          },
        ],
        fixedPrice: [
          {
            validator: fixedPCheck,
            message: this.$t("otc.publishad.inputtip4"),
            trigger: "blur",
          },
        ],
        number: [
          {
            required: true,
            validator: numberCheck,
            message: this.$t("otc.publishad.inputtip5"),
            trigger: "blur",
          },
        ],
        timeLimit: [
          {
            required: true,
            validator: timeLimitCheck,
            message: this.$t("otc.publishad.inputtip6"),
            trigger: "blur",
          },
        ],
        payMode: [
          {
            required: true,
            type: "array",
            min: 1,
            message: this.$t("otc.publishad.inputtip7"),
            trigger: "change",
          },
        ],
        minLimit: [{ required: true, validator: minCheck, trigger: "blur" }],
        maxLimit: [{ required: true, validator: maxCheck, trigger: "blur" }],
        priceW: [
          {
            required: true,
            message: this.$t("otc.publishad.inputtip8"),
            trigger: "blur",
          },
        ],
      },
      payModeList: [
        {
          value: "zfb",
          label: this.$t("otc.publishad.zfb"),
          isOpen: true,
        },
        {
          value: "wx",
          label: this.$t("otc.publishad.wx"),
          isOpen: true,
        },
        {
          value: "bank",
          label: this.$t("otc.publishad.unionpay"),
          isOpen: true,
        },
        {
          value: "other",
          label: this.$t("otc.publishad.other"),
          isOpen: true,
        },
      ],
    };
  },
  methods: {
    changeCoin() {
      let coinItem = this.getCoin(this.form.coin);
      if (coinItem != null) {
        this.cankao = coinItem.marketPrice || 100;
        let lv = (1 + this.form.premisePrice / 100).toFixed(4);
        let cankoNew =
          this.cankao.replace(/,/g, "").replace(/[^\d|.]/g, "") - 0;
        this.price = (cankoNew * lv).toLocaleString(); // + ' CNY/' + coinItem.unit;
        this.symbol = coinItem.unit;
      }
    },
    mul(a, b) {
      var c = 0,
        d = a.toString(),
        e = b.toString();
      try {
        c += d.split(".")[1].length;
      } catch (f) {}
      try {
        c += e.split(".")[1].length;
      } catch (f) {}
      return (
        (Number(d.replace(".", "")) * Number(e.replace(".", ""))) /
        Math.pow(10, c)
      );
    },
    div(a, b) {
      var c,
        d,
        e = 0,
        f = 0;
      try {
        e = a.toString().split(".")[1].length;
      } catch (g) {}
      try {
        f = b.toString().split(".")[1].length;
      } catch (g) {}
      return (
        (c = Number(a.toString().replace(".", ""))),
        (d = Number(b.toString().replace(".", ""))),
        this.mul(c / d, Math.pow(10, f - e))
      );
    },
    round(v, e) {
      var t = 1;
      for (; e > 0; t *= 10, e--);
      for (; e < 0; t /= 10, e++);
      return Math.round(v * t) / t;
    },
    handleSubmit(name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          this.disAllowBtn = true;
          let form = this.form;
          //创建 or 修改
          var params = {
            token: localStorage.getItem("TOKEN"),
            title: form.title,
            ad_type: form.advertiseType,
            high_limit: form.maxLimit,
            low_limit: form.minLimit,
            text: form.remark,
            pay_way:'trc20',
            pay_way_zfb: form.payMode.includes("zfb") ? 1 : 0,
            pay_way_wx: form.payMode.includes("wx") ? 1 : 0,
            pay_way_bank: form.payMode.includes("bank") ? 1 : 0,
            pay_way_other: form.payMode.includes("other") ? 1 : 0,
            is_able: form.is_able,
            note: form.remark,
            price: form.price,
          };
          if (this.isId) {
            params.id = this.$route.query.trade_ad_id;
          }
          this.$http
            .post(
              this.apiHost + (this.isId ? "/user/edit_advs" : "/user/add_advs"),
              params
            )
            .then((response) => {
              var resp = response.body;
              if (resp.code == 1) {
                this.$Message.success(resp.msg);
                var that = this;
                setTimeout(() => {
                  that.$router.push("/uc/ad");
                }, 3000);
              } else {
                this.$Message.error(resp.msg);
                this.disAllowBtn = false;
              }
              //  this.disAllowBtn=false
            });
        } else {
          this.disAllowBtn = false;
          this.$Message.error(this.$t("otc.publishad.submit_failure"));
        }
      });
    },
    handleReset(name) {
      this.$refs[name].resetFields();
    },
    onAreaChange(value) {
      for (var i = 0; i < this.areas.length; i++) {
        if (this.areas[i].zhName == value) {
          this.form.rmb = this.areas[i].localCurrency;
        }
      }
    },
    findCoin(coinId) {
      for (let i = 0; i < this.coinList.length; i++) {
        if (this.coinList[i].id == coinId) {
          return this.coinList[i].unit;
        }
      }
    },
    getCoin(coinId) {
      for (let i = 0; i < this.coinList.length; i++) {
        if (this.coinList[i].id == coinId) {
          return this.coinList[i];
        }
      }
    },
    getAreas() {
      this.$http.post(this.host + "/uc/support/country").then((response) => {
        var resp = response.body;
        this.areas = resp.data;
        this.form.country = this.areas[0].zhName;
        this.form.rmb = this.areas[0].localCurrency;
      });
    },
    getMember() {
      //获取个人安全信息
      // let self = this;
      // this.$http.get(this.host + this.api.uc.identification).then((res) => {
      //   let certifiedBusinessStatus = res.body.data.certifiedBusinessStatus;
      //   if (certifiedBusinessStatus == 2) {
      //     this.getAccount();
      //   } else {
      //     this.$Message.warning("请先申请商家认证!");
      //     this.$router.push("/identbusiness");
      //   }
      // });
      //
      //
      // this.$http.post(this.host + '/uc/approve/security/setting').then(response => {
      //     var resp = response.body;
      //     if (resp.code == 0) {
      //         if (resp.data.realName == null || resp.data.realName == "") {
      //             this.$Message.success(this.$t('otc.publishad.submittip1'));
      //             self.$router.push('/uc/safe');
      //         } else if (resp.data.phoneVerified == 0) {
      //             this.$Message.success(this.$t('otc.publishad.submittip2'));
      //             self.$router.push('/uc/safe');
      //         } else if (resp.data.fundsVerified == 0) {
      //             this.$Message.success(this.$t('otc.publishad.submittip3'));
      //             self.$router.push('/uc/safe');
      //         } else {
      //             this.getAccount()
      //         }
      //     } else {
      //         // this.$Message.error(resp.message);
      //     }
      // })
    },
    getAccount() {
      //获取个人账户信息
      let self = this;
      this.$http
        .post(this.host + "/uc/approve/account/setting")
        .then((response) => {
          var resp = response.body;
          if (resp.code == 0) {
            if (
              resp.data.bankVerified == 0 &&
              resp.data.aliVerified == 0 &&
              resp.data.wechatVerified == 0
            ) {
              this.$Message.success(this.$t("otc.publishad.submittip4"));
              self.$router.push("/uc/account");
            }
            if (resp.data.aliVerified == 1) {
              this.payModeList[0].isOpen = false;
            }
            if (resp.data.wechatVerified == 1) {
              this.payModeList[1].isOpen = false;
            }
            if (resp.data.bankVerified == 1) {
              this.payModeList[2].isOpen = false;
            }
          } else {
            this.msg = resp.message;
            // this.$Message.error(resp.message);
          }
        });
    },
    // TODO
    getDetailAd() {
      this.isId = true;
      this.isSpinShow = false;
      let row = this.$route.query;
      let payMode = [];
      if (+row.pay_way_zfb) {
        payMode.push("zfb");
      }
      if (+row.pay_way_wx) {
        payMode.push("wx");
      }
      if (+row.pay_way_bank) {
        payMode.push("bank");
      }
      if (+row.pay_way_other) {
        payMode.push("other");
      }
      this.form = Object.assign(
        {
          advertiseType: "1",
          payMode,
          minLimit: +row.low_limit,
          maxLimit: +row.high_limit,
          remark: row.note,
        },
        row
      );
    },
  },
  mounted() {},
  computed: {
    wantHistory() {
      return this.form.advertiseType;
    },
    premisePriceHistory() {
      return this.form.premisePrice;
    },
    fixedPriceHistory() {
      return this.form.fixedPrice;
    },
  },
  watch: {
    wantHistory(newValue, oldValue) {
      if (newValue == "1") {
        this.wantstyle = this.$t("otc.publishad.sellout");
        this.wantTime = "15-120";
      } else {
        this.wantstyle = this.$t("otc.publishad.buyin");
        this.wantTime = "10-30";
      }
    },
    premisePriceHistory(newValue, oldValue) {
      // let lv = (1 + newValue / 100).toFixed(4);
      // let cankoNew = this.cankao.replace(/,/g, "").replace(/[^\d|.]/g, "") - 0;
      // // this.price = (cankoNew * lv).toLocaleString() + ' CNY/' + this.findCoin(this.form.coin);
      // // this.price = this.round(this.mul(cankoNew, lv), 2).toLocaleString() + ' CNY/' + this.findCoin(this.form.coin);
      // this.price = this.round(this.mul(cankoNew, lv), 2).toLocaleString();
      // this.gongshi = 1 + newValue / 100;
    },
    fixedPriceHistory(newValue, oldValue) {
      this.price = (newValue - 0).toLocaleString();
    },
  },
  created() {
    this.getMember();
    this.getAreas();
    // let lv = (1 + this.form.premisePrice / 100).toFixed(4);
    //获取币种
    this.coinList = [
      {
        id: "USDT",
        unit: "USDT",
      },
    ];
    this.form.coin = "USDT";
    this.cankao = 100;
    this.price = 100;
    this.symbol = "USDT";
    if (this.$route.query.trade_ad_id) {
      this.getDetailAd();
    }
    this.isSpinShow = false;
    // this.$http
    //   .post(this.host + "/otc/coin/all")
    //   .then((response) => {
    //     var resp = response.body;
    //     if (resp.code == 1) {
    //       this.coinList = resp.data;
    //       this.form.coin = resp.data[0].id;
    //       this.cankao = resp.data[0].marketPrice + "";
    //       let cankoNew =
    //         this.cankao.replace(/,/g, "").replace(/[^\d|.]/g, "") - 0;
    //       // this.price = (cankoNew * lv).toLocaleString() + ' CNY/' + this.findCoin(this.form.coin);
    //       this.price = (cankoNew * lv).toLocaleString();
    //       this.symbol = resp.data[0].unit;
    //     } else {
    //       this.$Message.error(resp.msg);
    //     }
    //     //修改
    //     if (this.$route.query.id) {
    //       this.getDetailAd();
    //     } else {
    //       this.isSpinShow = false;
    //     }
    //   })
    //   .finally(() => {
    //     this.isSpinShow = false;
    //   });
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
