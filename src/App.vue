<script>
import RangeInput from './components/RangeInput.vue';
export default {
  components: {
    RangeInput,
  },
  data() {
    return {
      data: [1500, 4000],
    };
  },
};
</script>

<template>
  <RangeInput :v-model="data"></RangeInput>
</template>

