<template>
  <div class="control-panel">
    <div class="control-panel__btns">
      <button class="control-panel__btn" @click="changeColor">Color</button>
      <button class="control-panel__btn" @click="changeFont">Font</button>
      <button class="control-panel__btn" @click="changeBG">Bg</button>
      <button class="control-panel__btn" @click="toJSON">
        {{ btnStatus ? "hide json" : "to json" }}
      </button>
      <button class="control-panel__btn" @click="clearAll">Clear all</button>
    </div>

    <div class="options">
      <div class="options__color">
        <label for="head">color</label>
        <input type="color"  name="head" v-model="color" />
      </div>
      <div class="options__store">
        <span v-if="localStore" @click="store" class="material-icons">
          check_box
        </span>
        <span v-else @click="store" class="material-icons">
          check_box_outline_blank
        </span>
        <span>Save text to local storage</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ControlPanel",
  data() {
    return {
      color: "#e66465",
    };
  },
  props: {
    btnStatus: {
      type: Boolean,
    },
    localStore: {
      type: Boolean,
    },
  },
  methods: {
    changeColor() {
      let newColor = { color: this.color };
      this.$emit("changeColor", newColor);
    },
    changeFont() {
      let newFont = { font: "2em" };
      this.$emit("changeFont", newFont);
    },
    changeBG() {
      let newBG = { bg: this.color };
      this.$emit("changeBG", newBG);
    },
    toJSON() {
      this.$emit("toJSON");
    },
    store() {
      this.$emit("toggleStore", !this.localStore);
    },
    clearAll() {
      this.$emit("clearAll");
    },
  },
};
</script>

<style lang="scss">
.control-panel {
  width: 500px;
  margin: 30px auto;
  display: flex;

  flex-wrap: wrap;
  justify-content: space-between;
  -moz-user-select: none;
  -khtml-user-select: none;
  user-select: none;
  &__btns {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  &__btn {
    width: 100px;
    padding: 8px;
    margin: 10px;
    text-align: center;
    text-transform: uppercase;
    transition: 0.5s;
    background-size: 200% auto;
    color: white;
    border-color: transparent;
    border-radius: 10px;
    background-image: linear-gradient(
      to right,
      #fbc2eb 0%,
      #a6c1ee 51%,
      #fbc2eb 100%
    );
  }
  &__btn:hover {
    cursor: pointer;
    background-position: right center;
  }
}
.options {
  font-size: 24px;
  margin-top: 25px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  
  &__store{
    display: flex;
    align-items: center;
  }
  input{
    margin-left: 15px;
  }
  
}
</style>
