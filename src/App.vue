<script>
import RangeInput from './components/RangeInput.vue';
export default {
  components: {
    RangeInput,
  },
  data() {
    return {
      data: [1700, 4000],
      labels: ['Min', 'Max'],
    };
  },
};
</script>

<template>
  <RangeInput
    v-model="data"
    :min="100"
    :max="5000"
    :step="100"
    :labels="labels"
  ></RangeInput>
</template>

