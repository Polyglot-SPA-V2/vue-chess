<template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <slot name="header">
              Advanced Config Options
            </slot>
          </div>
          <div class="modal-body">
            <slot name="body">
              <div>
                <label for="inputFEN" class="form-label">Input FEN</label>
                <input type="text" class="form-control" id="inputFEN" v-model="fen"/>
                <div id="fenHelp" class="form-text" aria-describedby="fenHelp" >If the FEN is invalid the field will be ignored.</div>
              </div>
              <div>
                Pick Orientation
                <div class="form-check">
                  <input class="form-check-input" type="radio" name="inputOrientation" id="inputRadioWhite" value="white" v-model="color" checked/>
                  <label class="form-check-label" for="inputRadioWhite"> White </label>
                </div>
                <div class="form-check">
                  <input class="form-check-input" type="radio" name="inputOrientation" id="inputRadioBlack" value="black" v-model="color"/>
                  <label class="form-check-label" for="inputRadioBlack"> Black </label>
                </div>
              </div>
              <div>
                <div class="form-check">
                  <input class="form-check-input" type="checkbox" v-model="selfPlay" id="inputSelfPlay"/>
                  <label class="form-check-label" for="inputSelfPlay">
                    Check to make computer play itself.
                  </label>
                </div>
              </div>
            </slot>
          </div>
          <div class="modal-footer">
            <slot name="footer">
              <button id="advancedConfigStartGameBtn" type="submit" class="btn btn-dark btn-sm" @click="advancedConfigStartGame( color, fen, selfPlay )">
                OK
              </button>
              <button id="closeAdvancedConfigModalBtn" type="button" class="btn btn-dark btn-sm" @click="closeAdvancedConfigModal">
                Cancel
              </button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "advanced-config-modal",
  data() {
    return {
      color: "white",
      fen: "",
      selfPlay: false
    }
  },
  methods: {
    closeAdvancedConfigModal() {
      this.$emit('closeAdvancedConfigModal');
    },
    advancedConfigStartGame(color, fen, selfPlay) {
      const data = {};
      data["fen"] = fen;
      data["color"] = color;
      data["selfPlay"] = selfPlay;
      this.$emit('advancedConfigStartGame', data);
      this.$emit('closeAdvancedConfigModal');
    }
  }
}
</script>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0 auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  display: block;
  margin-top: 1rem;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>
