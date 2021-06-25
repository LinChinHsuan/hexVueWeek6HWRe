<template>
  <div class="container">
    <div class="text-end mt-4">
      <button
        class="btn btn-primary"
        data-bs-toggle="modal"
        data-bs-target="#productModal"
        @click.prevent="editProduct()"
      >
        建立新的產品
      </button>
    </div>
    <table class="table mt-4">
      <thead>
        <tr>
          <th width="120">
            分類
          </th>
          <th>產品名稱</th>
          <th width="120">
            原價
          </th>
          <th width="120">
            售價
          </th>
          <th width="100">
            是否啟用
          </th>
          <th width="120">
            編輯
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in products" :key="item.id">
          <td>{{ item.category }}</td>
          <td>{{ item.title }}</td>
          <td class="text-end">{{ item.origin_price }}</td>
          <td class="text-end">{{ item.price }}</td>
          <td>
            <span :class="{ 'text-success': item.is_enabled }">{{
              item.is_enabled ? '啟用' : '未啟用'
            }}</span>
          </td>
          <td>
            <div class="btn-group">
              <button
                type="button"
                class="btn btn-outline-primary btn-sm"
                data-bs-toggle="modal"
                data-bs-target="#productModal"
                @click.prevent="editProduct(item)"
              >
                編輯
              </button>
              <button
                type="button"
                class="btn btn-outline-danger btn-sm"
                data-bs-toggle="modal"
                data-bs-target="#delProductModal"
                @click.prevent="editProduct(item)"
              >
                刪除
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
const productSample = {
  category: '',
  content: '',
  description: '',
  id: '',
  is_enabled: 1,
  origin_price: 0,
  price: 0,
  title: '',
  unit: '個',
  num: 0,
  imageUrl: '',
  imagesUrl: []
};
export default {
  data () {
    return {
      products: [],
      editingProduct: {
        data: {}
      },
      status: ''
    };
  },
  methods: {
    getProducts (page = 1) {
      this.$http
        .get(
          `${process.env.VUE_APP_URL}api/${process.env.VUE_APP_PATH}/admin/products?page=${page}`
        )
        .then(res => {
          if (res.data.success) {
            this.products = res.data.products;
            // this.pagination = res.data.pagination;
            this.editingProduct.data = {};
          } else {
            alert(res.data.message);
          }
        })
        .catch(err => {
          console.log(err);
        });
    },
    editProduct (item) {
      if (item) {
        this.status = '編輯';
        this.editingProduct = {
          data: { ...item }
        };
      } else {
        this.status = '新增';
        this.editingProduct = {
          data: { ...productSample }
        };
      }
    },
    delProduct () {
      this.$http
        .delete(
          `${process.env.VUE_APP_URL}api/${process.env.VUE_APP_PATH}/admin/product/${this.editingProduct.data.id}`
        )
        .then(res => {
          console.log(res);
          if (res.data.success) {
            console.log('刪除成功');
            this.getProducts();
          } else {
            console.log(res.data.message);
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  created () {
    this.getProducts();
  }
};
</script>
