<template>
  <div>
    <div class="row">
      <div class="col-sm-4 ">
        <b-button v-b-modal.modal-form-post>Crear post</b-button>
      </div>
    </div>    
    <br>
    <tab-posts/>
  </div>
</template>
<script>
import ModalFormPost from './ModalFormPost.vue'
import TabPosts from './TabPosts.vue'
export default {
  name: 'Categories',
  components: {
    'modal-form-post': ModalFormPost,
    'tab-posts': TabPosts
  },
  methods: {
    name() {
        
    }
  },        
}
</script>

<style lang="scss" scoped>

</style>