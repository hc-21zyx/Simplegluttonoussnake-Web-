<template>
  <ContentField v-if="show_content" style="background-color: aqua">
    <div class="row justify-content-center" style="background-color: azure">
      <div class="col-3">
        <form @submit.prevent="login">
          <div class="mb-3">
            <label for="username" class="form-label">用户名</label>
            <input
              v-model="username"
              type="text"
              class="form-control"
              id="username"
              placeholder="请输入用户名"
            />
          </div>
          <div class="mb-3">
            <label for="password" class="form-label">密码</label>
            <input
              v-model="password"
              type="password"
              class="form-control"
              id="password"
              placeholder="请输入密码"
            />
          </div>
          <div class="error_message">{{ error_message }}</div>
          <button type="submit" class="btn btn-primary">提交</button>
        </form>
      </div>
    </div>
  </ContentField>
</template>

<script>
import ContentField from "../../../components/ContentField.vue";
import { useStore } from "vuex";
import { ref } from "vue";
import router from "../../../router/index";
//import $ from "jquery";

export default {
  components: {
    ContentField,
  },
  setup() {
    const store = useStore();
    let username = ref("");
    let password = ref("");
    let error_message = ref("");
    let show_content = ref(false);

    const jwt_token = localStorage.getItem("jwt_token");
    console.log(jwt_token);

    if (jwt_token) {
      store.commit("updateToken", jwt_token);
      store.dispatch("getinfo", {
        success() {
          router.push({ name: "home" });
        },
        error() {
          show_content.value = true;
        },
      });
    } else {
      show_content.value = true;
    }

    const login = () => {
      error_message.value = "";
      store.dispatch("login", {
        username: username.value,
        password: password.value,
        success() {
          store.dispatch("getinfo", {
            success(resp) {
              router.push({ name: "home" });
              console.log(resp);
            },
          });
          router.push({ name: "home" });
        },
        error() {
          error_message.value = "用户名或密码错误";
        },
      });
    };

    return {
      username,
      password,
      error_message,
      login,
      show_content,
    };
  },
};
</script>

<style scoped>
button {
  width: 100%;
}
div.error_message {
  color: red;
}
</style>
