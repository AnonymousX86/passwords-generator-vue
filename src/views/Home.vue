<template>
  <main class="main">
    <form @submit.prevent="generate">
      <label for="length">
        <span>Length</span>
        <input
          type="number"
          min="6"
          max="50"
          id="length"
          v-model.number="options.length"
        />
      </label>
      <label for="numbers">
        <span>Use numbers</span>
        <input type="checkbox" id="numbers" v-model="options.useNumbers" />
      </label>
      <label for="lowercase">
        <span>Use lowercase</span>
        <input type="checkbox" id="lowercase" v-model="options.useLowercase" />
      </label>
      <label for="uppercase">
        <span>Use uppercase</span>
        <input type="checkbox" id="uppercase" v-model="options.useUppercase" />
      </label>
      <label for="symbols">
        <span>Use symbols</span>
        <input type="checkbox" id="symbols" v-model="options.useSymbols" />
      </label>
      <label for="quantity">
        <span>Quantity</span>
        <input
          type="number"
          min="1"
          max="50"
          id="quantity"
          v-model.number="options.quantity"
        />
      </label>
      <textarea
        id="password"
        readonly
        v-model="options.password"
        style="resize: none"
        :rows="options.quantity + 1"
      />
      <input type="submit" value="Generate" />
    </form>
  </main>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from "vue";

export default defineComponent({
  name: "Home",
  setup() {
    const numbers: string[] = [..."0123456789"];
    // noinspection SpellCheckingInspection
    const lowercase: string[] = [..."qwertyuiopasdfghjklzxcvbnm"];
    // noinspection SpellCheckingInspection
    const uppercase: string[] = [..."QWERTYUIOPASDFGHJKLZXCVBNM"];
    const symbols: string[] = [..."!@#$%^&*-_=+()[]{};:,.?"];

    const options = ref({
      length: 16,
      useNumbers: true,
      useLowercase: true,
      useUppercase: true,
      useSymbols: true,
      quantity: 1,
      password: "",
    });

    const chars = computed(() => {
      const { useNumbers, useLowercase, useUppercase, useSymbols } =
        options.value;
      let newArr: string[] = [];
      if (useNumbers) newArr = [...numbers];
      if (useLowercase) newArr = [...newArr, ...lowercase];
      if (useUppercase) newArr = [...newArr, ...uppercase];
      if (useSymbols) newArr = [...newArr, ...symbols];
      return newArr;
    });

    const generate = () => {
      const { quantity, length } = options.value;
      options.value.password = "";
      for (let i = 0; i < quantity; i++) {
        for (let j = 0; j < length; j++) {
          options.value.password +=
            chars.value[Math.floor(Math.random() * chars.value.length)];
        }
        options.value.password += "\n";
      }
    };

    return { options, generate };
  },
});
</script>

<style lang="scss" scoped>
.rounded {
  border-radius: 2px;
}

.main {
  display: flex;
  align-content: center;
  padding: 64px 0;

  form {
    background-color: #cccccc;
    padding: 32px;
    @extend .rounded;
    width: 500px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    grid-template-columns: repeat(2, 1fr);

    label {
      padding: 16px 0;
      border-bottom: dotted grey 1px;
    }

    textarea {
      padding: 8px;
      margin-top: 16px;
      @extend .rounded;
    }

    input {
      border: solid #333 1px;
      @extend textarea;

      &[type="number"] {
        width: 100%;
        padding: 4px 8px;
      }

      &[type="submit"] {
        background-color: dodgerblue;
        color: white;
        font-size: 1.2rem;
        cursor: pointer;
      }
    }
  }
}
</style>
