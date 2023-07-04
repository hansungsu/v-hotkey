<template>
  <transition>
    <div ref="hotkeyRef" class="hotkey" :style="hotkeyStyle" :class="isTooltip?'visiblity':'' ">
    <div class="wrap">
      <slot></slot>
    </div>
  </div>
  </transition>
</template>

<script lang="ts">
import {
  ref, watch, getCurrentInstance, onMounted, reactive, onBeforeUnmount,
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
    const hotkeyRef = ref<any>(null);

    const tooltipPosition = () => {
      const parent = instance?.vnode?.el?.parentElement;
      const parentTop = parent?.getBoundingClientRect().top ?? 0;
      const parentHeight = parent?.getBoundingClientRect().height ?? 0;
      const parentTopLeft = parent?.getBoundingClientRect().left ?? 0;
      const parentTopwidth = parent?.getBoundingClientRect().width ?? 0;

      const nodeSize = hotkeyRef.value.offsetWidth;

      hotkeyStyle.value = {
        top: `${parentTop + parentHeight}px`,
        left: `${parentTopLeft + (parentTopwidth / 2) - (nodeSize / 2)}px`,
      };
    };

    // tooltip resize event
    const resizeData = reactive({
      windowWidth: window.innerWidth,
      windowHeight: window.innerHeight,
    });

    const handleResize = () => {
      resizeData.windowWidth = window.innerWidth;
      resizeData.windowHeight = window.innerHeight;
      console.log(resizeData.windowWidth, resizeData.windowHeight);
      tooltipPosition();
    };

    onMounted(() => {
      window.addEventListener('resize', handleResize);
    });

    onBeforeUnmount(() => {
      window.removeEventListener('resize', handleResize);
    });

    // tooltip position
    onMounted(() => {
      tooltipPosition();
    });

    return {
      isTooltip,
      hotkeyStyle,
      hotkeyRef,
      resizeData,
    };
  },
};
</script>
<style scoped>
.hotkey {
  position: absolute;
  display: inline-block;
  visibility: hidden;
  /* display: none; */
  padding: 5px 10px;
  border-radius: 3px;
  background-color: #3a384e;
  color: #ebebeb;
  font-size: 15px;
  line-height: 1.5;
  top:0px;
  left:0px;
  margin-top:10px;
  opacity:0;
  transition:opacity 0.5s ease;
}
.visiblity{
  visibility:visible;
  opacity: 1;
  transition:opacity 0.5s ease;
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
