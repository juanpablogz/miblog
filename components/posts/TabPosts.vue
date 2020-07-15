<template>
  <div class="container tab-post">
    <b-tabs content-class="mt-3">
      <b-tab title="Publicados" active>
        <table-posts :posts="postsPublish" state="publish" v-if="Object.keys(postsPublish).length > 0"/>
      </b-tab>
      <b-tab title="Sin publicar">
        <table-posts :posts="postsUnPublish" state="unpublish" v-if="Object.keys(postsUnPublish).length > 0"/>           
      </b-tab>
    </b-tabs>
  </div>
</template>

<script>
import { DB } from './../../plugins/firebase'
import TablePosts from './TablePosts'
export default {
  components: {
    'table-posts': TablePosts
  },
  data() {
    return {
      postsPublish: {},
      postsUnPublish: {}
    }
  },
  firebase: {
    postsPublish: DB.ref('posts/publish'),
    postsUnPublish: DB.ref('posts/unpublish')
  },  
}
</script>

<style lang="scss" scoped>

</style>