<template>
  <div>
    <nav
      class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-2 shadow mt--1 "
    >
      <a class="navbar-brand col-md-3 col-lg-2 mr-0 px-3" href="#"
        >管理者平台</a
      >
      <button
        class="navbar-toggler position-absolute d-md-none collapsed"
        type="button"
        data-toggle="collapse"
        data-target="#sidebarMenu"
        aria-controls="sidebarMenu"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <input
        class="form-control form-control-dark w-100"
        type="text"
        placeholder="Search"
        aria-label="Search"
      />
      <ul class="navbar-nav px-3">
        <li class="nav-item text-nowrap" @click.prevent="signout">
          <a class="nav-link" href="#">登出</a>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  //?
  name: 'Navbar',
  methods: {
    signout(){
      const vm = this
      const api = `${process.env.VUE_APP_APIPATH}/logout`
      this.$http.post(api).then((response)=>{
        if(response.data.success){
          vm.$router.push("/signin")
        }
      })
    }
  }

}
</script>

<style lang="sass">
</style>