
<template>
    <div class="lds-ripple" v-if="isVisible">
        <div></div>
        <div></div>
    </div>
</template>

<script>
    export default {
        name: "Loader2",
        props: {
            isVisible: {
                type: Boolean,
                required: true
            },
        },
}
</script>

<style lang="scss">
    .lds-ripple {
        display: inline-block;
        position: fixed;
        z-index: 100;
        bottom: 0;
        width: 50px;
        height: 50px;
        div {
            position: absolute;
            border: 4px solid #b4bdbf;
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

</style>
