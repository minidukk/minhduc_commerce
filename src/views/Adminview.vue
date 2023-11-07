<script>
import HeaderShopVue from "../components/HeaderShop.vue";
import ListUser from "../components/ListUser.vue";
import UserService from "../services/User.service";
import toastsVue from "../components/toasts.vue";
import ProductService from "../services/Product.service";
import ListProduct from "../components/ListProduct.vue";
import Productcard from "../components/Productcard.vue";
import Usercard from "../components/Usercard.vue";
import toast from "../assets/js/toasts";
export default {
    data() {
        return {
            users: [],
            products: [],
            activeIndex: -1,
            activeUser: -1,
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
        }
    },
    components: {
        HeaderShopVue,
        ListUser,
        ListProduct,
        toastsVue,
        Productcard,
        Usercard
    },
    methods: {
        toast,
        async getall() {
            try {
                this.products = await ProductService.getAll();
                this.users = await UserService.getAll();
            } catch (error) {
                console.log(error);
                this.toast();
                setTimeout(() => {
                    this.$router.push({ name: "NovelMain" });
                }, 1000);
            }
        },
    },
    created() {
        this.getall();
    },
}
</script>

<template>
    <HeaderShopVue></HeaderShopVue>
    <toastsVue></toastsVue>
    <h2>Quản trị viên</h2>
    <div style="margin-top:30px; margin-left: 100px;">
        <div class="list_users w-50 ">
            <div class="user_heading">
                <h4>Danh Sách Người Dùng</h4>
            </div>
            <div class="list_item_user d-flex" id="user">
                <ListUser :users="users" :refeshlist="getall" v-model:activeUser="activeUser"></ListUser>
                <div class="card_product border border-light border-2 h-100 bg-light text-dark" style="padding: 10px;"
                    v-if="getindexuser">
                    <h5>Chi tiết</h5>
                    <Usercard :users="getindexuser"></Usercard>
                </div>
            </div>
        </div>
    </div>
    <div style="margin-top:30px; margin-left: 100px;">
        <div class="list_products">
            <div class="product_heading">
                <h4>Danh Sách Truyện</h4>
            </div>
            <div class="list_item_product d-flex" id="product">
                <ListProduct :products="products" :refeshlist="getall" :getindex="getindex"
                    v-model:activeIndex="activeIndex"></ListProduct>
                <div class="card_product border border-light border-2 h-100 bg-light text-dark" style="padding: 10px;"
                    v-if="getindex">
                    <h5>Chi tiết truyện</h5>
                    <Productcard :products="getindex"></Productcard>
                    <router-link :to="{
                        name: 'editproduct',
                        params: { id: getindex._id },
                    }">
                        <span class="badge bg-warning text-dark">
                            <i class="bi bi-pencil-square"></i> Hiệu chỉnh</span>
                    </router-link>
                </div>
            </div>
            <router-link to="/addproduct">
                <button class="btn btn-danger">Thêm truyện</button>
            </router-link>
        </div>
    </div>
</template>
<style scoped>
.list_item_product,
.list_item_user {
    max-height: 500px;
    overflow-y: scroll;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
}

#product::-webkit-scrollbar,
#user::-webkit-scrollbar {
    width: 6px;
    background-color: #F5F5F5;
}</style>