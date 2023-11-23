<script>
    import Formlogin from "../components/Formlogin.vue";
    import toastsVue from "../components/toasts.vue";
    export default {
        components: {
            Formlogin,
            toastsVue
        },
    };

</script>
<template>
<toastsVue></toastsVue>
<Formlogin></Formlogin>
<h6 class="mb-0"><router-link to="/" class="text-body">Trở về</router-link></h6>

</template>
