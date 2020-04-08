<template>
  <div id="app" :class="{'light-app':!darkOn, 'dark-app':darkOn }">
    <div id="nav" :class="{'light-nav':!darkOn, 'dark-nav':darkOn }">
      <router-link to="/" :class="{'light-nav':!darkOn,
       'dark-nav':darkOn }"><h2>Where in the  world?</h2></router-link>
      <div id="mode" @click="darkMode()">
        <img :src="darkOn? moonBlack : moonLine"
          alt="moon icon">
        <p>Dark Mode</p>
      </div>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
  },
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
#app{
  font-family: 'Nunito Sans', sans-serif;
}
#nav{
  display: flex;
  justify-content: space-between;

  & a {
    text-decoration: none;
  }

  & #mode {
    cursor: pointer;
    display: flex;
    align-items: center;
  }

  & img {
    max-height: 15px;
    padding-right: 5px;
  }
}

.light-nav{
  background: var(--var-white);
  color: var(--var-very-dark-blue);
}

.dark-nav{
  background: var(--var-dark-blue);
  color: var(--var-very-light-gray);
}

.light-app{
  background: var(--var-very-light-gray);
}

.dark-app{
  background: var(--var-very-dark-blue);
}
</style>
