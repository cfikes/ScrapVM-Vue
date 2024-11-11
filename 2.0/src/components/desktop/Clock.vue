<template>
    <span class="systemClock">{{ clock }}</span>
</template>

<script>
export default {
    data() {
        return {
         clock: '',   
        }
    },
    mounted() {
        this.updateClock();
        setInterval(this.updateClock, 1000);
    },
    methods: {
        updateClock() {
            const now = new Date();
            const hours = now.getHours();
            const minutes = now.getMinutes();
            const ampm = hours >= 12 ? 'PM' : 'AM';
            const hours12 = hours % 12 || 12;
            const minutesStr = minutes < 10 ? '0' + minutes : minutes;
            this.clock = `${hours12}:${minutesStr} ${ampm}`;
        }   
    }
}
</script>

<style scoped>
.systemClock {
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}

.systemClock:hover  {
    cursor: pointer;
    color: aqua;
}

</style>