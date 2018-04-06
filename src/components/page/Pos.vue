<template>
    <div class="pos">
        <div>
            <el-row>
                <el-col :span="7" class="pos-order" id="order-list">
                    <el-tabs>
                        <el-tab-pane label="点餐">
                            <el-table :data="tableData" border style="width=100%">
                                <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                                <el-table-column prop="count" label="数量" width="70"></el-table-column>
                                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                                <el-table-column label="操作" width="100" fixed="right">
                                    <template slot-scope="scope">
                                        <el-button type="text" size="small" v-on:click="delSingleGoods(scope.row)">删除</el-button>
                                        <el-button type="text" size="small" v-on:click="addOrderList(scope.row)">增加</el-button>                                        
                                    </template>
                                </el-table-column>
                            </el-table>
                            <div class="total">
                                <span><small>数量：</small>{{totalCount}}件</span>
                                <span><small>金额：</small>{{totalMoney}}元</span>
                            </div>
                            <div class="btn">
                                <el-button type="warning">挂单</el-button>
                                <el-button type="danger" @click="delAllGoods()">删除</el-button>
                                <el-button type="success" @click="checkout()">结账</el-button>
                            </div>
                        </el-tab-pane>
                        <el-tab-pane label="挂单">挂单</el-tab-pane>
                        <el-tab-pane label="外卖">外卖</el-tab-pane>
                    </el-tabs>
                </el-col>
                <!-- 商品展示 -->
                <el-col :span="17">
                    <div class="often-goods">
                        <div class="title">常用商品</div>
                        <div class="often-goods-list">
                            <ul>
                                <li v-for="goods in oftenGoods" v-on:click="addOrderList(goods)">
                                    <span>{{ goods.goodsName }}</span>
                                    <span class="o-price">￥{{ goods.price }}元</span>
                                </li>
                            </ul>
                        </div>
                    </div>
                    <div class="goods-type">
                        <el-tabs>
                            <el-tab-pane label="汉堡">
                                <ul class="cookList">
                                    <li v-for="goods in type0Goods" v-on:click="addOrderList(goods)">
                                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                        <span class="foodName">{{ goods.goodsName }}</span>
                                        <span class="foodPrice">￥{{ goods.price }}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="小食">
                                <ul class="cookList">
                                    <li v-for="goods in type1Goods" v-on:click="addOrderList(goods)">
                                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                        <span class="foodName">{{ goods.goodsName }}</span>
                                        <span class="foodPrice">￥{{ goods.price }}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="饮料">
                                <ul class="cookList">
                                    <li v-for="goods in type2Goods" v-on:click="addOrderList(goods)">
                                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                        <span class="foodName">{{ goods.goodsName }}</span>
                                        <span class="foodPrice">￥{{ goods.price }}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="套餐">
                                <ul class="cookList">
                                    <li v-for="goods in type3Goods" v-on:click="addOrderList(goods)">
                                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                        <span class="foodName">{{ goods.goodsName }}</span>
                                        <span class="foodPrice">￥{{ goods.price }}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                        </el-tabs>
                    </div>
                </el-col>
            </el-row>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Pos',
    mounted: function () {
        var orderHeight = document.body.clientHeight;
        document.getElementById('order-list').style.height = orderHeight + 'px';
        document.getElementsByClassName('el-tabs__nav-scroll')[1].style.backgroundColor = '#F9FAFC';
    },
    data () {
        return {
            tableData: [],
            oftenGoods: [
            //     {
            //         goodsId: 1,
            //         goodsName: '香辣鸡腿堡',
            //         price: 18
            //     }, {
            //         goodsId: 2,
            //         goodsName: '田园鸡腿堡',
            //         price: 15
            //     }, {
            //         goodsId: 3,
            //         goodsName: '和风汉堡',
            //         price: 15
            //     }, {
            //         goodsId: 4,
            //         goodsName: '欢乐全家桶',
            //         price: 80
            //     }, {
            //         goodsId: 5,
            //         goodsName: '脆皮炸鸡腿',
            //         price: 10
            //     }, {
            //         goodsId: 6,
            //         goodsName: '魔法鸡块',
            //         price: 20
            //     }, {
            //         goodsId: 7,
            //         goodsName: '可乐大杯',
            //         price: 10
            //     }, {
            //         goodsId: 8,
            //         goodsName: '雪顶咖啡',
            //         price: 18
            //     }, {
            //         goodsId: 9,
            //         goodsName: '大块鸡米花',
            //         price: 15
            //     }, {
            //         goodsId: 10,
            //         goodsName: '香脆鸡柳',
            //         price: 17
            //     }
            ],
            type0Goods: [
            //     {
            //         goodsId: 1,
            //         goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
            //         goodsName: '香辣鸡腿堡',
            //         price: 18
            //     }, {
            //         goodsId: 2,
            //         goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
            //         goodsName: '田园鸡腿堡',
            //         price: 15
            //     }, {
            //         goodsId: 3,
            //         goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
            //         goodsName: '和风汉堡',
            //         price: 15
            //     }, {
            //         goodsId: 4,
            //         goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
            //         goodsName: '快乐全家桶',
            //         price: 80
            //     }, {
            //         goodsId: 5,
            //         goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
            //         goodsName: '脆皮炸鸡腿',
            //         price: 10
            //     }, {
            //         goodsId: 6,
            //         goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
            //         goodsName: '魔法鸡块',
            //         price: 20
            //     }, {
            //         goodsId: 7,
            //         goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
            //         goodsName: '可乐大杯',
            //         price: 10
            //     }, {
            //         goodsId: 8,
            //         goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
            //         goodsName: '雪顶咖啡',
            //         price: 18
            //     }, {
            //         goodsId: 9,
            //         goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
            //         goodsName: '大块鸡米花',
            //         price: 15
            //     }, {
            //         goodsId: 10,
            //         goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
            //         goodsName: '香脆鸡柳',
            //         price: 18
            //     }
            ],
            type1Goods: [],
            type2Goods: [],
            type3Goods: [],
            totalCount: 0,
            totalMoney: 0          
        }
    },
    created () {
        axios.get('http://jspang.com/DemoApi/oftenGoods.php')
            .then(response => {
                console.log(response);
                this.oftenGoods = response.data;
            })
            .catch(error => {
                console.log(error);
                alert('网络错误，不能访问');
            });
        axios.get('http://jspang.com/DemoApi/typeGoods.php')
            .then(response => {
                console.log(response);
                this.type0Goods = response.data[0];
                this.type1Goods = response.data[1];
                this.type2Goods = response.data[2];
                this.type3Goods = response.data[3]; 
            })
            .catch(error => {
                console.log(error);
                alert('网络错误，不能访问');                
            })
    },
    methods: {
        addOrderList (goods) {
            var _this = this;
            var isHave = false;
            for (var i = 0; i < this.tableData.length; i ++) {
                if (this.tableData[i].goodsId == goods.goodsId) {
                    isHave = true;
                }
            }
            if (isHave) {
                var arr = this.tableData.filter(function (ele, index) {
                    return ele.goodsId == goods.goodsId
                });
                arr[0].count ++;  
            } else {
                var newGoods = {
                    goodsId: goods.goodsId,
                    goodsName: goods.goodsName,
                    price: goods.price,
                    count: 1
                };
                this.tableData.push(newGoods);
            }
            this.getAllMoney();
        },
        delSingleGoods (goods) {
            if (goods.count > 1) {
                for (var i = 0; i < this.tableData.length; i ++) {
                    if (this.tableData[i].goodsId == goods.goodsId) {
                        this.tableData[i].count --;
                        break;
                    }
                }
            } else {
                this.tableData = this.tableData.filter(function (ele, index) {
                    return ele.goodsId != goods.goodsId;
                })
            }
            this.getAllMoney();
        },
        getAllMoney () {
            this.totalCount = 0;
            this.totalMoney = 0;
            var _this = this;
            if (this.tableData) {
                this.tableData.forEach(function (ele, index) {
                    _this.totalCount += ele.count;
                    _this.totalMoney = _this.totalMoney + (ele.count * ele.price);
                })
            }
        },
        delAllGoods () {
            this.tableData = [];
            this.totalCount = 0;
            this.totalMoney = 0;
        },
        checkout () {
            // 1、设置我们Aixos的Pos方法。
            // 2、接受返回值进行处理。
            // 3、如果成功，清空现有构造器里的tableData，totalMoney，totalCount数据。
            // 4、进行用户的友好提示。
            //因为服务器没写好几口，这里只实现后两步
            if (this.totalCount != 0) {
                this.tableData = [];
                this.totalCount = 0;
                this.totalMoney = 0;
                this.$message({
                    message: '结账成功',
                    type: 'success'
                });
            } else {
                this.$message.error('不能空结')
            }
        }
    }
}
</script>

<style scoped>
    .pos-order {
        background-color: #F9FAFC;
        border-right: 1px solid #C0CCDA;
    }
    .btn {
        margin-top: 10px;
        text-align: center;
    }
    .title {
        height: 20px;
        border-bottom: 1px solid #D3DCE6;
        background-color: #F9FAFC;        
        padding: 10px;
    }
    .often-goods-list ul li {
        list-style: none;
        float: left;
        border: 1px solid #E5E9F2;
        padding: 10px;
        margin: 5px;
        background-color: #fff;
        cursor: pointer;
    }
    .o-price {
        color: #58B7FF;
    }
    .goods-type {
        clear: both;
    }
    .cookList li {
        list-style: none;
        width: 27%;
        border: 1px solid #E5E9F2;
        height: auto;
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
        font-size: 16px;
        padding-left: 10px;
        color: brown;
    }
    .foodPrice {
        font-size: 16px;
        padding-left: 10px;
        padding-top: 10px;
    }
    .total {
        text-align: center;
        padding: 10px;
        border-bottom: 1px solid #D3DCE6;
    }
    .total span {
        margin: 0 10px;
    }
</style>

