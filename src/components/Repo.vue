<template>
    <div class="repo">
        <div class="repo-avatar" :style="`background-image:url(${repo.owner.avatar_url})`">
        </div>
        <div class="repo-content">
            <h1 class="repo-name">{{repo.name}}</h1>
            <p class="repo-desc">{{repo.description==null?'No Description':repo.description}}</p>
            <div class="repo-footer">
                <span class="stars-nb">Stars <span>{{intToString(repo.stargazers_count)}}</span></span>
                <span class="issues-nb">Issues <span>{{intToString(repo.open_issues_count)}}</span></span>
                <div>
                    <span class="time-interval">Submitted <span>{{time_interval}} days ago </span></span>
                    <span class="repo-owner"> by <span>{{repo.owner.login}}</span></span> 
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name:"Repository",
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
        // Method to abbreviate big numerical values into representative strigns (1340 => 1.3K)
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
            height:150px;
            background-size:contain;
            display: flex;
            justify-content: center;
            align-items: center;
            position:relative;
            border-radius: 0.5em;
        }
        .repo-content{
            padding:0 .5em 0 1em;
            display: flex;
            flex-wrap: wrap;
            flex: 1;
            .repo-name{
                width:100%;
                font-size: 1.5rem;
                font-family: Roboto;
                font-weight: 600;
                margin: 0.5rem 0 0.1em 0 !important;
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
                width:100%;
                padding-top: 1rem;
                div{
                    display:inline-block;
                }
                span{
                    font-weight: 500;
                    color:#474a4c;
                    font-size: 0.9rem;
                }
                .stars-nb,.issues-nb{
                    padding: 0.7rem 1rem;
                    display: inline-block;
                    border-radius: 1.5rem;
                    font-size: 0.8rem;
                    font-weight: 600;
                    margin-right:1rem;
                    background: #f3f7fa;
                    color: #96a1a9;
                    transition:all .3s ease-in-out;
                    &:hover{
                        background:#eceff1;
                    }
                    span{
                        color:#4f565a;
                        font-weight:bold;
                    }
                }
                .time-interval,.repo-owner{
                    color:#babeca;
                    span{
                        color:#758188;
                        letter-spacing: -0.02em;
                    }
                }
                .repo-owner{
                    white-space: nowrap;
                    text-overflow: ellipsis;
                    width: 130px;
                    display: inline-block;
                    overflow: auto;
                    position: relative;
                    top: 3px;
                }
            }
        }

    }
    // Media queries
    // Small devices
    @media screen and (max-width:800px) {
        .repo{
            width: 80%;
            .repo-avatar{
                height:auto;
                background-size:cover;
                background-position:center center;
            }
            .repo-footer{
                div{
                    display:block !important;
                }
                .repo-owner,.time-interval{
                    display:inline-block;
                    width: 50% !important;
                    margin-top: 1rem;
                    span{
                        font-weight:bold;
                        text-overflow:none;
                        width:auto;
                    }
                }
                .repo-owner{
                    text-align:right;
                }
            }
        }

    }
    // Extra small devices
    @media screen and (max-width:630px){
        .repo{
            flex-wrap:wrap;
            .repo-avatar{
                height:180px;
                width:100%;
            }
            .repo-content{
                .repo-name{
                    margin:1rem 0 .5rem 0 !important;
                }
            }
        }
    }
</style>