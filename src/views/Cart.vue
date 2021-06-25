<template>
  購物車
  <div class="container">
    <div class="text-end">
      <button
        class="btn btn-outline-danger"
        type="button"
        @click="delAllCartProduct"
      >
        清空購物車
      </button>
    </div>
    <table class="table align-middle">
      <thead>
        <tr>
          <th></th>
          <th>品名</th>
          <th style="width: 150px">數量/單位</th>
          <th>單價</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in cart.carts" :key="item">
          <td>
            <button
              type="button"
              class="btn btn-outline-danger btn-sm"
              @click.prevent="delCartProduct(item.id)"
            >
              x
            </button>
          </td>
          <td>
            {{ item.product.title }}
            <div class="text-success">
              已套用優惠券
            </div>
          </td>
          <td>
            <div class="input-group input-group-sm">
              <input
                type="number"
                v-model.number="item.qty"
                @blur.prevent="updateCartProduct(item)"
              />
              / {{ item.product.unit }}
            </div>
          </td>
          <td class="text-end">
            <small class="text-success">折扣價：</small>
            {{ item.final_total }}
          </td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <td colspan="3" class="text-end">總計</td>
          <td class="text-end">{{ cart.total }}</td>
        </tr>
        <tr>
          <td colspan="3" class="text-end text-success">折扣價</td>
          <td class="text-end text-success">{{ cart.final_total }}</td>
        </tr>
      </tfoot>
    </table>
    <div class="my-5 row justify-content-center">
      <Form class="col-md-6" v-slot="{ errors }" @submit.prevent="onSubmit">
        <div class="mb-3">
          <label for="email" class="form-label">Email</label>
          <Field
            id="email"
            name="email"
            type="email"
            class="form-control"
            :class="{ 'is-invalid': errors['email'] }"
            placeholder="請輸入 Email"
            rules="email|required"
            v-model="user.email"
          ></Field>
          <error-message name="email" class="invalid-feedback"></error-message>
        </div>

        <div class="mb-3">
          <label for="username" class="form-label">收件人姓名</label>
          <Field
            id="username"
            name="姓名"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['姓名'] }"
            placeholder="請輸入收件人姓名"
            rules="required"
            v-model="user.username"
          ></Field>
          <error-message name="姓名" class="invalid-feedback"></error-message>
        </div>

        <div class="mb-3">
          <label for="phone" class="form-label">收件人電話</label>
          <Field
            id="phone"
            name="電話"
            type="tel"
            class="form-control"
            :class="{ 'is-invalid': errors['電話'] }"
            placeholder="請輸入收件人電話"
            :rules="isPhone"
            v-model="user.phone"
          ></Field>
          <error-message name="電話" class="invalid-feedback"></error-message>
        </div>

        <div class="mb-3">
          <label for="address" class="form-label">收件人地址</label>
          <Field
            id="address"
            name="地址"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['地址'] }"
            placeholder="請輸入收件人地址"
            rules="required"
            v-model="user.address"
          ></Field>
          <error-message name="地址" class="invalid-feedback"></error-message>
        </div>

        <div class="mb-3">
          <label for="message" class="form-label">留言</label>
          <textarea
            name=""
            id="message"
            class="form-control"
            cols="30"
            rows="10"
          ></textarea>
        </div>

        <div class="text-end">
          <button type="submit" class="btn btn-danger">送出訂單</button>
        </div>
      </Form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      cart: {},
      user: {
        email: '',
        username: '',
        phone: '',
        address: ''
      }
    };
  },
  methods: {
    getCart () {
      this.$http
        .get(`${process.env.VUE_APP_URL}api/${process.env.VUE_APP_PATH}/cart`)
        .then(res => {
          if (res.data.success) {
            this.cart = res.data.data;
          } else {
            alert(res.data.message);
          }
        })
        .catch(err => {
          console.log(err);
        });
    },
    updateCartProduct (item) {
      this.$http
        .put(
          `${process.env.VUE_APP_URL}api/${process.env.VUE_APP_PATH}/cart/${item.id}`,
          {
            data: {
              product_id: item.id,
              qty: item.qty
            }
          }
        )
        .then(res => {
          if (res.data.success) {
            console.log(res.data.message);
            this.getCart();
          } else {
            alert(res.data.message);
          }
        })
        .catch(err => {
          console.log(err);
        });
    },
    delCartProduct (id) {
      this.$http
        .delete(
          `${process.env.VUE_APP_URL}api/${process.env.VUE_APP_PATH}/cart/${id}`
        )
        .then(res => {
          if (res.data.success) {
            console.log(res.data.message);
            this.getCart();
          } else {
            alert(res.data.message);
          }
        })
        .catch(err => {
          console.log(err);
        });
    },
    delAllCartProduct () {
      this.$http
        .delete(
          `${process.env.VUE_APP_URL}api/${process.env.VUE_APP_PATH}/carts`
        )
        .then(res => {
          if (res.data.success) {
            console.log(res.data.message);
            this.getCart();
          } else {
            alert(res.data.message);
          }
        })
        .catch(err => {
          console.log(err);
        });
    },
    isPhone (value) {
      const phoneNumber = /^(09)[0-9]{8}$/;
      return phoneNumber.test(value) ? true : '需要正確的電話號碼';
    },
    onSubmit () {
      this.$http
        .post(
          `${process.env.VUE_APP_URL}api/${process.env.VUE_APP_PATH}/pay/:order_id`
        )
        .then(res => {
          if (res.data.success) {
            console.log(res.data.message);
            this.delAllCartProduct();
            this.getCart();
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
    this.getCart();
  }
};
</script>
