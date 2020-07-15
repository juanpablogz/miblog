<template>
  <div>
    <form-post :category="category"/>
  </div>
</template>

<script>
import FormPost from './FormPosts.vue'
export default {
  name: 'ModalFormPost',
  props: ['category'],  
  components: {
    'form-post': FormPost
  },
  methods: {
    hideModal() {
      this.$refs['modal-form-post'].hide()
    },
  },
}
</script>

<style lang="scss" scoped>

</style>