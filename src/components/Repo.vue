<template>
    <div class="repo">
        <div class="repo-avatar" :style="`background-image:url(${repo.owner.avatar_url})`">
        </div>
        <div class="repo-content">
            <h1 class="repo-name">{{repo.name}}</h1>
            <p class="repo-desc">{{repo.description}}</p>
            <div class="repo-footer">
                <span class="stars-nb">Stars <span>{{intToString(repo.stargazers_count)}}</span></span>
                <span class="issues-nb">Issues <span>{{intToString(repo.open_issues_count)}}</span></span>
                <span>Submitted <span class="time-interval">{{time_interval}} days ago</span> by <span class="repo-owner">{{repo.owner.login}}</span></span>
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
    methods:{
        intToString(value){
            let suffixes = ["", "k", "m", "b","t"];
            let suffixNum = Math.floor((""+value).length/3);
            let shortValue = parseFloat((suffixNum != 0 ? (value / Math.pow(1000,suffixNum)) : value).toPrecision(2));
            if (shortValue % 1 != 0) {
                shortValue = shortValue.toFixed(1);
            }
            return shortValue+suffixes[suffixNum];
        }
    }
    
}
</script>

<style lang="scss" scoped>
    .repo{
        box-shadow:2px 14px 76px -26px rgba(0,0,0,0.26);
        border-radius: 0.5em;
        background: white;
        width: 700px;
        margin: 2em auto;
        text-align: left;
        display: flex;
        padding: 1rem;
        min-height:150px;
        .repo-avatar{
            width: 150px;
            background-size:cover;
            display: flex;
            justify-content: center;
            align-items: center;
            position:relative;
            border-radius: 0.5em;
        }
        .repo-content{
            padding:0 1.5em;
            display: flex;
            flex-wrap: wrap;
            flex: 1;
            .repo-name{
                width:100%;
                font-size: 1.5rem;
                font-family: Roboto;
                font-weight: 600;
                margin: .5rem 0 !important;
            }
            .repo-desc{
                font-size: 1rem;
                font-family: Roboto;
                color: #aaabb7;
                font-weight: 400;
                letter-spacing: 0.3pt;
                line-height: 1.3rem;
            }
            .repo-footer{
                align-self:flex-end;
                span{
                    font-weight: 500;
                    color:#474a4c;
                    font-size: 0.9rem;
                }
                .stars-nb,.issues-nb{
                    padding: 0.5rem 1rem;
                    display: inline-block;
                    border: 2px solid #6d6d6d;
                    border-radius: 1.5rem;
                    font-size: 0.8rem;
                    font-weight: 600;
                    margin: 1rem 1rem 1rem 0;
                    span{
                        color: #4e4e4e;
                        font-weight:bold;
                    }
                }
                .time-interval,.repo-owner{
                    font-weight: 600;
                    letter-spacing: -0.04em;
                    color: #797979;
                }

            }
        }

    }
</style>