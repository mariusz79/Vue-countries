<template>
  <div id="app" :class="{'light-app':!darkOn, 'dark-app':darkOn }">
    <div class="nav" :class="{'light-nav':!darkOn, 'dark-nav':darkOn,
      'nav-shadow-light':!darkOn, 'nav-shadow-dark':darkOn }">
      <div class="container-nav">
        <div class="navbar">
          <div
          :class="{'light-nav':!darkOn, 'dark-nav':darkOn }">
            <h2 class="logo">Where in the  world?</h2>
          </div>
          <div id="mode" @click="darkMode()">
            <img :src="darkOn? moonBlack : moonLine" alt="moon icon">
            <p class="mode-text">Dark Mode</p>
          </div>
        </div>
      </div>
    </div>
    <div class="home">
      <div class="container">
        <div class="searching">
          <SearchInput v-model="searchValue" @input="handleInput" :dark="darkOn"/>
          <div class="region">
            <div class="filter"
                :class="{'dark1':darkOn}"
                @click="visibility()"
            >
              <p>{{regionValue}}</p>
              <span class="chevron">&#8250;</span>
            </div>
            <div :class="{'dark-options':darkOn}" class="options"
            v-if="visible" @click="visibility()">
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
              <img v-bind:src=key.flag class="flag" :class="{'dark-flag':darkOn}">
            </div>
            <div class="info" >
              <h3>{{key.name}}</h3>
              <div><b>Population: </b><span>{{addComas(key.population)}}</span></div>
              <div><b>Region: </b>{{key.region}}</div>
              <div><b>Capital: </b>{{key.capital}}</div>
            </div>
          </div>
        </div>
        <transition name="modal" v-if="modal">
            <div class="modal-mask">
                <div class="modal-container" :class="{'dark-modal':darkOn}">
                  <div class="modal-content">
                    <div class="button-container">
                      <button class="modal-button" :class="{'dark1':darkOn}"
                      @click="modal = false">&#8592; &nbsp;&nbsp;Back</button>
                    </div>
                    <div class="info-container">
                        <div class="modal-flag">
                          <img v-bind:src=flag class="mflag" :class="{'dark-flag':darkOn}">
                        </div>
                        <div class="infos">
                          <div class="capital">
                              <h3>{{name}}</h3>
                          </div>
                          <div class="rest-info">
                            <div class="minfo-left">
                                <div><b>Native name: </b><span>{{nativeName}}</span></div>
                                <div><b>Population: </b><span>{{addComas(population)}}</span></div>
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
                </div>
                </div>
            </div>
        </transition>
      </div>
    </div>
    <ScrollUp :scroll-duration="1000" :scroll-y="250">Up</ScrollUp>
  </div>
</template>

<script>
import SearchInput from '@/components/SearchInput.vue';
import axios from 'axios';
import ScrollUp from 'vue-scroll-up';
import 'vue-scroll-up/dist/style.css';

export default {
  name: 'app',
  components: {
    SearchInput, ScrollUp,
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
    scrollToTop() {
      window.scrollTo(0, 0);
    },
    visibility() {
      this.visible = !this.visible;
    },
    addComas(num) {
      const numparts = num.toString().split('.');
      numparts[0] = numparts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ',');
      return numparts.join('.');
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
//variables------------------------------
*{
--var-very-dark-blue: #202D36;
--var-dark-gray: hsl(0, 0%, 52%);
--var-very-light-gray: hsla(0, 0%, 96.5%, 0.79);
--var-white: hsl(0, 0%, 100%);
}
//main app-------------------------------
#app{
  font-family: 'Nunito Sans', sans-serif;
  overflow-x: hidden;
  min-height: 100vh;
}
//navbar--------------------------------
.nav{
  display: flex;
  justify-content: center;
}

.container-nav{
  width: 92vw;
}

.nav{
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

.navbar{
  display: flex;
  justify-content: space-between;
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

.nav-shadow-light{
   box-shadow: 0 0px 2px 1px rgba(210, 206, 206, 0.5);
}

.nav-shadow-dark{
  box-shadow: none;
}

@media (max-width: 500px) {
  .logo{
    font-size: 1rem;
  }
  .mode-text{
    font-size: 12px;
  }
}
//home section------------------------------
.home{
  display: flex;
  justify-content: center;
}
.container{
  max-width: 99vw;
}
//search-input------------------------------
.searching{
  margin-top: 2rem;
  @media (min-width: 500px) {
  display: flex;
  justify-content: space-between;
  }
}
//filter region-----------------------------
.filter {
    text-indent: 20px;
    width: 12rem;
    border: none;
    border-radius: 3px;
    background-color: var(--var-white);
    display: flex;
    align-items: center;
    justify-content: space-between;
    -webkit-box-shadow: 0 0px 1px 1px rgba(230, 226, 226, 0.62);
    -moz-box-shadow: 0 0px 1px 1px rgba(230, 226, 226, 0.62);
    box-shadow: 0 0px 1px 1px rgba(230, 226, 226, 0.62);
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

.filter:hover{
    outline: none;
    -webkit-box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
    -moz-box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
    box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
  }

.options {
    width: 12rem;
    border: none;
    border-radius: 3px;
    background-color: var(--var-white);
    position: absolute;
    z-index: 2;
    margin-top: 5px;
    outline: none;
    -webkit-box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
    -moz-box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
    box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
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

.dark1:hover{
    outline: none;
    -webkit-box-shadow: 0 0px 2px 3px rgba(255,255,255, .5);
    -moz-box-shadow: 0 0px 2px 1px rgba(255,255,255, .5);
    box-shadow: 0 0px 2px 1px rgba(255,255,255, .5);
  }

.dark-options li:hover {
      border-bottom: 1px solid rgba(255,255,255, .5);
    }

.visible {
    display: flex;
  }

.dark-options {
    background-color: var(--var-dark-blue);
    color: #abb6c1;
  }
//results----------------------------------------
.results{
  margin-top: 50px;
  display: grid;
  grid-template-columns: 1fr;
  grid-gap: 6vw;
  justify-items: center;
  @media (min-width: 500px) {
    grid-template-columns: 1fr 1fr;
    grid-gap: 3vw;
  }
  @media (min-width: 768px) {
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 3vw;
  }
  @media (min-width: 1100px) {
    grid-template-columns: 1fr 1fr 1fr 1fr;
    grid-gap: 4vw;
  }
}

.item{
    width: 80vw;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: 50%;
    background-color: var(--var-white);
    border-radius: 5px;
    box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
    @media (min-width: 500px) {
    width: 45vw;
    height: 345px;
  }
    @media (min-width: 768px) {
      width: 30vw;
      height: 345px;
    }
    @media (min-width: 1100px) {
      width: 20vw;
      height: 345px;
    }
}

.item:hover{
    outline: none;
    box-shadow: 0 0px 4px 7px rgba(204, 204, 204, 0.619);
  }

.flag{
  object-fit: cover;
  height: 180px;
  border-radius: 5px 5px 0 0;
  -webkit-box-shadow: 0px 2px 1px 0px hsla(0, 0%, 96.5%, 0.79);
  -moz-box-shadow: 0px 2px 1px 0px hsla(0, 0%, 96.5%, 0.79);
  box-shadow: 0px 2px 1px 0px hsla(0, 0%, 96.5%, 0.79);
  width: 80vw;
  @media (min-width: 500px) {
    width: 45vw;
    height: 150px;
  }
  @media (min-width: 768px) {
      width: 30vw;
    }
  @media (min-width: 1100px) {
      width: 20vw;
    }
}

.info{
  padding: 10px;
}

//modal ---------------------------------------------
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.98);
  display: flex;
  justify-content: center;
  align-items: center;
  transition: opacity 0.3s ease;
}

.modal-container {
  width: 99%;
  margin: 0px auto;
  padding: 20px 30px;
  border-radius: 2px;
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
  background-color: #f5f4f4;
  @media (min-width: 430px) {
    width: 70%;
    }
  @media (min-width: 540px) {
    width: 60%;
    }
  @media (min-width: 640px) {
    width: 50%;
    }
  @media (min-width: 768px) {
    width: 40%;
    }
  @media (min-width: 992px) {
    width: 99%;
    height: 98%;
    display: flex;
    justify-content: center;
    flex-direction: column;
    padding: 0 10vw;
    align-items: center;
    }
}

.modal-content{
  @media (min-width: 992px) {
    margin-bottom: 7rem;
    }
}

.info-container {
  @media (min-width: 992px) {
    display: flex;
    justify-content: space-between;
    }
}

.modal-button{
  padding: 10px 20px;
  border-radius: 4px;
  background-color: var(--var-white);
  border: none;
  -webkit-box-shadow: 0 0px 1px 1px rgba(230, 226, 226, 0.62);
  -moz-box-shadow: 0 0px 1px 1px rgba(230, 226, 226, 0.62);
  box-shadow: 0 0px 1px 1px rgba(230, 226, 226, 0.62);
  font-size: 14px;
  @media (min-width: 992px) {
    margin-bottom: 7rem;
    }
}

.modal-button:hover {
    outline: none;
    -webkit-box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
    -moz-box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
    box-shadow: 0 0px 4px 1px rgba(204, 204, 204, 0.619);
  }

.mflag{
  object-fit: cover;
  width: 100%;
  height: 30vh;
  -webkit-box-shadow: 0px 2px 1px 0px hsla(0, 0%, 96.5%, 0.79);
  -moz-box-shadow: 0px 2px 1px 0px hsla(0, 0%, 96.5%, 0.79);
  box-shadow: 0px 2px 1px 0px hsla(0, 0%, 96.5%, 0.79);
  margin: 1.5rem 0 .5rem 0;
  @media (min-width: 992px) {
    height: 35vh;
    }
}

.rest-info{
   @media (min-width: 992px) {
    display: flex;
    align-items: baseline;
    }
}

.dark-info{
  background-color: var(--var-dark-blue);
  color: var(--var-very-light-gray);
  box-shadow: none;
}

.dark-flag{
  box-shadow: none;
}

.dark-info:hover{
    outline: none;
    box-shadow: 0 0px 4px 3px rgba(255,255,255, .5);
  }

.dark-modal{
  background-color: var(--var-very-dark-blue);
  color: var(--var-very-light-gray);
  box-shadow: none;
}

.dark-modal:hover{
    box-shadow: none;
  }

.minfo-left div, .minfo-right div{
  padding: 5px 0;
}

.minfo-right{
  margin-top: 1rem;
}
.minfo-left{
  @media (min-width: 992px) {
  padding: 0 4vw;
    }
}

.capital h3{
  @media (min-width: 992px) {
  padding-left: 4vw;
  font-size: 1.6rem;
    }
}

.dark1 {
    background-color: var(--var-dark-blue);
    color: #abb6c1;
    box-shadow: none;
  }
/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
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
