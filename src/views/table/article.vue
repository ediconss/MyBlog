<template>
<div>
  <el-form :inline="true" style=" margin-top : 20px; margin-left: 10px" >
          <el-form-item label="标题">
<el-input v-model="searchMap.title" placeholder=""></el-input></el-form-item>
              <el-form-item label="创建时间">
 <div class="block">
    <el-date-picker
      v-model="searchMap.create_time"
      type="daterange"
      align="right"
      unlink-panels
      range-separator="至"
      start-placeholder="开始日期"
      end-placeholder="结束日期"
      :picker-options="pickerOptions2">
    </el-date-picker>
  </div>
  </el-form-item>
          <el-form-item label="标签">
  <el-select v-model="selectLabelList" multiple placeholder="请选择">
    <el-option
      v-for="item in labelList"
      :key="item.id"
      :label="item.labelName"
      :value="item.labelName">
    </el-option>
  </el-select>
</el-form-item>
          <el-form-item label="主题">

<el-select v-model="selectTopicList" multiple placeholder="请选择">
    <el-option
      v-for="item in topicList"
      :key="item.id"
      :label="item.topic_name"
      :value="item.topic_name">
    </el-option>
  </el-select>
</el-form-item>
          <el-form-item label="状态">
  <el-select v-model="searchMap.start" placeholder="请选择">
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value">
    </el-option>
    
  </el-select></el-form-item>
          <el-form-item label="作者">
<el-input v-model="searchMap.founder" placeholder=""></el-input></el-form-item>

    <el-button  @click="fetchData()">查询</el-button>
    <router-link tag="el-button" :to="'/form/index'">新增</router-link>
    </el-form>
  <el-table
    :data="list"
    border
    style="width: 100%">
          <el-table-column prop="id" label="id" width="120"></el-table-column>
          <el-table-column prop="title" label="标题" width="130"></el-table-column>
          <el-table-column prop="summary" label="概要" width="200"></el-table-column>
          <el-table-column prop="create_time" label="创建时间" width="100"></el-table-column>
          <el-table-column prop="label" label="标签" width="200"></el-table-column>
          <el-table-column prop="topic" label="主题" width="200"></el-table-column>
          <el-table-column prop="start" label="状态" width="80"></el-table-column>
          <el-table-column prop="founder" label="作者" width="80"></el-table-column>
    <el-table-column
      label="操作"
      width="100">
      <template slot-scope="scope">
        
         <router-link tag="a" :to="'/form/index?id='+scope.row.id">修改</router-link>
      <a @click="handleDelete(scope.row.id)" >删除</a>
      </template>
    </el-table-column>
  </el-table>
      <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="[5,10,20]"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
    </el-pagination>
  <el-dialog title="编辑" :visible.sync="dialogFormVisible">
    <el-form  label-width="100">
        <el-form-item label="标题" ><el-input v-model="pojo.title"></el-input></el-form-item>
        <el-form-item label="概要"><el-input v-model="pojo.summary"></el-input></el-form-item>
        <el-form-item label="创建时间"><el-input v-model="pojo.create_time"></el-input></el-form-item>
        <el-form-item label="标签"><el-input v-model="pojo.label"></el-input></el-form-item>
        <el-form-item label="主题"><el-input v-model="pojo.topic"></el-input></el-form-item>
        <el-form-item label="状态"><el-input v-model="pojo.start"></el-input></el-form-item>
        <el-form-item label="作者"><el-input v-model="pojo.founder"></el-input></el-form-item>

        <el-button type="primary" @click="handleSave()">保存</el-button>
        <el-button @click="dialogFormVisible = false" >关闭</el-button>
    </el-form>
  </el-dialog>
</div>
</template>
<script>
import articleApi from '@/api/article'
import labelApi from '@/api/label'
import {getToken} from "@/utils/auth"
import topicApi from '@/api/topic'
export default {
  data() {
    return {
      options: [{
          value: '',
          label: '所有'
        }, {
          value: 0,
          label: '已删除'
        }, {
          value: 1,
          label: '未删除'
        }],
      pickerOptions2: {
          shortcuts: [{
            text: '最近一周',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit('pick', [start, end]);
            }
          }, {
            text: '最近一个月',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
              picker.$emit('pick', [start, end]);
            }
          }, {
            text: '最近三个月',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
              picker.$emit('pick', [start, end]);
            }
          }]
        },
        selectLabelList:[],
        labelList:[],
         selectTopicList:[],
        topicList:[],
      list: [],
      total: 0, // 总记录数
      currentPage: 1, // 当前页
      pageSize: 10, // 每页大小
      searchMap: {}, // 查询条件
      dialogFormVisible: false, // 编辑窗口是否可见
      pojo: {}, // 编辑表单绑定的实体对象
      cityList: [], // 城市列表
      id: '' // 当前用户修改的ID
    }
  },
  created() {
    this.fetchData()
    this.getLabelList()
     this.getTopicList()
  },
  methods: {
      // 初始页currentPage、初始每页数据数pagesize和数据data
        handleSizeChange: function (size) {
                this.pageSize = size;
              this.fetchData()
        },
        handleCurrentChange: function(currentPage){
                this.currentPage = currentPage;
               this.fetchData()
        },
    fetchData() {
      if(this.selectLabelList!=undefined){
        for(var i=0;i<=this.selectLabelList.length;i++){
          this.searchMap.label=this.selectLabelList.join(',')        
        }
      }
      if(this.selectTopicList!=undefined){
        for(var i=0;i<=this.selectTopicList.length;i++){
          this.searchMap.topic=this.selectTopicList.join(',')    
        }
      }
      articleApi.search(this.currentPage, this.pageSize, this.searchMap).then(response => {
        this.list = response.data.rows
        this.total = response.data.total
      })
    },
    getLabelList(){
       labelApi.getList().then(response => {
        this.labelList = response.data
      })
    },getTopicList(){
       topicApi.getList().then(response => {
        this.topicList = response.data
      })
    },
    handleSave() {
      articleApi.update(this.id, this.pojo).then(response => {
        this.$message({
          message: response.message,
          type: (response.flag ? 'success' : 'error')
        })
        if (response.flag) { // 如果成功
          this.fetchData() // 刷新列表
        }
      })
      this.dialogFormVisible = false // 关闭窗口
    },
    handleDelete(id) {
      this.$confirm('确定要删除此纪录吗?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        articleApi.deleteById(id).then(response => {
          this.$message({ message: response.message, type: (response.flag ? 'success' : 'error') })
          if (response.flag) {
            this.fetchData() // 刷新数据
          }
        })
      })
    }
  }
}
</script>
