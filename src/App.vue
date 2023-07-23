<script setup>
import { ref, computed } from "vue";
import Header from "./components/Header.vue";
import Section from "./components/Section.vue";
import ModalDialog from "./components/ModalDialog.vue";
import data from "./stays.json";
let showModal = ref(false);
let city = ref("Helsinki");
let guests = ref({
  adults: 0,
  children: 0,
});
function clickShowModal() {
  showModal.value = true;
}
const dataComputed = computed(() => {
  return data
    .filter((item) => {
      return item.city.includes(city.value);
    })
    .filter((item) => {
      const sum = guests.value.children + guests.value.adults;
      return item.maxGuests >= sum;
    });
});

const cityComputed = computed(() => {
  const set = new Set();
  return data
    .map((item) => ({ city: item.city, country: item.country }))
    .filter(({ city }) => (set.has(city) ? false : set.add(city)));
});
function changeCity(cty) {
  city.value = cty;
}
function incOrDecValueNumber(type) {
  if (type.includes("addAdult")) {
    guests.value.adults = guests.value.adults + 1;
  }
  if (type.includes("minAdult")) {
    if (guests.value.adults > 0) {
      guests.value.adults = guests.value.adults - 1;
    }
  }
  if (type.includes("addChildren")) {
    guests.value.children = guests.value.children + 1;
  }
  if (type.includes("minChildren")) {
    if (guests.value.children > 0) {
      guests.value.children = guests.value.children - 1;
    }
  }
}
</script>

<template>
  <div class="container">
    <Header :clickShowModal="clickShowModal" :value="city" />
    <Section :data="dataComputed" />
    <ModalDialog
      :show="showModal"
      @close="showModal = false"
      :dataCity="cityComputed"
      :value="city"
      :changeCity="changeCity"
      :valueGuests="guests"
      :incOrDecValueNumber="incOrDecValueNumber"
    />
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
}
</style>
