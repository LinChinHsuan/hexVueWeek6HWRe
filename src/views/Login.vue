<template>
  <div class="container">
    <div class="row justify-content-center">
      <h1 class="h3 mb-3 font-weight-normal text-center">
        請先登入
      </h1>
      <div class="col-8">
        <form id="form" class="form-signin" @submit.prevent="login">
          <div class="form-floating mb-3">
            <input
              type="email"
              class="form-control"
              id="username"
              placeholder="name@example.com"
              required
              autofocus
              v-model="username"
            />
            <label for="username">Email address</label>
          </div>
          <div class="form-floating">
            <input
              type="password"
              class="form-control"
              id="password"
              placeholder="Password"
              required
              v-model="password"
            />
            <label for="password">Password</label>
          </div>
          <button class="btn btn-lg btn-primary w-100 mt-3" type="submit">
            登入
          </button>
        </form>
      </div>
    </div>
    <p class="mt-5 mb-3 text-muted text-center">
      &copy; 2021~∞ - 六角學院
    </p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      username: '',
      password: ''
    };
  },
  methods: {
    login () {
      this.$http
        .post(`${process.env.VUE_APP_URL}admin/signin`, {
          username: this.username,
          password: this.password
        })
        .then(res => {
          console.log(res);
          if (res.data.success) {
            const [token, expired] = [res.data.token, res.data.expired];
            document.cookie = `hexToken=${token}; expires=${new Date(expired)}`;
            this.$router.push('/admin');
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>
