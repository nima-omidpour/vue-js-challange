<template>
  <div>
    <div class="container">
      <div class="multi-select-parent">
        <p class="header">Select Users</p>

        <div class="flex-row">
          <Multiselect
            @input="updateSelectedOptions"
            :options="options"
            :value="selectedOptions"
            :minSelected="2"
            :maxSelected="4"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Multiselect from "./Multiselect.vue";

export default {
  components: { Multiselect },
  data() {
    return {
      formStatus: {
        error: false,
      },
      selectedOptions: [],
      options: ["Asma", "Reza", "Nima", "David", "Sam"],
    };
  },
  methods: {
    updateSelectedOptions(value) {
      this.selectedOptions = [...value];
    },
  },
};
</script>

<style scoped>
.container {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  text-align: center;
  width: fit-content;
  height: fit-content;
  max-width: 22em;
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 5px 5px rgba(0, 0, 0, 0.2);
  background: #fff;
  z-index: 999;
  transform: none;
  background: linear-gradient(
    135deg,
    rgba(65, 184, 131, 0.9),
    rgba(52, 73, 94, 0.9)
  );
}

.flex-row {
  display: flex;
  margin-bottom: 1em;
}

.multi-select-parent {
  border-radius: 0 0 10px 10px;
  min-width: 300px;
  padding: 2em;
  margin-top: 30px;
  position: relative;
  background: rgba(0, 0, 0, 0.15);
}
.multi-select-parent::before {
  content: "";
  position: absolute;
  top: -2px;
  left: 0;
  height: 2px;
  width: 100%;
  background: #35c3c1;
}

@media screen and (min-width: 640px) {
  .multi-select-parent {
    width: 50vw;
    max-width: 15em;
  }
}

@media screen and (max-width: 600px) {
  .multi-select-parent {
    padding: 1.2em;
  }
}
@media screen and (max-width: 480px) {
  .header {
    font-size: 18px;
  }
  .multi-select-parent {
    margin: 0 auto;
    margin-top: 89px;
    background-color: unset;
    min-width: unset;
    max-width: 93vw;
    padding: 0.3em;
  }
  .container {
    height: 100%;
    width: 100%;
    padding: 0 0;
    border-radius: 0;
  }
}
</style>
