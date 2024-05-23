<template>
  <v-app id="inspire">
    <v-main>
      <div class="container">
        <Header />
        <Cotacao />
      </div>
    </v-main>
    <Footer />
  </v-app>
</template>

<script setup>
import Header from "@/components/Header.vue";
import Cotacao from "@/components/Cotacao.vue";
import Footer from "@/components/AppFooter.vue";
</script>
<style>
#inspire {
  background-image: url("./assets/background.jpg");
  background-size: cover;
}
.container {
  width: 70%;
  margin-left: 20%;
  padding: 20px;
}
.corAzul {
  color: #73baec;
}
@media screen and (max-width: 1024px) {
  .container {
    width: 80%;
    margin-left: 10%;
  }
}
@media screen and (max-width: 768px) {
  .container {
    width: 100%;
    margin-left: 0%;
  }
}
</style>
