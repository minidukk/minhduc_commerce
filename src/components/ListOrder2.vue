<script>
import HeaderShopVue from "../components/HeaderShop.vue";
import ListUser from "../components/ListUser.vue";
import UserService from "../services/User.service";
import toastsVue from "../components/toasts.vue";
import ProductService from "../services/Product.service";
import ListProduct from "../components/ListProduct.vue";
import Productcard from "../components/Productcard.vue";
import OrderService from "../services/Order.service";
import ListOrder from "../components/ListOrder.vue";
import Usercard from "../components/Usercard.vue";
import Ordercard from "../components/Ordercard.vue";
import toast from "../assets/js/toasts";
export default {
    data() {
        return {
            users: [],
            products: [],
            orders: [],
            activeIndex: -1,
            activeUser: -1,
            activeOrder: -1,
            toasts: {
                title: "Warning",
                msg: "Bạn không phải ADMIN",
                type: "warn",
                duration: 3000
            },
        }
    },
    computed: {
        getindex() {
            if (this.activeIndex != -1) {
                const list = document.querySelectorAll(".product-item");
                list.forEach(element => {
                    element.classList.remove("active");
                });
                list[this.activeIndex].classList.add("active");
                return this.products[this.activeIndex];
            }
        },
        getindexuser() {
            if (this.activeUser != -1) {
                const list = document.querySelectorAll(".user-item");
                list.forEach(element => {
                    element.classList.remove("active");
                });
                list[this.activeUser].classList.add("active");
                return this.users[this.activeUser];
            }
        },
        getindexorder() {
            if (this.activeOrder != -1) {
                const list = document.querySelectorAll(".order-item");
                list.forEach(element => {
                    element.classList.remove("active");
                });
                list[this.activeOrder].classList.add("active");
                return this.users[this.activeOrder];
            }
        }
    },
    components: {
        HeaderShopVue,
        ListUser,
        ListProduct,
        ListOrder,
        toastsVue,
        Productcard,
        Usercard,
        Ordercard
    },
    methods: {
        toast,
        async getall() {
            try {
                this.products = await ProductService.getAll();
                this.users = await UserService.getAll();
                this.orders = await OrderService.getAll();
            } catch (error) {
                console.log(error);
                this.toast();
                setTimeout(() => {
                    this.$router.push({ name: "ShopMain" });
                }, 1000);
            }
        },
        emitOrderId(orderId) {
        this.$emit('selectedOrder', orderId);
  }
    },
    created() {
        this.getall();
    },
}
</script>

<template>
    <div style="margin-top:30px; margin-left: 100px;">
        <div class="list_users w-50 ">
            <div class="user_heading">
                <h4>Danh Sách Order</h4>
            </div>
            <div class="list_item_user d-flex">
                <ListOrder :orders="orders" :refeshlist="getall" v-model:activeOrder="activeOrder"></ListOrder>
                <div class="card_product border border-light border-2 h-100 bg-light text-dark" style="padding: 10px;"
                    v-if="getindexorder">
                </div>
                
            </div>
        </div>
    </div>
</template>
<style scoped>


#product::-webkit-scrollbar,
#user::-webkit-scrollbar {
    width: 6px;
    background-color: #F5F5F5;
}</style>