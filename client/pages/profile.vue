<template>
  <main>
    <div class="row">
      <div class="col-sm-3"></div>
      <div class="col-sm-6">
        <div class="a-spacing-top-medium"></div>
        <h2>Profile Page</h2>
        <a href="#" @click="onLogout">Logout</a>
        <form>
          <div class="a-spacing-top-medium">
            <label>Name</label>
            <input class="a-input-text" style="width:100%" v-model="name" :placeholder="$auth.$state.user.name"/>
          </div>
            <div class="a-spacing-top-medium">
            <label>Email</label>
            <input class="a-input-text" style="width:100%" v-model="email" :placeholder="$auth.$state.user.email"/>
          </div>
            <div class="a-spacing-top-medium">
            <label>Password</label>
            <input class="a-input-text" style="width:100%" v-model="password"/>
          </div>
           <hr/> 
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onUpdateProfile">Update Profile</span>
                  </span>
                </span>
              </div>
        </form>
        <hr/>
      </div>
      <div class="col-sm-3"></div>
    </div>
  </main>
</template>
<script>
export default {
  data(){
    return{
      name:"",
      email:"",
      password:""
    }
  },
  methods:{
    async onUpdateProfile(){
      let data={
        name: this.name,
        email: this.email,
        password: this.password
      }
      try{
        let response = await this.$axios.$put('/api/auth/user',data); 

        if(response.success){
          this.name = "";
          this.email = "";
          this.password = "";

          await this.$auth.fetchUser();
        }
      } catch(err){
        console.log(err)
      }
    },
    async onLogout(){
      await this.$auth.logout();
    }
  }
}
</script>