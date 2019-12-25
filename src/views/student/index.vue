<template>
  <div class="dashboard-container">
    <div>
      <el-select v-model="form.academy" placeholder="请选择"  @change="formChange">
        <el-option
          v-for="item in academys"
          :key="item._id"
          :label="item.name"
          :value="item._id">
        </el-option>
      </el-select>
      <el-select v-model="form.classs" @change="formChange" placeholder="请选择">
        <el-option
          v-for="item in classs"
          :key="item._id"
          :label="item.name"
          :value="item._id">
        </el-option>
      </el-select>
      <el-select v-model="form.gender" @change="formChange" placeholder="请选择">
        <el-option
          label="男"
          value="男">
        </el-option>
        <el-option
          label="女"
          value="女">
        </el-option>
      </el-select>
    </div>
    <el-table
      :data="users"
      style="width: 100%">
      <el-table-column
        prop="name"
        label="名字"
        width="50">
      </el-table-column>
      <el-table-column
        prop="age"
        label="年龄"
        width="40">
      </el-table-column>
      <el-table-column
        prop="student_number"
        label="学号">
      </el-table-column>
      <el-table-column
        prop="gender"
        label="性别">

        <template slot-scope="scope" >
          <el-tag v-if="scope.row.gender=='男'">{{scope.row.gender}}</el-tag>
          <el-tag v-else type="danger">{{scope.row.gender}}</el-tag>
        </template>
      </el-table-column>
      <!--      列表添加项目
-->
      <el-table-column
        prop="school"
        label="学校名称"
        width="140">
        <template slot-scope="scope" >
          <span class="" v-if="scope.row.school">
            <el-tag
              :type="scope.row.school.name === '深圳信息职业技术学院' ? 'primary' : 'success'"
              disable-transitions>{{scope.row.school.name}}</el-tag>
          </span>
        </template>
      </el-table-column>
      <el-table-column
        prop="academy"
        label="学院名称"
        width="180">
        <template slot-scope="scope" >
          <span class="" v-if="scope.row.academy">
            <el-tag
              :type="scope.row.academy.name === '软件学院' ? 'primary' : 'success'"
              disable-transitions>{{scope.row.academy.name}}</el-tag>
          </span>

        </template>
      </el-table-column>

      <el-table-column
        prop="classs"
        label="班级名称"
        width="180">
        <template slot-scope="scope" >
          <span class="" v-if="scope.row.classs">
            <el-tag
              :type="scope.row.classs.name === '18软工4-3' ? 'primary' : 'success'"
              disable-transitions>{{scope.row.classs.name}}</el-tag>
          </span>

        </template>
      </el-table-column>

      <el-table-column label="操作" width="180">
        <template slot-scope="scope">
          <el-button
            size="mini"
            @click="handleEdit(scope.$index, scope.row)">编辑
          </el-button>
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)">删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'

  export default {
    name: 'student',
    computed: {
      ...mapGetters([
        'name'
      ])
    },
    data() {
      return {
        apiModel:'student',
        users: {},
        form:{},
        classs:[],
        academys:[]
      }
    },
    methods: {
      onSubmit() {
        console.log(123434)
      },
      handleEdit(index, item) {
        this.$router.push({ path: '/'+this.apiModel+'/editor', query: {_id:item._id} })
      },
      handleDelete(index, item) {
        this.$http.post('/api/'+this.apiModel+'/delete', item).then(res => {
          console.log('res:', res)
          this.findUser()
        })

      },
      findUser(){
        this.$http.post('/api/'+this.apiModel+'/find', this.form).then(res => {
          console.log('res:', res)
          this.users = res
        })
      },
      formChange(val){
        this.findUser()
      }
    },
    mounted() {
      this.findUser()

      //显示班级栏目
      this.$http.post('/api/classs/find').then(res => {
        if(res&&res.length>0){
          this.classs = res
          console.log('res:', res)
        }
      })

      //显示学院选择栏目
      this.$http.post('/api/academy/find').then(res => {
        if(res&&res.length>0){
          this.academys = res
          console.log('res:', res)
        }
      })
    }
  }
</script>

<style lang="scss" scoped>
  .dashboard {
    &-container {
      margin: 30px;
    }

    &-text {
      font-size: 30px;
      line-height: 46px;
    }
  }

  .button{
    display: flex;
    flex-wrap: nowrap;
  }
</style>

