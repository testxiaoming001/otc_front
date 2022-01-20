<template>
  <div class="nav-rights">
    <div class="nav-right">
      <div class="bill_box_order">
        <div class="order_box">
          <Tabs value="name1" @on-click="showItem">
            <TabPane :label="$t('uc.otcorder.unpaid')" name="name1">
              <div class="order-table">
                <Table
                  :no-data-text="$t('common.nodata')"
                  :columns="tableColumnsOrder"
                  :data="tableOrder"
                  :loading="loading"
                  :disabled-hover="true"
                ></Table>
                <div style="margin: 10px;overflow: hidden" class="page">
                  <div style="float: right;">
                    <Page
                      v-if="totalPage > 0"
                      :total="totalNum"
                      :current="currentPage"
                      show-total
                      @on-change="changePage"
                      :page-size="pageSize"
                    ></Page>
                  </div>
                </div>
              </div>
            </TabPane>
            <TabPane :label="$t('uc.otcorder.paided')" name="name2">
              <div class="order-table">
                <Table
                  :no-data-text="$t('common.nodata')"
                  :columns="tableColumnsOrder"
                  :data="tableOrder"
                  :loading="loading"
                  :disabled-hover="true"
                ></Table>
                <div style="margin: 10px;overflow: hidden" class="page">
                  <div style="float: right;">
                    <Page
                      v-if="totalPage > 0"
                      :total="totalNum"
                      :current="currentPage"
                      show-total
                      @on-change="changePage"
                      :page-size="pageSize"
                    ></Page>
                  </div>
                </div>
              </div>
            </TabPane>
            <TabPane :label="$t('uc.otcorder.finished')" name="name3">
              <div class="order-table">
                <Table
                  :no-data-text="$t('common.nodata')"
                  :columns="tableColumnsOrder"
                  :data="tableOrder"
                  :loading="loading"
                  :disabled-hover="true"
                ></Table>
                <div style="margin: 10px;overflow: hidden" class="page">
                  <div style="float: right;">
                    <Page
                      v-if="totalPage > 0"
                      :total="totalNum"
                      :current="currentPage"
                      show-total
                      @on-change="changePage"
                      :page-size="pageSize"
                    ></Page>
                  </div>
                </div>
              </div>
            </TabPane>
            <TabPane :label="$t('uc.otcorder.canceled')" name="name0">
              <div class="order-table">
                <Table
                  :no-data-text="$t('common.nodata')"
                  :columns="tableColumnsOrder"
                  :data="tableOrder"
                  :loading="loading"
                  :disabled-hover="true"
                ></Table>
                <div style="margin: 10px;overflow: hidden" class="page">
                  <div style="float: right;">
                    <Page
                      v-if="totalPage > 0"
                      :total="totalNum"
                      :pageSize="pageSize"
                      show-total
                      :current="currentPage"
                      @on-change="changePage"
                    ></Page>
                  </div>
                </div>
              </div>
            </TabPane>
            <TabPane :label="$t('uc.otcorder.appealing')" name="name4">
              <div class="order-table">
                <Table
                  :no-data-text="$t('common.nodata')"
                  :columns="tableColumnsOrder"
                  :data="tableOrder"
                  :loading="loading"
                  :disabled-hover="true"
                ></Table>
                <div style="margin: 10px;overflow: hidden" class="page">
                  <div style="float: right;">
                    <Page
                      v-if="totalPage > 0"
                      :total="totalNum"
                      :current="currentPage"
                      show-total
                      @on-change="changePage"
                      :page-size="pageSize"
                    ></Page>
                  </div>
                </div>
              </div>
            </TabPane>
            <Input
              v-model="ordKeyword"
              slot="extra"
              @on-click="handleSearch"
              icon="ios-search"
              :placeholder="$t('uc.otcorder.searchtip')"
              style="width: 250px;margin-right: 20px;"
            ></Input>
          </Tabs>
        </div>
      </div>
    </div>
       <Modal v-model="modal" title="选择付款地址" @on-ok="confirm">
      <p>
        <Select v-model="bank_id" style="width:200px">
        <Option v-for="item in bankList" :value="item.value" :key="item.value">{{ item.label }}</Option>
    </Select>
      </p>
    </Modal>
  </div>
</template>
<script>
export default {
  components: {},
  data() {
    return {
      bank_id: "",
      bankList: [],
      modal: false,
      ordKeyword: "",
      choseBtn: 0,
      whichItem: 1,
      tableOrder: [],
      loading: true,
      totalPage: 0,
      pageSize: 10,
      totalNum: 0,
      currentPage: 1,
      order_id: null,
      loginmsg: this.$t("common.logintip"),
    };
  },
  methods: {
    confirm() {
      this.$http
        .post(this.apiHost + "/Usdt_order/set_order_bank", {
          token: localStorage.getItem("TOKEN"),
          bank_id: this.bank_id,
          order_id: this.order_id,
        })
        .then((resp) => {
          console.log(resp);
        });
    },
    setPay() {
      this.modal = true;
      this.$http
        .post(this.apiHost + "/user/card_info_list", {
          token: localStorage.getItem("TOKEN"),
        })
        .then((resp) => {
          var resp = response.body;
          if (resp.code == 1) {
            this.bankList = resp.data.data;
          } else {
            this.$Message.error(resp.msg);
          }
          console.log(resp);
        });
    },
    changePage(pageNo) {
      if (pageNo > 0) pageNo = pageNo - 1;
      if (this.whichItem == 1) {
        this.getOrder(1, pageNo);
      } else if (this.whichItem == 2) {
        this.getOrder(2, pageNo);
      } else if (this.whichItem == 3) {
        this.getOrder(3, pageNo);
      } else if (this.whichItem == 0) {
        this.getOrder(0, pageNo);
      } else if (this.whichItem == 4) {
        this.getOrder(4, pageNo);
      }
    },
    getOrder(status, pageNo) {
      this.loading = true;
      this.tableOrder = [];
      let params = {};
      params["status"] = status;
      params["pageNo"] = pageNo;
      params["pageSize"] = this.pageSize;
      params.token = localStorage.getItem("TOKEN");
      this.currentPage = pageNo + 1;
      this.$http
        .post(this.apiHost + "/Usdt_order/seller_order_list", params)
        .then((response) => {
          var resp = response.body;
          if (resp.code == 1) {
            console.log(resp);
            this.tableOrder = resp.data.data;
            this.totalPage = resp.data.total;
            this.totalNum = resp.data.total;
          } else {
            this.$Message.error(resp.msg);
          }
          this.loading = false;
        })
        .catch(() => {
          this.loading = false;
        });
    },
    init() {},
    handleSearch() {
      this.tableOrder = [];
      let params = {};
      params["status"] = this.whichItem;
      params["pageNo"] = 0;
      params["pageSize"] = this.pageSize;
      if (this.ordKeyword != "") {
        params["orderSn"] = this.ordKeyword;
      }
      this.currentPage = 1;
      this.$http
        .post(this.host + "/otc/order/self", params)
        .then((response) => {
          var resp = response.body;
          if (resp.code == 0 && resp.data.content) {
            this.tableOrder = resp.data.content;
            this.totalPage = resp.data.totalPages;
            this.totalNum = resp.data.totalElements;
          } else {
            // this.$Message.error(resp.message);
          }
          this.loading = false;
        });
    },
    showItem(name) {
      if (name == "name1") {
        this.whichItem = 1;
      } else if (name == "name2") {
        this.whichItem = 2;
      } else if (name == "name3") {
        this.whichItem = 3;
      } else if (name == "name0") {
        this.whichItem = 0;
      } else if (name == "name4") {
        this.whichItem = 4;
      }
      this.changePage(0);
    },
    strpro(str) {
      let newStr = str;
      str = str.slice(1);
      var re = /[\D\d]*/g;
      var str2 = str.replace(re, function(str) {
        var result = "";
        for (var i = 0; i < str.length; i++) {
          result += "*";
        }
        return result;
      });
      return newStr.slice(0, 1) + str2;
    },
  },
  created() {
    this.changePage(0);
  },
  mounted() {
    // this.init();
  },
  computed: {
    tableColumnsOrder() {
      let self = this;
      let columns = [];
      columns.push({
        title: this.$t("uc.otcorder.orderno"),
        key: "order_id",
        minWidth: 60,
        align: "center",
      });
      columns.push({
        title: this.$t("uc.otcorder.symbol"),
        key: "pay_address_type",
        align: "center",
      });
      columns.push({
        title: this.$t("uc.otcorder.type"),
        key: "type",
        align: "center",
        render: (h, params) => {
          let text = "";
          if (params.row.type == 0) {
            text = self.$t("uc.otcorder.type_buy");
          } else {
            text = self.$t("uc.otcorder.type_sell");
          }
          return h("div", [h("p", text)]);
        },
      });
      columns.push({
        title: this.$t("uc.otcorder.tradename"),
        key: "seller_address",
        ellipsis: "true",
        align: "center",
        render: function(h, params) {
          return h("p", [
            h(
              "a",
              {
                on: {
                  click: function() {
                    self.$router.push("/checkuser?id=" + params.row.seller_id);
                  },
                },
              },
              params.row.seller_address
            ),
          ]);
        },
      });
      columns.push({
        title: this.$t("uc.otcorder.amount"),
        key: "btc_number",
        align: "center",
      });
      columns.push({
        title: this.$t("uc.otcorder.money"),
        key: "order_price",
        align: "center",
      });
      columns.push({
        title: "订单状态",
        key: "order_status",
        align: "center",
        render: function(h, params) {
          return h("p", [
            h(
              "span",
              params.row.seller_address === 1 ? "未付款" : "已付款到平台"
            ),
          ]);
        },
      });
      columns.push({
        title: "操作", // 国际化
        ellipsis: "true",
        align: "center",
        width: 230,
        render: function(h, params) {
          return h("p", [
            h(
              "a",
              {
                on: {
                  click: function() {
                    self.$router.push(
                      "/uc/adOrderDetail?id=" + params.row.order_id
                    );
                  },
                },
              },
              "详情"
            ),
            h(
              "a",
              {
                on: {
                  click: function() {
                    self.$router.push(
                      "/uc/adOrderDetail?id=" + params.row.order_id
                    );
                  },
                },
              },
              "放行"
            ),
            h(
              "a",
              {
                on: {
                  click: function() {
                    self.setPay();
                  },
                },
              },
              "设置收款方式"
            ),
          ]);
        },
      });
      return columns;
    },
  },
};
</script>
<style scoped>
/deep/ .ivu-table-column-center p > a {
  margin-right: 10px;
}
.bill_box_order {
  width: 99%;
  padding-left: 20px;
  height: auto;
  margin: 0 auto;
  /* overflow: hidden; */
}

.order_box {
  text-align: left;
}

.order_box a {
  color: #0b0d1b;
  font-size: 16px;
  padding: 0 30px;
  cursor: pointer;
  text-decoration: none;
  text-align: center;
  line-height: 54px;
  display: inline-block;
}

.order_box .active {
  border-bottom: 2px solid #00b5f6;
}

.order_box .search {
  position: absolute;
  width: 300px;
  height: 32px;
  top: 12px;
  right: 0;
  display: flex;
}
</style>

<style lang="scss">
.bill_box_order {
  .order_box {
    .ivu-tabs {
      // overflow:initial;
      color: #fff;
      .ivu-tabs-content.ivu-tabs-content-animated {
        .ivu-tabs-tabpane {
          .ivu-table-wrapper {
            border: none;
            box-shadow: none;
            a {
              color: #f0a70a;
            }
          }
        }
      }
      .ivu-tabs-bar {
        border-color: #273742 !important;
        .ivu-tabs-nav-container {
          .ivu-tabs-nav-scroll {
            .ivu-tabs-ink-bar.ivu-tabs-ink-bar-animated {
              background-color: #f0a70a;
            }
            .ivu-tabs-tab.ivu-tabs-tab-active.ivu-tabs-tab-focused {
              color: #f0a70a;
              &:hover {
                color: #f0a70a;
              }
            }
            .ivu-tabs-tab {
              &:hover {
                color: #f0a70a;
              }
            }
          }
        }
      }
      .ivu-tabs-content {
        .ivu-tabs-tabpane {
          .ivu-table-header,
          .ivu-table-body {
            table {
              width: 100% !important;
              thead {
                .ivu-table-cell {
                  padding: 0;
                }
              }
            }
          }
        }
      }
    }
  }
}
</style>
