<template>
  <div class="dashboard-container">
    <div class="item">
      <h1>{{studentCount}}</h1>
      <h4>全校总人数</h4>
    </div>

    <div class="item">
      <h1>{{classsCount}}</h1>
      <h4>全校班级总数</h4>
    </div>

    <div class="item">
      <h1>{{classCount}}</h1>
      <h4>全校老师总数</h4>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'Dashboard',
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  data(){
    return{
      studentCount:'',
      classsCount: 10
    }
  },
  mounted() {
    this.$http.get('/api/student/count').then(res =>{
      this.studentCount = res
    })

    this.$http.get('/api/classs/count').then(res =>{
      this.classsCount = res
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

.dashboard-container{
  display: flex;
}

  .item{
    text-align: center;
    transition: all 0.3s;
    position: relative;
    border-radius: 3px;
    top: 0;
    box-shadow: 0 0 0 0 rgba(0,0,0,0.1);
    padding: 10px;
    margin: 10px;
  }
  .item:hover{

    top: -20;
    box-shadow: 1px 10px 15px 2px rgba(0,0,0,0.1);
  }
</style>
