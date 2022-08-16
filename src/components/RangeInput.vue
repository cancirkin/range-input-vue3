<template>
  <div class="h-screen flex justify-center items-center">
    <div class="relative max-w-xl w-full">
      <div>
        <input
          type="range"
          step="100"
          :min="min"
          :max="max"
          @input="minTrigger"
          v-model="minValue"
          class="
            absolute
            pointer-events-none
            appearance-none
            z-20
            h-2
            w-full
            opacity-0
            cursor-pointer
          "
        />

        <input
          type="range"
          @input="maxTrigger"
          step="100"
          :min="min"
          :max="max"
          v-model="maxValue"
          class="
            absolute
            pointer-events-none
            appearance-none
            z-20
            h-2
            w-full
            opacity-0
            cursor-pointer
          "
        />

        <div class="relative z-10 h-2">
          <div
            class="
              absolute
              z-10
              left-0
              right-0
              bottom-0
              top-0
              rounded-md
              bg-gray-200
            "
          ></div>

          <div
            class="absolute z-20 top-0 bottom-0 rounded-md bg-green-300"
            :style="'right:' + maxThumb + '%; left:' + minThumb + '%'"
          ></div>

          <div
            class="
              absolute
              z-30
              w-6
              h-6
              top-0
              left-0
              bg-green-300
              rounded-full
              -mt-2
              -ml-1
            "
            :style="'left: ' + minThumb + '%'"
          ></div>

          <div
            class="
              absolute
              z-30
              w-6
              h-6
              top-0
              right-0
              bg-green-300
              rounded-full
              -mt-2
              -mr-3
            "
            :style="'right: ' + maxThumb + '%'"
          ></div>
        </div>
      </div>
      <div class="flex justify-between items-center py-5">
        <div>
          <input
            type="text"
            maxlength="5"
            v-model="minValue"
            @input="minTrigger"
            class="px-3 py-2 border border-gray-200 rounded w-24 text-center"
          />
        </div>
        <div>
          <input
            type="text"
            @input="maxTrigger"
            maxlength="5"
            v-model="maxValue"
            class="px-3 py-2 border border-gray-200 rounded w-24 text-center"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    labels: {
      type: Array,
      default: () => ['Bad', 'Good'],
    },
    min: {
      type: Number,
      default: 100,
    },
    max: {
      type: Number,
      default: 10000,
    },
    step: {
      type: Number,
      default: 1,
    },
    values: {
      type: Array,
      default: () => [0, 0],
    },
  },
  data() {
    return {
      minValue: 1000,
      maxValue: 7000,
      minThumb: 0,
      maxThumb: 0,
    };
  },
  mounted() {
    this.init();
    if (this.values[0] > 0 || this.values[1] > 0) {
      this.minValue = this.values[0];
      this.maxValue = this.values[1];
    }
  },
  methods: {
    init() {
      this.minTrigger();
      this.maxTrigger();
    },
    minTrigger() {
      this.minValue = Math.min(this.minValue, this.maxValue - 500);
      this.minThumb =
        ((this.minValue - this.min) / (this.max - this.min)) * 100;
      this.$emit('input', [this.minValue, this.maxValue]);
    },
    maxTrigger() {
      this.maxValue = Math.max(this.maxValue, this.minValue + 500);
      this.maxThumb =
        100 - ((this.maxValue - this.min) / (this.max - this.min)) * 100;
      this.$emit('input', [this.minValue, this.maxValue]);
    },
  },
};
</script>

<style>
input[type='range']::-webkit-slider-thumb {
  pointer-events: all;
  width: 24px;
  height: 24px;
  -webkit-appearance: none;
  /* @apply w-6 h-6 appearance-none pointer-events-auto; */
}
</style> 