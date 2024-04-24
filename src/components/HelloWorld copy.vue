<template>
  <div id="app">
    <h1>{{ message }}</h1>
    <div>
      <el-form  label-width="auto" style="max-width: 600px; align-items: center;">
        <el-form-item label="用户名" >
          <el-input v-model="username" />
        </el-form-item>
        <el-form-item label="密码">
          <el-input v-model="password" />
        </el-form-item>
        <el-button @click="submit">提交</el-button>
      </el-form>

    </div>
  </div>
</template>

<script>
import { ref, onMounted, reactive, toRefs } from 'vue';
import axios from 'axios';

export default {
  setup() {
    const message = ref('php');
    const form = reactive({
      username: '',
      password: '',
    })
    async function submit() {
      const obj = { username: form.username, password: form.password }
      // console.log(obj)
      try {
        const res = await axios.post("http://test:8055/api.php", obj)
        console.log(res)
        if (res.status != 200) {
          console.log("请求错误")
        }
        else {
          // console.log('请求正确')
        }
      }
      catch (e) {
        console.log(e)
      }
    }

    onMounted(() => {
      // axios.get('http://test:8055/api.php')
      //   .then(response => {
      //     message.value = response.data.message
      //     console.log(response);
      //   })
      //   .catch(error => {
      //     console.error('There was an error!', error);
      //   });
    });
    return { ...toRefs(form), message, submit }
  }

}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
