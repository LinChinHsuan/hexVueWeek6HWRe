<template>
  單一頁面
  <div class="container">
    <table class="table align-middle">
      <thead>
        <tr>
          <th>圖片</th>
          <th>商品名稱</th>
          <th>分類</th>
          <th>原價</th>
          <th>售價</th>
          <th>內容</th>
          <th>功能</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td style="width: 200px">
            <div
              style="height: 100px; background-size: cover; background-position: center;"
              :style="{ backgroundImage: `url(${product.imageUrl})` }"
            ></div>
          </td>
          <td>
            {{ product.title }}
          </td>
          <td>
            {{ product.category }}
          </td>
          <td>
            <div class="h5">{{ product.origin_price }}元</div>
          </td>
          <td>
            <div class="h5">{{ product.price }}元</div>
          </td>
          <td>
            {{ product.content }}
          </td>
          <td>
            <button
              type="button"
              class="btn btn-outline-danger"
              @click.prevent="addCart(product.id)"
            >
              加到購物車
            </button>
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
      product: {}
    };
  },
  methods: {
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
    const id = this.$route.params.id;
    console.log(id);
    this.$http
      .get(
        `${process.env.VUE_APP_URL}api/${process.env.VUE_APP_PATH}/product/${id}`
      )
      .then(res => {
        if (res.data.success) {
          this.product = res.data.product;
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
