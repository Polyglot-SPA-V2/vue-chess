<template>
  <header class="container-fluid" id="vueSecondaryHeader">
    <div class="row p-2">
      <div class="col-3">
        <img class="w-100" alt="Vue logo" src="./../assets/vue-logo.png">
      </div>
      <div class="col-6 d-grid gap-2">
        <button id="quickStartBtn" type="button" class="btn btn-light w-100 btn-sm" @click="quickStartGame" v-bind:disabled="disabled">Quick Start</button>
        <button id="advancedConfigBtn" type="button" class="btn btn-light w-100 btn-sm" @click="showAdvancedConfigModal" v-bind:disabled="disabled">Advanced Config</button>
      </div>
      <div class="col-3">
        <img class="w-100" alt="Chess logo" src="./../assets/Centaur.png">
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "vue-chess-header",
  data() {
    return {
      disabled: false
    }
},
  methods: {
    quickStartGame() {
      this.disabled = true;
      this.$emit('quickStartGame');
    },
    showAdvancedConfigModal() {
      this.disabled = true;
      this.$emit('showAdvancedConfigModal');
    }
  }
}
</script>

<style scoped>
#vueSecondaryHeader {
  background-image: url("./../assets/board-background.png");
}
</style>
