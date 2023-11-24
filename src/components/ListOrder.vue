<script>
import OrderService from "../services/Order.service";
import toastjs from "../assets/js/toasts";
export default {
    data() {
        return {
            toasts: {
                title: "",
                msg: "",
                type: "",
                duration: 0
            },
        }
    },
    props: {
        orders: Array,
        refeshlist: Function,
        activeUser: { type: Number, default: -1 },
    },
    emits: ['update:activeUser'],
    methods: {
        toastjs,
        async deleteuser(id) {
            try {
                await OrderService.delete(id);
                this.refeshlist();
                this.toasts.title = "Success",
                    this.toasts.msg = "Đã xóa order",
                    this.toasts.type = "success",
                    this.toasts.duration = 2000
                this.toastjs();
            } catch (error) {
                console.log(error);
                this.toasts.title = "Warning",
                    this.toasts.msg = "Tài khoản không phải ADMIN",
                    this.toasts.type = "warn",
                    this.toasts.duration = 2000
                this.toastjs();
            }
        },
        emitOrder(userId) {
            this.$emit('selectedOrder', userId);
        }
    }
}

</script>
<template>
    <ul class="list-group">
        <li class="list-group-item order-item d-flex justify-content-between" v-for="user in orders"
            v-show="!user.isAdmin" :key="user._id" v-on:click="emitOrder(user.userId)">
            <span>{{ user.userId }}</span>
            <button class="btn btn-outline-danger btn-sm" @click="deleteuser(user._id)">X</button>
            
        </li>
    </ul>
</template>
<style scoped>
.list-group-item:hover {
    background-color: #0d6efd;
    color: azure;
}
</style>