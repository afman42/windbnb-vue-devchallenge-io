<script setup>
import { mdiStar } from "@mdi/js";
import SvgIcon from "@jamescoyle/vue-icon";
const props = defineProps({
  data: {
    default: [],
    required: true,
  },
});
</script>

<template>
  <div class="section-container">
    <h1>Stays in Finland</h1>
    <h6>12+ stays</h6>
  </div>
  <div class="cube">
    <div v-for="data in props.data" class="cube-row" :key="data.title">
      <img :src="data.photo" />
      <div class="cube-row-title">
        <div class="cube-row-title-left">
          <h4 v-if="data.superHost" class="">SUPER HOST</h4>
          <h5>
            {{ data.type }} .
            {{ data.beds != null ? data.beds + " beds" : null }}
          </h5>
        </div>
        <div class="cube-row-title-right">
          <svg-icon
            type="mdi"
            :path="mdiStar"
            :size="16"
            class="cube-row-title-right-icon"
          />
          <h4>{{ data.rating }}</h4>
        </div>
      </div>
      <h3>{{ data.title }}</h3>
    </div>
  </div>
</template>

<style lang="scss">
.section-container {
  margin: 70px 0px 10px 0px;

  @media screen and (max-width: 375px) {
    margin: 40px 0px 10px 0px;
  }
  display: flex;
  justify-content: space-between;
  h1 {
    font-family: "Montserrat", sans-serif;
    font-weight: 700;
    font-size: 24px;
  }
  h6 {
    font-family: "Montserrat", sans-serif;
    font-weight: 500;
    font-size: 14px;
  }
}
.cube {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  .cube-row {
    margin-right: 10px;
    margin-bottom: 10px;
    display: flex;
    flex-direction: column;
    h3 {
      margin-top: 10px;
      font-family: "Montserrat", sans-serif;
      font-weight: 600;
      font-size: 14px;
    }
    .cube-row-title {
      margin-top: 10px;
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      .cube-row-title-left {
        display: flex;
        align-items: center;
        h4 {
          padding: 2px 8px;
          border-radius: 10px;
          border: 1px solid #4f4f4f;
          font-family: "Montserrat", sans-serif;
          font-weight: 700;
          font-size: 10px;
          margin-right: 5px;
        }
        h5 {
          font-family: "Montserrat", sans-serif;
          font-weight: 500;
          font-size: 12px;
          color: gray;
        }
      }
      .cube-row-title-right {
        display: flex;
        .cube-row-title-right-icon {
          color: #ffb14e;
        }
        h4 {
          margin-left: 5px;
          font-family: "Montserrat", sans-serif;
          font-weight: 500;
          font-size: 12px;
          color: #4f4f4f;
        }
      }
    }
  }
  img {
    width: 370px;
    height: 267px;
    border-radius: 10px;
    @media screen and (max-width: 768px) {
      width: 670px;
    }
    @media screen and (max-width: 375px) {
      width: 340px;
      height: 200px;
    }
  }
}
</style>
