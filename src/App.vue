<template>
  <div class="inner-wrapper">
    <app-select :items="items" @selected="value" placeholder="выберите элемент" :sort="true" > </app-select>
  </div>
</template>

<script>
import appSelect from "./components/input.vue";
export default {
  name: "App",
  components: {
    appSelect,
  },
  data() {
    return {
      items: [
        { value: "val1", text: "Узбекистан" },
        { value: "val2", text: "Россия" },
        { value: "val3", text: "Германия" },
        { value: "val4", text: "Бразилия" },
        { value: "val5", text: "Грузия" },
      ],
    };
  },
  methods: {
    value(val) {
      console.log(val);
    },
  },
};
</script>

<style>
.inner-wrapper {
  /* width: 100%;
  height: 100vh;
  display: flex;
  padding-top: 100px;
  justify-content: center; */
}
</style>
