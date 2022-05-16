<template>
  <MainPage />
</template>

<script>
import MainPage from "./components/MainPage.vue";

export default {
  name: "App",
  components: {
    MainPage,
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Bangers&family=Josefin+Sans&family=Nunito:wght@300;400&display=swap');

:root {
  /** Declaração de Variáveis */
  --bg-primary-light: #ffa689;
  --bg-primary: #ff815a;
  --bg-primary-dark: #ff6334;
  --bg-secondary: #ffab00;
  --bg-success-light: #cdffd6;
  --bg-warning-light: #fdfcb8;
  --bg-danger-light: #ffe3e3;
  --bg-white: #263238;
  --bg-dark: #263238;

  --bg-body: #1c2732;
  --bg-card: #293845;
  --white: #fff;
  --dark: #263238;

  --bg-orange-gradient: linear-gradient(to right, #ff7878, #ff9c6b);
  --bg-purple-gradient: linear-gradient(to right, #aa99ff, #e09cff);
  --bg-blue-gradient: linear-gradient(to right, #4497f5, #75ddff);
  --bg-red-gradient: linear-gradient(to right, #f58080, #f5a1a1);

  --font-type-one: "Nunito", sans-serif;
  --font-type-two: "Bangers", cursive;
  --font-type-three: "Josefin Sans", sans-serif;
}
</style>
