<template>
    <div id="repos">
        <repo v-for="repo in repos" :repo="repo"/>
    </div>
</template>

<script>
import repo from './Repo.vue'
import axios from 'axios' 
import $ from 'jquery'
export default {
    components:{
        repo,
    },
    data(){
        return {
            //loaded repos
            repos:[
                {
                    id:1,
                    title:"Repo 1"
                },
                {
                    id:2,
                    title:"Repo 2"
                }
            ],
            // page counter 
            currentPage:1,
            // the date 30 days before
            priorDate:String
        }
    },
    methods:{
        // method to get the next page of repos from the api response and add it to the previously loaded repos  
        getMoreRepos(){
            // increment page counter to get the next page
            this.currentPage++;
            axios.get(`https://api.github.com/search/repositories?q=created:>${this.priorDate}&sort=stars&order=desc&page=${this.currentPage}`).then(res=>{
                this.repos=[...this.repos,...res.data.items];    
            }).catch(err=>{
                console.log(err);
            });
        },
        // method to tell if the user reached the bottom of the page, if so execute getMoreRepos method 
        hasReachedBottom(){
            if($(window).scrollTop() + $(window).height() == $(document).height()) {
                 this.getMoreRepos();
            }
        }
    },
    created(){
        // Get the date 30 days before 
        let priorDate=(new Date().setDate(new Date().getDate()-30));  
        // Convert the date into yyyy-mm-dd format and store it in the component state
        this.priorDate=new Date(priorDate).toJSON().slice(0,10);
        // Send an HTTP get request to the GitHub API to get the most starred repos 
        axios.get(`https://api.github.com/search/repositories?q=created:>${this.priorDate}&sort=stars&order=desc`).then(res=>{
            this.repos=res.data.items;
            console.log(res.data.items[0]);
        }).catch(err=>{
            console.log(err);
        });
        // Execute hasReachedBottom method whenever the user scrolls 
        window.addEventListener('scroll', this.hasReachedBottom);

    }

}
</script>

<style lang="scss" scoped>

</style>