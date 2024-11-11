<template>
    <div class="globalHeader">
        Content
        <div class="left">
            <GlobalMenu></GlobalMenu>
        </div>
        
        <div class="center">
        </div>

        <div class="right">
            <Clock></Clock>
        </div>
        
    </div> 
</template>

<script>
import GlobalMenu from './GlobalMenu.vue';
import Clock from './Clock.vue';

export default {
    name: 'GlobalHeader',
    components: {
        GlobalMenu,
        Clock
    },
    data() {
        return {
            
        }
    },
    mounted() {
        
    },
    methods: {
        
    }
}
</script>

<style scoped>
</style>