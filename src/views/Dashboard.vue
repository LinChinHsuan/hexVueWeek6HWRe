<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item">
          <router-link class="nav-link" to="/admin">首頁</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/admin/products">後台產品列表</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/admin/order">訂單</router-link>
        </li>
      </ul>
    </div>
  </div>
</nav>
  <h1>後台</h1>
  <router-view></router-view>
</template>

<script>
export default {
  methods: {
    checkLogin () {
      const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1');
      this.$http.defaults.headers.common.Authorization = token;
      this.$http
        .post(`${process.env.VUE_APP_URL}api/user/check`)
        .then(res => {
          console.log(res);
          if (!res.data.success) {
            this.$router.push('/');
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  created () {
    this.checkLogin();
  }
}
</script>
