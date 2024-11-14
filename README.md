# vue-3-mask-updated

A simple input mask library for Vue.js 3, inspired by `vue-3-mask` by Alisson Ryan. This library allows you to create input masks for fields in your Vue.js 3 application easily.

For the original version, Go to

https://www.npmjs.com/~alissonrayan

## Installation

Install the library using npm or yarn:

```bash
npm install vue-3-mask-updated
```

or 

```
yarn add vue-3-mask-updated
```

## Usage

### Global registration

In your Vue 3 application, open the `main.js` (or `main.ts`) file and import the `MaskInput` component from the `vue-3-mask` library. Register it globally:

```javascript
import { createApp } from 'vue';
import App from './App.vue';
import { MaskInput } from 'vue-3-mask';

const app = createApp(App);

app.component('MaskInput', MaskInput);

app.mount('#app');
```

Now you can use the MaskInput component throughout your application.



### Local registration

If you prefer to register the component locally, you can do so in the components section of your Vue component:

```javascript
import { MaskInput } from 'vue-3-mask';

export default {
  components: {
    MaskInput,
  },
};
```

## Using MaskInput in your components

Using MaskInput in your components
To use the MaskInput component in your Vue components, simply include it in your template and provide a mask prop with the desired pattern. The following mask patterns are supported:

```javascript
#: Any digit (0-9)
X: Any alphanumeric character (0-9, a-z, A-Z)
S: Any alphabetic character (a-z, A-Z)
A: Any uppercase alphabetic character (A-Z)
a: Any lowercase alphabetic character (a-z)
!: Escape character (use the next character in the mask as a literal)
```

## Attribute description

| Attribute | Required | Default | Values | Description |
|-----------|--------- |---------|--------|-------------|
| mask | Yes | None |As given in the above table | This is the most important attribute without which the mask will not work. Please follow the above guidelines to create a mask. |
| class | No | None | - | You can assign classes to the input field. |
| id | No | None | - | You can assign an ID to the input field. |
| placeholder | No | empty | - | You can assign a placeholder to the input field. |
| readonly | No | false/empty | - | You can make the field read only. |
| v-model | Yes | None | - | You can set the v-model of the input element. |
| textmode | No | empty | uppercase - If the text should be converted to upppercase. Eg. NEW YORK<br>lowercase - If the text should be converted to lowercase. Eg. new york<br>sentencecase - If the text should be converted to sentence case. Eg. New york |

Here's an example of using the MaskInput component with a phone number mask:

```vue
<template>
  <div>
    <h1>Phone number input example</h1>
    <MaskInput v-model="phoneNumber" mask="(##) ####-####" class="form-control"/>
  </div>
</template>

<script>
export default {
  data() {
    return {
      phoneNumber: '',
    };
  },
};
</script>

```

In this example, the MaskInput component will enforce the provided phone number mask as the user types, and the masked value will be stored in the phoneNumber data property.

You will be able to assign classes to the component.

## Contributing

We welcome contributions to the vue-3-mask-updated library. If you find a bug or have an idea for a new feature, please feel free to open an [issue](https://github.com/briquetech/vue-3-mask-updated/issues).