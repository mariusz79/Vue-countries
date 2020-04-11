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
    <div class="home">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <div class="searching">
      <SearchInput v-model="searchValue" @input="handleInput" :dark="darkOn"/>
      <SelectRegion v-model="searchValue" @input="handleInput" :dark1="darkOn"/>
    </div>
    <div class="results">
      <div v-for="key in result" :key="result[key]" class="item" :class="{'dark-info':darkOn}">
        <div class="top-flag">
          <img v-bind:src=key.flag class="flag">
        </div>
        <div class="info" >
          <h3>{{key.name}}</h3>
          <div><b>Population: </b><span>{{key.population}}</span></div>
          <div><b>Region: </b>{{key.region}}</div>
          <div><b>Capital: </b>{{key.capital}}</div>
        </div>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue';
import SearchInput from '@/components/SearchInput.vue';
import SelectRegion from '@/components/SelectRegion.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    HelloWorld, SearchInput, SelectRegion,
  },
  data() {
    return {
      darkOn: false,
      result: [],
      searchValue: '',
      moonLine: 'https://mariusz-ecommerce.s3-eu-west-1.amazonaws.com/static/Vue/moon-line.svg',
      moonBlack: 'https://mariusz-ecommerce.s3-eu-west-1.amazonaws.com/static/Vue/moon-black.svg',
    };
  },
  methods: {
    darkMode() {
      this.darkOn = !this.darkOn;
    },
    handleInput() {
      axios.get('https://restcountries.eu/rest/v2/region/europe')
        .then((response) => {
          this.result = response.data;
        });
    },
  },
  created() {
    this.handleInput();
  },
};
</script>

<style lang="scss">
*{
--var-dark-blue: #2B3744;
--var-darker-blue: hsl(207, 26%, 17%);
--var-very-dark-blue: #202D36;
--var-dark-gray: hsl(0, 0%, 52%);
--var-very-light-gray: hsl(0, 6%, 87%);
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

.searching{
  display: flex;
  justify-content: space-between;
}

.results{
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
}

.item{
  width: 300px;
  margin: 2rem;
  background-color: var(--var-white);
  border-radius: 5px;
}

.flag{
  object-fit: cover;
  width: 300px;
  height: 150px;
  border-radius: 5px 5px 0 0;
  -webkit-box-shadow: 0px 2px 1px 0px rgba(0, 0, 0, 0.400);
  -moz-box-shadow: 0px 2px 1px 0px rgba(0,0,0,0.400);
  box-shadow: 0px 2px 1px 0px rgba(0,0,0,0.400);
}

.info{
  padding: 10px;
}

.dark-info{
  background: var(--var-dark-blue);
  color: var(--var-white);
}
</style>
