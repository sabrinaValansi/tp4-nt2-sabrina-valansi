<template >

  <section class="src-components-usuarios">
        <div class="jumbotron">
      <h2>USUARIOS - MOCKAPI</h2>
      <hr>
      <h5 v-if="posts.length">Datos encontrados: {{posts.length}} </h5>
      <hr>
      <br>

      <button class="btn btn-primary my-3 mr-3" @click="getPostsAxios()">Obtener datos Axios</button>
      <button class="btn btn-secondary my-3 mr-3" @click="getPostsXHRcb()">Obtener datos XHR</button>
      <button class="btn btn-success my-3 mr-3" @click="getPostsFetch()">Obtener datos fetch</button>
      <button class="btn btn-warning my-3 mr-3" @click="deletePosts() " v-if="posts.length">Limpiar datos </button>
      <br>

      <div v-if="posts.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>Nombre</th>
            <th>Email</th>
            <th>Telefono</th>
          </tr>
          <tr v-for="(post, index) in posts" :key="index">
              <td>{{ post.name}}</td>
              <td>{{ post.email }}</td>
              <td>{{ post.telefono }}</td>
          </tr>
        </table>
        
      </div>
      <h4 v-else class="alert alert-danger text-center">No se encontraron posts</h4>

     </div>
  </section>

</template>

<script >

  export default  {
    name: 'src-components-usuarios',
    props: [],
    mounted () {

    },
    data () {
      return {
        url: 'https://6284e6763060bbd347412de1.mockapi.io/usuarios',
        posts: []
      }
    },
    methods: {
      /* -------------------------------------------------------------------------------------------- */
      /*                                      Petición Ajax con Axios                                            */
      /* -------------------------------------------------------------------------------------------- */
       async getPostsAxios() {              
        try {
          let { data } = await this.axios(this.url)
          this.posts = data
        }
        catch(error) {
          console.error('Error Axios', error)
        }   
      },
      /* -------------------------------------------------------------------------------------------- */
      /*                                      Petición Ajax con XHR                                   */
      /* -------------------------------------------------------------------------------------------- */
      getPostsXHRcb() {
          const xhr = new XMLHttpRequest()
          xhr.open('get', this.url)

          xhr.addEventListener('load', () => {
            if(xhr.status == 200) {
              let respuesta = JSON.parse(xhr.response)
              //console.log(respuesta)
              this.posts = respuesta
            }
            else {
              console.error('ERROR XHR cb (status)', xhr.status)
            }
          })

          xhr.addEventListener('error', e => {
              console.error('ERROR XHR cb (ajax)', e)
          })

          xhr.send()
      },
      /* -------------------------------------------------------------------------------------------- */
      /*                                      Petición Ajax con Fetch                                 */
      /* -------------------------------------------------------------------------------------------- */
      async getPostsFetch() {
        try {
          let response = await fetch(this.url)
          //console.log(response)
          let respuesta = await response.json()
          this.posts = respuesta
        }
        catch(error) {
          console.error('Error Fetch', error)
        }
      },
      /* -------------------------------------------------------------------------------------------- */
      /*                                      Borrar datos                                            */
      /* -------------------------------------------------------------------------------------------- */
      deletePosts(){
        this.posts=[]
      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  .src-components-usuarios {

  }
</style>
