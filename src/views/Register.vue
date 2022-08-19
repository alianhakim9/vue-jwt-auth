<template>
  <form @submit.prevent="submit">
    <h1 class="h3 mb-3 fw-normal">Please register</h1>

    <input
      v-model="data.name"
      class="form-control"
      placeholder="Name"
      required
    />

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

    <button class="w-100 btn btn-lg btn-primary" type="submit">Submit</button>
  </form>
</template>

<script lang="ts">
import { onMounted, reactive } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
export default {
  name: "Register",
  setup() {
    const data = reactive({
      name: "",
      email: "",
      password: "",
    });
    const store = useStore();

    onMounted(() => {
      if (store.state.authenticated) {
        router.replace({ name: "home" });
      }
    });

    const router = useRouter();
    const submit = async () => {
      await fetch("http://localhost:8000/api/register", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data),
      });
      await router.push("/login");
    };
    return {
      data,
      submit,
    };
  },
};
</script>