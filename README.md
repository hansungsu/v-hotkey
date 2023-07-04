# hotkey
## 사용방법
### 1. 마크업
```javascript
import VHotkey from './VHotkeyTooltip.vue';

<h3>
    <v-hotkey keys="Ctrl_alt_s" showKey="Ctrl" @handleHotkey="handleHotkey1">
        Ctrl + Alt + S
    </v-hotkey>
</h3>
```
### 2. keys
`keys`는 vueuse의 `useMagicKeys`의 _ 를 이용해 합성할 단축키를 입력합니다.

### 3. showKey
`showKey`는 tooltip을 노출할 시작키를 입력합니다.

### 4. @handelHotkey
`@handelHotkey`는 부모 컴포넌트에서 `keys`에 입력된 단축키가 실행될때 실행할 methods를 넣어주세요.
### 예외사항
`<v-hotkey>`가 삽입될 부모 component는 `display:inline-block;` 이어야 합니다.


## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```
