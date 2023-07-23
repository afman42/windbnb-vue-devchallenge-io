<script setup>
import { mdiMinus, mdiPlus } from "@mdi/js";
import SvgIcon from "@jamescoyle/vue-icon";
const props = defineProps({
  text: {
    default: "",
    type: String,
  },
  description: {
    default: "",
    type: String,
  },
  value: {
    type: Number,
    default: 0,
  },
  addOrMinusFunc: {
    type: Function,
  },
  textAddOrMinus: {
    type: Array,
  },
});
</script>
<template>
  <div class="text-box">
    <h5>{{ props.text }}</h5>
    <h5>{{ props.description }}</h5>
  </div>
  <div class="box">
    <button @click="() => props.addOrMinusFunc(props.textAddOrMinus[0])">
      <svg-icon type="mdi" :path="mdiMinus" :size="20" />
    </button>
    {{ props.value }}
    <button @click="() => props.addOrMinusFunc(props.textAddOrMinus[1])">
      <svg-icon type="mdi" :path="mdiPlus" :size="20" />
    </button>
  </div>
</template>

<style lang="scss">
.text-box {
  display: flex;
  padding: 20px 0px 0px;
  flex-direction: column;
  h5:nth-child(1) {
    font-family: "Mulish", sans-serif;
    font-size: 14px;
    font-weight: 400;
  }
  h5:nth-child(2) {
    font-family: "Mulish", sans-serif;
    font-size: 14px;
    font-weight: 400;
    color: #bdbdbd;
    margin-bottom: 10px;
  }
}
.box {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 0px 0px 20px;
  user-select: none;
  button {
    cursor: pointer;
    border: 1px solid #828282;
    background: white;
    border-radius: 5px;
  }
  button:nth-child(1) {
    color: #828282;
    margin-right: 10px;
  }
  button:nth-child(2) {
    color: #828282;
    margin-left: 10px;
  }
}
</style>
