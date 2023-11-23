<script>
import CartService from '../services/Cart.service';
import { mapState, mapActions } from "pinia";
import { useAuthStore } from "@/stores/Auth.store";
import toast from '../assets/js/toasts';
export default {
  data() {
    return {
      carts: [],
      toasts: {
        title: "Warning",
        msg: "Bạn chưa đăng nhập",
        type: "warn",
        duration: 3000
      },
    }
  },
  computed: {
    ...mapState(useAuthStore, {
      currentUser: "user",
    }),
    getlengthcarts() {
      return this.carts.length;
    },
  },
  methods: {
    toast,
    ...mapActions(useAuthStore, ["logout", "loadAuthState"]),
    slideSearch: function () {
      this.$el.querySelector("#input_search").classList.toggle("input_search");
      this.$el.querySelector("#input_search").focus();
    },
    async showcarts() {
      try {
        this.showuser();
        if (this.currentUser != null) {
          this.carts = await CartService.get(this.currentUser._id);
        }
      } catch (error) {
        console.log(error);
      }
    },
    showuser() {
      if (this.currentUser == null) {
        document.querySelector('.login').style.display = "none";
        document.querySelector('.not-login').style.display = "block";
      } else {
        document.querySelector('.login').style.display = "block";
        document.querySelector('.not-login').style.display = "none";
        document.querySelector('.data_user').innerHTML = this.currentUser.username;
      }
    },
    handlelogout() {
      this.logout();
      this.$router.push({ name: "login" });
    },
    gotocart() {
      if (!this.currentUser) {
        this.toast();
      } else {
        this.$router.push({ name: "CartShop" });
      }
    }
  },
  created() {
    this.loadAuthState();
  },
  mounted() {
    this.showcarts();
  },
};
</script>
<template>
  <nav class="navbar navbar-expand-sm header" style="background-color:#ec4c2d">
    <div class="container-fluid">
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <router-link to="/" class="nav-link " aria-current="page">
              <img src="https://i.pngimg.me/thumb/f/720/m2H7H7G6i8H7b1A0.jpg" height="50">
            </router-link>
          </li>
        </ul>

        <div>
          <button style="background-color:#ec4c2d" class=" navbar-nav me-auto mb-2 mb-lg-0 btn " @click="gotocart"><i class="bi bi-bag-fill icon" style="color: rgb(255, 255, 255);"></i></button>
        </div>

        <div class="User nav-item">
          <div class="not-login">
            <i class="bi bi-person-circle icon" style="color: rgb(255, 255, 255);" data-bs-toggle="collapse" href="#user"></i>
            <div class="collapse user_link" id="user">
              <div class="card card-body bg-dark connect-shop">
                <router-link to="/login" class="text-white">Đăng nhập</router-link>
                <router-link to="/register" class="text-white">Đăng ký</router-link>
              </div>
            </div>
          </div>
          <div class="login">
            <span class="text-light data_user" data-bs-toggle="collapse" href="#user"></span>
            <div class="collapse user_link" id="user">
              <div class="card card-body bg-dark connect-shop">
                <router-link to="/profile" class="text-white">Trang cá nhân</router-link>
                <a to="/" class="text-white" @click="handlelogout()">Đăng xuất</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>
<style scoped>
.connect-shop {
  padding: 0;
}

.connect-shop a {
  padding: 10px 10px;
}



.user_link {
  width: 150px;
  position: absolute;
  right: 10px;
  z-index: 10;
}

.user_link a {
  display: block;
  text-decoration: none;
}


@media only screen and (max-width:1024px) {

  .User,
  .Cart {
    display: none;
  }

  @keyframes Search {
    0% {
      transform: translateX(5%);
    }

    100% {
      transform: translateX(0%);
    }
  }
}

</style>

