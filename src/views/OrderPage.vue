<script>
import HeaderShop from '@/components/HeaderShop.vue';
import CartService from '../services/Cart.service';
import toastsVue from '../components/toasts.vue';
import toastsjs from '../assets/js/toasts.js'
import CartItem from "../components/CartItem.vue";
import ListOrder2 from "../components/ListOrder2.vue";
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
        ListOrder2,
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
            const userId = this.userInput;
            if (userId) {
                try {
                    this.carts = await CartService.get(userId);
                } catch (error) {
                    console.log(error);
                }
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
    <ListOrder2></ListOrder2>
    <input type="text" v-model="userInput" placeholder="Paste user ID here" style="margin-left: 100px; margin-top: 20px; font-size: large;">
    <button @click="getcarts()">Fetch Order</button>

    <h1>Order</h1>
    <CartItem :refeshlistcart="refeshlistcart" :carts="carts" @deleted:cartIndex="delcart"></CartItem>



    <h6 class="mb-0"><router-link to="/" class="text-body">Trở về</router-link></h6>
</template>