<template>
  <header class="header">
    <div class="header__content">
      <h1 class="title" ref="header">
        Прямолинейное движение с переменным ускорением
      </h1>
      <p class="subtitle" :class="{ subtitleActive: issubtitleActive }">
        Матвеев Евгений Михайлович, 9 класс <br />Умеренков Михаил Владимирович,
        учитель физики
      </p>
    </div>
  </header>
</template>

<script setup>
import { ref, onMounted } from "vue";

const header = ref(null);
const issubtitleActive = ref(false);

const opts = {
  rootmargin: "0px",
  threshold: 1.0,
};

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    if (entries[0].isIntersecting) {
      issubtitleActive.value = true;
    } else {
      issubtitleActive.value = false;
    }
  });
  observer.observe(header.value);
});
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300..800;1,300..800&family=Playfair+Display:wght@400..900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@100..900&display=swap");

.header {
  height: 100vh;
  width: 100%;
  background-image: url("../img/header.png");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  color: white;
}

.header__content {
  padding-top: 240px;
  max-width: 1440px;
  margin: 0 auto;
}
.title {
  font-size: 96px;
  font-weight: lighter !important;
}
.subtitle {
  font-family: "Montserrat", sans-serif;
  margin-top: 24px;
  font-size: 28px;
  font-weight: 200 !important;
  position: relative;
  right: 10%;
  opacity: 0;
  transition: all 2s;
}
.subtitleActive {
  right: 0;
  opacity: 100;
}

@media screen and (width < 1200px) {
  .title {
    font-size: 64px;
    text-align: center;
  }
  .subtitle {
    text-align: center;
  }
}
@media screen and (width < 660px) {
  .title {
    font-size: 64px;
  }
  .subtitle {
    display: none;
  }
  .header {
    height: 50vh;
  }
  .title {
    max-width: 100%;
    font-size: 36px;
    font-family: "Montserrat", sans-serif;
    font-weight: 200 !important;
  }
  .header__content {
    height: 100%;
    display: flex;
    align-items: center;
    padding: 10px;
  }
}
</style>
