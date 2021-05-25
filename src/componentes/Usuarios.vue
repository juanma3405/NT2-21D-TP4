<template>
  <section class="src-componentes-usuarios">
    <div class="jumbotron">
      <h1>Usuarios</h1>
      <hr />
      <br />

      <button class="btn btn-success my-3 mr-3" @click="getPostsCb()">
        Pedir XMLHttpRequest
      </button>
      <button class="btn btn-success my-3 mr-3" @click="getPostsFetch()">
        Pedir FETCH
      </button>
      <button class="btn btn-success my-3 mr-3" @click="getPostsAxios()">
        Pedir AXIOS
      </button>

      <div v-if="posts.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th v-for="(col, index) in getCols" :key="index">{{ col }}</th>
          </tr>
          <tr v-for="(post, index) in posts" :key="index">
            <th v-for="(col, index) in getCols" :key="index">
              {{ post[col] }}
            </th>
          </tr>
        </table>
      </div>
    </div>
  </section>
</template>

<script lang="js">

  export default  {
    name: 'src-componentes-usuarios',
    props: [],
    mounted () {

    },
    data () {
      return {
        url: 'https://60a96f6d20a641001730725a.mockapi.io/usuarios',
        posts: []
      }
    },
    methods: {
      getPostsCb(){
       let xhr = new XMLHttpRequest
       xhr.open('get', this.url)
       xhr.addEventListener('load', () => {  
         if (xhr.status == 200) {
           let respuesta= JSON.parse(xhr.response)
           console.log('XMLHttpRequest',respuesta)
           this.posts = respuesta
         }
         else {
           console.error(`Error en GET -> status : ${xhr.status}`)
         }
       })
       xhr.addEventListener('error', e => {
          console.error(`Error XMLHttpRequest->`, e)
       })
       xhr.send()
      },
     
      getPostsFetch() {
        fetch(this.url)
        .then(datos => datos.json())
        .then(respuesta => {
          console.log('FETCH',respuesta)
           this.posts = respuesta
        })
        .catch(error => console.error(error))
      },

      getPostsAxios() {
          this.axios(this.url)
          .then( respuesta => {
          console.log('AXIOS', respuesta.data)
          this.posts = respuesta.data
          })
          .catch(error => console.error(error))
      }

    },
    computed: {
       getCols() {
        return Object.keys(this.posts[0])
      }
    }
}


</script>

<style scoped lang="css">
.src-componentes-usuarios {
}

.jumbotron {
  background-color: blue;
  color: white;
}
</style>
