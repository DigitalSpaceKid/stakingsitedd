<template>
  <button :disabled="disabled">
    <svg fill="#6D0083" stroke="#ae86b6"   stroke-width="1" width="120"
      height="120" :class=" { left: left, disabled: disabled }" viewBox="0 0 20 20">
            <path d="M8.59,16.59L13.17,12L8.59,7.41L10,6l6,6l-6,6L8.59,16.59z"></path>

      alt="right"
      :width="120"
      :height="120"
      :class="{ left: left, disabled: disabled }"
    </svg>
  </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
  props: {
    left: Boolean,
    disabled: Boolean,
  },
  setup() {
    return {};
  },
});
</script>

<style scoped>
button,
img {
  outline: none !important;
}

.left {
  transform: scaleX(-1);
}
.disabled {
  opacity: 0;
}
</style>
