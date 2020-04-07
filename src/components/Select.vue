<template>
  <div class="c-select">
    <label for="selector">
      Filter:
      <select
        @change="$emit('change', selected)"
        v-model="selected"
        id="selector"
      >
        <option
          v-for="item in items"
          :key="item.key"
          :value="item.key"
          v-text="item.name"
        ></option>
      </select>
    </label>
  </div>
</template>

<script>
export default {
  name: "Select",
  props: {
    items: {
      required: true
    }
  },
  data() {
    return {
      selected: this.items[0].key
    };
  }
};
</script>
