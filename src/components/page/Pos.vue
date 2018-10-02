<template>
  <div class="pos">
    <el-row>
      <el-col :span="8" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border show-summary style="width: 100%">
              <el-table-column prop="goodsName" label="商品"></el-table-column>
              <el-table-column prop="count" label="数量" width="70"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column label="操作" width="110" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addGoods(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="div-total">
              <small>数量:</small>{{totalQty}}
              &nbsp;&nbsp;&nbsp;&nbsp;
              <small>合计:</small>￥{{totalPrice}}元</div>
            <div class="div-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods">删除</el-button>
              <el-button type="success" @click="checkout">结帐</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">hangup</el-tab-pane>
          <el-tab-pane label="外卖">out-service</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="16">
        <div class="often-goods">
          <div class="often-goods-tilte">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" :key="goods.goodsId" @click="addGoods(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="often-goods-price">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="主食">
              <ul class="cookList">
                <li v-for="goods in type0Goods"  :key="goods.goodsId" @click="addGoods(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="小吃">
              <ul class="xiaoshiList">
                <li v-for="goods in type1Goods"  :key="goods.goodsId" @click="addGoods(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              yinliao
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Pos',
  data () {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      totalQty: 0,
      totalPrice: 0
    };
  },
  created: function () {
    axios
      .get(
        'https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods'
      )
      .then(response => {
        // console.log(response)
        this.oftenGoods = response.data;
      })
      .catch(error => {
        console.log(error);
      });
    axios
      .get(
        'https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods'
      )
      .then(response => {
        // console.log(response)
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
      })
      .catch(error => {
        console.log(error);
      });
  },
  mounted: function () {
    var orderHeight = document.body.clientHeight;
    document.getElementById('order-list').style.height = orderHeight + 'px';
  },
  methods: {
    addGoods (goods) {
      let addId = -1;
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId === goods.goodsId) {
          addId = i;
        }
      }
      //
      if (addId !== -1) {
        this.tableData[addId].count++;
      } else {
        let newGoods = goods;
        newGoods.count = 1;
        this.tableData.push(newGoods);
      }
      this.getTotal();
    },
    delGoods (goods) {
      this.tableData = this.tableData.filter(o => o.goodsId !== goods.goodsId);
      this.getTotal();
    },
    delAllGoods () {
      this.tableData = [];
      this.totalQty = 0;
      this.totalPrice = 0;
    },
    checkout () {
      if (this.totalQty !== 0) {
        this.totalQty = 0;
        this.totalPrice = 0;
        this.tableData = [];
        this.$message('结帐成功！')
      } else {
        this.$message.error('订单为空')
      }
    },
    getTotal () {
      this.totalQty = 0;
      this.totalPrice = 0;
      this.tableData.forEach(e => {
        this.totalQty += e.count;
        this.totalPrice += e.count * e.price;
      });
    }
  }
};
</script>
<style scoped>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.div-total {
  background-color: #fff;
  padding: 10px;
  border-bottom: 1px #d3dce6;
}
.div-btn {
  margin-top: 10px;
}
.often-goods-tilte {
  height: 20px;
  border-bottom: 1px #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: left;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  cursor: pointer;
}
.often-goods-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}
.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auto;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  cursor: pointer;
}
.cookList li span {
  display: block;
  float: left;
}
.xiaoshiList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auto;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  cursor: pointer;
}
.xiaoshiList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
</style>
