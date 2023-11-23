<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";
import AuthService from "../services/Auth.service";
import toast from "../assets/js/toasts";
import toastsVue from "../components/toasts.vue";
export default {
    components: {
        Form,
        Field,
        ErrorMessage,
        toastsVue
    },
    data() {
        const Regform = yup.object().shape({
            name: yup
                .string()
                .required("Tên phải có giá trị.")
                .min(2, "Tên phải ít nhất 2 ký tự."),
            email: yup
                .string()
                .required("Email phải có giá trị.")
                .email("E-mail không đúng.")
                .max(50, "E-mail tối đa 50 ký tự."),
            pwd: yup
                .string()
                .required("Mật khẩu phải có giá trị.")
        });
        return {
            Regform,
            message: "Đăng ký thành công",
            usernew: {
                username: "",
                email: "",
                password: "",
            },
            toasts: {
                title: "",
                msg: "",
                type: "",
                duration: 0
            },
        };
    },
    methods: {
        toast,
        async postuser() {
            try {
                await AuthService.createsignup(this.usernew);
                this.toasts.title = "Success",
                    this.toasts.msg = "Đăng ký thành công",
                    this.toasts.type = "success",
                    this.toasts.duration = 2000
                this.toast();
                setTimeout(() => {
                    this.$router.push({ name: "login" });
                }, 2000);
            } catch (erorr) {
                console.log(erorr);
                this.toasts.title = "Faild",
                    this.toasts.msg = "Thông tin bạn nhập đã được dùng",
                    this.toasts.type = "error",
                    this.toasts.duration = 2000
                this.toast();
            }
        }
    }
};
</script>

<template>
    <toastsVue></toastsVue>
    <section>
        <h1>Tạo Tài Khoản Mới</h1>
        <Form :validation-schema="Regform">
            <div>
                <label class="form-label" for="name">Tên Tài Khoản:</label>
                <Field id="name" name="name" type="text" class="form-control" v-model="usernew.username" />
                <ErrorMessage name="name" class="text-danger" />
            </div>
            <div>
                <label class="form-label" for="email">Email:</label>
                <div class="form-outline">
                    <Field id="email" name="email" type="email" class="form-control" v-model="usernew.email" />
                    <ErrorMessage name="email" class="text-danger" />
                </div>
            </div>
            <div>
                <label for="pwd">Mật Khẩu:</label>
                <Field id="pwd" name="pwd" type="password" class="form-control" v-model="usernew.password" />
                <ErrorMessage name="pwd" class="text-danger" />
            </div>
            <div>
                <input type="checkbox" value="" id="form2Example33" checked />
                <label for="form2Example33">
                    Đồng ý với các điều khoản
                </label>
            </div>

            <button type="button" @click="postuser()">
                Đăng Ký
            </button>
            <p>Nếu bạn đã có tài khoản hãy =><router-link to="/login"> Đăng nhập</router-link></p>
        </Form>
        <h6 class="mb-0"><router-link to="/" class="text-body">Trở về</router-link></h6>
    </section>
</template>
