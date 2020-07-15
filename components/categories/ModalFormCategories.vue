<template>
  <div>
    <div>
      <div class="row">
        <div class="col-sm-4 ">
          <b-button v-b-modal.modal-form-category>Crear categoría</b-button>
        </div>
      </div>      
      <b-modal ref="modal-category" id="modal-form-category" title="Categrías" hide-footer>
        <form-categories/>
        <hr>
        <b-button class="mt-3" variant="outline-primary" block @click="hideModal">Cerrar</b-button>
      </b-modal>
    </div>    
  </div>
</template>

<script>
import FormCategories from './FormCategories'
export default {
  name: 'ModalFormCategories',
  components: {
    'form-categories': FormCategories,
  },
  methods: {
    hideModal() {
      this.$refs['modal-category'].hide()
    },
  },
}
</script>

<style lang="scss" scoped>

</style>