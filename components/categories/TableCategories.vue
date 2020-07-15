<template>
  <div>
    <div class="container" v-if="Object.keys(categories).length > 0">
      <table role="table" aria-busy="false" aria-colcount="3" class="table b-table table-striped table-hover" id="__BVID__1100">
        <thead role="rowgroup" class="">
          <tr role="row" class="">
            <th role="columnheader" scope="col" aria-colindex="2" class="">Nombre</th>
            <th role="columnheader" scope="col" aria-colindex="2" class="">Crear post</th>
            <th role="columnheader" scope="col" aria-colindex="2" class="">Eliminar</th>
          </tr>
        </thead>
        <tbody role="rowgroup">
          <tr role="row" class="" v-for="category in Object.keys(categories)" :key="category">
            <td aria-colindex="1" role="cell" class="">
              <b-form-input :value="categories[category].name" @input="updateAttribute(`/categories/${category}`, $event)"></b-form-input>
            </td>
            <td aria-colindex="1" role="cell" class="">
              <b-button v-b-modal.modal-form-post @click.prevent="categoryPost = category">Crear post</b-button>
            </td>
            <td aria-colindex="1" role="cell" class="">
              <b-button variant="danger" @click.prevent="removeCategory(`/categories/${category}`)">Eliminar</b-button>
            </td>            
          </tr>
        </tbody>
      </table>
      <modal-form-post :category="categoryPost"/>
    </div>
  </div>
</template>

<script>
import ModalFormPost from './../posts/ModalFormPost.vue'
import { DB } from './../../plugins/firebase'
export default {
  name: 'Tableategories',
  components: {
    'modal-form-post': ModalFormPost,
  },
  data() {
    return {
      categories: {},
      categoryPost: ''
    }
  },
  methods: {
    updateAttribute (path, $event) {
      DB.ref(path).set($event).then(() => {
        this.toast('b-toaster-bottom-right', false, 'Categoría guardada', 'success')
			},(err) => {
        this.toast('b-toaster-bottom-right', false, 'No se pudo guardar', 'danger')
			})      
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
    removeCategory(path){
      DB.ref(path).remove()
    }
  },
  firebase: {
    categories: DB.ref('categories')
  }
}
</script>

<style lang="scss" scoped>

</style>