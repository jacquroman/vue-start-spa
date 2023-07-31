<template>
  <div class="container mb-3">
      <form>
          <div class="mb-3">
              <label class="form-label">Page Title</label>
              <input type="text" class="form-control" v-model="pageTitle"/>
          </div>
          <div>
              <label class="form-label">
                  Content
              </label>
              <textarea type="text" class="form-control" rows="5" v-model="content"></textarea>
          </div>
              <div class="col">
              <div class="mb-3">
                  <label class="form-label">Link Text</label>
                  <input type="text" class="form-control" v-model="linkText"/>
              </div>
              <div class="mb-3">
                  <label class="form-label">
                      Link Url
                  </label>
                  <textarea type="text" class="form-control" rows="5" v-model="linkUrl"></textarea>
              </div>
                  <div class="row mb-3">
                      <div class="form-check">
                          <input class="form-check-input" type="checkbox" v-model="published">
                          <label class="form-check-label" for="gridCheck1">Published</label>
                      </div>
                  </div>
              <div>
                  <button class="btn btn-primary" :disabled="isFormInvalid" @click.prevent="submitForm">Create Page</button>
              </div>
          </div>
      </form>
  </div>
</template>

<script>
export default {
    name: "CreatePage",
    //emits is how you declare an event
    //creating custom events helps a lot in development
    //events bubble, you can trigger an event on an element and then you can listen for it higher up on the hierarchy
    emits: {
        //you can declare and validate your events like below but it is not required
        pageCreated(pageTitle, content, link, published){
            if(!pageTitle){
                return false;
            }
            if(!content){
                return false;
            }
            if(!link || !link.text || !link.url){
                return false;
            }
            return true;
        }
    },
    props: ['pageCreated'],
    computed:{
         isFormInvalid(){
            return !this.pageTitle || !this.content || !this.linkText || !this.linkUrl;
         }
    },
    data(){
         return{
             pageTitle:'',
             content: '',
             linkText: '',
             linkUrl: '',
             published: true
         }
    },
    methods: {
         submitForm(){
             if(!this.pageTitle || !this.content || !this.linkText || !this.linkUrl){
                 alert('Please fill in the form');
                 return;
             }

             this.$emit('pageCreated')

             this.pageCreated({
                 pageTitle: this.pageTitle,
                 content: this.content,
                 link: {
                    text: this.linkText,
                     url: this.linkUrl
                 },
                 published: this.published
             });

             this.pageTitle ='';
             this.content = '';
             this.linkText = '';
             this.linkUrl = '';
             this.published= true;
         }
    },
    watch: {
         //this works by watching the variable pageTitle and whenever that variable changes it will then execute
         pageTitle(newTitle, oldTitle){
             if(this.linkText == oldTitle){
                 this.linkText = newTitle;
             }
         }
    }
}
</script>

<style scoped>

</style>