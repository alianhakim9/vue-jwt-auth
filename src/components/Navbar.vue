<template>
  <nav class="navbar navbar-expand-md navbar-dark bg-dark mb-4">
    <div class="container-fluid">
      <router-link :to="{ name: 'home' }" class="navbar-brand"
        >Home</router-link
      >

      <div>
        <ul class="navbar-nav me-auto mb-2 mb-md-0" v-if="!auth">
          <li class="nav-item">
            <router-link :to="{ name: 'login' }" class="nav-link"
              >Login</router-link
            >
          </li>
          <li class="nav-item">
            <router-link :to="{ name: 'register' }" class="nav-link"
              >Register</router-link
            >
          </li>
        </ul>

        <ul class="navbar-nav me-auto mb-2 mb-md-0" v-if="auth">
          <li class="nav-item">
            <a href="#" class="nav-link" @click="logout">Logout</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script lang="ts">
import { computed } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
export default {
  name: "Nav",
  setup() {
    const store = useStore();
    const auth = computed(() => store.state.authenticated);
    const router = useRouter();

    const logout = async () => {
      await fetch("http://localhost:8000/api/auth/user/logout", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        credentials: "include",
      });

      await store.dispatch("setAuth", false);
      await router.replace({ name: "login" });
    };
    return {
      auth,
      logout,
    };
  },
};
</script>