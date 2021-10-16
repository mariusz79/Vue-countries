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
          <ToggleTheme
          v-on:darkMode="darkMode()"/>
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
              <img v-bind:src=key.flags.svg class="flag" :class="{'dark-flag':darkOn}">
            <div class="info" >
              <h3>{{key.name.common}}</h3>
              <div><b>Population: </b><span>{{addComas(key.population)}}</span></div>
              <div><b>Region: </b>{{key.region}}</div>
              <div v-if="key.capital"><b>Capital: </b>{{key.capital[0]}}</div>
              <div v-else-if="!key.capital"><b>Capital: </b>No data</div>
            </div>
          </div>
        </div>
        <transition name="modal" v-if="modal">
            <div class="modal-mask">
                <div class="modal-container" :class="{'dark-modal':darkOn}">
                    <div class="info-container">
                          <img v-bind:src=flag class="mflag" :class="{'dark-flag':darkOn}">
                        <div class="infos">
                          <div class="capital">
                              <h3>{{name}}</h3>
                          </div>
                          <div class="rest-info">
                            <div class="minfo-left">
                                <div><b>Population: </b><span>{{addComas(population)}}</span></div>
                                <div><b>Region: </b>{{region}}</div>
                                <div><b>Sub Region: </b>{{subregion}}</div>
                                <div v-if="capital"><b>Capital: </b>{{capital[0]}}</div>
                                <div v-else-if="!capital"><b>Capital: </b>No data</div>
                              </div>
                            <div class="minfo-right">
                                <div><b>Top Level Domain: </b>{{topLevelDomain[0]}}</div>
                                <div><b>Currencies: </b>{{currencies}}</div>
                                <div><b>Languages: </b>{{languages}}</div>
                              </div>
                            </div>
                          </div>
                    </div>
                    <div class="button-container">
                      <button class="modal-button" :class="{'dark1':darkOn}"
                      @click="modal = false">Close</button>
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
import ToggleTheme from '@/components/ToggleTheme.vue';
import axios from 'axios';
import ScrollUp from 'vue-scroll-up';
import 'vue-scroll-up/dist/style.css';
import './assets/main.scss';

export default {
  name: 'app',
  components: {
    SearchInput, ScrollUp, ToggleTheme,
  },
  data() {
    return {
      result: [],
      searchValue: '',
      visible: false,
      regionValue: 'Filter by Region',
      darkOn: false,
      modal: false,
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
      axios.get(`https://restcountries.com/v3.1/name/${this.searchValue}`)
        .then((response) => {
          this.result = response.data;
          this.regionValue = 'Filter by Region';
        });
    },
    handleRegion() {
      axios.get(`https://restcountries.com/v3.1/region/${this.regionValue}`)
        .then((response) => {
          this.result = response.data;
          this.searchValue = '';
        });
    },
    itemClicked(key) {
      this.name = key.name.common;
      this.flag = key.flags.svg;
      this.capital = key.capital;
      this.population = key.population;
      this.region = key.region;
      this.subregion = key.subregion;
      this.topLevelDomain = key.tld;
      const lang = key.languages;
      const langs = [];
      Object.entries(lang).forEach(([key]) => { langs.push(lang[key]); });
      this.languages = langs.join(', ');
      const { currencies } = key;
      const currency = [];
      Object.entries(currencies).forEach(([key]) => { currency.push(currencies[key].name); });
      this.currencies = currency.join(', ');
      this.modal = !this.modal;
    },
  },
  created() {
    axios.get('https://restcountries.com/v3.1/region/europe')
      .then((response) => {
        this.result = response.data;
      });
  },
};
</script>

<style lang="scss">

</style>
