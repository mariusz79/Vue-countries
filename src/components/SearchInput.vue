<template>
  <input
    id="search"
    name="search"
    placeholder="Search for a country"
    :class="{ dark }"
    :value="value"
    @keyup.enter="handleChange"
  />
</template>
<script>
export default {
  name: 'SearchInput',
  props: {
    value: {
      type: String,
      required: true,
    },
    dark: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    handleChange(e) {
      this.$emit('input', e.target.value);
    },
  },
};
</script>
<style lang="scss">
*{
--var-dark-blue: #2B3744;
--var-darker-blue: hsl(207, 26%, 17%);
--var-very-dark-blue: #202D36;
--var-dark-gray: hsl(0, 0%, 52%);
--var-very-light-gray: hsl(0, 0%, 98%);
--var-white: hsl(0, 0%, 100%);
}
  input {
    text-indent: 20px;
    width: 200px;
    height: 30px;
    border: none;
    background-image: url('https://mariusz-ecommerce.s3-eu-west-1.amazonaws.com/static/Vue/search-line.svg');
    background-repeat: no-repeat;
    background-size: 16px;
    background-position-y: 5px;
    background-color: var(--var-white);
  }
  input:focus {
    outline: none;
    box-shadow: 0 10px 20px -8px rgba(255,255,255, .5);
  }

  .dark {
    background-image: url('https://mariusz-ecommerce.s3-eu-west-1.amazonaws.com/static/Vue/search-black.svg');
    background-repeat: no-repeat;
    background-size: 16px;
    background-position-y: 5px;
    background-color: var(--var-dark-blue);
    color: var(--var-very-light-gray);
  }
</style>
