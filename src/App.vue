<template>
    <navbar
        :pages="pages"
        :active-page="activePage"
    ></navbar>

    <router-view></router-view>

<!--    //below is no longer needed because the router will handle the information-->
<!--    <page-viewer :page="pages[activePage]"></page-viewer>-->

<!--    <create-page :page-created="pageCreated"></create-page>-->

</template>

<script>
import Navbar from './components/Navbar.vue';
import PageViewer from './components/PageViewer.vue';
import CreatePage from './components/CreatePage.vue';

export default {
    components: {
        Navbar,
        PageViewer,
        CreatePage
    },
    created(){
        this.getPages();

        this.$bus.$on('navbarLinkActived', (index) =>{
            this.activePage = index;
        });
    },
    data(){
        return {
            activePage: 0,
            pages: []
        };
    },
    methods: {
        async getPages(){
            let res = await fetch('pages.json');
            let data = await res.json();

            this.pages = data;
        },
        pageCreated(pageObj){
            //we need to modify the pages array here and all we need to do is push in the new page object
            //anything that modifies or mutates the pages array will signify to VUE to update anything in the code that uses the array
            this.pages.push(pageObj);
            console.log(pageObj);
        }
    }
}
</script>