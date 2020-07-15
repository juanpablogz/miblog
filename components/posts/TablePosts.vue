<template>
  <div v-if="Object.keys(posts).length > 0">
    <table role="table" aria-busy="false" aria-colcount="3" class="table b-table table-striped table-hover" id="__BVID__1100" v-if="postEdit == null">
      <thead role="rowgroup" class="">
        <tr role="row" class="">
          <th role="columnheader" scope="col" aria-colindex="2" class="">Título</th>
          <th role="columnheader" scope="col" aria-colindex="2" class="">Contenido</th>
          <th role="columnheader" scope="col" aria-colindex="2" class="">Eliminar</th>
          <th role="columnheader" scope="col" aria-colindex="2" class="">Editar</th>
        </tr>
      </thead>
      <tbody role="rowgroup">
        <tr role="row" class="" v-for="post in Object.keys(posts)" :key="post">
          <td aria-colindex="1" role="cell" class="">{{posts[post].title}}</td>
          <td aria-colindex="1" role="cell" class="">{{posts[post].content.substring(0, 20)}}</td>
          <td aria-colindex="1" role="cell" class="">
            <b-button @click.prevent="edit(post)">Editar</b-button>
          </td>                                  
          <td aria-colindex="1" role="cell" class="">
            <b-button variant="danger" @click.prevent="removePost(`/posts/${post}`)">Eliminar</b-button>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="form-edit" v-else>
      <button @click.prevent="postEdit = null" type="button" class="close" aria-label="Close">
        <span aria-hidden="true">&times;</span>
      </button>
      <form-edit-post :post.sync="postEdit" :state="state" v-if="postEdit != null"/>
    </div>
  </div>
</template>

<script>
import FormEditPost from './FormEditPosts.vue'
export default {
  name: 'TablePosts',
  props: ['posts', 'state'],
  components: {
    'form-edit-post': FormEditPost
  },
  data() {
    return {
      postEdit: null
    }
  },
  methods: {
    edit(post){
      this.postEdit = null
      this.postEdit = post
    },
    removePost(path){
      DB.ref(path).remove()
    }
  },
}
</script>

<style lang="scss" scoped>

</style>