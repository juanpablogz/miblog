<template>
  <div>
    <modal-form-categories/>
    <br>
    <table-categories/>
  </div>
</template>
<script>
import ModalFormCategories from './ModalFormCategories.vue'
import TableCategories from './TableCategories.vue'
export default {
  name: 'Categories',
  components: {
    'modal-form-categories': ModalFormCategories,
    'table-categories': TableCategories
  },
  methods: {
    name() {
        
    }
  },        
}
</script>

<style lang="scss" scoped>

</style>