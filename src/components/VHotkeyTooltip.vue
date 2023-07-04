<template>
  <transition>
    <div ref="hotkeyRef" class="hotkey" v-show="isTooltip" :style="hotkeyStyle">
    <div class="wrap">
      <slot></slot>
    </div>
  </div>
  </transition>
</template>

<script lang="ts">
import {
  ref, watch, getCurrentInstance, onMounted,
} from 'vue';
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
    console.log('=== created ===');
    const isTooltip = ref(false);
    const magicKey = useMagicKeys();
    const runEvent = () => {
      emit('handleHotkey');
    };

    watch(magicKey[props.showKey], (v) => {
      if (v) {
        isTooltip.value = true;
      } else {
        isTooltip.value = false;
      }
    });

    watch(magicKey[props.keys], (v) => {
      if (v) {
        console.log(props.keys);
        runEvent();
      }
    });

    const instance = getCurrentInstance();
    const hotkeyStyle = ref({
      top: '0px',
      left: '0px',
    });

    onMounted(() => {
      const parent = instance?.vnode?.el?.parentElement;
      const parentTop = parent?.getBoundingClientRect().top ?? 0;
      const parentHeight = parent?.getBoundingClientRect().height ?? 0;
      const parentTopLeft = parent?.getBoundingClientRect().left ?? 0;
      const parentTopwidth = parent?.getBoundingClientRect().width ?? 0;
      hotkeyStyle.value = {
        top: `${parentTop + (parentHeight / 2) - (32.5 / 2)}px`,
        left: `${parentTopLeft + parentTopwidth}px`,
      };
    });

    return {
      isTooltip,
      hotkeyStyle,
    };
  },
};
</script>
<style scoped>
.hotkey {
  position: absolute;
  display: inline-block;
  padding: 5px 10px;
  border-radius: 3px;
  background-color: #3a384e;
  color: #ebebeb;
  font-size: 15px;
  line-height: 1.5;
  top:0px;
  left:0px;
  margin-left:10px;
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
