<template>
  <main class="main">
    <div class="wrapper">
      <form @submit.prevent="generate" @input.passive="generate">
        <h1>Passwords generator</h1>
        <p>
          Made by Jakub Suchenek (<a
            href="https://github.com/AnonymousX86/passwords-generator-vue"
            >GitHub</a
          >)
        </p>
        <label for="length">
          <span>Length &mdash; {{ options.length }}</span>
          <input
            type="range"
            min="6"
            max="50"
            step="1"
            id="length"
            v-model.number="options.length"
          />
        </label>
        <label for="lowercase" title="Forced 'yes' for safety">
          <span>Use lowercase</span>
          <input
            type="checkbox"
            id="lowercase"
            disabled
            v-model="options.useLowercase"
          />
          {{ boolText(options.useLowercase) }}
        </label>
        <label for="uppercase">
          <span>Use uppercase</span>
          <input
            type="checkbox"
            id="uppercase"
            v-model="options.useUppercase"
          />
          {{ boolText(options.useUppercase) }}
        </label>
        <label for="numbers">
          <span>Use numbers</span>
          <input type="checkbox" id="numbers" v-model="options.useNumbers" />
          {{ boolText(options.useNumbers) }}
        </label>
        <label for="symbols">
          <span>Use symbols</span>
          <input type="checkbox" id="symbols" v-model="options.useSymbols" />
          {{ boolText(options.useSymbols) }}
        </label>
        <label for="quantity">
          <span>Quantity &mdash; {{ options.quantity }}</span>
          <input
            type="range"
            min="1"
            max="20"
            step="1"
            id="quantity"
            v-model.number="options.quantity"
          />
        </label>
        <input type="submit" value="Generate" />
      </form>
      <div class="result">
        <h2>Result</h2>
        <p v-for="(password, index) of options.passwordsList" :key="index">
          <code>{{ password }}</code>
        </p>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from "vue";

export default defineComponent({
  name: "Home",
  setup() {
    // noinspection SpellCheckingInspection
    const lowercase: string[] = [..."qwertyuiopasdfghjklzxcvbnm"];
    // noinspection SpellCheckingInspection
    const uppercase: string[] = [..."QWERTYUIOPASDFGHJKLZXCVBNM"];
    const numbers: string[] = [..."0123456789"];
    const symbols: string[] = [..."!@#$%^&*-_=+()[]{};:,.?"];

    const options = ref({
      length: 16,
      useLowercase: true,
      useUppercase: true,
      useNumbers: true,
      useSymbols: true,
      quantity: 1,
      passwordsList: [""],
    });

    const chars = computed(() => {
      const { useNumbers, useLowercase, useUppercase, useSymbols } =
        options.value;
      let newArr: string[] = [];
      if (useLowercase) newArr = [...lowercase];
      if (useUppercase) newArr = [...newArr, ...uppercase];
      if (useNumbers) newArr = [...newArr, ...numbers];
      if (useSymbols) newArr = [...newArr, ...symbols];
      if (!newArr.length) {
        return lowercase;
      } else {
        return newArr;
      }
    });

    const generate = () => {
      const { quantity, length } = options.value;
      let newPasswords: string[] = [];
      let tempPassword = "";
      for (let i = 0; i < quantity; i++) {
        tempPassword = "";
        for (let j = 0; j < length; j++) {
          tempPassword +=
            chars.value[Math.floor(Math.random() * chars.value.length)];
        }
        newPasswords.push(tempPassword);
      }
      options.value.passwordsList = newPasswords;
    };

    const boolText = (value: boolean) => {
      return value ? "Yes" : "No";
    };

    return { options, generate, boolText };
  },
  created() {
    this.generate();
  },
});
</script>

<style lang="scss" scoped>
.rounded {
  border-radius: 8px;
}

.main {
  display: flex;
  padding: 64px 0;
  min-height: 100vh;
  justify-content: center;
  align-items: flex-start;

  .wrapper {
    display: flex;
    width: 585px + 326px + 64px;
    justify-content: space-between;
    align-items: flex-start;
    gap: 64px;
  }

  form,
  .result {
    padding: 32px;
    background-color: white;
    box-shadow: 3px 3px 8px #121212;
    @extend .rounded;

    h1,
    h2,
    p {
      margin-top: 0;
    }
  }

  .result {
    text-align: right;

    p {
      margin-bottom: 8px;

      code {
        font-family: "Ubuntu Mono", monospace;
        background-color: #eee;
        padding: 3px 9px;
        border-radius: 50px;
      }
    }
  }

  form {
    display: flex;
    flex-direction: column;
    gap: 8px;

    label {
      border-bottom: solid #cccccc 1px;
      padding-bottom: 8px;

      &:last-of-type {
        border-bottom: hidden;
        padding-bottom: 20px;
      }
    }

    span {
      display: block;
    }

    input {
      &[type="range"] {
        width: 100%;
        margin-top: 4px;
      }

      &[type="submit"] {
        padding: 16px 0;
        background-color: dodgerblue;
        border: none;
        color: white;
        cursor: pointer;
        font-size: 1.125rem;
        @extend .rounded;
      }
    }
  }
}
</style>
