<template>
  <input
    id="search"
    name="search"
    placeholder="Search for a country ..."
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
    text-indent: 3rem;
    width: 40vw;
    border: none;
    border-radius: 3px;
    background-image: url('https://mariusz-ecommerce.s3-eu-west-1.amazonaws.com/static/Vue/search-line.svg');
    background-repeat: no-repeat;
    background-size: 16px;
    background-position-y: 1rem;
    background-position-x: 1rem;
    background-color: var(--var-white);
  }
  input:focus, input:hover {
    outline: none;
    -webkit-box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
    -moz-box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
    box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
  }

  input .dark:focus, input .dark:hover {
    outline: none;
    -webkit-box-shadow: 0 0px 2px 1px rgba(255,255,255, .5);
    -moz-box-shadow: 0 0px 2px 1px rgba(255,255,255, .5);
    box-shadow: 0 0px 2px 1px rgba(255,255,255, .5);
  }

  .dark {
    background-image: url('https://mariusz-ecommerce.s3-eu-west-1.amazonaws.com/static/Vue/search-black.svg');
    background-repeat: no-repeat;
    background-size: 16px;
    background-position-y: 1rem;
    background-position-x: 1rem;
    background-color: var(--var-dark-blue);
    color: var(--var-very-light-gray);
  }
</style>
