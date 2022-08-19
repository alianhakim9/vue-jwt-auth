<template>
  <form @submit.prevent="submit">
    <h1 class="h3 mb-3 fw-normal">Please sign in</h1>

    <input
      v-model="data.email"
      type="email"
      class="form-control"
      placeholder="Email"
      required
    />

    <input
      v-model="data.password"
      type="password"
      class="form-control"
      placeholder="Password"
      required
    />
    <button
      class="w-100 btn btn-lg btn-primary"
      type="submit"
      :disabled="disableBtn"
    >
      Sign in
    </button>
    <p class="mt-4" v-if="error">{{ error }}</p>
  </form>
</template>

<script lang="ts">
import { onMounted, reactive, ref, watchEffect } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
export default {
  name: "Login",
  setup() {
    const data = reactive({
      email: "",
      password: "",
    });
    const router = useRouter();
    const error = ref("");
    const disableBtn = ref(true);
    const store = useStore();

    onMounted(() => {
      if (store.state.authenticated) {
        router.replace({ name: "home" });
      }
    });

    const submit = async () => {
      try {
        const response = await fetch("http://localhost:8000/api/auth/login", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          credentials: "include",
          body: JSON.stringify(data),
        });

        if (response.status === 200) {
          await router.replace({ name: "home" });
        } else if (response.status === 400) {
          error.value = "Login failed, check your email or password";
        }
      } catch (e) {
        console.log(e);
      }
    };

    watchEffect(() => {
      if (data.email !== "" && data.password !== "") {
        disableBtn.value = false;
      }
    });

    return {
      data,
      submit,
      error,
      disableBtn,
    };
  },
};
</script>