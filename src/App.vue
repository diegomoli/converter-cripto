<template>
  <div class="container">
    <h1>Cotizador de criptomonedas</h1>
    <grid>
      <formulario @info-monedas="obtener" />
      <Data
        :cripto="info.cripto"
        :moneda="info.moneda"
        :img="info.img"
        :precio="info.precio"
      />
    </grid>
  </div>
  <reload-prompt />
</template>

<script>
import Data from "./components/Data.vue";
import Formulario from "./components/Formulario.vue";
import Grid from "./components/Grid.vue";
import ReloadPrompt from "./components/ReloadPrompt.vue";
import img from "./assets/logo.svg";
export default {
  components: { Formulario, Data, Grid, ReloadPrompt },
  data: () => ({
    info: {
      cripto: "-",
      moneda: "-",
      img: "/media/37746251/btc.png",
      precio: 0,
    },
  }),
  methods: {
    async obtener(cripto, moneda) {
      const resp = await fetch(
        `https://min-api.cryptocompare.com/data/pricemultifull?fsyms=${encodeURI(
          cripto
        )}&tsyms=${encodeURI(moneda)}`
      );

      const { RAW } = await resp.json();
      const dataCripto = RAW[cripto];
      const data = dataCripto[moneda];
      console.log(data);
      console.log(data.IMAGEURL);
      this.info.cripto = cripto;
      this.info.moneda = moneda;
      this.info.img = data.IMAGEURL;
      this.info.precio = data.PRICE;
    },
  },
};
</script>
