<script setup>
import { computed, reactive } from "vue";
import Header from "./components/Header.vue";
import Section from "./components/Section.vue";
import ModalDialog from "./components/ModalDialog.vue";
import data from "./stays.json";
let allData = reactive({
  showModal: false,
  city: "Helsinki",
  guests: {
    adults: 0,
    children: 0,
  },
  changeCity(cty) {
    allData.city = cty;
  },
  incOrDecValueNumber(type) {
    if (type.includes("addAdult")) {
      allData.guests.adults = allData.guests.adults + 1;
    }
    if (type.includes("minAdult")) {
      if (allData.guests.adults > 0) {
        allData.guests.adults = allData.guests.adults - 1;
      }
    }
    if (type.includes("addChildren")) {
      allData.guests.children = allData.guests.children + 1;
    }
    if (type.includes("minChildren")) {
      if (allData.guests.children > 0) {
        allData.guests.children = allData.guests.children - 1;
      }
    }
  },
  clickShowModal() {
    allData.showModal = true;
  },
});
const dataComputed = computed(() => {
  return data
    .filter((item) => {
      return item.city.includes(allData.city);
    })
    .filter((item) => {
      const sum = allData.guests.children + allData.guests.adults;
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
    <Header :clickShowModal="allData.clickShowModal" :value="allData.city" />
    <Section :data="dataComputed" :dataLength="dataComputed.length" />
    <ModalDialog
      :show="allData.showModal"
      @close="allData.showModal = false"
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
