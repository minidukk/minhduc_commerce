<script>
import HeaderShopVue from "../components/HeaderShop.vue";
import ListUser from "../components/ListOrder.vue";
import UserService from "../services/Order.service";
import toastsVue from "../components/toasts.vue";
import Productcard from "../components/Productcard.vue";
import Usercard from "../components/Usercard.vue";
import toast from "../assets/js/toasts";
export default {
    data() {
        return {
            users: [],
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
        getindexuser() {
            if (this.activeUser != -1) {
                const list = document.querySelectorAll(".order-item");
                list.forEach(element => {
                    element.classList.remove("active");
                });
                list[this.activeUser].classList.add("active");
                return this.users[this.activeUser];
            }
        },
    },
    components: {
        HeaderShopVue,
        ListUser,
        toastsVue,
        Productcard,
        Usercard
    },
    methods: {
        toast,
        async getall() {
            try {
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
                <div class="card_product border border-light border-2 h-100 bg-light text-dark" style="padding: 10px;">
                </div>
            </div>
        </div>
    </div>

</template>
<style scoped>
.list_item_product {
    max-height: 500px;
    overflow-y: scroll;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
}
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