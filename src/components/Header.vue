<script setup>
import imgWindbnb from "../assets/logo.svg";
import { mdiMagnify } from "@mdi/js";
import SvgIcon from "@jamescoyle/vue-icon";
const props = defineProps({
  clickShowModal: {
    type: Function,
  },
  value: {
    type: String,
    default: "",
  },
});
</script>
<template>
  <div class="header">
    <img :src="imgWindbnb" class="header-img" />
    <div class="header-search" @click="props.clickShowModal">
      <h4>{{ props.value }} Finland</h4>
      <div class="divider">
        <h4 style="color: #bdbdbd">Add Guest</h4>
      </div>
      <svg-icon
        type="mdi"
        :path="mdiMagnify"
        :size="24"
        class="header-search-icon"
      />
    </div>
  </div>
</template>

<style lang="scss">
.header {
  display: flex;
  justify-content: space-between;
  flex-direction: row;
  @media screen and (max-width: 768px) {
    flex-direction: column;
  }
  @media screen and (max-width: 375px) {
    display: block;
    margin-top: -20px;
  }
  .header-img {
    @media screen and (max-width: 375px) {
      width: 80px;
      height: 80px;
    }
  }

  .header-search {
    cursor: pointer;
    margin-left: 20px;
    h4 {
      font-family: "Mulish", sans-serif;
      font-weight: 400;
      font-size: 14px;
    }
    .divider {
      display: flex;
      align-items: center;
      border-right: 2px solid #f2f2f2;
      border-left: 2px solid #f2f2f2;
      padding: 0px 10px;
      height: inherit;
      h4 {
        font-family: "Mulish", sans-serif;
        font-weight: 400;
        font-size: 14px;
      }
    }
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    width: 300px;
    height: 55px;
    border-radius: 10px;
    box-shadow: 1px 0px 5px -4px white;
    -webkit-box-shadow: 1px 0px 5px -4px rgba(0, 0, 0, 0.75);
    -moz-box-shadow: 1px 0px 5px -4px rgba(0, 0, 0, 0.75);
    .header-search-icon {
      color: red;
    }
  }
}
</style>
