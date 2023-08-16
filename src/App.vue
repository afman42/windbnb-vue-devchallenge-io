<script setup>
import { ref, computed, reactive } from "vue";
import Header from "./components/Header.vue";
import Section from "./components/Section.vue";
import ModalDialog from "./components/ModalDialog.vue";
import data from "./stays.json";
let showModal = ref(false);
let allData = reactive({
  city: "Helsinki",
  adults: 0,
  children: 0,
  changeCity(cty) {
    this.city = cty;
  },
  incOrDecValueNumber(type) {
    if (type.includes("addAdult")) {
      this.adults = this.adults + 1;
    }
    if (type.includes("minAdult")) {
      if (this.adults > 0) {
        this.adults = this.adults - 1;
      }
    }
    if (type.includes("addChildren")) {
      this.children = this.children + 1;
    }
    if (type.includes("minChildren")) {
      if (this.children > 0) {
        this.children = this.children - 1;
      }
    }
  },
});
function clickShowModal() {
  showModal.value = true;
}
const dataComputed = computed(() => {
  return data
    .filter((item) => {
      return item.city.includes(allData.city);
    })
    .filter((item) => {
      const sum = allData.children + allData.adults;
      return item.maxGuests >= sum;
    });
});

const cityComputed = computed(() => {
  const set = new Set();
  return data
    .map((item) => ({ city: item.city, country: item.country }))
    .filter(({ city }) => (set.has(city) ? false : set.add(city)));
});
</script>

<template>
  <div class="container">
    <Header :clickShowModal="clickShowModal" :value="city" />
    <Section :data="dataComputed" />
    <ModalDialog
      :show="showModal"
      @close="showModal = false"
      :dataCity="cityComputed"
      :value="allData.city"
      :changeCity="allData.changeCity"
      :valueGuests="allData.guests"
      :incOrDecValueNumber="allData.incOrDecValueNumber"
    />
    <div class="footer">created by @afman42 - devChallenges.io</div>
  </div>
</template>

<style lang="scss">
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.container {
  margin: 30px 100px;
  @media screen and (max-width: 768px) {
    margin: 30px 30px;
  }
  @media screen and (max-width: 375px) {
    margin: 5px 10px;
  }
}

.footer {
  display: flex;
  justify-content: center;
  color: #828282;
  font-family: "Montserrat", sans-serif;
  font-weight: 500;
  font-size: 12px;
}
</style>
