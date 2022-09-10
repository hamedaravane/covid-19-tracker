<template>
  <select
    v-model="selected"
    class="form-select mt-10 block border p-3 rounded w-full"
    @change="onChange()"
  >
    <option value="0">Select Country</option>
    <option v-for="country in countries" :value="country.ID" :key="country.ID">
      {{ country.Country }}
    </option>
  </select>
</template>

<script>
export default {
  name: "CountrySelect",
  props: ["countries"],
  data() {
    return {
      selected: 0,
    };
  },
  methods: {
    onChange() {
      this.$emit("get-country", country);
      const country = countries.find((item) => item.ID === selected.value);
    },
  },
};
</script>

<style>
</style>