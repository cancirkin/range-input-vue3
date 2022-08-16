<template>
  <div class="h-screen flex justify-center items-center">
    <div class="relative max-w-xl w-full p-3">
      <div class="mb-2 flex justify-between">
        <span>{{ labels[0] }}</span>
        <span>{{ labels[1] }}</span>
      </div>
      <div>
        <input
          type="range"
          :step="step"
          :min="min"
          :max="max"
          @input="minTrigger"
          :value="modelValue[0]"
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
          :step="step"
          :min="min"
          :max="max"
          :value="modelValue[1]"
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
            :value="modelValue[0]"
            @input="minTrigger"
            class="px-3 py-2 border border-gray-200 rounded w-24 text-center"
          />
        </div>
        <div>
          <input
            type="text"
            @input="maxTrigger"
            maxlength="5"
            :value="modelValue[1]"
            class="px-3 py-2 border border-gray-200 rounded w-24 text-center"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  emits: ['update:modelValue'],
  props: {
    labels: {
      type: Array,
      default: () => ['', ''],
    },
    min: {
      type: Number,
      required: true,
    },
    step: {
      type: Number,
      required: true,
    },
    modelValue: {
      type: Array,
      required: true,
    },
    max: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      minThumb: 0,
      maxThumb: 0,
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.minTrigger();
      this.maxTrigger();
    },
    minTrigger(e) {
      //max value'yi geçmemesi için. Sondaki eksi de aralarındaki mesafeyi ayarlamak için
      this.modelValue[0] = Math.min(
        e ? e.target.value : this.modelValue[0],
        this.modelValue[1] - this.step
      );
      this.minThumb =
        ((this.modelValue[0] - this.min) / (this.max - this.min)) * 100;
      this.$emit('update:modelValue', [this.modelValue[0], this.modelValue[1]]);
    },
    maxTrigger(e) {
      //min value'den daha düşük olmaması için.
      this.modelValue[1] = Math.max(
        e ? e.target.value : this.modelValue[1],
        this.modelValue[0] + this.step
      );
      this.maxThumb =
        100 - ((this.modelValue[1] - this.min) / (this.max - this.min)) * 100;
      this.$emit('update:modelValue', [this.modelValue[0], this.modelValue[1]]);
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