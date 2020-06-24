<template>
  <div id="app">
    <div>
  <b-navbar toggleable="lg" type="light" variant="light">
    <b-navbar-brand><router-link to="/">Cocktailspedia
</router-link></b-navbar-brand>


    <b-collapse id="nav-collapse" is-nav>
    

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        {{ timestamp }}
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</div>
    <div id="nav">
      <router-link to="/">Home</router-link> 
      <!-- <router-link to="/details">Details</router-link> -->
    </div>
    <router-view/>
  </div>
</template>

<script>
         export default {
            name: 'App',
            data() {
              return {

                timestamp: ""
              }
            },
            created() {
                setInterval(this.getNow, 1000);
            },
            methods: {
                getNow: function() {
                    const today = new Date();
                    const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
                    const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
                    const dateTime = date +' '+ time;
                    this.timestamp = dateTime;
                }
            }
         };
      </script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
