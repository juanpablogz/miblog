<template>
  <div>
    <b-modal size="xl" ref="modal-form-post" id="modal-form-post" title="Post" hide-footer>
      <b-form-checkbox id="checkbox-status" v-model="status" name="checkbox-status" value="publish" unchecked-value="unpublished">
        <p v-if="status == 'publish'">Estado: Publicado</p>
        <p v-if="status == 'unpublish'">Estado: No publicado</p>
      </b-form-checkbox>
      
      <div class="row">
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
          <b-form-textarea rows="20" v-model="content" placeholder="Contenido"></b-form-textarea>
        </div>
        <div class="col-sm-6">
          <div class="content-post">
            <div id="preview" v-html="compiledMarkdown"></div>
          </div>
        </div>      
      </div>
      <br>
      <div class="row">
        <div class="col-sm-4">
          <b-button @click.prevent="save" variant="success">Guardar</b-button>
        </div>          
      </div>
    </b-modal>    

  </div>
</template>

<script>
import marked from "marked";
import { DB } from './../../plugins/firebase'
export default {
  name: 'FormPost',
  props: ['category'],
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
        DB.ref(`posts/${this.status}`).push({ categories: this.categorieSelected, title: this.title, content: this.content}).then( ref => {
          this.toast('b-toaster-bottom-right', false, 'Post guardado', 'success')
          this.reset()
        }, (err) => {
          this.toast('b-toaster-bottom-right', false, 'No se pudo guardar el post', 'danger')
        })
      }else{
        this.toast('b-toaster-bottom-right', false, 'Todos los campos son obligatorios', 'warning')
      }
    },
    reset () {
      this.categorieSelected = []
      this.title = ''
      this.content = ''
      this.status = 'unpublish'
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
    getCategories(){
      let categoriesTransform = []
      Object.keys(this.categories).forEach(category => {
        categoriesTransform.push({ item: category, name: this.categories[category].name})
      });
      return categoriesTransform
    }
  },
  created () {
    if(this.category != ''){
      this.categorieSelected.push(this.category)
    }
  },
  firebase: {
    categories: DB.ref('categories')
  }
}
</script>