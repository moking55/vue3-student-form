<script setup>
import { reactive } from "vue";

const props = defineProps({
  default: String,

  digitCount: {
    type: Number,
    required: true,
  },
});

const digits = reactive([]);

if (props.default && props.default.length === props.digitCount) {
  for (let i = 0; i < props.digitCount; i++) {
    digits[i] = props.default.charAt(i);
  }
} else {
  for (let i = 0; i < props.digitCount; i++) {
    digits[i] = null;
  }
}
</script>
<template>
  <div ref="stdIdCont">
    <a-space
      direction="horizontal"
      :style="{ width: '100%' }"
      compact
      :size="11"
    >
      <a-input
        type="text"
        class="digit-box"
        v-for="(el, ind) in digits"
        :key="el + ind"
        v-model="digits[ind]"
        :autofocus="ind === 0"
        :maxlength="1"
      />
    </a-space>
  </div>
</template>
