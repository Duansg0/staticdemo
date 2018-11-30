<template>
  <div >
    <!--<span class="demonstration">Click 指示器触发</span>-->
    <el-carousel trigger="click" height="150px">
      <el-carousel-item v-for="item in 4" :key="item">
       <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1543511609567&di=1f51dcbd5da3cc059f9da227285e3dca&imgtype=0&src=http%3A%2F%2Fpic1.win4000.com%2Fwallpaper%2F8%2F588077fd34646.jpg"/>
      </el-carousel-item>
    </el-carousel>
    <el-menu :default-active="activeIndex2"
      class="el-menu-demo"
      mode="horizontal"
      background-color="#C0C4CC"
      text-color="black"
      active-text-color="black"
      font-family="Microsoft YaHei">
      <el-menu-item index="1" v-on:click="userInfo(ok)"><b>使用指南</b></el-menu-item>
      <el-menu-item index="2" v-on:click="indexInfo()"><b>首页管理</b></el-menu-item>
      <el-menu-item index="3" ><b>模板管理</b></el-menu-item><!--disabled-->
      <el-menu-item index="4"><!--<a href="https://www.ele.me" target="_blank">--><b>全局管理</b><!--</a>--></el-menu-item>
      <el-menu-item style="float:right;" index="5" ><i class="el-icon-setting" style="color: black" ></i></el-menu-item>
      <el-menu-item style="float:right;" index="6" @:click="dateChoise"><!--<i class="el-icon-date" style="color: black" ></i>--><el-date-picker prefix-icon="el-icon-date">
      </el-date-picker></el-menu-item>
    </el-menu>
    <div v-show="indexinfo" style="width: 100%;height: 100%"><!--轮播图-->
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column prop="name" label="姓名" width="180"></el-table-column>
        <el-table-column prop="address" label="地址"></el-table-column>
        <el-table-column label="操作">
          <template slot-scope="row">
            <el-button size="small" type="text" @click="edit(row.row.name)">编辑</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <div class="block">
     <!-- <span class="demonstration">页数较少时的效果</span>-->
      <el-pagination
        layout="prev, pager, next"
        :total="50">
      </el-pagination>
    </div>
    <div v-show="ok"><!--使用指南-->
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span><b>码梦为生?</b></span>
          <!--<el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button>-->
        </div>
        <div v-for="o in 4" :key="o" class="text item" >
         <!-- {{'列表内容 ' + o }}-->
          <b>码梦为生?</b>
        </div>
      </el-card>
    </div>
    <div v-show="manage">
      <br/>
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="活动名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="活动区域">
          <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="活动时间">
          <el-col :span="11">
            <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>
          </el-col>
          <el-col class="line" :span="2">-</el-col>
          <el-col :span="11">
            <el-time-picker type="fixed-time" placeholder="选择时间" v-model="form.date2" style="width: 100%;"></el-time-picker>
          </el-col>
        </el-form-item>
        <el-form-item label="即时配送">
          <el-switch v-model="form.delivery"></el-switch>
        </el-form-item>
        <el-form-item label="活动性质">
          <el-checkbox-group v-model="form.type">
            <el-checkbox label="美食/餐厅线上活动" name="type"></el-checkbox>
            <el-checkbox label="地推活动" name="type"></el-checkbox>
            <el-checkbox label="线下主题活动" name="type"></el-checkbox>
            <el-checkbox label="单纯品牌曝光" name="type"></el-checkbox>
          </el-checkbox-group>
        </el-form-item>
        <el-form-item label="特殊资源">
          <el-radio-group v-model="form.resource">
            <el-radio label="线上品牌商赞助"></el-radio>
            <el-radio label="线下场地免费"></el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="活动形式">
          <el-input type="textarea" v-model="form.desc"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">立即创建</el-button>
          <el-button>取消</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      activeName: 'second',
      activeIndex: '1',
      activeIndex2: '1',
      ok: true,
      manage: false,
      isShow: false,
      indexinfo: false,
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      },
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1519 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      },
      {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      },
      {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      },
      {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      },
      {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      },
      {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      }
      ],
      gridData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }]
    }
  },
  methods: {
    handleClick (tab, event) {
      console.log(tab, event)
    },
    handleSelect (key, keyPath) {
      console.log(key, keyPath)
    },
    dateChoise () {

    },
    userInfo (isShow) {
      if (isShow === true) {
        this.ok = false
      } else {
        this.ok = true
        this.manage = false
        this.isShow = false
      }
    },
    edit (name) {
      this.manage = true
      this.isShow = false
    },
    showImg (isShow) {
      if (isShow === true) {
        this.isShow = false
      } else {
        this.isShow = true
        this.manage = false
        this.ok = false
      }
    },
    indexInfo () {
      this.indexinfo = true
      this.ok = false
    },
    manageInfo (isShow) {
      if (isShow === true) {
        this.manage = false
      } else {
        this.manage = true
        this.ok = false
        this.isShow = false
      }
    },
    onSubmit () {
      console.log('submit!')
    }
  }
}
</script>
<style>
  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 150px;
    margin: 0;
  }

  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }

  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }
  .text {
    font-size: 14px;
  }

  .item {
    margin-bottom: 80px;
  }
</style>
