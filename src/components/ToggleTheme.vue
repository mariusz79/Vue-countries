<template>
  <div id="mode" v-on:click='darkMode'>
            <img :src="darkOn? moonBlack : moonLine" alt="moon icon">
            <p class="mode-text">Dark Mode</p>
          </div>
</template>
<script>
export default {
  name: 'ToggleTheme',
  data() {
    return {
      darkOn: false,
      moonLine: 'https://mariusz-ecommerce.s3-eu-west-1.amazonaws.com/static/Vue/moon-line.svg',
      moonBlack: 'https://mariusz-ecommerce.s3-eu-west-1.amazonaws.com/static/Vue/moon-black.svg',
    };
  },
  methods: {
    darkMode() {
      this.darkOn = !this.darkOn;
      this.$emit('darkMode');
    },
  },
};
</script>
<style lang="scss">
</style>
