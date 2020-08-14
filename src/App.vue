<template>
  <div id="root">
    <div class="wrapper">
      <input type="text" v-model="username" spellcheck="false" placeholder="Enter channel name" />
      <Button :callback="getChannelInfo" width="6rem" height="3rem">
        <i class="fa fa-search"></i>
      </Button>
    </div>
    <div v-if="hasLoaded" class="result">
      <span>{{loadResult}}</span>
    </div>
    <div v-if="loading" class="loader">
      <div></div>
      <div></div>
      <div></div>
    </div>
  </div>
</template>

<script>
// BASE COMPONENTS
import Button from './components/Button';

export default {
  name: 'App',
  components: {
    Button
  },
  data: function() {
    return {
      username: "",
      foo: {
        followers: "",
        channel: ""
      },
      dataLoaded: false,
      loading: false
    }
  },
  computed: {
    hasLoaded: function() {
      return this.dataLoaded && !this.loading;
    },
    loadResult: function() {
      return this.dataLoaded && !this.loading && this.foo.error ? 
        this.foo.error : 
        this.foo.channel + " has " + this.foo.followers + " followers.";
    }
  },
  methods: {
    getChannelInfo() {
      this.loading = true;

      const requestOptions = {
        method: 'GET',
        headers: {'Content-Type': 'application/json'}
      };
      let baseUrl = 'https://twitch-express.herokuapp.com/';
      fetch(`${baseUrl}channels/${this.username}`, requestOptions)
        .then(response => {
          if (response.ok)
            return response.json();
        })
        .then(response_data => {
          this.loading = false;
          this.dataLoaded = true;
          this.foo = response_data;
        })
        .catch(err => console.log(err));
    },
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto+Condensed&display=swap');
body {
  margin: 0;
}
#root {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  background: url('./assets/dark-geometric.png');
  background-color: $dark;
  font-family: 'Roboto Condensed', sans-serif;
}
.wrapper {
  display: flex;
  flex-wrap: wrap;
  margin-bottom: 30px;
  div {
    position: relative;
  }
}
input {
  font-size: 1.5rem;
  font-family: inherit;
  margin-right: 50px;
  color: $primary-light;
  caret-color: $primary;
  background: transparent;
  border: none;
  border-bottom: 3px solid $primary-light;
  padding: 5px 8px;
  &:focus {
    outline: none;
  }
  &::placeholder {
    color: $primary;
  }
  &::selection {
    background-color: $primary-light;
  }
}
@keyframes fadeInLeft {
  from { opacity: 0; transform: translateX(-25%)}
  to { opacity: 1; transform: translateX(0)}
}
.result {
  color: $primary-light;
  font-size: 2rem;
  word-spacing: 5px;
  animation: fadeInLeft 0.5s;
  span {
    font-size: 3rem;
      font-weight: bold;
  }
}
$big: 0.75rem;
$small: 0.4rem;
@keyframes lateLoading {
  0% { opacity: 0.5; width: $small; height: $small}
  50% { opacity: 0.7; width: $big; height: $big;}
  100% { opacity: 0.5; width: $small; height: $small;}
}
@keyframes loading {
  0% { opacity: 0.7; width: $big; height: $big;}
  50% { opacity: 0.5; width: $small; height: $small;}
  100% { opacity: 0.7; width: $big; height: $big;}
}
.loader {
  height: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  div{
    border-radius: 50%;
    margin: 0px 4px;
    background-color: $primary-light;
    animation: lateLoading 0.25s infinite;
    &:nth-child(2n) {
      animation: loading 0.25s infinite;
    }
  }
}
</style>
