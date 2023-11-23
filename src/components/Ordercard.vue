<script>
export default {
    data() {
        return {
            products: [],
        }
    },
    props: {
        refeshlistcart: Function,
        carts: Array,
    },
    emit: [
        "deleted:cartIndex"
    ],
    methods: {
        deletedcart(index) {
            this.$emit("deleted:cartIndex", index);
        },
    },
}
</script>
<template>
    <div class="row align-items-center mt" v-for="(item, index) in carts">
        <div class="col">
            <img :src="item.img" width="150" alt="bruh">
        </div>
        <div class="col">
            <h6 class="text-muted">{{ item.title }}</h6>
            <span class="text-muted">Giá: <span>{{ item.price }}</span></span>

        </div>
        <div class="col">
            <span class="text-muted">Số lượng: <span>{{ item.quantity }}</span></span>
        </div>
        <div class="col">
            <h6 class="mb-0 price">{{ item.price * item.quantity }}</h6>
        </div>
        <div class="col text-end">
            <button class="btn btn-outline-danger btn-sm" @click="deletedcart(index)">X</button>
        </div>
    </div>
</template>