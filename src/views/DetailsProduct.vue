<script>
import HeaderShop from '@/components/HeaderShop.vue'
import ProductService from '../services/Product.service'
import CartService from '../services/Cart.service'
import toastsVue from '../components/toasts.vue'
import toastsjs from '../assets/js/toasts.js'
import { mapState } from 'pinia'
import { useAuthStore } from "@/stores/Auth.store";
export default {
    data() {
        return {
            detailproduct: [],
            cartitem: {
                userId: '',
                productId: this.$route.params.id,
                quantity: 1,
                title: "",
                img: "",
                price: "",
            },
            carts: [],
            toasts: {
                title: "Success",
                msg: "Thêm vào danh sách thành công",
                type: "success",
                duration: 2000
            },
            sub_quantity: 1,
        }
    },
    computed: {
        ...mapState(useAuthStore, {
            currentUser: "user",
        }),
    },
    components: {
        HeaderShop,
        toastsVue
    },
    methods: {
        toastsjs,
        async getproduct() {
            try {
                this.detailproduct = await ProductService.get(this.$route.params.id);
                this.cartitem.title = this.detailproduct.title;
                this.cartitem.img = this.detailproduct.img[0];
                this.cartitem.price = this.detailproduct.price;
                this.cartitem.size = this.detailproduct.size;
                this.cartitem.color = this.detailproduct.color;
            } catch (error) {
                console.log(error);
            }
        },
        async getidcart() {
            this.cartitem.quantity = this.sub_quantity;
            var existcart = false;

            try {
                this.carts = await CartService.get(this.currentUser._id);
                this.cartitem.userId = this.currentUser._id;
                this.carts.map((cartproduct) => {
                    if (cartproduct.productId == this.cartitem.productId) {
                        this.cartitem.quantity += cartproduct.quantity;
                        CartService.update(cartproduct._id, this.cartitem);
                        exitcart = true;
                        this.toastsjs();
                    }
                })
                if (existcart === false) {
                    this.cartitem.userId = this.currentUser._id;
                    CartService.create(this.cartitem);
                    this.toastsjs();
                }
            } catch (error) {
                this.toasts.title = "Message",
                    this.toasts.msg = "Bạn chưa đăng nhập!",
                    this.toasts.type = "warn",
                    this.toasts.duration = 3000,
                    this.toastsjs();
                console.log(error);
            }
        },
    },
    created() {
        this.getproduct();
    },

}
</script>
<template>
    <HeaderShop></HeaderShop>
    <toastsVue></toastsVue>
    <div class="heading">
        <div class="title" style="background-color: #e9ecef;">
            <h2 class="text-dark text-center" style="padding-left: 20px;">Chi tiết sản phẩm</h2>
        </div>
    </div>
    <div class="wrapper">
        <div class="details d-flex justify-content-between">
            <div class="img_product col">
                <div id="carouselExampleControls" class="carousel slide" data-bs-ride="carousel">
                    <div class="carousel-inner">
                        <div class="carousel-item" v-for="(img, index) in detailproduct.img" :class="{ active: index == 0 }">
                            <img :src="img" class="d-block w-100" alt="..." style="border-radius: 8px ;">
                        </div>
                    </div>
                </div>
            </div>
            <div class="info_product col" style="margin-left: 20px;">
                <h5 style="font-weight: 400;">{{ detailproduct.title }}</h5>
                <span class="pricte text-danger">{{ detailproduct.price }} VNĐ</span>
                <div class="quatitly-product">
                    <h5>Số lượng</h5>
                    <div class="col-md-3 col-lg-3 col-xl-2 d-flex">
                        <input id="quantity" name="quantity" type="number" v-model="sub_quantity"
                            class="form-control form-control-sm" style="width:50px; border: none;" />
                    </div>
                </div>
                <div class="btn_product">
                    <button type="submit" class="btn btn-danger" style="width:150px;" @click="getidcart()">THÊM</button>
                    <router-link to="/">
                        <button type="button" class="btn btn-danger" style="width:150px;">Trở Về</button>
                    </router-link>
                </div>
            </div>
            <div class="describe_product col">
                <h4>Description</h4>
                <p>{{ detailproduct.desc }}
                </p>
            </div>
        </div>
    </div>
</template>
<style scoped>
.btn_product {
    display: flex;
    justify-content: space-around;
}

.size_product,
.color_product {
    display: flex;
    flex-direction: column;
    max-width: 400px;
}

.list_btn_size,
.list_btn_color {
    margin: 0 10px;
    display: flex;
    flex-wrap: wrap;
}

.list_btn_size button,
.list_btn_color button {
    margin: 10px 5px;
}

.btn {
    width: 100px;
}

.wrapper {
    margin: 50px 100px;
}

.heading {
    margin: 0 100px;
}

.title {
    display: flex;
    justify-content: center;
    flex-direction: column;
    width: 100%;
    height: 100px;
}</style>