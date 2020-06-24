<template>
  <div class="details">
    <div class="mt-4" v-for="items in details" :key="items.id" >
      <b-row>
        <b-col>
          <div>
            <b-img thumbnail fluid rounded :src="items.strDrinkThumb"  alt="Responsive image" width="300px"></b-img>
          </div>
        </b-col>
        <b-col  sm="6" >
          <h1 >{{items.strDrink}}</h1>
          <h2>Ingredients</h2>
          <div v-for="index in 15" :key="index" >
            <li  v-if="items[`strMeasure${index}`] !== null">
                    {{items[`strMeasure${index}`]}} {{items[`strIngredient${index}` ]}}
            </li>
          </div>
          <div>
            <h2>Glass</h2>
            {{items.strGlass}}
          </div>
          <div>
            <h2>Instructions</h2>
              <button @click="englishLang"><flag iso="gb" /></button>
              <button @click="germanLang"><flag iso="de" /></button>
              <div>
                {{enLang? items.strInstructions : items.strInstructionsDE}}
              </div>
          </div>
        </b-col>
      </b-row>
    </div>
    <!-- {{details}} -->
  </div>
</template>
<script>
import axios from 'axios'
import Vue from 'vue'
import FlagIcon from 'vue-flag-icon'
Vue.use(FlagIcon);

export default {
  name:'Details',
  props: {
    item: {
      type: String,
      required: true
    }
  },
  data(){
    return {
      details :  null,
      enLang : true,
    }
  },

  mounted(){
    console.log('dari rpouter', this.$route.params.item)
    this.getDetails()
    console.log(this.item)
  },

  methods : {
    getDetails() {
      axios
      .get(`https://www.thecocktaildb.com/api/json/v1/1/lookup.php?i=${this.item}`)
      .then(response => (this.details = response.data.drinks))
    },
    englishLang() {
      this.enLang = true
    },
    germanLang(){
      this.enLang = false
    }
  }

}
</script>