<template>
  <div class="show">
    <b-modal id="modal-xl" size="xl" hide-header hide-footer>
      <div class="row">
        <div class="col-sm-12">
          <div id="preview" class="col-md-12" v-html="markdown(postChild.content)"></div>
					<b-button type="button" aria-label="Close" class="close" @click="postChild = null">×</b-button>
        </div>
      </div>			
		</b-modal>
  </div>
</template>

<script>
import marked from "marked";
import highlight from 'highlight.js'
import 'highlight.js/styles/github-gist.css'
import { DB } from './../../plugins/firebase'

export default {
	name:"Show",
	props: ['post'],
	computed: {
		postChild: {
			get () {
				return this.post
			},
			set (value){
				this.$emit("update:post", value)
			}      
		},	
	},
  components: {

	},
	methods: {
    markdown(content) {
      return marked(content, {
        highlight(md){
          return highlight.highlightAuto(md).value
        },
      })      
    }
	},
  data() {
    return {

    }
  },
  firebase: {

  },  
}
</script>

<style lang="scss" scoped>

</style>