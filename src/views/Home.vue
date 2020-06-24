<template>
  <div>
    <p style="font-size: 60px">Cocktailspedia!</p>
    <div>
      <input type="text" v-model="searchedText" @keyup="searchDrinks" placeholder="Search your favorite drink" style="margin-bottom:20px">
    </div>
    <div>
  <b-carousel
    id="carousel-fade"
    style="text-shadow: 0px 0px 2px #000; margin-bottom: 30px"
    fade
    indicators
    img-width="1024"
    img-height="480"
    v-if="showCarousel"
  >
    <b-carousel-slide
      caption="First slide"
      img-src="https://picsum.photos/1024/480/?image=10"
    ></b-carousel-slide>
    <b-carousel-slide
      caption="Second Slide"
      img-src="https://picsum.photos/1024/480/?image=12"
    ></b-carousel-slide>
  </b-carousel>
</div>
    <b-container class="bv-example-row">
      <b-row>
        <div v-for="item in info" :key="item.id" @click="clickMethod(item)">
          <b-col>
            <b-card
              :title="item.strDrink"
              :img-src="item.strDrinkThumb"
              img-alt="Image"
              img-top
              tag="article"
              style="max-width: 20rem;"
              class="mb-2"
            >
            </b-card>
          </b-col>
        </div>
      </b-row>
    </b-container>
  </div>
  
</template>

<script>
// @ is an alias to /src
import axios from 'axios';

export default {
  name: 'Home',
  data() {
    return {
      info : null,
      searchedText : '',
      itemID: '',
      mojito: null,
      longIsland: null,
      showCarousel: true
    }
  },
  
  mounted () {
   this.getList()
   this.getMojito()
   this.getLongIsland()
  },

  methods : {

    getList(){
       axios
      .get('https://www.thecocktaildb.com/api/json/v1/1/search.php?f=a')
      .then(response => (this.info = response.data.drinks))
    },

    getMojito() {
      axios
      .get(`https://www.thecocktaildb.com/api/json/v1/1/lookup.php?i=11000`)
      .then(response => {
        this.mojito = response.data.drinks[0];
        console.log(this.mojito)
       }  
      )
    },

    getLongIsland() {
      axios
      .get(`https://www.thecocktaildb.com/api/json/v1/1/lookup.php?i=11002`)
      .then(response => {
        this.longIsland = response.data.drinks[0];
        console.log(this.longIsland)
       }  
      )
    },


    searchDrinks(){
      if(this.searchedText.length > 1  ) {
        this.showCarousel = false;
        axios
        .get(`https://www.thecocktaildb.com/api/json/v1/1/search.php?s=${this.searchedText}`)
        .then(response => (this.info = response.data.drinks))  
      }
      else {
        this.showCarousel = true;
        this.getList()
      }
    },

    clickMethod(item){
      const index = this.info.indexOf(item)
      this.itemID = this.info[index].idDrink
      this.$router.push({ name: 'Details', params: {item : this.itemID } });
    }
  }

}
</script>

