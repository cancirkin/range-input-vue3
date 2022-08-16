<script>
import RangeInput from './components/RangeInput.vue';
export default {
  components: {
    RangeInput,
  },
  data() {
    return {
      data: [4, 8],
      labels: ['Min', 'Max'],
    };
  },
};
</script>

<template>
  <RangeInput
    v-model="data"
    :min="1"
    :max="10"
    :step="1"
    :labels="labels"
  ></RangeInput>
</template>

