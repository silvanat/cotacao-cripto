<template>
  <v-card class="box colorBlack">
    <div class="search">
      <label>Criptomoeda:</label>
      <v-combobox
        v-model="selectCripto"
        class="colorBlack"
        :items="[
          'Binance-coin',
          'Bitcoin',
          'Cardano',
          'Dogecoin',
          'Ethereum',
          'Litecoin',
          'Monero',
          'Solana',
          'Stellar',
          'Tether',
          'XRP',
        ]"
        variant="outlined"
      ></v-combobox>
      <v-btn class="btncorAzul" @click="cotacao"
        >Cotar em tempo real
        <span v-if="showLoading" class="loading"
          ><v-progress-circular
            indeterminate
            :size="20"
            color="blue-lighten-3"
          ></v-progress-circular></span
      ></v-btn>
    </div>

    <div class="result_block" v-if="resultCripto">
      <h2>
        <img :src="imageCoin" class="imgcoin" />
        {{ resultCripto.name }}
      </h2>

      <p v-if="resultCripto.priceUsd">
        Preço:
        <span>
          US$
          {{
            parseFloat(resultCripto.priceUsd).toLocaleString("pt-br", {
              minimumFractionDigits: 2,
              maximumFractionDigits: 2,
            })
          }}</span
        >
      </p>
      <p v-if="resultCripto.vwap24Hr">
        Preço médio (24hs):
        <span>
          US$
          {{
            parseFloat(resultCripto.vwap24Hr).toLocaleString("pt-br", {
              minimumFractionDigits: 2,
              maximumFractionDigits: 2,
            })
          }}</span
        >
      </p>
      <p v-if="resultCripto.volumeUsd24Hr">
        Volume (24hs):
        <span>{{
          parseFloat(resultCripto.volumeUsd24Hr).toLocaleString("pt-br", {
            minimumFractionDigits: 2,
            maximumFractionDigits: 2,
          })
        }}</span>
      </p>
      <p v-if="resultCripto.supply">
        Em circulação (supply):
        <span>{{
          parseFloat(resultCripto.supply).toLocaleString("pt-br", {
            minimumFractionDigits: 2,
            maximumFractionDigits: 2,
          })
        }}</span>
      </p>
      <p v-if="resultCripto.maxSupply">
        Máximo disponível:
        <span>{{
          parseFloat(resultCripto.maxSupply).toLocaleString("pt-br", {
            minimumFractionDigits: 2,
            maximumFractionDigits: 2,
          })
        }}</span>
      </p>
      <p v-if="resultCripto.marketCapUsd">
        Market Cap (supply x preço):
        <span>{{
          parseFloat(resultCripto.marketCapUsd).toLocaleString("pt-br", {
            minimumFractionDigits: 2,
            maximumFractionDigits: 2,
          })
        }}</span>
      </p>
      <p v-if="resultCripto.changePercent24Hr">
        Variação (24hs):
        <span>{{
          parseFloat(resultCripto.changePercent24Hr).toLocaleString("pt-br", {
            minimumFractionDigits: 2,
            maximumFractionDigits: 2,
          })
        }}</span>
      </p>
    </div>
  </v-card>

  <v-snackbar color="red-lighten-1" v-model="snackbar">
    {{ text }}

    <template v-slot:actions>
      <v-btn color="white" variant="text" @click="snackbar = false"> X </v-btn>
    </template>
  </v-snackbar>
</template>

<script setup>
import { ref } from "vue";

const selectCripto = ref("");
const selectCoin = ref("");

const resultCripto = ref(null);
const text = ref("");
const snackbar = ref(false);
const imageCoin = ref("");
const showLoading = ref(false);

const cotacao = () => {
  if (!selectCripto.value) {
    snackbar.value = true;
    text.value = "Escolha a criptomoeda.";
    return false;
  }
  let cripto = selectCripto.value.toLowerCase();
  showLoading.value = true;
  resultCripto.value = null;
  fetch("https://api.coincap.io/v2/assets")
    .then((response) => response.json())
    .then((data) => {
      showLoading.value = false;
      resultCripto.value = data.data.find((t) => t.id == cripto);
      //console.log(resultCripto.value);

      imageCoin.value = "images/" + resultCripto.value.id + ".png";
    })
    .catch((error) => console.log(error));
};
</script>

<style scoped>
.loading {
  right: 10px;
  top: 6px;
  position: absolute;
}
.imgcoin {
  width: 40px;
  margin-right: 10px;
  float: left;
}

.colorBlack {
  color: #111111;
}
.btncorAzul {
  background-color: #2c6892;
  width: 50%;
  margin: 10px auto;
  display: block;
  border-radius: 10px;
  position: relative;
}
label {
  font-weight: 600;
  font-size: 1.3rem;
  margin: 10px 0;
  display: block;
}
.box {
  height: 800px;
  width: 50%;
  border-radius: 20px;
  background-color: #ffffff;
  margin: 20px 0;
  padding: 20px;
}
.result_block p {
  font-size: 1.2rem;
  margin: 20px 0;
  border: 1px solid #ccc;
  padding: 10px;
}
.result_block p span {
  font-weight: 600;
}
@media screen and (max-width: 1600px) {
  .box {
    width: 60%;
  }
  .btncorAzul {
    width: 70%;
  }
}
@media screen and (max-width: 1100px) {
  .box {
    width: 75%;
  }
  .btncorAzul {
    width: 80%;
  }
}
@media screen and (max-width: 990px) {
  .box {
    width: 100%;
  }
  .btncorAzul {
    width: 100%;
  }
}
</style>
