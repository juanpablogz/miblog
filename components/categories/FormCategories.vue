<template>
  <div>
    <div class="row">
      <div class="col-sm-8">
        <b-form-input v-model="name" placeholder="Nombre de la categoría"></b-form-input>
      </div>
      <div class="col-sm-4">
        <b-button @click.prevent="save" variant="success">Guardar</b-button>
      </div>
    </div>    
  </div>
</template>

<script>
import { DB } from './../../plugins/firebase'
export default {
  data() {
    return {
      name: ''
    }
  },
  methods: {
    save() {
      if (this.name != '') {
        DB.ref('/categories').push({ name: this.name })        
      }
    }
  },
}
</script>