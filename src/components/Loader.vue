
<template>
<div class="loader-container" v-if="isVisible"> 
    <div class="lds-ripple">
        <div></div>
        <div></div>
    </div>
</div>
</template>

<script>
    export default {
        name: "Loader",
        props: {
            isVisible: {
                type: Boolean,
                required: true
            },
            text: { 
                type: String,
                required: false, 
                default: ''
            },
        },
}
</script>

<style lang="scss">
.loader-container{
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    background: #00000057;
    .lds-ripple {
        display: inline-block;
        position: absolute;
        z-index: 100;
        top:50%;
        width: 80px;
        height: 80px;
        div {
            position: absolute;
            border: 4px solid #fff;
            opacity: 1;
            border-radius: 50%;
            animation: lds-ripple 1s cubic-bezier(0, 0.2, 0.8, 1) infinite;
        }
        div:nth-child(2) {
            animation-delay: -0.5s;
        }
    }

        @keyframes lds-ripple {
        0% {
            top: 36px;
            left: 36px;
            width: 0;
            height: 0;
            opacity: 1;
        }
        100% {
            top: 0px;
            left: 0px;
            width: 72px;
            height: 72px;
            opacity: 0;
        }
        }
}
</style>
