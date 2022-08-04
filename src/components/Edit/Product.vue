<template>
    <div class="product-content">
        <p class="tit">组件列表 <span>（可拖动排序）</span></p>
        <el-button class="add-btn" type="primary" @click="toggleSearchPopup"><i class="el-icon-plus"></i> 添加组件</el-button>
        <template v-if="list.data && list.data.length > 0">
            <vuedraggable
                v-model="list.data"
                tag="ul"
                draggable="li"
                v-if="list.data && list.data.length > 0" 
                class="list"
            >
                <li 
                    class="item"
                    v-for="(item, index) in list.data"
                    :key="index"
                >
                    <img :src="item.productImg">
                    <i class="el-icon-error" @click="deleteItem(index)"></i>
                </li>
            </vuedraggable>
        </template>

        <div class="options">
            <el-form label-width="80px">
                <template v-for="(key, val, index) in options">
                    <el-form-item :label="key" :key="index" v-if="loadingOption">

                    </el-form-item>
                </template>
            </el-form>
        </div>

        <el-dialog title="添加组件" :visible.sync="show" @close="close">
            <el-form label-width="100px">
                <el-form-item label="选择组件" >
                    <el-select
                        v-model="selectProduct"
                        filterable
                        remote
                        reserve-keyword
                        placeholder="请输入组件名称"
                        :remote-method="searchProductList"
                        @change="addProduct"
                        :loading="loading"
                    >
                        <el-option
                            v-for="item in productList"
                            :key="item.productId"
                            :label="item.productName"
                            :value-key="item.productName"
                            :value="item"
                        >
                        </el-option>
                    </el-select>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="confirm">确定</el-button>
                </el-form-item>
            </el-form>
        </el-dialog>

    </div>
</template>

<script>
// import { searchProduct } from '@/api/pageDecoration.js'
import vuedraggable from 'vuedraggable'
export default {
    name: 'Product',
    components: {
        vuedraggable
    },
    props: {
        data: {
            type: Object,
            default: () => {}
        }
    },
    data() {
        return {
            list: {},
            productList: [],
            loading: false,
            show: false,

            selectItem: null,
            selectProduct: '',

            options: {
                originalPrice: '',
                goodRatio: '',
                volumeStr: ''
            },
            loadingOption: false
        }
    },
    mounted() {
        this.list = this.data
        
        if (!this.data.tabType) {
            this.$emit('changeTab', 2)
        }

        // 默认关闭所有选项
        for (let key in this.options) {
            if (this.data.options[key] == undefined) {
                this.$set(this.list.options, key, false)
            }
        }
        this.loadingOption = true
    },
    methods: {
        optionsChange(key, result) {
            this.$set(this.list.options, key, result)
        },
        deleteItem(index) {
            this.list.data.splice(index, 1)
        },
        // 搜索商品
        searchProductList(productName) {
            this.productList = productList
        },
        confirm() {
            this.list.data.push(this.selectItem)
            this.close()
        },
        toggleSearchPopup() {
            this.show = true
        },
        close() {
            this.show = false
            this.selectItem = null
            this.selectProduct = ''
        },
        addProduct(data) {
            this.selectItem = data
        }
    }
}
// 模拟产品列表
var productList = [{
    "productId": 3601,
    "productName": "柱状图",
    "productImg": "../image/2号图片.jpeg",
    "productPrice": 299,
    "originalPrice": 598,
    "volumeStr": "741",
    "goodRatio": 98
}, {
    "productId": 3268,
    "productName": "饼状图",
    "productImg": "http://81.70.132.82:16384/static/media/img/3.jpg",
    "productPrice": 370,
    "originalPrice": 1388,
    "volumeStr": "400",
    "goodRatio": 99
}, {
    "productId": 3343,
    "productName": "树状图",
    "productImg": "http://81.70.132.82:16384/static/media/img/4.jpg",
    "productPrice": 34.5,
    "originalPrice": 72,
    "volumeStr": "258",
    "goodRatio": 98
}, {
    "productId": 3330,
    "productName": "折线图",
    "productImg": "https://wx1.sinaimg.cn/large/008h07t6ly1h4uupmabuej310y0mat9v.jpg",
    "productPrice": 75,
    "originalPrice": 128,
    "volumeStr": "258",
    "goodRatio": 98
}, ]
</script>

<style lang="scss">
.product-content{
    .tit{
        text-align: center;
        font-size: 12px;
        color: #666;
        margin: 18px 0;
        padding-bottom: 10px;
        border-bottom: 1px dashed #ddd;
    }
    .add-btn{
        width: calc(100% - 30px);
        height: 34px;
        line-height: 34px;
        padding: 0;
        font-size: 12px;
        margin-left: 15px;
        margin-top: 5px;
    }
    .list{
        display: flex;
        flex-wrap: wrap;
        padding: 12px;
        margin: 0;
        .item{
            width: 70px;
            height: 70px;
            border-radius: 6px;
            margin: 4px;
            position: relative;
            transition: all .3s;
            list-style: none;
            img{
                width: 100%;
                height: 100%;
                border-radius: 4px;
            }
            i{
                position: absolute;
                top: -6px;
                right: -6px;
                cursor: pointer;
                opacity: 0;
                transition: all .3s;
                color: red;
            }
            &::before{
                content: '';
                height: 100%;
                width: 100%;
                position: absolute;
                top: 0;
                right: 0;
                background: rgba(0, 0, 0, .4);
                border-radius: 4px;
                opacity: 0;
                transition: all .3s;
            }
            &:hover{
                cursor: grab;
                &::before, i {
                    opacity: 1;
                }
            }
        }
    }
    .options{
        padding: 15px;
        border-radius: 6px;
        .el-form{
            background: #f7f8f9;
            overflow: hidden;   
            padding: 10px 0;
            .el-form-item{
                margin: 0;
                label{
                    font-size: 12px;
                }
            }
        }
    }
}
</style>