<template>
  <div>
      <div class="header ">
        <b-navbar type="dark" variant="dark">
          <b-navbar-nav>
            <div class="navleft">
            <b-nav-item href="">Lœsus Pedagogia</b-nav-item>

            <b-nav-item-dropdown text="Categories" right>
              <b-dropdown-item v-for="cat in this.categories" :key="cat" @click="seeCategory(cat)">{{cat}}</b-dropdown-item>
            </b-nav-item-dropdown>
            </div>
            <b-nav-form class="navright">
              <b-form-input class="input mr-sm-2" placeholder="Search"></b-form-input>
              <b-button variant="outline-success" class="btnSearch my-2 my-sm-0" type="submit">Search</b-button>
            </b-nav-form>
          </b-navbar-nav>
        </b-navbar>
      </div>
    <div class="prods row aos-init aos-animate">
      <div v-for="prod in prods" :key="prod.id" class="col-xl-3 col-lg-4 col-md-6 prod ">
        <b-card
          :img-src="prod.image"
          img-alt="Image"
          img-top
          class="card mb-2 shadow p-3 mb-5 bg-white rounded"
        >
          <h3 class="title" v-if="prod.title.length > 50" data-toggle="tooltip" data-placement="right" :title="prod.title">{{prod.title.substring(0,50) + '...'}}</h3>
          <h3 class="title" v-else data-toggle="tooltip" data-placement="right" :title="prod.title">{{prod.title}}</h3>
          <h4 class="price">
            {{prod.price}} €
          </h4>
          <div class="rating">
            <rate :length="5" :value="prod.rating.rate" :disabled="true" class="rate"/>
            <span class="count"> ({{prod.rating.count}})</span>
          </div>

          <p class="description" v-if="prod.description.length > 200" data-toggle="tooltip" data-placement="right" :title="prod.description"> {{prod.description.substring(0,200) + '...'}}</p>
          <p class="description" v-else data-toggle="tooltip" data-placement="right" :title="prod.description"> {{prod.description}}</p>
        </b-card>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "Products",
  data(){
    return{
      prods : [],
      allProds : [],
      categories : [],
    }
  },
  mounted(){
    axios
    .get("https://fakestoreapi.com/products")
    .then((res) =>{
      this.prods = res.data
      this.allProds = res.data
      this.getCategories(this.allProds)
    })
    
  },
  methods: {
    getCategories: function(list) {
      let res = 0 
      this.categories.push("All categories")
      for(let i = 0;i< list.length;i++){
        res = 1
        for(let j =0; j<this.categories.length ;j++){
          if(list[i].category == this.categories[j] ){
            res = -1
            break
          }
        }
        if(res == 1){
          this.categories.push(list[i].category)
        }
      }
    },
    seeCategory: function(cat){
        if(cat == "All categories"){
          this.prods = this.allProds
          return
        }
        let t = this.allProds.length
        this.prods = []
        for(let j =0; j< t ;j++){
          if(this.allProds[j].category == cat ){
            this.prods.push(this.allProds[j])
          }
        }
        // this.prods.push(this.allProds[0])
        // this.prods.push(this.allProds[1])
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header{
    position: fixed;
    top: 0px;
    left: 0px;
    width: 100%;
    z-index: 99;
    box-shadow: 0px 5px 8px rgb(56, 56, 56);
}
.prod{
  margin-top: 0.5rem;
  margin-bottom: 1rem;
  min-width: 18rem;
}
.prods{
  margin: 0.5rem;
}
.card-img-top{
  height: 17rem;
}
.title{
  font-family: 'Times New Roman', Times, serif;
  font-size: 1.5rem;
  font-weight: bold;
  text-align: start;
  cursor: pointer;
  height: 5rem;
}
.title:hover{
  text-decoration: underline;
}
.rating{
  display: flex;
  justify-content: center;
  align-content: center;
  margin-bottom: 0.5rem;
}
.rate{
  cursor: none !important;
}
.count{
  /* border: green 1px solid; */
  line-height: 40px;
}
.price{
  display: flex;
  justify-content: end;
  align-content: center;
  font-weight: bold;
  font-size: 30px;
}
.description{
  text-align: justify;
}
.card{
  height: 100% !important;
  padding: 0.5rem;
  border: none;
}
.form-inline{
  display: flex;
  justify-content: center;
  align-content: center;
  flex-direction: row;
}

.btnSearch{
  margin-left: 0.5rem;
  margin-right: 1rem;
}
.navbar-nav{
  display: flex;
  justify-content: space-between;
  align-content: center;
  flex-direction: row;
  width: 100%;
}
.navleft{
   display: flex;
  justify-content: center;
  align-content: center;
  flex-direction: row;
}
</style>
