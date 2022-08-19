<template>
  <div class="container">
    <h1>{{ message }}</h1>
  </div>
</template>

<script lang="ts">
import { onMounted, ref } from "vue";
import { useStore } from "vuex";

export default {
  setup() {
    const message = ref("You are not logged in");
    const store = useStore();

    onMounted(async () => {
      try {
        const response = await fetch("http://localhost:8000/api/auth/user", {
          method: "GET",
          credentials: "include",
        });

        const content = await response.json();

        if (content.message !== "Unauthorized") {
          message.value = `Welcome back, ${content.name} !`;
          await store.dispatch("setAuth", true);
        }
      } catch (e) {
        await store.dispatch("setAuth", false);
      }
    });

    return {
      message,
    };
  },
};
</script>

<style>
</style>