<script>
import SliderShop from '@/components/SliderShop.vue'
import HeaderShop from '@/components/HeaderShop.vue'
import ProductService from '../services/Product.service'
import { mapState } from "pinia";
import { useAuthStore } from "@/stores/Auth.store";
import toastsVue from '../components/toasts.vue';
import InputSearch from '../components/InputSearch.vue';
import FooterShop from '../components/FooterShop.vue';
export default {
    data() {
        return {
            Products: [],
        }
    },
    components: {
        HeaderShop,
        SliderShop,
        FooterShop,
        InputSearch,
        toastsVue
    },
    computed: {
        ...mapState(useAuthStore, {
            currentUser: "user",
        }),
    },
    methods: {
        async retrieveProduct() {
            try {
                this.Products = await ProductService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
    },
    mounted() {
        this.retrieveProduct();
    },
}
</script>
<template>
    <div class="header">
        <HeaderShop></HeaderShop>
    </div>
    <toastsVue></toastsVue>
    <div class="slider">
        <SliderShop></SliderShop>
    </div>
    <div>
        <div style="text-align: center; margin-top:30px">
            <h3>Đồng hồ luxury</h3>
            <h6>những chiếc đồng hồ sang trọng nhất</h6>
        </div>
        <div style="margin-top:20px">
            <div class="d-sm-flex flex-wrap" id="x">
                <div class="card m-1" style="width: 18rem;" v-for="item in Products">
                    <div>
                        <div>
                            <div class="image_item" v-for="img in item.img">
                                <img :src="img" class="card-img-top" alt="bruh">
                            </div>
                        </div>
                    </div>
                    <div class="card-body product">
                        <h5 class="card-title">{{ item.title }}</h5>
                        <h6 class="price text-primary">{{ item.price }} VNĐ</h6>
                        <router-link :to="{
                            name: 'details',
                            params: { id: item._id },
                        }">
                            <button type="button" class="btn btn-outline-dark">Xem</button>
                        </router-link>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div>
    </div>
    <div class="footer">
        <footer>
        <FooterShop></FooterShop>
    </footer>
    </div>
    
        
    
</template>
<style scoped>   .image_slider:hover {
       transform: translateX(-100%);
   }

   .image_item {
       flex: 1 0 100%;
   }

</style>


