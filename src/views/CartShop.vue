<script>
import HeaderShop from '@/components/HeaderShop.vue';
import CartService from '../services/Cart.service';
import toastsVue from '../components/toasts.vue';
import toastsjs from '../assets/js/toasts.js'
import CartItem from "../components/CartItem.vue";
import OrderService from '../services/Order.service';
import { mapState } from 'pinia'
import { useAuthStore } from "@/stores/Auth.store";
export default {
  data() {
    return {
      carts: [],
      orderItem: {
        userId: '',
      },
      toasts: {
        title: "",
        msg: "",
        type: "",
        duration: 0
      },
    }
  },
  components: {
    HeaderShop,
    toastsVue,
    CartItem
  },
  computed: {
    ...mapState(useAuthStore, {
      currentUser: "user",
    }),
  },
  methods: {
    getiduser() {
      const user = JSON.parse(localStorage.getItem("user"));
      return user._id;
    },
    async getcarts() {
      try {
        this.carts = await CartService.get(this.getiduser());
      } catch (error) {
        console.log(error);
      }
    },
    async delcart(index) {
      this.toasts.title = "Deleted",
        this.toasts.msg = "Đã xóa sản phẩm",
        this.toasts.type = "error",
        this.toasts.duration = 2000
      this.toastsjs();
      await CartService.delete(this.carts[index]._id)
      this.refeshlistcart();
    },
    toastsjs,
    refeshlistcart() {
      this.getcarts();
    },
    async registerproduct() {
      this.orderItem.userId = this.currentUser._id
      await OrderService.create(this.orderItem);
      if (this.carts.length > 0) {
        this.toasts.title = "Success",
          this.toasts.msg = "Đã đặt hàng",
          this.toasts.type = "success",
          this.toasts.duration = 2000,
          this.toastsjs();
      } else {
        this.toasts.title = "Failed",
          this.toasts.msg = "Bạn chưa có sản phẩm",
          this.toasts.type = "error",
          this.toasts.duration = 2000,
          this.toastsjs();
      }
    },
    total() {
      var total = 0;
      for (var i in this.carts) {
        total += (this.carts[i].price * this.carts[i].quantity);
      }
      return total;
    }
  },
  created() {
    this.refeshlistcart();
  },
}
</script>
<template>
  <HeaderShop></HeaderShop>
  <toastsVue></toastsVue>

  <h1>Giỏ hàng</h1>
  <CartItem :refeshlistcart="refeshlistcart" :carts="carts" @deleted:cartIndex="delcart"></CartItem>

  <div>
    <div>
      <div class="d-flex justify-content-between mt-5">
        <h5>Tổng giá:</h5>
        <h5>{{ total() }}<span> VNĐ</span></h5>
      </div>

      <button type="button" class="btn-lg text-light" style="background-color: #ec4c2d; "
        @click="registerproduct()">Đặt hàng</button>
      <div class="pt-5">
        <h6 class="mb-0"><router-link to="/" class="text-body">Trở về</router-link></h6>
      </div>
    </div>
  </div>
</template>