<template>
  <div class="container">
    <table class="table align-middle">
      <thead>
        <tr>
          <th>圖片</th>
          <th>商品名稱</th>
          <th>價格</th>
          <th>功能</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in products" :key="item">
          <td style="width: 200px">
            <div
              style="height: 100px; background-size: cover; background-position: center;"
              :style="{ backgroundImage: `url(${item.imageUrl})` }"
            ></div>
          </td>
          <td>
            {{ item.title }}
          </td>
          <td>
            <div class="h5">{{ item.price }}元</div>
          </td>
          <td>
            <div class="btn-group btn-group-sm">
              <button
                type="button"
                class="btn btn-outline-primary"
                data-bs-toggle="modal"
                data-bs-target="#productModal"
                @click.prevent="getProductDetail(item.id)"
              >
                查看更多
              </button>
              <button
                type="button"
                class="btn btn-outline-danger"
                @click.prevent="addCart(item.id)"
              >
                加到購物車
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      products: []
    };
  },
  methods: {
    getProductDetail (id) {
      this.$router.push(`/product/${id}`);
    },
    addCart (id) {
      this.$http
        .post(
          `${process.env.VUE_APP_URL}api/${process.env.VUE_APP_PATH}/cart`,
          {
            data: {
              product_id: id,
              qty: 1
            }
          }
        )
        .then(res => {
          if (res.data.success) {
            alert('新增成功');
          } else {
            alert(res.data.message);
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  created () {
    const url = `${process.env.VUE_APP_URL}api/${process.env.VUE_APP_PATH}/products`;
    this.$http
      .get(url)
      .then(res => {
        if (res.data.success) {
          this.products = res.data.products;
        } else {
          alert(res.data.message);
        }
      })
      .catch(err => {
        console.log(err);
      });
  }
};
</script>
