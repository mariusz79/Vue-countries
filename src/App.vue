<template>
  <div id="app1" :class="{'light-app':!darkOn, 'dark-app':darkOn }">
    <div id="nav" :class="{'light-nav':!darkOn, 'dark-nav':darkOn }">
      <div to="/" :class="{'light-nav':!darkOn,
       'dark-nav':darkOn }"><h2>Where in the  world?</h2></div>
       <div id="mode" @click="darkMode()">
        <img :src="darkOn? moonBlack : moonLine"
          alt="moon icon">
        <p>Dark Mode</p>
      </div>
    </div>
      <div class="home">
    <div class="searching">
      <SearchInput v-model="searchValue" @input="handleInput" :dark="darkOn"/>
      <div>
      <div class="filter"
          id="filter"
          name="filter"
          :class="{'dark1':darkOn}"
          @click="visibility()"
      >
      <p>{{regionValue}}</p>
      <span class="chevron">&#8250;</span>
      </div>
      <div class="options" v-if="visible" :class="{'dark1':darkOn}" @click="visibility()">
        <ul>
          <li @click="regionValue = 'Africa', handleRegion()">Africa</li>
          <li @click="regionValue = 'Americas', handleRegion()">Americas</li>
          <li @click="regionValue = 'Asia', handleRegion()">Asia</li>
          <li @click="regionValue = 'Europe', handleRegion()">Europe</li>
          <li @click="regionValue = 'Oceania', handleRegion()">Oceania</li>
        </ul>
      </div>
      </div>
    </div>
    <div class="results">
      <div v-for="key in result" :key="result[key]" class="item"
      :class="{'dark-info':darkOn}" @click="itemClicked(key)">
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
    <transition name="modal" v-if="modal">
        <div class="modal-mask">
            <div class="modal-container" :class="{'dark-info':darkOn}">
                <div class="button-container">
                  <button class="modal-default-button" @click="modal = false">Close</button>
                </div>
                <div class="info-container">
                  <div class="modal-flag">
                    <img v-bind:src=flag class="mflag">
                  </div>
                  <div class="minfo-left">
                    <h3>{{name}}</h3>
                    <div><b>Native name: </b><span>{{nativeName}}</span></div>
                    <div><b>Population: </b><span>{{population}}</span></div>
                    <div><b>Region: </b>{{region}}</div>
                    <div><b>Sub Region: </b>{{subregion}}</div>
                    <div><b>Capital: </b>{{capital}}</div>
                  </div>
                  <div class="minfo-right">
                    <div><b>Top Level Domain: </b>{{topLevelDomain[0]}}</div>
                    <div><b>Currencies: </b>{{currencies}}</div>
                    <div><b>Languages: </b>{{languages}}</div>
                  </div>
                </div>
            </div>
        </div>
      </transition>
  </div>
  </div>
</template>

<script>
import SearchInput from '@/components/SearchInput.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    SearchInput,
  },
  data() {
    return {
      result: [],
      searchValue: '',
      visible: false,
      regionValue: 'Filter by Region',
      darkOn: false,
      modal: false,
      moonLine: 'https://mariusz-ecommerce.s3-eu-west-1.amazonaws.com/static/Vue/moon-line.svg',
      moonBlack: 'https://mariusz-ecommerce.s3-eu-west-1.amazonaws.com/static/Vue/moon-black.svg',
    };
  },
  methods: {
    visibility() {
      this.visible = !this.visible;
    },
    darkMode() {
      this.darkOn = !this.darkOn;
    },
    handleInput() {
      axios.get(`https://restcountries.eu/rest/v2/name/${this.searchValue}`)
        .then((response) => {
          this.result = response.data;
          this.regionValue = 'Filter by Region';
        });
    },
    handleRegion() {
      axios.get(`https://restcountries.eu/rest/v2/region/${this.regionValue}`)
        .then((response) => {
          this.result = response.data;
          this.searchValue = '';
        });
    },
    itemClicked(key) {
      this.name = key.name;
      this.flag = key.flag;
      this.nativeName = key.nativeName;
      this.capital = key.capital;
      this.population = key.population;
      this.region = key.region;
      this.subregion = key.subregion;
      this.topLevelDomain = key.topLevelDomain;
      this.currencies = key.currencies[0].name;
      const lang = key.languages;
      const langs = [];
      for (let item = 0; item < lang.length; item += 1) {
        langs.push(lang[item].name);
      }
      this.languages = langs.join(', ');
      this.modal = !this.modal;
    },
  },
  created() {
    axios.get('https://restcountries.eu/rest/v2/region/europe')
      .then((response) => {
        this.result = response.data;
      });
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
#app1{
  font-family: 'Nunito Sans', sans-serif;
  overflow-x: hidden;
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

.moveInUp-enter-active{  opacity: 0;  transition: opacity 1s ease-in;}
.moveInUp-enter-active{  animation: fadeIn 1s ease-in;}
@keyframes fadeIn{  0%{    opacity: 0;  }  50%{    opacity: 0.5;  }  100%{    opacity: 1;  }}
.moveInUp-leave-active{  animation: moveInUp .3s ease-in;}
@keyframes moveInUp{ 0%{  transform: translateY(0); }  100%{  transform: translateY(-400px); }}

.searching{
  display: flex;
  justify-content: space-between;
}

.filter {
    text-indent: 20px;
    width: 200px;
    height: 30px;
    border: none;
    background-color: var(--var-white);
    display: flex;
    align-items: center;
    justify-content: space-between;
    cursor: pointer;

    & p {
      font-size: 1rem;
    }

    & .chevron {
      position: relative;
      transform: rotate(90deg);
      bottom: 7px;
      font-size: 2rem;
    }
  }

  .options {
    width: 200px;
    border: none;
    background-color: var(--var-white);
    position: absolute;
    z-index: 2;
    margin-top: 5px;

    & ul {
      list-style: none;
      padding: 10px 20px;
    }

    & li {
      padding: 8px 0;
      cursor: pointer;
    }

    & li:hover {
      border-bottom: 1px solid var(--var-very-dark-blue);
    }
  }

  .visible {
    display: flex;
  }

  .dark1 {
    background-color: var(--var-dark-blue);
    color: #abb6c1;
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

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  transition: opacity 0.3s ease;
}

.modal-container {
  width: 90vw;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: var(--var-white);
  border-radius: 2px;
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.mflag{
  object-fit: cover;
  width: 300px;
  height: 150px;
  -webkit-box-shadow: 0px 2px 1px 0px rgba(0, 0, 0, 0.400);
  -moz-box-shadow: 0px 2px 1px 0px rgba(0,0,0,0.400);
  box-shadow: 0px 2px 1px 0px rgba(0,0,0,0.400);
}

.dark-info{
  background-color: var(--var-dark-blue);
  color: var(--var-white);
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>
