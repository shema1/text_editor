<template>
  <span
  title="double click to edit"
    @mousedown="start()"
    @mouseup="end()"
    @dblclick="editText"
    :style="styleText"
  >
    {{ edit ? "" : textInfo.text }}
    <input
      class="text-input"
      v-if="edit"
      type="text"
      v-model="text"
      @blur="close()"
      :style="styleInput"
    />
  </span>
</template>

<script>
export default {
  name: "TextElem",
  data() {
    return {
      edit: false,
      text: this.textInfo.text,
    };
  },
  computed: {
    styleText: function() {
      return {
        color: this.textInfo.color,
        fontSize: this.textInfo.font,
        "background-color": this.textInfo.bg,
      };
    },
    styleInput: function() {
      return {
        ...this.styleText,
        width: this.textInfo.text.length * 11 + "px",
      };
    },
  },
  props: {
    textInfo: {
      type: Object,
    },
    index: {
      type: Number,
    },
  },
  methods: {
    start() {
      this.$emit("startIndex", this.index);
    },
    end() {
      this.$emit("endIndex", this.index);
    },
    editText() {
      this.edit = true;
    },
    close() {
      this.edit = false;
      let payload = {
        index: this.index,
        text: this.text,
      };
      this.$emit("saveChange", payload);
    },
  },
};
</script>

<style lang="scss">
.color {
  color: red;
}

.text-input {
  margin: 0;
  padding: 0;
  font-size: 21px;
}

span {
  border-radius: 8px;  
  cursor: pointer;
}
</style>
