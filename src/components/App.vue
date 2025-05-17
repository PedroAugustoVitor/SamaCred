<script>
import 'bootstrap'

export default {
  data: function () {
    return {
      rates: {
        receivable: [0, 1.0918, 1.1098, 1.1184, 1.1269, 1.1356, 1.1442, 1.1535, 1.1620, 1.1710, 1.1826, 1.1890, 1.1993, 1.2070, 1.2160, 1.2251, 1.2342, 1.2435, 1.2506],
        limit: [0, 0.08408133357757797, 0.09893674535952401, 0.10586552217453504, 0.11260981453545105, 0.11940824233885206, 0.12602691837091395, 0.13307325530992598, 0.13941480206540402, 0.14602903501280995, 0.15440554709961096, 0.158957106812447, 0.16618027182523099, 0.17149958574979296, 0.17763157894736803, 0.18374010284874698, 0.18975854804731807, 0.195818254925613, 0.200383815768431]
      },
      value: 1000,
      term: 1,
      limit: false,
      result: {
        willCharge: 0,
        willReceive: 0,
        portion: 0
      }
    }
  },
  mounted() {
    this.process()
  },
  methods: {
    process: function () {
      this.limit ? this.result = {
        willCharge: this.formatter(this.value),
        willReceive: this.formatter(this.value - (this.value * this.rates.limit[this.term])),
        portion: this.formatter(this.value / this.term)
      } : this.result = {
        willCharge: this.formatter(this.value * this.rates.receivable[this.term]),
        willReceive: this.formatter(this.value),
        portion: this.formatter(this.value * this.rates.receivable[this.term] / this.term)
      }
    },
    formatter: function (e) {
      return e.toLocaleString("pt-BR", {
        style: "currency",
        currency: "BRL"
      })
    },
    copy: function () {
      let e = `Simulador SamaCred\nSerá cobrado no cartão: ${this.result.willCharge}.\nVocê receberá: ${this.result.willReceive}.\nSer${this.term > 1 ? "ão" : "á"} ${this.term} parcela${this.term > 1 ? "s" : ""} de ${this.result.portion}`;
      navigator.clipboard.writeText(e)
    }
  }
}
</script>

<template>
  <div class="text-bg-dark">
    <div class="col-6 offset-3">
      <div class="col-4 offset-4 pt-5">
        <img alt="Logo" class="img-fluid" src="../img/logo.png">
      </div>
      <div class="mb-3">
        <label for="exampleFormControlInput1" class="form-label">Valor: </label>
        <input type="number" class="form-control" v-model="value" @change="process">
      </div>
      <div class="col-12">
        <label class="form-label">Número de parcelas: </label>
        <input type="range" class="form-range" min="1" max="18" v-model="term" @change="process">
        <h2 class="text-center">{{ term }}</h2>
      </div>
      <div class="form-check form-switch">
        <input class="form-check-input" type="checkbox" role="switch" v-model="limit" @change="process">
        <label class="form-check-label">Calcular {{limit?"a partir do limite":"valor à receber"}}</label>
      </div>
      <div class="jumbotron">
        <h1 class="display-6">Detalhes da simulação</h1>
        <p class="lead">Será cobrado no cartão: R$: {{ result.willCharge }}</p>
        <p class="lead">Você receberá: R$: {{ result.willReceive }}</p>
        <hr class="my-4">
        <p>Ser{{ parseInt(term) === 1 ? "á" : "ão" }} {{ term }} parcela{{ parseInt(term) === 1 ? "" : "s" }} de R$:
          {{ result.portion }}</p>
      </div>
      <p class="lead">
        <a class="btn btn-outline-primary btn-lg" role="button" @click="copy">Copiar</a>
      </p>
    </div>
  </div>
</template>

<style scoped>

</style>
