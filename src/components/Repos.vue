<template>
    <div id="repos">
        <Repo v-for="repo in repos" :repo="repo"/>
        <Loader :is-visible="isLoading"/>
    </div>
</template>
<script>
import Repo from './Repo.vue'
import Loader from './Loader.vue'
import axios from 'axios' 
import $ from 'jquery'
export default {
    name:"Repositories",
    components:{
        Repo,Loader
    },
    data(){
        return {
            //loaded repos
            repos:[],
            // page counter 
            currentPage:1,
            // the date 30 days before
            priorDate:String,
            isLoading:false,
        }
    },
    props:{
            // The search value that the user types in search bar
        searchTerm:{
            type:String,
            default:''
        }
    },
    watch:{
        // Whenever the searchTerm changes (whenever the user types smthng in the form and want to look for it) we will execute an HTTP request to the API to get new repos based on the search term
        searchTerm(newV,oldV){
            this.isLoading=true;
            $('html').css('overflow','hidden');
            axios.get(`https://api.github.com/search/repositories?q=${newV}+created:>${this.priorDate}&sort=stars&order=desc`).then(res=>{
                this.repos=res.data.items;
            }).catch(err=>{
                console.log(err);
            }).finally(()=>{
            $('html').css('overflow','auto');
            this.isLoading=false;
            });
        }
    },
    methods:{
        // method to get the next page of repos from the api response and add it to the previously loaded repos  
        getMoreRepos(){
            // increment page counter to get the next page
            this.currentPage++;
            axios.get(`https://api.github.com/search/repositories?q=${this.searchTerm}+created:>${this.priorDate}&sort=stars&order=desc&page=${this.currentPage}`).then(res=>{
                this.repos=[...this.repos,...res.data.items];  
            }).catch(err=>{
                console.log(err);
            });
        },
        // method to tell if the user reached the bottom of the page, if so execute getMoreRepos method 
        hasReachedBottom(){
            if($(window).scrollTop() + $(window).height() > $(document).height()*(0.8)) {
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
            this.isLoading=true;
        axios.get(`https://api.github.com/search/repositories?q=created:>${this.priorDate}&sort=stars&order=desc`).then(res=>{
            this.repos=res.data.items;
        }).catch(err=>{
            // Appending an error message to the page
            $("#repos").append('<p class="error"><svg id="bold" enable-background="new 0 0 24 24" height="512" viewBox="0 0 24 24" width="512" xmlns="http://www.w3.org/2000/svg"><path d="m14.25 0h-11.5c-1.52 0-2.75 1.23-2.75 2.75v15.5c0 1.52 1.23 2.75 2.75 2.75h4.9c.1-.4.28-.78.51-1.13l5.22-8.48c.67-1.15 1.97-1.89 3.37-1.89.08 0 .17 0 .25.01v-6.76c0-1.52-1.23-2.75-2.75-2.75zm-10.25 4h4c.55 0 1 .45 1 1s-.45 1-1 1h-4c-.55 0-1-.45-1-1s.45-1 1-1zm5 10h-5c-.55 0-1-.45-1-1s.45-1 1-1h5c.55 0 1 .45 1 1s-.45 1-1 1zm4-4h-9c-.55 0-1-.45-1-1s.45-1 1-1h9c.55 0 1 .45 1 1s-.45 1-1 1z"/><path d="m23.707 21.024-5.278-8.574c-.345-.586-.989-.95-1.679-.95s-1.334.364-1.669.934l-5.266 8.556c-.206.306-.315.671-.315 1.055 0 1.078.88 1.955 1.962 1.955h10.576c1.082 0 1.962-.877 1.962-1.955 0-.384-.109-.749-.293-1.021zm-6.957.976c-.552 0-1-.448-1-1s.448-1 1-1 1 .448 1 1-.448 1-1 1zm1-3.5c0 .552-.447 1-1 1s-1-.448-1-1v-2c0-.552.447-1 1-1s1 .448 1 1z"/></svg>There was an error while fetching repositories. Please try again!</p>');
        }).finally(()=>{
            this.isLoading=false;
        });
        // Throttle function to fire hasRecheadBottom once when the user is near bottom of the page ( Throttling )
        const throttle=(fn,delay)=>{
            let last=0;
            return()=>{
                const now=new Date().getTime();
                if(now - last < delay+1000){
                    return;
                }
                last=now;
                return fn();
            }
        }
        window.addEventListener('scroll',throttle(e=>{
            this.hasReachedBottom();
        },50));
        window.addEventListener('scroll',function(){
            console.log($(window).scrollTop(),$(window).height(),$(document).height());
        })

    }

}
</script>
<style lang="scss">
    #repos{
        .error{
            font-weight: bold;
            font-size: 1.2rem;
            color: #687680;
            svg{
                display: block;
                width: 50px;
                height: 50px;
                margin-bottom: 1em;
                margin: 1em auto;
                fill: #607D8B;
            }
        }
    }

</style>