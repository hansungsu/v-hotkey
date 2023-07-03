<template>
  <div v-if="isTooltip">
    <div>
      <slot></slot>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, watch, inject } from 'vue';
import { useMagicKeys } from '@vueuse/core';

export default {
  props: {
    keys: {
      type: String,
      required: true,
    },
    showKey: {
      type: String,
      required: true,
    },
  },
  emits: ['handleHotkey'],
  setup(props : any, { emit } : any) {
    const isTooltip = ref(false);
    /* eslint-disable */

    const { Ctrl_alt_a, Ctrl_alt_s, Ctrl_alt_d, Ctrl } = useMagicKeys();
    const testKeys = useMagicKeys();

    const runEvent = () =>  {
      emit('handleHotkey');
    }

    watch(testKeys[props.showKey], (v) => {
      if (v) {
        isTooltip.value = true;
      } else {
        isTooltip.value = false;
      }
    });

    watch(testKeys[props.keys], (v) => {
      if (v) {
        console.log(props.keys);
        runEvent();
      }
    })
    return {
      isTooltip,
    };
  },
  methods: {

  }
};
</script>
