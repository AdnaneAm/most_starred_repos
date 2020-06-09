<template>
    <div class="repo">
        {{repo.full_name}}
    </div>
</template>

<script>
export default {
    props:{
        repo:Object
    }
}
</script>

<style lang="scss" scoped>
    .repo{
        height:50px;
    }
</style>