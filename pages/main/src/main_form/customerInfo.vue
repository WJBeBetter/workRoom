<template>
    <div id="customerInfo">
        <!-- 返回键 -->
        <el-button type="warning" id="back_btn" @click="backhome('firstForm')">
            <i class="el-icon-d-arrow-left"></i><span style="margin-left:10px">返回</span>
        </el-button>
        <!-- 总容器 -->
        <div class="content">
            <div class="left_content">
                <div class="customer_search_form">
                    <!--查询表单-->
                    <el-form :model="firstForm_Data" ref="firstForm_Data" label-width="210px" style="margin-left: -70px">
                        <el-form-item label="客户名称">
                            <el-autocomplete class="inline-input" v-model="firstForm_Data.customerName" :fetch-suggestions="querySearch" placeholder="请输入客户名称" @select="handleSelect"></el-autocomplete>
                        </el-form-item>
                        <el-form-item label="客户电话">
                            <el-input v-model="firstForm_Data.phone_number" placeholder="请输入客户电话"></el-input>
                        </el-form-item>
                        <el-form-item label="所在地区">
                            <el-cascader请输入客户名称 :options="addressOptions" v-model="firstForm_Data.address">
                                </el-cascader>
                        </el-form-item>
                        <el-form-item label="详细地址">
                            <el-input placeholder="请输入详细地址" v-model="firstForm_Data.addressDetail"></el-input>
                        </el-form-item>
                        <el-form-item>
                            <a @click="search" class="search">
                                <el-button type="primary" size="large" :disabled=true>查询</el-button>
                            </a>
                            <el-button @click="resetForm" size="large">重置</el-button>
                            <a @click="nextStep('firstForm')" class="nextStep">
                                <el-button type="success" size="large" v-bind:disabled="isComplete">确定</el-button>
                            </a>
                        </el-form-item>
                    </el-form>
                </div>
                <div class="customerTable">
                    <!--表格内容-->
                    <el-table stripe :data="customerData" border height="400" @selection-change="handleSelectionChange">
                        <el-table-column label="常用联系人">
                            <el-table-column type="selection">
                            </el-table-column>
                            <el-table-column prop="customerName" label="姓名">
                            </el-table-column>
                            <el-table-column prop="phone_number" label="手机号">
                            </el-table-column>
                            <el-table-column prop="province" label="省份">
                            </el-table-column>
                            <el-table-column prop="address" label="市区" width="200">
                            </el-table-column>
                            <el-table-column prop="addressDetail" label="地址" width="200">
                            </el-table-column>
                        </el-table-column>
                    </el-table>
                </div>
            </div>
            <div class="right_content">
                <el-alert title="小提示:" type="info" :closable="false" description="点击下面的图像按钮可添加新的用户哦~~">
                </el-alert>
                <img src="../asset/img/linkman.png" alt="" width="250" height="300" class="linkman" @click="dialogFormVisible = true">
            </div>
        </div>
        <!-- dialog表单 -->
        <el-dialog title="添加用户" v-model="dialogFormVisible" class="dialogTool">
            <el-form>
                <el-form-item label="用户名称">
                    <el-autocomplete class="inline-input" v-model="firstForm_Data.customerName" :fetch-suggestions="querySearch" placeholder="请输入客户名称" @select="handleSelect"></el-autocomplete>
                </el-form-item>
                <el-form-item label="用户电话">
                    <el-input v-model="firstForm_Data.phone_number" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="用户地址">
                    <el-cascader :options="addressOptions" v-model="firstForm_Data.address" id="address">
                    </el-cascader>
                </el-form-item>
                <el-form-item label="详细地址">
                    <el-input placeholder="请输入详细地址" v-model="firstForm_Data.addressDetail"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="nextStep('firstForm')">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>
<script type="text/ecmascript-6">
let searchTool = require('../compoents/search/search.vue');
export default {
    props: {
        firstForm_Data: Object
    },
    data() {
        return {
            restaurants: [],
            customerData: [{
                customerName: '张三',
                phone_number: '1216654',
                province: '上海',
                addressDetail: '龙港巷32号东门',
                address: ['广东省', '广州市', '南沙区'],
            }, {
                customerName: '李四',
                phone_number: '1216654',
                province: '上海',
                addressDetail: '龙港巷32号东门',
                address: ['广东省', '广州市', '珠海区'],

            }, {
                customerName: '王五',
                phone_number: '1216654',
                province: '上海',
                addressDetail: '龙港巷32号东门',
                address: ['广东省', '东莞市', '东城区'],

            }, {
                customerName: '赵六',
                phone_number: '1216654',
                province: '上海',
                addressDetail: '龙港巷32号东门',
                address: ['广东省', '东莞市', '西城区'],

            }, {
                customerName: '周七',
                phone_number: '1214654',
                province: '上海',
                addressDetail: '龙港巷32号东门',
                address: ['广东省', '东莞市', '北城区'],

            }, {
                customerName: '陈八',
                phone_number: '1214654',
                province: '上海',
                addressDetail: '龙港巷32号东门',
                address: ['广东省', '东莞市', '南城区'],

            }, {
                customerName: '何九',
                phone_number: '1214654',
                province: '上海',
                addressDetail: '龙港巷32号东门',
                address: ['广东省', '东莞市', '西城区'],

            }, {
                customerName: '测试员',
                phone_number: '1214654',
                province: '上海',
                addressDetail: '龙港巷32号东门',
                address: ['广东省', '东莞市', '西城区'],

            }],
            addressOptions: [{
                value: '广东省',
                label: '广东省',
                children: [{
                    value: '东莞市',
                    label: '东莞市',
                    children: [{
                        value: '东城区',
                        label: '东城区'
                    }, {
                        value: '南城区',
                        label: '南城区'
                    }, {
                        value: '西城区',
                        label: '西城区'
                    }, {
                        value: '北城区',
                        label: '北城区'
                    }, ]
                }, {
                    value: '广州市',
                    label: '广州市',
                    children: [{
                        value: '珠海区',
                        label: '珠海区'
                    }, {
                        value: '南沙区',
                        label: '南沙区'
                    }]
                }]
            }, {
                value: '江苏省',
                label: '江苏省',
                children: [{
                    value: '南京',
                    label: '南京',
                    children: [{
                        value: '玄武区',
                        label: '玄武区'
                    }, {
                        value: '雨花区',
                        label: '雨花区'
                    }, {
                        value: '鼓楼区',
                        label: '鼓楼区'
                    }, {
                        value: '六合区',
                        label: '六合区'
                    }]
                }, {
                    value: '常州',
                    label: '常州',
                    children: [{
                        value: '武进区',
                        label: '武进区'
                    }, {
                        value: '新北区',
                        label: '新北区'
                    }]
                }]
            }],
            dialogFormVisible: false,
            form: {
                name: null,
                region: '',
                address: ''
            },
            multipleSelection: []
        }
    },
    computed: {
        isComplete() {
            if (this.multipleSelection.length > 0 || (this.firstForm_Data.customerName && this.firstForm_Data.phone_number && this.firstForm_Data.address.length > 0)) {
                return false;
            }
            return true;
        }
    },
    methods: {
        search() {
            this.$message.error('暂时无真实数据,正在维护中....');
        },
        resetForm() {
            for (let key in this.firstForm_Data) {
                if (Array.isArray(this.firstForm_Data[key])) {
                    this.firstForm_Data[key] = [];
                }else{
                this.firstForm_Data[key] = "";}
            }
        },
        backhome(form) {
            this.$emit('back', form);
        },
        handleSelectionChange(val) {
            val.splice(0, val.length - 1);

            this.multipleSelection = val;
            if (val) {
                this.firstForm_Data.customerName = val[0].customerName;
                this.firstForm_Data.phone_number = val[0].phone_number;
                this.firstForm_Data.address = val[0].address;
                this.firstForm_Data.addressDetail = val[0].addressDetail;
            }
        },
        nextStep(formName) {
            if (this.multipleSelection.length > 0 || (this.firstForm_Data.customerName && this.firstForm_Data.phone_number && this.firstForm_Data.address.length > 0)) {
                this.$emit('complete', formName, "firstForm_Data", this.firstForm_Data);
                this.dialogFormVisible = false;
            } else {
                this.$message({
                    showClose: true,
                    message: '请选择下列表格中的用户,或者新建用户进行下一步操作!!',
                    type: 'warning',
                });
            }
        },
        querySearch(queryString, cb) {
            var restaurants = this.restaurants;
            var results = queryString ? restaurants.filter(this.createFilter(queryString)) : restaurants;
            // 调用 callback 返回建议列表的数据
            cb(results);
        },
        createFilter(queryString) {
            return (restaurant) => {
                return (restaurant.value.indexOf(queryString.toLowerCase()) === 0);
            };
        },
        loadAll() {
            return [{
                "value": "王杰",
                "address": "长宁区新渔路144号"
            }, {
                "value": "王五",
                "address": "上海市长宁区淞虹路661号"
            }, {
                "value": "张三",
                "address": "上海市普陀区真北路988号创邑金沙谷6号楼113"
            }, {
                "value": "李四",
                "address": "天山西路438号"
            }, {
                "value": "赵六",
                "address": "上海市长宁区金钟路968号1幢18号楼一层商铺18-101"
            }, {
                "value": "周七",
                "address": "上海市长宁区金钟路633号"
            }, {
                "value": "孙七",
                "address": "上海市嘉定区曹安公路曹安路1685号"
            }, {
                "value": "何九",
                "address": "上海市普陀区同普路1435号"
            }];
        },
        handleSelect(item) {
            console.log(item);
        }
    },
    components: {
        searchTool: searchTool
    },
    mounted() {
        this.restaurants = this.loadAll();
    }
};
</script>
<style lang="less" rel="stylesheet/less">
#customerInfo {
    height: inherit;
    overflow: hidden;
    th .el-checkbox__inner {
        //隐藏table第一栏的checkbox
        display: none;
    }
    #back_btn {
        //返回键
        margin: 0 1px;
        position: absolute;
    }
    //更改UI框架样式
    .el-button {
        margin-top: 28px;
        span {
            font-size: 30px;
        }
    }
    .el-form-item__label {
        text-align: right;
    }
    .el-input {
        width: 450px;
    }
    .el-input__inner {
        height: 45px;
        width: 450px;
        font-size: 26px;
    }
    .el-alert__content {
        .el-alert__title,
        .el-alert__description {
            font-size: 24px;
        }
    }
    .searchTool {
        margin-left: 10px;
    }
    //内容
    .content {
        height: inherit;
        display: flex;
        //修改UI框架
        .el-form-item {
            margin-bottom: 10px;
        }
        .left_content {
            //左边的格子
            display: flex;
            flex: 3;
            height: 100%;
            justify-content: space-between;
            flex-direction: column;
            padding: 2% 0 -2% 0;
            overflow: hidden;
            .customer_search_form,
            .customerTable {
                box-sizing: border-box;
                padding: 0 10px;
                overflow: hidden;
                text-align: center;
                .el-table__header th .cell:first-child {
                    padding: 1%;
                    text-align: center;
                    font-weight: bold;
                }
            }
            .customer_search_form {
                flex: 1;
                margin-top: 55px;
                .el-button {
                    margin-top: 10px;
                }
                .search {
                    margin-right: 20px;
                }
                .nextStep {
                    margin-left: 20px;
                }
            }
            .customerTable {
                flex: 1.4;
                .el-table {
                    //下面的table
                    width: 99%;
                    font-size: 17px;
                    .el-checkbox__inner {
                        width: 25px;
                        height: 25px;
                    }
                    .el-table__body-wrapper{
                    overflow-x: hidden;
                    }
                }
            }
        }
        .right_content {
            flex: 1;
            height: 100%;
            display: block;
            box-sizing: border-box;
            border-left: 4px ridge #cccccc;
            padding: 2%;
            overflow: hidden;
            .linkman {
                position: relative;
                bottom: -170px;
                opacity: 0.8;
                cursor: pointer;
            }
        }
    }
    .dialogTool {
        .el-cascader__label {
            line-height: 50px;
            height: auto;
            left: 30px;
            font-size: 20px;
        }
        .el-input__icon {
            right: -30px;
        }
        #address {
            width: 270px;
            .el-input {
                width: 270px;
                margin-left: 30px;
            }
            .el-input__inner {
                height: 50px;   
                width: 270px;
                max-width: 270px;
                font-size: 26px;
            }
        }
        .el-input {
            width: 230px;
            max-width: 230px;
            margin-left: 30px;
        }
        .el-input__inner {
            height: 50px;
            width: 230px;
            max-width: 230px;
            font-size: 26px;
        }
        .dialog_search {
            margin-left: 150px;
        }
    }
}
</style>
