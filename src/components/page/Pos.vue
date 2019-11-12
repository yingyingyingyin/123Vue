<template>
    <div class="pos">
        <div>
            <el-row>
                <!---第一栏-->
                <el-col :span="7" class="order-list" id="order-list">
                    <el-tabs>
                        <el-tab-pane label="点餐" name="first">
                            <el-table :data="tableData" border show-summary  style="width: 100%">
                                <!--tableData-->
                                <el-table-column prop="goodsName" label="商品"></el-table-column>
                                <el-table-column prop="count" label="量" width="50"></el-table-column>
                                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                                <el-table-column label="操作" width="100" fixed="right">
                                    <template scope="scope">
                            <el-button
                              type="text"
                              size="small"
                              @click="delSingleGoods(scope.row)"
                              >删除</el-button
                            >

                            <el-button
                              type="text"
                              size="small"
                              @click="addOrderList(scope.row)"
                              >增加</el-button
                            >
</template>
                </el-table-column>
              </el-table>
             <!-- <div class="totalDiv">
                <small>数量：{{totalcount }}</small
                >&nbsp;&nbsp;&nbsp;&nbsp;<small
                  >总金额:{{totalmoney}}</small
                >
              </div>-->

              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="alldel">删除</el-button>
              <el-button type="success">结账</el-button>
            </el-tab-pane>

            <el-tab-pane label="挂单" name="second"></el-tab-pane>
            <el-tab-pane label="外卖" name="third">外卖</el-tab-pane>
          </el-tabs>
        </el-col>
        <!--常用商品栏-->
        <el-col :span="17" class="order-tabs">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
              <ul>
                <li
                  v-for="goods in type0Goods"
                  :key="goods"
                  @click="addOrderList(goods)"
                >
                  <!--goods-->
                  <span>{{ goods.goodsName }}</span>
                  <span class="o-price">{{ goods.price }}</span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class="cookList">
                  <li
                    v-for="goods in type0Goods"
                    :key="goods"
                    @click="addOrderList(goods)"
                  >
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{ goods.goodsName }}</span>
                    <span class="foodPrice">￥{{ goods.price }}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">小食</el-tab-pane>
              <el-tab-pane label="饮料">饮料</el-tab-pane>
              <el-tab-pane label="套餐">套餐</el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
    name: "Pos",
    activeName: "second",
    data() {
        return {
            totalcount: 0,
            totalmoney: 0,
            tableData: [],

            type0Goods: [{
                    goodsId: 1,
                    goodsImg: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2562751340,157779294&fm=26&gp=0.jpg",
                    goodsName: "香辣鸡腿堡",
                    price: 18
                },
                {
                    goodsId: 2,
                    goodsImg: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2562751340,157779294&fm=26&gp=0.jpg",
                    goodsName: "田园鸡腿堡",
                    price: 15
                },
                {
                    goodsId: 3,
                    goodsImg: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2562751340,157779294&fm=26&gp=0.jpg",
                    goodsName: "和风汉堡",
                    price: 15
                },
                {
                    goodsId: 4,
                    goodsImg: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2562751340,157779294&fm=26&gp=0.jpg",
                    goodsName: "快乐全家桶",
                    price: 80
                },
                {
                    goodsId: 5,
                    goodsImg: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2562751340,157779294&fm=26&gp=0.jpg",
                    goodsName: "脆皮炸鸡腿",
                    price: 10
                },
                {
                    goodsId: 6,
                    goodsImg: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2562751340,157779294&fm=26&gp=0.jpg",
                    goodsName: "魔法鸡块",
                    price: 20
                },
                {
                    goodsId: 7,
                    goodsImg: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2562751340,157779294&fm=26&gp=0.jpg",
                    goodsName: "可乐大杯",
                    price: 10
                },
                {
                    goodsId: 8,
                    goodsImg: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2562751340,157779294&fm=26&gp=0.jpg",
                    goodsName: "雪顶咖啡",
                    price: 18
                },
                {
                    goodsId: 9,
                    goodsImg: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2562751340,157779294&fm=26&gp=0.jpg",
                    goodsName: "大块鸡米花",
                    price: 15
                },
                {
                    goodsId: 20,
                    goodsImg: "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2562751340,157779294&fm=26&gp=0.jpg",
                    goodsName: "香脆鸡柳",
                    price: 17
                }
            ]
        };
    },
    methods: {
        //添加列表订单的方法
        addOrderList(goods) {
            let totalcount = 0;
            let totalmoney = 0;
            let ishave = false;
            for (let i = 0; i < this.tableData.length; i++) {
                console.log(this.tableData[i].goodsId);
                if (this.tableData[i].goodsId == goods.goodsId) {
                    ishave = true;
                }
            }
            if (ishave) {
                let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
                arr[0].count++;
                arr[0].price =goods.price * arr[0].count;
            } else {
                let newGoods = {
                    goodsId: goods.goodsId,
                    price: goods.price,
                    goodsName: goods.goodsName,
                    count: 1
                };
                this.tableData.push(newGoods);
            }
            this.tableData.forEach(element => {
                this.totalcount += element.count;
                this.totalmoney = this.totalmoney + element.price * element.count;
            });
        },
        delSingleGoods(goods) {
            this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
        },
        alldel(){
          this.tableData=[];
         this. count=0;

        }
    },
    mounted: function() {
        let Oheight = document.body.clientHeight;
        document.getElementById("order-list").style.height = Oheight + "px";
    }
    /* created() {
                    axios.get('http://old.jspag.com/DemoApi/oftenGoods.php')
                        .then(response => {
                            console.log(response);
                            this.oftenGoods = response.data;
                        })
                        .catch(error => {
                            console.log(error);
                            alert('网络错误，不能访问');
                        })
                    axios.get('http://jspang.com/DemoApi/typeGoods.php')
                        .then(response => {
                            console.log(response);
                            //this.oftenGoods=response.data;
                            this.type0Goods = response.data[0];
                            this.type1Goods = response.data[1];
                            this.type2Goods = response.data[2];
                            this.type3Goods = response.data[3];

                        })
                        .catch(error => {
                            console.log(error);
                            alert('网络错误，不能访问');
                        })
                }*/
};
</script>

<style scoped>
.order-list {
    background-color: rgb(223, 245, 245);
    border-right: 1px solid #c0ccda;
}

.title {
    background-color: #f9fafc;
    border-bottom: 1px solid #d3dce6;
    height: 20px;
    padding: 10px;
}

.often-goods-list ul li {
    list-style: none;
    float: left;
    border: 1px solid #e5e9f2;
    padding: 10px;
    margin: 5px;
    background-color: #fff;
    cursor: pointer;
}

.order-tabs {
    display: flex;
    flex-direction: column;
}

.goods-type {
    margin-top: 40px;
}

.cookList li {
    list-style: none;
    width: 23%;
    border: 1px solid #e5e9f2;
    height: auot;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 2px;
    cursor: pointer;
}

.cookList li span {
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
