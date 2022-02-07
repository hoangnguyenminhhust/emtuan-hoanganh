<template>
  <v-main style="background-color: #FFFFFF;">
    <v-container style="height: 100px">
      <div
        class="d-flex justify-space-between"
      >
        <div class="drawer">
          <v-app-bar-nav-icon
            @click="drawer = !drawer"
          />
        </div>
        <div class="wrapper">
          <div class="logo mt-2">
            <a href="/">
              <img src="../../assets/images/logo/logo-black.png">
            </a>
          </div>
        </div>
        <div class="wrapper menu mt-4">
          <ul class="nav-list">
            <li><a href="/">Trang chủ</a></li>
            <li>
              <div class="text-center">
                <v-menu
                  offset-y
                  open-on-hover
                >
                  <template v-slot:activator="{ on, attrs }">
                    <!-- <v-btn
                      color="white"
                      elevation="0"
                      v-bind="attrs"
                      v-on="on"
                    >
                      Sản phẩm
                    </v-btn> -->
                    <li>
                      <a
                        v-bind="attrs"
                        v-on="on"
                      >Sản phẩm</a>
                    </li>
                  </template>
                  <v-list>
                    <v-list-item
                      v-for="(item, index) in categories"
                      :key="index"
                      link
                      @click="routeCategory(item._id)"
                    >
                      <v-list-item-title v-text="item.name" />
                    </v-list-item>
                  </v-list>
                </v-menu>
              </div>
            </li>
            <li><a @click="$router.push({ path: `/order-history/${orderUser}`})">Hoá đơn</a></li>
          </ul>
        </div>
        <div class="d-flex align-center">
          <div class="d-flex">
            <div v-if="user">
              <v-tooltip bottom>
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    text
                    v-bind="attrs"
                    class="header_icon"
                    v-on="on"
                    @click="profileDialog = true"
                  >
                    <v-icon>
                      mdi-account
                    </v-icon>
                  </v-btn>
                </template>
                <span>Profile</span>
              </v-tooltip>
            </div>
            <div v-if="!user">
              <v-tooltip bottom>
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    text
                    v-bind="attrs"
                    class="header_icon"
                    v-on="on"
                    @click="loginDialog = true"
                  >
                    <v-icon>
                      mdi-account
                    </v-icon>
                  </v-btn>
                </template>
                <span>Đăng nhập</span>
              </v-tooltip>
            </div>
            <div v-if="!user">
              <v-tooltip bottom>
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    text
                    v-bind="attrs"
                    class="header_icon"
                    v-on="on"
                    @click="registerDialog = true"
                  >
                    <v-icon>
                      mdi-account-plus
                    </v-icon>
                  </v-btn>
                </template>
                <span>Đăng kí</span>
              </v-tooltip>
            </div>
            <div>
              <v-tooltip bottom>
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    text
                    v-bind="attrs"
                    class="header_icon"
                    @click="$router.push({ path: '/cart' })"
                    v-on="on"
                  >
                    <v-badge
                      bordered
                      color="orange"
                      :content="cartNumber"
                    >
                      <v-icon>
                        mdi-cart
                      </v-icon>
                    </v-badge>
                  </v-btn>
                </template>
                <span>Giỏ hàng</span>
              </v-tooltip>
            </div>
            <div v-if="user">
              <v-menu
                offset-y
                left
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    text
                    v-bind="attrs"
                    class="header_icon"
                    v-on="on"
                  >
                    <v-icon>
                      mdi-chevron-down
                    </v-icon>
                  </v-btn>
                </template>
                <v-list>
                  <v-list-item>
                    <v-list-item-title>
                      <v-btn
                        text
                        @click="changePassDialog = true"
                      >
                        Đổi mật khẩu
                      </v-btn>
                    </v-list-item-title>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-title>
                      <v-btn
                        text
                        width="100%"
                        @click="logout()"
                      >
                        Đăng xuất
                      </v-btn>
                    </v-list-item-title>
                  </v-list-item>
                </v-list>
              </v-menu>
            </div>
            <!-- <div>
              <v-menu
                open-on-hover
                bottom
                offset-y
                :disabled="menuStatus"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    text
                    v-bind="attrs"
                    class="header_dropdown"
                    v-on="on"
                  >
                    <v-icon>
                      mdi-chevron-down
                    </v-icon>
                  </v-btn>
                </template>
                <v-list>
                  <v-list-item>
                    <v-list-item-title>
                      <v-btn
                        color="white"
                        elevation="0"
                        width="150px"
                      >
                        Trang chủ
                      </v-btn>
                    </v-list-item-title>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-title>
                      <v-btn
                        color="white"
                        elevation="0"
                        width="150px"
                      >
                        Sản phẩm
                      </v-btn>
                    </v-list-item-title>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-title>
                      <v-btn
                        color="white"
                        elevation="0"
                        width="150px"
                      >
                        Shop
                      </v-btn>
                    </v-list-item-title>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-title>
                      <v-btn
                        color="white"
                        elevation="0"
                        width="150px"
                      >
                        Page
                      </v-btn>
                    </v-list-item-title>
                  </v-list-item>
                </v-list>
              </v-menu>
            </div> -->
          </div>
        </div>
      </div>
      <v-navigation-drawer
        v-model="drawer"
        fixed
        temporary
        class="navigation"
      >
        <v-list>
          <v-list-item>
            <v-list-item-title>
              <a href="/">Trang chủ</a>
            </v-list-item-title>
          </v-list-item>
          <v-list-group :value="false">
            <template v-slot:activator>
              <v-list-item-title>
                <a>Sản phẩm</a>
              </v-list-item-title>
            </template>
            <v-list-item
              v-for="category in categories"
              :key="category._id"
            >
              <v-list-item-title>
                <a
                  :href="`/category/${category._id}`"
                  style="padding: 20px"
                >{{ category.name }}</a>
              </v-list-item-title>
            </v-list-item>
          </v-list-group>
          <v-list-item>
            <v-list-item-title>
              <a @click="$router.push({ path: `/order-history/${orderUser}`})">Hoá đơn</a>
            </v-list-item-title>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
      <login-modal
        :login-dialog="loginDialog"
        :login-dialog-status="loginModalStatus"
      />
      <register-modal
        :register-dialog="registerDialog"
        :register-dialog-status="registerModalStatus"
      />
      <profile-modal
        :profile-dialog="profileDialog"
        :profile-dialog-status="profileModalStatus"
      />
      <change-pass-modal
        :change-pass-dialog="changePassDialog"
        :change-pass-dialog-status="changePassModalStatus"
      />
    </v-container>
  </v-main>
</template>

<script>
import loginModal from './loginModal';
import registerModal from './registerModal';
import profileModal from './profileModal';
import changePassModal from './changePassModal';
export default {
    name: 'Header',
    components: {
      loginModal,
      registerModal,
      profileModal,
      changePassModal
    },
    data: () => ({
        drawer: false,
        w: window.innerWidth,
        menuStatus: false,
        loginDialog: false,
        registerDialog: false,
        profileDialog: false,
        changePassDialog: false,
        menu: ['Trang chủ', 'Sản phẩm'],
    }),
    computed: {
      cartNumber() {
        return (this.$store.state.cart.cartCount).toString();
      },
      categories() {
        return this.$store.state.category.categories;
      },
      user() {
        return this.$store.state.user.user;
      },
      orderUser() {
        return this.$store.state.user.deviceId;
      }
    },
    watch: {
        w: function(val) {
            if (val > 968) {
                this.drawer = false
                this.menuStatus = true
            } else {
                this.menuStatus = false
            }
        }
    },
    mounted() {
        window.onresize = () => {
            this.w = window.innerWidth
        }
    },
    async created() {
      await this.$store.dispatch('category/getAllCategory');
    },
    methods: {
      loginModalStatus(value) {
        this.loginDialog = value;
      },
      registerModalStatus(value) {
        this.registerDialog = value;
      },
      profileModalStatus(value) {
        this.profileDialog = value;
      },
      changePassModalStatus(value) {
        this.changePassDialog = value;
      },
      async routeCategory(id) {
        this.$router.push({ path: `/category/${id}`});
        await this.$store.dispatch('product/getProductByCategory', { categoryId: id });
      },
      logout() {
        const result = confirm('Bạn có chắc chắn muốn đăng xuất?');
        if (result) this.$store.dispatch('user/logout');
      },
    }
}
</script>

<style lang="scss">
    .logo {
        display: block;
    }
    .header {
        position: relative;
        z-index: 6;
    }
    .drawer {
        display: none !important;
    }
    .header_dropdown {
        display: none !important;
    }

    @media only screen and (max-width: 968px){
        .drawer {
            display: inline-flex !important;
            align-items: center;
        }
        .menu {
            display: none !important;
        }
        .icons {
            margin: 0px;
            float: right;
        }
        .header_dropdown {
            display: initial !important;
        }

        .logo {
          margin: auto;
          width: 50%;
        }


    }
</style>