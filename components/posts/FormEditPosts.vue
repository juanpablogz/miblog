<template>
  <div>
    <div class="row text-left">
      <div class="col-sm-12">
        <b-form-checkbox id="checkbox-status" v-model="status" name="checkbox-status" value="publish" unchecked-value="unpublish">
          <p v-if="status == 'publish'">Publicado</p>
          <p v-if="status == 'unpublish'">No publicado</p>
        </b-form-checkbox>
      </div>
    </div>
    
    <div class="row text-left">
      <div class="col-sm-12">
        <b-form-checkbox-group v-model="categorieSelected" :options="getCategories()" class="mb-3" value-field="item" text-field="name" disabled-field="notEnabled"></b-form-checkbox-group>
      </div>
    </div>
    <div class="row">
      <div class="col-sm-12">
        <b-form-input v-model="title" placeholder="Título"></b-form-input>
      </div>
    </div> 
    <br>   
  <div class="row">
    <div class="col-sm-6">
      <b-form-textarea class="text-edit" rows="20" v-model="content" placeholder="Contenido"></b-form-textarea>
    </div>
    <div class="col-sm-6">
      <div class="content-post">
        <div id="preview" v-html="compiledMarkdown"></div>
      </div>
    </div>      
  </div>
  <br>
  <div class="row text-left">
    <div class="col-sm-12">
      <b-button @click.prevent="save" variant="success">Guardar</b-button>
    </div>          
  </div>
  </div>
</template>

<script>
import marked from "marked";
import { DB } from './../../plugins/firebase'
export default {
  name: 'FormPost',
  props: ['post', 'state'],
  computed : {
    compiledMarkdown: function () {
      return marked(this.content, { sanitize: true })
    }
  },    
  data() {
    return {
      categorieSelected: [],
      title: '',
      content: '',
      categories: {},
      status: 'unpublish'
    }
  },
  methods: {
    save() {
      if (this.categorieSelected.length > 0 && this.title != '' && this.content != '') {
        DB.ref('posts/' + this.state + "/" + this.post + "/categories").remove()
        if(this.status != this.state){
          DB.ref('posts/' + this.state + "/" + this.post + "/categories").remove()
          DB.ref('posts/' + this.status + "/").push({ categories: this.categorieSelected, title: this.title, content: this.content}).then( ref => {
            this.toast('b-toaster-bottom-right', false, 'Post guardado', 'success')
          }, (err) => {
            this.toast('b-toaster-bottom-right', false, 'No se pudo guardar el post', 'danger')
          })          
        }else{
          DB.ref('posts/' + this.state + "/" + this.post).update({ categories: this.categorieSelected, title: this.title, content: this.content})
          this.toast('b-toaster-bottom-right', false, 'Post guardado', 'success')
        }        
      }else{
        console.log("No valido")
      }

    },
    toast(toaster, append, message, variant) {
      this.$bvToast.toast(message, {
        title: 'Mensaje',
        toaster: toaster,
        solid: false,
        appendToast: append,
        autoHideDelay: 50,
        variant: variant
      })
    },    
    getPost(){
      let postEdit = DB.ref()
      this.status = this.state
      console.log('this.post ' + this.post)
      DB.ref('posts/' + this.state + '/' + this.post).once('value', snapshot => {
        const object = snapshot.val()
        if (object.categories != null) {
          object.categories.forEach(element => {
            this.categorieSelected.push(element)
          });
        }        
        this.title =  object.title
        this.content =  object.content
      })      
    },
    getCategories(){
      let categories = []
      Object.keys(this.categories).forEach(element => {
        categories.push({ item: element, name: this.categories[element]["name"] })
      });
      return categories
    }
  },
  created () {
    this.getPost()    
      this.getPost()
    this.getPost()    
  },
  firebase: {
    categories: DB.ref('categories')
  },
  watch: {
    post() {
      this.getPost()
    }
  },
}
</script>
<style scoped>
  .text-edit{
    border: none;
  }
</style>