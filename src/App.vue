<template>
  <div style="display: flex; flex-flow: column wrap; justify-content: center">
    <div style="text-align: center">
      <h3 style="margin-bottom: 5px">DVD Cropper</h3>
    </div>
    <div style="text-align: center; opacity: 0.8">
      <a target="blank" href="https://github.com/porn-vault/dvd-cropper">
        <svg
          style="width: 24px; height: 24px; margin-right: 20px"
          role="img"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
        >
          <title>View source</title>
          <path
            d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"
          /></svg
      ></a>
      <a target="blank" href="https://www.patreon.com/pornvault">
        <svg
          style="width: 24px; height: 24px"
          role="img"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
        >
          <title>Support on Patreon</title>
          <path
            d="M0 .48v23.04h4.22V.48zm15.385 0c-4.764 0-8.641 3.88-8.641 8.65 0 4.755 3.877 8.623 8.641 8.623 4.75 0 8.615-3.868 8.615-8.623C24 4.36 20.136.48 15.385.48z"
          /></svg
      ></a>
    </div>
    <hr
      style="
        margin-bottom: 20px;
        margin-top: 20px;
        width: 200px;
        opacity: 0.25;
        border-width: 1px;
      "
    />
    <div style="text-align: center">
      <cropper />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Cropper from "./components/Cropper.vue";

export default defineComponent({
  name: "App",
  components: {
    Cropper,
  },
});
</script>

<style>
body {
  margin: 0;
}

#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 25px;
}
</style>
