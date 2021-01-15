<!--suppress ALL -->
<template>
  <div>
    <el-form :inline="true" class="demo-form-inline">
      <el-form-item>
        <el-input
          v-model="search"
          class="search_name"
          size="mini"
          placeholder="输入姓名查询">
        </el-input>
      </el-form-item>
      <el-form-item>
        <el-button
          type="text"
          @click="onSearch()"
          class="el-icon-search">查询
        </el-button>
      </el-form-item>
      <el-form-item>
        <el-button
          class="el-icon-refresh"
          type="text"
          @click="refreshData">刷新
        </el-button>
      </el-form-item>
      <el-form-item>
        <el-button
          class="el-icon-circle-plus-outline"
          type="text"
          @click="dialogVisible = true">添加
        </el-button>
      </el-form-item>
    </el-form>
    <el-table
      :data="tableData"
      highlight-current-row
      border
      style="width: 100%">
      <el-table-column
        label="编号">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="父亲姓名">
        <template slot-scope="scope">
          <span>{{ scope.row.fatherName }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="姓名">
        <template slot-scope="scope">
          <span>{{ scope.row.name }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="性别">
        <template slot-scope="scope">
          <span>{{ scope.row.sex == 0 ? '男' : '女' }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="创建日期">
        <template slot-scope="scope">
          <i class="el-icon-time hidden-sm-and-down"></i>
          <span>{{ scope.row.createDate }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="更新日期">
        <template slot-scope="scope">
          <i class="el-icon-time hidden-sm-and-down"></i>
          <span>{{ scope.row.updateDate }}</span>
        </template>
      </el-table-column>
      <!--<el-table-column-->
        <!--label="姓名">-->
        <!--<template slot-scope="scope">-->
          <!--<el-popover trigger="hover" placement="right">-->
            <!--<p>姓名: {{ scope.row.userName }}</p>-->
            <!--<p>住址: {{ scope.row.userAddress }}</p>-->
            <!--<p>日期：{{ scope.row.userDate }}</p>-->
            <!--<div slot="reference" class="name-wrapper">-->
              <!--<el-button type="text">{{ scope.row.userName }}</el-button>-->
            <!--</div>-->
          <!--</el-popover>-->
        <!--</template>-->
      <!--</el-table-column>-->
      <el-table-column
        label="操作"
        fixed="right"
        width="200">
        <template slot-scope="scope">
          <el-button
            size="mini"
            icon="el-icon-edit"
            @click="handleEdit(scope.$index, scope.row)">编辑
          </el-button>
          <el-button
            size="mini"
            type="danger"
            icon="el-icon-delete"
            @click="handleDelete(scope.$index, scope.row)">删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="70px" class="demo-ruleForm" size="medium">
      <el-dialog
        title="添加"
        :append-to-body='true'
        :visible.sync="dialogVisible"
        width="80%"
        :before-close="handleClose">
        <!--<el-input type="hidden" v-model="ruleForm.id"/>-->
   <!--     <el-form-item label="时间" prop="userDate">
          <el-date-picker type="datetime" placeholder="选择日期" v-model="ruleForm.userDate" style="width: 100%;"></el-date-picker>
        </el-form-item>-->
        <el-form-item label="父亲姓名:" prop="fatherName" label-width="100px">
          <el-input v-model="ruleForm.fatherName"></el-input>
        </el-form-item>
        <el-form-item label="姓名:" prop="name" label-width="100px">
          <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>
        <el-form-item label="性别：" prop="sex" size="mini" class='form-label' label-width="100px">
          <el-radio-group v-mode="ruleForm.sex" @change="getRadioDate">
            <el-radio :label="0" >男</el-radio>
            <el-radio :label="1" >女</el-radio>
          </el-radio-group>
        </el-form-item>

        <span slot="footer" class="dialog-footer">
            <el-button @click="cancel()" size="medium">取 消</el-button>
            <el-button @click="addNameMenu('ruleForm')" type="primary" size="medium">确 定</el-button>
          </span>
      </el-dialog>
    </el-form>

    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="70px" class="demo-ruleForm" size="medium">
      <el-dialog
        title="编辑"
        :append-to-body='true'
        :visible.sync="dialogUpdate"
        width="80%"
        :before-close="handleClose">
        <el-input type="hidden" v-model="ruleForm.id"/>
        <el-input type="hidden" v-model="ruleForm.parentId"/>
      <!--  <el-form-item label="时间" prop="userDate">
          <el-date-picker type="datetime" placeholder="选择日期" v-model="ruleForm.userDate" style="width: 100%;"></el-date-picker>
        </el-form-item>-->
        <el-form-item label="父亲姓名:" prop="fatherName" label-width="100px">
          <el-input v-model="ruleForm.fatherName"></el-input>
        </el-form-item>
        <el-form-item label="姓名:" prop="name" label-width="100px">
          <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>
        <el-form-item label="性别：" prop="sex" size="mini" class='form-label' label-width="100px">
          <el-radio-group v-mode="ruleForm.sex" @change="getRadioDate">
            <el-radio :label="0" >男</el-radio>
            <el-radio :label="1" >女</el-radio>
          </el-radio-group>
        </el-form-item>

        <span slot="footer" class="dialog-footer">
            <el-button @click="cancel()" size="medium">取 消</el-button>
            <el-button @click="updateNameMenu()" type="primary" size="medium">确 定</el-button>
          </span>
      </el-dialog>
    </el-form>
    <br>
    <div class="pages">
      <el-pagination
        background
        :disabled = "disablePage"
        :current-page.sync="currentPage"
        small
        layout="prev, pager, next"
        :page-size="pageSize"
        :total="total"
        @current-change="queryByPage">
      </el-pagination>
    </div>
  </div>
</template>

<script>
let baseUrl = 'http://localhost:8081'
export default {
  data () {
    return {
      ruleForm: {
        id: '',
        fatherName: '',
        name: '',
        sex: 0,
        createDate: '',
        updateDate: '',
        parentId: ''
      },
      rules: {
        fatherName: [
          { required: true,
            message: '请输入父亲姓名',
            trigger: 'blur'
          },
          { min: 2, max: 7, message: '长度在 2 到 7 个字符', trigger: 'blur' }
        ],
        name: [
          { required: true, message: '请输入姓名', trigger: 'blur' },
          { min: 2, max: 7, message: '长度在 2 到 7 个字符', trigger: 'blur' }
        ],
        createDate: [
          { required: true, message: '请输入时间', trigger: 'blur' }
          // { min: 2, max: 7, message: '长度在 2 到 7 个字符', trigger: 'blur' }
        ],
        updateDate: [
          { required: true, message: '请输入时间', trigger: 'blur' }
          // { min: 2, max: 7, message: '长度在 2 到 7 个字符', trigger: 'blur' }
        ]
      },
      tableData: [],
      search: '',
      dialogVisible: false,
      dialogUpdate: false,
      pageSize: 1,
      currentPage: 1,
      total: 0,
      disablePage: true
    }
  },
  methods: {
    getRadioDate: function (val) {
      console.log('获得单选框值是：', val, typeof val)
    },
    handleEdit (index, row) {
      this.dialogUpdate = true
      this.ruleForm = Object.assign({}, row) // 这句是关键！！！
    },
    handleDelete (index, row) {
      console.log(index, row)
      this.$confirm('删除操作, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
      /* let paramData = this.qs.stringify({
          id: row.id
        })
        */
        let paramData = {
          id: row.id
        }
        this.axios({
          // method: 'post',
          method: 'delete',
          url: baseUrl + 'nameMenu/delete/',
          params: paramData
        }).then(response => {
          this.currentPage = 1
          // this.getPages()
          this.queryByPage()
          /* this.axios.post('/page').then(response => {
            this.tableData = response.data
          }).catch(error => {
            console.log(error)
          }) */
          this.$message({
            type: 'success',
            message: '删除成功!'
          })
          console.log(response)
        }).catch(error => {
          console.log(error)
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
    handleClose (done) {
      this.$confirm('确认关闭？')
        .then(_ => {
          done()
        })
        .catch(_ => {})
    },
    /* handleCurrentChange () {
      console.log(`当前页: ${this.currentPage}`)
      let postData = this.qs.stringify({
        page: this.currentPage
      })
      this.axios({
        method: 'get',
        url: '/page',
        data: postData
      }).then(response => {
        this.tableData = response.data
      }).catch(error => {
        console.log(error)
      })
    }, */
    cancel () {
      this.dialogUpdate = false
      this.dialogVisible = false
      this.emptyNameMenData()
    },
    emptyNameMenData () {
      this.ruleForm = {
        // id: '',
        fatherName: '',
        name: '',
        // sex: '',
        createDate: '',
        updateDate: '',
        parentId: ''
      }
    },
      addNameMenu (ruleForm) {
        // this.$refs['ruleForm1'].validate
        this.$refs[ruleForm].validate((valid) => {
        if (valid){
          let postData = {
            userDate: this.ruleForm.userDate,
            fatherName: this.ruleForm.fatherName,
            name: this.ruleForm.name,
            sex: this.ruleForm.sex
            // createDate: this.ruleForm.createDate,
            // updateDate: this.ruleForm.updateDate
          }
          this.axios({
            method: 'post',
            url: baseUrl + '/nameMenu/add',
            data: postData
          }).then(response => {
            console.log('t:' + response.data.code)
            if (response.data.code !== 0){
              this.$message({
                type: 'fail',
                message: '添加失败!' + response.data.msg
              })
            }else{
              this.$message({
                type: 'success',
                message: '添加成功!'
              })
            }
            this.currentPage = 1
            this.queryByPage()

            this.dialogVisible = false
            console.log(response)
          }).catch(error => {
            console.log(error)
          })
        } else { //校验不通过
          this.$message.error("数据校验失败!,请验证你输入的数据正确性。")
        }
      });

    },
    updateNameMenu () {
/*      let postData = this.qs.stringify({
        id: this.ruleForm.id,
        fatherName: this.ruleForm.fatherName,
        sex: this.ruleForm.sex,
        parentId: this.ruleForm.parentId
      })
      */
      let postData = {
        id: this.ruleForm.id,
        fatherName: this.ruleForm.fatherName,
        sex: this.ruleForm.sex,
        parentId: this.ruleForm.parentId
      }
      this.axios({
        method: 'put',
        url: baseUrl + '/nameMenu/update',
        data: postData
      }).then(response => {
        // this.handleCurrentChange()
        this.queryByPage()
        this.cancel()
        this.$message({
          type: 'success',
          message: '更新成功!'
        })
        console.log(response)
      }).catch(error => {
        this.$message({
          type: 'success',
          message: '更新失败!'
        })
        console.log(error)
      })
    },
    onSearch: function () {
      this.currentPage = 1
      this.queryByPage()
/*
      let pageNum = this.currentPage < 1 ? 1 : this.currentPage
      let pageSize = this.pageSize < 1 ? 5 : this.pageSize
      let getData = {
        name: this.search,
        num: pageNum,
        pageSize: this.pageSize
      }
      this.axios({
        method: 'get',
        url: baseUrl + '/nameMenu/queryLike',
        params: getData
      }).then(response => {
        /!* this.tableData = response.data.data.list
         this.disablePage = true
         *!/
        console.log('yy:' + response.data.data)
        this.tableData = response.data.data.list
        this.total = response.data.data.total
        this.disablePage = false
        console.log('this.disablePage:' + this.disablePage)
      }).catch(error => {
        console.log(error)
      })
      */
    },
    /* getPages () {
      this.axios.post('/rows').then(response => {
        this.total = response.data
      }).catch(error => {
        console.log(error)
      })
    },
    */
    refreshData () {
      location.reload()
    },
    queryByPage () {
      var name_ = this.search.trim().replace(/(^\s*)|(\s*$)/g, '');//去除空格;
      // if(name_ !== '' || name_ !== null || name_ !== undefined){//模糊查询
      // if("" !=name  || name_ !== null || name_ !== undefined){//模糊查询
      //   console.log('name:' + name_)
      //   this.onSearch()
      // }else {//分页
        // queryByPage()

        let pageNum = this.currentPage < 1 ? 1 : this.currentPage
        let pageSize = this.pageSize < 1 ? 5 : this.pageSize
        /*      let getData = this.qs.stringify({
                num: pageNum,
                pageSize: pageSize
              })
              */
        let getData = {
          name: name_,
          num: pageNum,
          pageSize: pageSize
        }
        this.axios({
          method: 'get',
          // url: 'http://localhost:8081/nameMenu/queryByPage',
          url: baseUrl +  '/nameMenu/queryByPage',
          params: getData,
          headers: {
            'Content-Type': 'application/json'
          }
        }).then(response => {
          this.tableData = response.data.data.list
          this.total = response.data.data.total
          this.disablePage = false
        }).catch(error => {
          console.log(error)
        })
      // }

    },

    changeQueryPage(){
      let name = this.search
      if(name != null){
        console.log('name:' + name)
        this.onSearch()
      }else {
        queryByPage()
      }
    }
  },
  created () {
    this.queryByPage()
  }

}
</script>
<style scoped>
  .search_name{
    width: 200px;
  }
  .pages{
    margin: 0px;
    padding: 0px;
    text-align: right;
  }
</style>
