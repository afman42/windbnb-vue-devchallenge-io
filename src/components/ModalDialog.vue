<script>
import { watch, ref } from "vue";
import { onClickOutside } from "@vueuse/core";
import { mdiMapMarker, mdiMagnify, mdiCheck, mdiClose } from "@mdi/js";
import SvgIcon from "@jamescoyle/vue-icon";
import Counter from "./Counter.vue";
const props = {
  show: {
    type: Boolean,
    default: false,
  },
  dataCity: {
    type: Array,
    default: [],
  },
  value: {
    type: String,
    default: "",
  },
  changeCity: {
    type: Function,
  },
  valueGuests: {
    type: Object,
    default: {},
  },
  incOrDecValueNumber: {
    type: Function,
  },
};

export default {
  name: "ModalDialog",
  props,
  components: {
    SvgIcon,
    Counter,
  },
  emits: ["close"],
  setup(props, { emit }) {
    const modal = ref(null);
    const showModal = ref(false);
    const transFade = ref({
      location: false,
      age: false,
    });
    const path = mdiMapMarker;
    const pathSearch = mdiMagnify;
    const pathChecklist = mdiCheck;
    const pathClose = mdiClose;
    function closeModal() {
      transFade.value = {
        location: false,
        age: false,
      };
      emit("close");
    }

    onClickOutside(modal, () => {
      if (showModal.value === true) {
        closeModal();
      }
    });

    watch(
      () => props.show,
      (show) => {
        showModal.value = !!show;
      }
    );

    return {
      closeModal,
      showModal,
      modal,
      path,
      transFade,
      pathSearch,
      pathChecklist,
      pathClose,
    };
  },
};
</script>

<template>
  <teleport to="body">
    <transition
      enter-active-class="enter-active"
      enter-from-class="enter-from-or-leave-to"
      enter-to-class="enter-to-or-leave-from"
      leave-active-class="leave-active"
      leave-from-class="enter-to-or-leave-from"
      leave-to-class="enter-from-or-leave-to"
    >
      <div ref="modal-backdrop" class="modal-backdrop" v-show="show">
        <div class="modal-container">
          <transition
            enter-active-class="enter-active"
            enter-from-class="enter-from-or-leave-to"
            enter-to-class="enter-to-or-leave-from"
            leave-active-class="leave-active"
            leave-from-class="enter-to-or-leave-from"
            leave-to-class="enter-from-or-leave-to"
          >
            <div
              role="dialog"
              ref="modal"
              aria-modal="true"
              v-show="show"
              class="modal-dialog"
              aria-labelledby="modal-headline"
            >
              <div class="modal-dialog-close">
                <h6>Edit Your Search</h6>
                <svg-icon
                  type="mdi"
                  :path="pathClose"
                  :size="24"
                  style="cursor: pointer"
                  @click="closeModal"
                />
              </div>
              <div class="modal-dialog-box">
                <div
                  class="modal-dialog-location"
                  @click="
                    transFade.location = true;
                    transFade.age = false;
                  "
                  :style="{
                    border: transFade.location ? '1px solid black' : '',
                  }"
                >
                  <h6>Location</h6>
                  <h6>{{ value }}, Finland</h6>

                  <transition
                    enter-active-class="enter-active"
                    enter-from-class="enter-from-or-leave-to"
                    enter-to-class="enter-to-or-leave-from"
                    leave-active-class="leave-active"
                    leave-from-class="enter-to-or-leave-from"
                    leave-to-class="enter-from-or-leave-to"
                  >
                    <div
                      class="modal-dialog-fill-box-location"
                      v-show="transFade.location"
                    >
                      <div
                        v-for="item in dataCity"
                        :key="item.city"
                        class="modal-dialog-fill-box-location-text"
                        @click="changeCity(item.city)"
                      >
                        <svg-icon
                          type="mdi"
                          :path="path"
                          :size="24"
                          class="cube-row-title-right-icon"
                        />
                        {{ item.city }}, {{ item.country }}
                        <svg-icon
                          type="mdi"
                          :path="pathChecklist"
                          v-if="item.city === value"
                          :size="16"
                          style="margin-left: 10px"
                        />
                      </div>
                    </div>
                  </transition>
                </div>
                <div
                  class="modal-dialog-guest"
                  @click="
                    transFade.location = false;
                    transFade.age = true;
                  "
                  :style="{
                    border: transFade.age ? '1px solid black' : '',
                  }"
                >
                  <h6>Guests</h6>
                  <h6>Add guests</h6>

                  <div
                    class="modal-dialog-fill-box-guest"
                    v-show="transFade.age"
                  >
                    <Counter
                      text="Adult"
                      description="Ages 13 or above"
                      :value="valueGuests.adults"
                      :addOrMinusFunc="incOrDecValueNumber"
                      :textAddOrMinus="['minAdult', 'addAdult']"
                    />
                    <Counter
                      text="Children"
                      description="Ages 2-12"
                      :value="valueGuests.children"
                      :addOrMinusFunc="incOrDecValueNumber"
                      :textAddOrMinus="['minChildren', 'addChildren']"
                    />
                  </div>
                </div>
                <div class="modal-dialog-button">
                  <button
                    class="modal-dialog-button-with-icon"
                    @click="closeModal"
                  >
                    <svg-icon
                      type="mdi"
                      :path="pathSearch"
                      :size="24"
                      class="but-icon"
                    />
                    Search
                  </button>
                </div>
              </div>
            </div>
          </transition>
        </div>
      </div>
    </transition>
  </teleport>
</template>

<style lang="scss">
.enter-active {
  transition: opacity 2s ease-out;
}

.leave-active {
  transition: opacity 2s ease-in;
}
.enter-from-or-leave-to {
  opacity: 0;
}
.enter-to-or-leave-from {
  opacity: 1;
}
.modal-backdrop {
  position: fixed;
  z-index: 10;
  inset: 0;
  overflow-y: auto;
  background-color: rgba(0, 0, 0, 0.5);
  .modal-container {
    display: flex;
    top: -300px;
    height: 80%;
    .enter-active {
      transition: top 1s ease-out;
    }
    .leave-active {
      transition: top 1s ease-in;
    }
    .enter-from-or-leave-to {
      top: -300px;
    }
    .enter-to-or-leave-from {
      top: 0px;
    }
    .modal-dialog {
      position: relative;
      background-color: white;
      display: flex;
      flex-direction: column;
      width: 100%;
      height: 80%;
      overflow: hidden;
      padding: 50px 80px 10px 80px;
      @media screen and (max-width: 375px) {
        padding: 10px 10px;
        height: 100%;
      }
      .modal-dialog-close {
        display: none;
        @media screen and (max-width: 375px) {
          display: flex;
          justify-content: space-between;
          margin-bottom: 10px;
          h6 {
            font-family: "Mulish", sans-serif;
            font-size: 14px;
            font-weight: 700;
          }
        }
      }
      .modal-dialog-box {
        @media screen and (max-width: 375px) {
          flex-direction: column;
        }
        width: inherit;
        height: 100%;
        display: flex;
        .modal-dialog-location {
          -webkit-tap-highlight-color: transparent;
          cursor: pointer;
          padding: 10px 10px;
          border-radius: 10px;
          background-color: white;
          box-shadow: 1px 0px 5px -4px white;
          -webkit-box-shadow: 1px 0px 5px -4px rgba(0, 0, 0, 0.75);
          -moz-box-shadow: 1px 0px 5px -4px rgba(0, 0, 0, 0.75);
          width: 33%;
          @media screen and (max-width: 375px) {
            width: 100%;
          }
          height: 60px;
          h6:nth-child(1) {
            font-family: "Mulish", sans-serif;
            font-size: 12px;
            font-weight: 800;
          }
          h6:nth-child(2) {
            font-family: "Mulish", sans-serif;
            font-size: 14px;
            font-weight: 400;
            color: #bdbdbd;
          }

          .modal-dialog-fill-box-location {
            width: 100%;
            height: 100%;
            margin: 50px 0px;
            display: flex;
            cursor: pointer;
            -webkit-tap-highlight-color: transparent;
            @media screen and (max-width: 375px) {
              margin: 100px 0px;
            }
            flex-direction: column;
            .modal-dialog-fill-box-location-text {
              padding-bottom: 30px;

              @media screen and (max-width: 375px) {
                padding-bottom: 30px;
              }
              width: 100%;
              display: flex;
              color: #4f4f4f;
              align-items: center;
              font-family: "Mulish", sans-serif;
              font-size: 14px;
              font-weight: 400;
              cursor: pointer;
              &:hover {
                color: lightblue;
              }
            }
          }
          &:hover {
            border: 1px solid black;
            color: black;
          }
        }
        .modal-dialog-guest {
          @media screen and (max-width: 375px) {
            width: 100%;
          }

          -webkit-tap-highlight-color: transparent;
          width: 33%;
          height: 60px;
          cursor: pointer;
          box-shadow: 1px 0px 5px -4px white;
          -webkit-box-shadow: 1px 0px 5px -4px rgba(0, 0, 0, 0.75);
          -moz-box-shadow: 1px 0px 5px -4px rgba(0, 0, 0, 0.75);
          padding: 10px 10px;
          border-radius: 10px;

          h6:nth-child(1) {
            font-family: "Mulish", sans-serif;
            font-size: 12px;
            font-weight: 800;
          }
          h6:nth-child(2) {
            font-family: "Mulish", sans-serif;
            font-size: 14px;
            font-weight: 400;
            color: #bdbdbd;
          }

          .modal-dialog-fill-box-guest {
            padding: 40px 0px;
            width: 100%;
            height: inherit;
            display: flex;
            flex-direction: column;

            -webkit-tap-highlight-color: transparent;
          }
          &:hover {
            border: 1px solid black;
            color: black;
          }
        }
        .modal-dialog-button {
          width: 33%;
          @media screen and (max-width: 375px) {
            width: 100%;
            position: absolute;
            bottom: 0;
          }

          height: 60px;
          display: flex;
          align-items: center;
          justify-content: center;
          .modal-dialog-button-with-icon {
            color: white;
            display: flex;
            cursor: pointer;
            align-items: center;

            @media screen and (max-width: 375px) {
              width: 33%;
            }
            padding: 10px 20px;
            border-radius: 10px;
            border: none;
            background-color: #eb5757;
            &:hover {
              background: white;
              border: 1px solid #eb5757;
              color: black;
              .but-icon {
                color: black;
              }
            }
          }
        }
      }
    }
  }
}
</style>
