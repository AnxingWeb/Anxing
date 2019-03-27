<template>
    <div class="fillcontain">
        <head-top></head-top>
        &nbsp;
        <h2>&nbsp;&nbsp;&nbsp;&nbsp;用户管理-用户信息</h2>
        &nbsp;
                <div ref="main"></div>
                
        <el-checkbox-group v-model="checkList">
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            <span>分类：</span>
            <el-checkbox label="全部"></el-checkbox>
            <el-checkbox label="医护人员"></el-checkbox>
            <el-checkbox label="维修人员"></el-checkbox>
            <p></p>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            <span>年龄：</span>
            <el-checkbox label="18-29岁"></el-checkbox>
            <el-checkbox label="30-39岁"></el-checkbox>
            <el-checkbox label="40-49岁"></el-checkbox>
            <el-checkbox label="50以上"></el-checkbox>
            <p></p>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            <span>省份：</span>
            <el-checkbox label="浙江"></el-checkbox>
            <el-checkbox label="上海"></el-checkbox>
            <el-checkbox label="北京"></el-checkbox>
            <el-checkbox label="江苏"></el-checkbox>
            <el-checkbox label="广东"></el-checkbox>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<el-button type="primary" size="mini">查询</el-button>
        </el-checkbox-group>
        <div class="table_container">
            <el-table
                :data="tableData"
                highlight-current-row
                style="width: 100%">
                <el-table-column
                  type="index"
                  width="100">
                </el-table-column>
                <el-table-column
                  property="registe_time"
                  label="注册日期"
                  width="220">
                </el-table-column>
                <el-table-column
                  property="username"
                  label="用户姓名"
                  width="220">
                </el-table-column>
                <el-table-column
                  property="city"
                  label="注册地址">
                </el-table-column>
            </el-table>
            <div class="Pagination" style="text-align: left;margin-top: 10px;">
                <el-pagination
                  @size-change="handleSizeChange"
                  @current-change="handleCurrentChange"
                  :current-page="currentPage"
                  :page-size="20"
                  layout="total, prev, pager, next"
                  :total="count">
                </el-pagination>
            </div>
        </div>
    </div>
</template>

<script>
            //引入
        var echarts = require('echarts');

        // 基于准备好的dom，初始化echarts实例
        var myChart = echarts.init(document.getElementById('main'));

        // 绘制图表
        myChart.setOption({
            title: {
                text: 'ECharts 入门示例'
            },
            tooltip: {},
            xAxis: {
                data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
            },
            yAxis: {},
            series: [{
                name: '销量',
                type: 'bar',
                data: [5, 20, 36, 10, 10, 20]
            }]
        });

    import VeLine from 'v-charts/lib/line.common'
    import headTop from '../components/headTop'
    import {getUserList, getUserCount} from '@/api/getData'
    export default {
        data(){
            return {
                tableData: [{
                  registe_time: '2016-05-02',
                  username: '王小虎',
                  city: '上海市普陀区金沙江路 1518 弄'
                }, {
                  registe_time: '2016-05-04',
                  username: '王小虎',
                  city: '上海市普陀区金沙江路 1517 弄'
                }, {
                  registe_time: '2016-05-01',
                  username: '王小虎',
                  city: '上海市普陀区金沙江路 1519 弄'
                }, {
                  registe_time: '2016-05-03',
                  username: '王小虎',
                  city: '上海市普陀区金沙江路 1516 弄'
                }],
                currentRow: null,
                offset: 0,
                limit: 20,
                count: 0,
                currentPage: 1,
                input: '',
                checkList: ['选中且禁用','复选框 A']
            }
        },
    	components: {
    		headTop,
    	},
        created(){
            this.initData();
        },
        methods: {
            async initData(){
                try{
                    const countData = await getUserCount();
                    if (countData.status == 1) {
                        this.count = countData.count;
                    }else{
                        throw new Error('获取数据失败');
                    }
                    this.getUsers();
                }catch(err){
                    console.log('获取数据失败', err);
                }
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                this.offset = (val - 1)*this.limit;
                this.getUsers()
            },
            async getUsers(){
                const Users = await getUserList({offset: this.offset, limit: this.limit});
                this.tableData = [];
                Users.forEach(item => {
                    const tableData = {};
                    tableData.username = item.username;
                    tableData.registe_time = item.registe_time;
                    tableData.city = item.city;
                    this.tableData.push(tableData);
                })
            }
        },
    }
</script>

<style lang="less">
	@import '../style/mixin';
    .table_container{
        padding: 20px;
    }
</style>
