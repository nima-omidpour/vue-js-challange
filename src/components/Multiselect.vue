<template>
  <div class="multi-select-container">
    <div
      class="multiselect"
      @click="showOptions"
      @blur="focused = false"
      tabindex="1"
      ref="parent"
      :style="{ width: width }"
    >
      <span class="multi-select-title" v-if="!existsValue">
        please select users...
      </span>

      <div
        class="multiselect-selected"
        v-for="(options, i) in formattedOptions"
        :key="i"
        v-show="options.checked"
      >
        {{ options.option }}
        <span
          class="multiselect-remove"
          @click="
            preventClose($event);
            selectOption(i);
          "
          >&times;</span
        >
      </div>
      <div
        class="multiselect-options"
        v-show="focused"
        :style="{ top: optionsTop }"
        @click="preventClose"
      >
        <div
          class="multiselect-option"
          :class="{ 'multiselect-option--checked': option.checked }"
          v-for="(option, i) in formattedOptions"
          :key="i"
          @click="selectOption(i)"
        >
          <div class="multiselect-checkbox"></div>
          <div class="multiselect__text">
            {{ option.option }}
          </div>
        </div>
      </div>
    </div>

    <p class="error-message" v-show="errors.isMinValid">
      You should choose at least {{ minSelected }} users
    </p>
    <p class="error-message" v-show="errors.isMaxValid">
      You should choose less than {{ maxSelected }} users
    </p>
  </div>
</template>

<script>
export default {
  props: {
    width: {
      type: String,
      default: "100%",
    },
    options: {
      type: Array,
      default: () => [],
    },
    value: {
      type: Array,
      default: () => [],
    },

    minSelected: {
      type: Number,
    },
    maxSelected: {
      type: Number,
    },
  },
  data() {
    return {
      focused: false,
      optionsTop: "58px",
      errors: {
        isMinValid: false,
        isMaxValid: false,
      },
    };
  },
  watch: {
    value: function () {
      this.checkMin();
      this.checkMax();
    },
    focused: function () {
      this.checkMin();
      this.checkMax();
    },
  },

  computed: {
    formattedOptions() {
      let fo = this.options.map((option) => {
        let checked = this.value.some((v) => v === option);
        return { option, checked };
      });

      return fo;
    },
    existsValue() {
      return this.value.length ? true : false;
    },
  },
  methods: {
    checkMin() {
      if (!this.focused && this.value.length < this.minSelected) {
        if (!this.errors.isMinValid) {
          this.errors.isMinValid = !this.errors.isMinValid;
        }
      } else {
        this.errors.isMinValid = false;
      }
    },
    checkMax() {
      if (!this.focused && this.value.length > this.maxSelected) {
        if (!this.errors.isMinValid) {
          this.errors.isMaxValid = !this.errors.isMaxValid;
        }
      } else {
        this.errors.isMaxValid = false;
      }
    },

    fixTop() {
      this.optionsTop = this.$refs.parent.clientHeight + 2 + "px";
    },
    showOptions() {
      this.focused = !this.focused;
    },
    preventClose(e) {
      e.stopPropagation();
    },
    selectOption(i) {
      let clickedValue = this.options[i];
      let newValue = [...this.value];

      let existIndex = this.value.findIndex((v) => v === clickedValue);

      if (existIndex === -1) {
        newValue.push(clickedValue);
      } else {
        newValue.splice(existIndex, 1);
      }

      this.$emit("input", newValue);
      setTimeout(this.fixTop, 100);
    },
  },

  mounted() {
    this.fixTop();
  },
};
</script>

<style scoped>
.multi-select-container {
  display: flex;
  flex-direction: column;
  width: 100%;
}
.multiselect {
  background: #fff;
  padding: 1.4em;
  display: flex;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  min-height: 33px;
  min-width: 222px;
  position: relative;
  flex-wrap: wrap;
  font-size: 12px;
  cursor: pointer;
}
.multi-select-title {
  font-size: 14px;
}
.multiselect:focus {
  outline: none;
}
.multiselect__placeholder {
  outline: #929292;
}
.multiselect-selected {
  display: flex;
  align-items: center;
  gap: 5px;
  background: #eee;
  border: 1px solid #e0e0e0;
  padding: 4px 8px;
  padding-right: 0px;
  margin: 3px;
  border-radius: 5px;
}

.multiselect-remove {
  cursor: pointer;
  padding-right: 7px;
  font-size: 18px;
}
.multiselect-remove:hover {
  cursor: red;
  font-weight: bold;
}

.multiselect-options {
  position: absolute;
  top: 34px;
  right: 0;
  left: 0;
  display: flex;
  background: #fff;
  flex-direction: column;
  box-shadow: 0 3px 3px 2px #e3e3e3;
  padding: 5px 0;
  min-height: 55px;
  max-height: 188px;
  overflow-y: auto !important;
}

.multiselect-option {
  padding: 6px 11px;
  cursor: pointer;
  display: flex;
  align-items: center;
}
.multiselect-option--checked {
  color: #1f7bb8;
  font-weight: bold;
}

.multiselect-checkbox {
  width: 22px;
  height: 22px;
  border: 1px solid #969696;
  margin-right: 7px;
  position: relative;
}
.multiselect-option--checked .multiselect-checkbox {
  border: 1px solid #1f7bb8;
  background: #1f7bb8;
}

.multiselect-option--checked .multiselect-checkbox::after {
  width: 11px;
  height: 6px;
  border-left: 2px solid rgb(255, 255, 255);
  border-bottom: 2px solid rgb(255, 255, 255);
  content: "";
  z-index: 9999;
  position: absolute;
  transform: rotate(-45deg);
  left: 3px;
  top: 4px;
}
.error-message {
  text-align: left !important;
  margin: 12px 0 16px 8px;
  font-size: 14px;
  color: #fd6262;
  animation: errorFadeInDown 0.5s cubic-bezier(0.25, 0.8, 0.5, 1);
}
@keyframes errorFadeInDown {
  0% {
    -webkit-transform: translate3d(0, -10px, 0);
    transform: translate3d(0, -10px, 0);
  }

  59% {
    opacity: 1;
    transform: skewX(20deg);
  }
  70%,
  90% {
    transform: skewX(-20deg);
  }
  100% {
    -webkit-transform: none;
    transform: none;
  }
}
</style>
