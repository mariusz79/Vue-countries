<template>
  <div>
      <div class="filter"
          id="filter"
          name="filter"
          :class="{ dark1 }"
          :value="value"
          @keyup.enter="handleChange"
          @click="visibility()"
      >
      <p>{{filterValue}}</p>
      <span class="chevron">&#8250;</span>
      </div>
      <div class="options" v-if="visible" :class="{dark1}" @click="visibility()">
        <ul>
          <li @click="filterValue = 'Africa'">Africa</li>
          <li @click="filterValue = 'America'">America</li>
          <li @click="filterValue = 'Asia'">Asia</li>
          <li @click="filterValue = 'Europe'">Europe</li>
          <li @click="filterValue = 'Oceania'">Oceania</li>
        </ul>
      </div>
  </div>
</template>
<script>
export default {
  name: 'SelectRegion',
  props: {
    value: {
      type: String,
      required: true,
    },
    dark1: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      visible: false,
      filterValue: 'Filter by Region',
    };
  },
  methods: {
    handleChange(e) {
      this.$emit('input', e.target.value);
    },
    visibility() {
      this.visible = !this.visible;
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
    border: none;
    background-color: var(--var-white);
    position: relative;
    bottom: 10px;
    z-index: 2;
    transition: display(1s);

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
    display: block;
  }

  .dark1 {
    background-color: var(--var-dark-blue);
    color: #abb6c1;
  }
</style>
