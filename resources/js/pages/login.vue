<template>
  <div>
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-sm-6 mt-4">

          <p class="text-danger" v-show="error">{{ error }}</p>

          <form @submit.prevent="login">

            <div class="form-group">
              <label for="email">Email Address</label>
              <input type="email" class="form-control" id="email" v-model="form.email">
            </div>

            <div class="form-group">
              <label for="password">password</label>
              <input type="password" class="form-control" id="password" v-model="form.password">
            </div>
            <button type="submit" class="btn btn-primary">Login</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive,ref } from 'vue'
import { useRouter } from 'vue-router';
import { useStore } from 'vuex';
//import axios from 'axios'
export default {
 setup(){
    let form = reactive({
      email:'',
      password:''
    });

    let error = ref('');
    const router = useRouter();
    const store = useStore();

    const login = async() => {
     
     await axios.post('/api/login',form).then(res=>{
        if(res.data.success){
          localStorage.setItem('token',res.data.data.token);
          store.dispatch('setToken',res.data.data.token);
          router.push({name:'Dashboard'});
        }else{
          error.value = res.data.message;
          //console.log(res.data.success);
        }
      })

      //console.log('Welcome');
    } 
    return{
      form,
      login,
      error
    }
  }
}
  
</script>

