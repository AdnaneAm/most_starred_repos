<template>
    <div id="header">
        <div class="search-bar">
            <input type="text" @keyup.enter="emitUserSearch" v-model="typedRepo" placeholder="Search for a repo ..">
            <svg @click="emitUserSearch" id="Capa_1" enable-background="new 0 0 515.558 515.558" height="512" viewBox="0 0 515.558 515.558" width="512" xmlns="http://www.w3.org/2000/svg"><path d="m378.344 332.78c25.37-34.645 40.545-77.2 40.545-123.333 0-115.484-93.961-209.445-209.445-209.445s-209.444 93.961-209.444 209.445 93.961 209.445 209.445 209.445c46.133 0 88.692-15.177 123.337-40.547l137.212 137.212 45.564-45.564c0-.001-137.214-137.213-137.214-137.213zm-168.899 21.667c-79.958 0-145-65.042-145-145s65.042-145 145-145 145 65.042 145 145-65.043 145-145 145z"/>
            </svg>
        </div>
    </div>
</template>
<script>
export default {
    name:"Header",
    data(){
        return{
            typedRepo:'',
        }
    },
    computed:{

    },
    methods:{
        emitUserSearch(){
            this.$emit('usr-search',this.typedRepo);
        }
    }
}
</script>

<style lang="scss">
    #header{
        width: 100%;
        padding: 1em;
        background: #fff;
        box-shadow: 2px 14px 76px -26px rgba(0, 0, 0, 0.26);
        margin-bottom: 4em;
        .search-bar{
            position:relative;
            width: 300px;
            margin:0 auto;
            input{
                width:100%;
                border: none;
                padding: 1.2em;
                background: #f3f9fd;
                font-weight: 600;
                color: #6f6d6d;
                font-size: .9rem;
                border-radius: 0.5em;
                &:focus{
                    border:none;
                    outline:none;
                }
                &::placeholder{
                    color:#c8d4e0;
                }
            }
            svg{
                position: absolute;
                width: 20px;
                height: 20px;
                right: -15px;
                top: 15px;
                fill: #687488;
                cursor:pointer;

            }
        }
    }
</style>