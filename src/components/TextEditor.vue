<template>
  <div class="container">
    <TextElem
      v-for="(textParam, index) in textData"
      :index="index"
      :textInfo="textParam"
      :key="textParam.id"
      :class="textParam.id"
      @startIndex="start"
      @endIndex="end"
      @saveChange="saveChange"
    />
    <ControlPanel
      @changeColor="selectText"
      @changeFont="selectText"
      @changeBG="selectText"
      @toJSON="showJSON"
      @toggleStore="saveToLocalStore"
      @clearAll="clearAll"
      :btnStatus="jsonBlock"
      :localStore="localStore"
    />
    <pre v-if="jsonBlock" class="json-block">
      {{ textData }}
    </pre>
  </div>
</template>

<script>
import ControlPanel from "./ControlPanel";
import TextElem from "./TextElem";
export default {
  name: "TextEditor",
  components: {
    TextElem,
    ControlPanel,
  },
  data() {
    return {
      startElemIndex: null,
      endElemIndex: null,
      selectedElem: [],
      jsonBlock: false,
      localStore: false,
      textData: [
        {
          id: Math.random(),
          text: "Hi My lovely little Ponny",
        },
      ],
    };
  },
  computed: {},
  methods: {
    start(indexNum) {
      this.startElemIndex = indexNum;
    },
    end(indexNum) {
      this.endElemIndex = indexNum;
    },
    showJSON() {
      this.jsonBlock = !this.jsonBlock;
    },
    saveChange(payload) {
      this.textData[payload.index].text = payload.text;
    },
    selectText(param) {
      if (window.getSelection().anchorNode === null)
        return alert("select text");
      let startSelection = window.getSelection().anchorOffset;
      let endSelection = window.getSelection().focusOffset;
      let select = window.getSelection();
      let text = select.focusNode.data;
      let startText = select.anchorNode.data;
      let endText = select.focusNode.data;

      //if choose from right to left change anchors
      if (
        (this.startElemIndex === this.endElemIndex &&
          startSelection > endSelection) ||
        (startText !== endText && this.startElemIndex > this.endElemIndex)
      ) {
        let tempVar = startSelection;
        startSelection = endSelection;
        endSelection = tempVar;
        tempVar = startText;
        startText = endText;
        endText = tempVar;
      }

      if (this.startElemIndex > this.endElemIndex) {
        let tempVar = this.startElemIndex;
        this.startElemIndex = this.endElemIndex;
        this.endElemIndex = tempVar;
      }

      // push to state index select span
      for (
        let index = this.startElemIndex;
        index <= this.endElemIndex;
        index++
      ) {
        this.selectedElem.push(index);
      }

      // initialization of new objects(span)
      let beforeElem = {
        ...this.textData[this.selectedElem[0]],
        id: Math.random(),
      };

      let newElem = {
        ...this.textData[this.selectedElem[0]],
        ...param,
        id: Math.random(),
      };

      let nextElem = {
        ...this.textData[this.selectedElem[this.selectedElem.length - 1]],
        id: Math.random(),
      };

      //   if one span is selected
      if (this.selectedElem.length <= 1) {
        beforeElem.text = text.slice(0, startSelection);
        newElem.text = text.slice(startSelection, endSelection);
        nextElem.text = text.slice(endSelection, select.focusNode.length);

        this.textData.splice(
          this.selectedElem[0],
          1,
          beforeElem,
          newElem,
          nextElem
        );
      }

      // if more than one span is selected
      if (this.selectedElem.length >= 2) {
        let textSelect = this.selectedElem.reduce((str, elem) => {
          if (elem === this.startElemIndex) {
            return str + this.textData[elem].text.slice(startSelection - 1);
          }
          if (elem === this.endElemIndex) {
            return str + this.textData[elem].text.slice(0, endSelection - 1);
          }
          return (str = str + this.textData[elem].text);
        }, "");

        beforeElem.text = startText.slice(0, startSelection);
        newElem.text = textSelect;
        nextElem.text = endText.slice(endSelection, select.focusNode.length);

        this.textData.splice(
          this.selectedElem[0],
          this.selectedElem.length,
          beforeElem,
          newElem,
          nextElem
        );
      }

      this.saveToLocalStore(this.localStore);

      // clear temporary data
      this.startElemIndex = null;
      this.endElemIndex = null;
      this.selectedElem = [];
    },
    saveToLocalStore(status) {
      this.localStore = status;
      localStorage.setItem("status", JSON.stringify(status));
      if (this.localStore) {
        localStorage.setItem("text", JSON.stringify(this.textData));
      }
    },
    getDataFromLocalStore() {
      if (localStorage.getItem("text")) {
        this.textData = JSON.parse(localStorage.getItem("text"));
      }
    },
    clearAll() {
      let text = this.textData.reduce((prev, elem) => {
        return prev + elem.text;
      }, "");
      this.textData = [{ text }];
      this.saveToLocalStore(this.localStore);
    },
  },
  mounted() {
    this.getDataFromLocalStore();
    this.localStore = JSON.parse(localStorage.getItem("status"));
  },
};
</script>

<style lang="scss">
.container {
  font-size: 24px;
  max-width: 600px;
  margin: 50px auto;
  text-align: center;
}

.json-block {
  text-align: left;
}
</style>
