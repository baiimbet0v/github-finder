
<template>
    <div class="wrapper-content wrapper-content--fixed">
      <section>
        <div class="container">

          <div class="error" v-if="error" style="margin-bottom: 20px">
            <p>{{ error }}</p>
          </div>
          <search 
            :value="search"
            placeholder="Type username..."
            @search="search = $event"         
          />
          
          <button v-if="!repos" class="btn btnPrimary" @click="getRepos">Search</button>
          <button v-else class="btn btnPrimary" @click="getRepos">Search Again!</button>

          <div class="repos__wrapper" v-if="repos">
            <!-- item -->
            <div class="repos-item" v-for="repo in repos" :key="repo.id">
              <div class="repos-info">
                
                <a class="link" target="_blank" :href="repo.html_url">{{ repo.name}}</a>
                <span>{{ repo.stargazers_count}} 🌟</span>
               
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
</template> 


<script>
import search from '@/components/Search.vue'
import axios from 'axios'

export default{
  components: { search },
  data(){
    return{
      search: '',
      error: null,
      repos: null
    }
  },
  methods: {
      getRepos(){
        axios
          .get(`https://api.github.com/users/${this.search}/repos`)
              .then(res => {
                this.error = null
                this.repos = res.data
                console.log(res)
              })
                .catch(err => {
                  console.log(err)
                  this.repos = null
                  this.error= 'Пользователь не найден'
                })  
      }
  }
}
</script>  

<style lang="scss" scoped>
.container{
    display: flex;
    align-items: center;
    flex-direction: column;
}
button{
    margin-top: 40px;
}
.repos__wrapper{
  width: 400px;
  margin: 30;
}
.repos-info{
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 10px 0;
  border-bottom: 1px solid #dbdbdb;
}
</style>

