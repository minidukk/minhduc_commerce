<script>
    import * as yup from "yup";
    import { Form, Field, ErrorMessage } from "vee-validate";
    import { mapActions } from "pinia";
    import { useAuthStore } from "@/stores/Auth.store";
    import toast from "../assets/js/toasts";
    export default {
        components: {
            Form,
            Field,
            ErrorMessage,
        },
        data() {
        const Loginform = yup.object().shape({
            username: yup
            .string()
            .required("Tên phải có giá trị.")
            .min(2, "Tên phải ít nhất 2 ký tự."),
           password: yup
            .string()
            .required("Mật khẩu phải có giá trị.")
            });
            return {
            Loginform,
            toasts:{
                    title:"Warning",
                    msg:"Tên đăng nhập hoặc tài khoản không đúng!",
                    type:"warn",
                    duration:2000
                 },
            }
        },
        methods:{
            toast,
            showPwd(){
            if(document.querySelector("#checkpwd").checked == true){
                  document.querySelector("#pwd").type = 'text';
            }else{
                  document.querySelector("#pwd").type = 'password';
            }
          },
          	...mapActions(useAuthStore, ["login"]),
            async handleLogin(user) {
              try {
                await this.login(user);
                location.href='http://localhost:3001/';
              } catch (error) {
                this.toast();
                console.log(error);
              }
            },
        },
    };
</script>
<template>
        <Form :validation-schema="Loginform" @submit="handleLogin">
          <h1>Đăng nhập</h1>
          <div>
            <label class="form-label" for="username">Tài Khoản</label>
            <Field 
                id="name"
                placeholder="Nhập tài khoản của bạn...."
                name="username"
                type="text"
                class="form-control form-control-lg"
            />
            <ErrorMessage name="username" class="text-danger" />
          </div>

          <div>
            <label class="form-label" for="pwd">Mật Khẩu</label>
             <Field 
                id="pwd"
                placeholder="Nhập mật khẩu của bạn...."
                name="password"
                type="password"
                class="form-control form-control-lg"
            />
            <ErrorMessage name="password" class="text-danger"/>
          </div>
          <div>
            
            <div class="form-check mb-0">
              <input class="form-check-input me-2" type="checkbox" value="" id="checkpwd" @click="showPwd()" />
              <label class="form-check-label" for="checkpwd">
                Hiện mật khẩu
              </label>
            </div>
          </div>
          <div>
              <button class="btn"
                style="padding-left: 2.5rem; padding-right: 2.5rem;">Đăng Nhập</button>
            <p>Bạn chưa có tài khoản? 
              <router-link to="/register" class="link-danger">Đăng Ký</router-link>
            </p>
          </div>
        </Form>
</template>