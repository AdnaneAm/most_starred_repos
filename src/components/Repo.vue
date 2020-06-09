<template>
    <div class="repo">
        <div class="repo-avatar" :style="`background-image:url(${repo.owner.avatar_url})`"></div>
        <div class="repo-content">
            <h1 class="repo-name">{{repo.name}}</h1>
            <p class="repo-desc">{{repo.description}}</p>
            <div class="repo-footer">
                <span class="stars-nb">{{repo.stargazers_count}}</span>
                <span class="issues-nb">{{repo.open_issues_count}}</span>
                <span>Submitted <span>{{time_interval}} days ago</span> By {{repo.owner.login}}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props:{
        repo:Object,
    },
    computed:{
        // Computed property for calculting how many days have passed since the creation of the repository
        time_interval(){
            let diff,
            aDay=86400000,
            end_date=new Date().toJSON().slice(0,10),
            start_date=this.repo.created_at.slice(0,10);
            diff = Math.floor((Date.parse(end_date.replace(/-/g, '\/')) - Date.parse(start_date.replace(/-/g, '\/')))/aDay);
            return diff;
        }
    },
}
</script>

<style lang="scss" scoped>
    .repo{
        .repo-avatar{
            height: 50px;
            width:50px;
            background-size:cover;
        }
    }
</style>