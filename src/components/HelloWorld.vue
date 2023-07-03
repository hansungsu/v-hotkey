<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-typescript" target="_blank" rel="noopener">typescript</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>

    </ul>
    <h3 v-hotkey="'alt_a'">Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
  </div>

</template>

<script lang="ts">
import { defineComponent, watch, ref } from 'vue';
import { useMagicKeys } from '@vueuse/core';

export default defineComponent({
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  setup() {
    const keys = useMagicKeys({ reactive: true });

    /* eslint-disable */
    // const { alt_a, alt_s } = useMagicKeys();
    // watch(alt_a, (v) => {
    //   if (v) {
    //     console.log('Alt + A Clicked');
    //   }
    // });
    // watch(alt_s, (v) => {
    //   if (v) {
    //     console.log('Alt + B Clicked');
    //   }
    // });

    return { keys };
  },
  directives: {
    hotkey: {
      created(el,binding, vnode){
        console.log('created')
        console.log(binding.value)
      },
      mounted(el, binding) {
        console.log(binding)
        console.log(el)

        //binding.value 가 'alt_s' 이면 alt_s 키를 누르면 binding.value() 함수를 실행한다.
        const { alt_a, alt_s, alt } = useMagicKeys();
        const test = ref(false);
        watch(alt, (v) => {
          if (v) {
            const newElement = document.createElement('div');
            newElement.innerHTML = '<div v-if="test">You are holding the Alt key!</div>';
            el.appendChild(newElement);
          }
        });
        watch(alt_a, (v) => {
          if (v) {
            console.log('Alt + A Clicked')
          }
        })
        watch(alt_s, (v) => {
          if (v) {
            console.log('Alt + S Clicked')
          }
        });
      },
    },
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
