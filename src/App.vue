<!-- 1、模板结构 -->
<template>
  <div id="app" align="center">
    <h1>User Manager</h1>
    <table>
      <tr><th></th><th>用户名</th><th>姓名</th><th></th></tr>
      <tr v-for="user in users" v-bind:class="{fontColor:user.checked}">
        <td><input type="checkbox" v-model="user.checked"/></td>
        <td>{{user.username}}</td>
        <td>{{user.name}}</td>
        <!-- 绑定删除事件方法 -->
        <td><button v-on:click="deleteUser(user)">删除</button></td>
      </tr>
      <tr><td colspan="4" align="center">
        <form>
          <!-- 将输入框与页面数据中的某个对象绑定，方便取值 -->
          <input type="text" placeholder="请输入用户名" v-model="newUser.username"/>
          <input type="text" placeholder="请输入姓名" v-model="newUser.name"/>
          <!-- 绑定提交事件方法 -->
          <input type="submit" value="提交" v-on:click="addUser"/>
        </form>
      </td></tr>
    </table>
  </div>
</template>
 
<!-- 2、JS处理 -->
<script>
export default {
  name: 'app',
  // 数据集合
  data(){
    return {
      users:[],
      newUser:{},
    };
  },
  // 初始化方法
  created:function(){
    this.$http.get("http://jsonplaceholder.typicode.com/users")
    .then(function(response){
      // console.log(response.data);
      this.users = response.data;
    });
  },
  // 方法集合
  methods:{
    deleteUser:function(user){
      // 获取当前对象在数组中的位置序号
      var index = this.users.indexOf(user);
      // 使用数组的splice方法删除对象元素
      this.users.splice(index,1);
    },
    addUser:function(event){
      // 屏蔽表单提交的默认事件
      event.preventDefault();
      // 创建一个新对象，并给对象的属性赋值
      var user = {
        username:this.newUser.username,
        name:this.newUser.name
      }
      // 将新对象插入到数组的末尾；
      this.users.push(user);
      // 清空新用户对象，方便再次输入
      this.newUser = {};
    },
  }
}
</script>
 
<!-- 2、样式处理 -->
<style scoped>
table,th,td{
  border:1px solid gray;
  border-collapse:collapse;
  padding:5px;
}
.fontColor{
  color:red;
}
</style>