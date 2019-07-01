<template>
    <div class="arrow">
        <arrow class="prev" :class="{disabled: currentSlide === 0}" @touchend="$emit('prev')"/>
        <arrow class="next" :class="{disabled: currentSlide === 5}" @touchend="$emit('next')"/>
    </div>
</template>

<script>
    import arrow from "@/assets/arrow.svg"
    export default {
        name: "arrows",

        components: {
            arrow
        },

        mounted(){

        },

        props: [
            'currentSlide'
        ]
    }
</script>

<style scoped lang="scss">
    .arrow {
        width: 577px;
        height: 213px;

        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        bottom: 53.2px;
        z-index: 1;

        display: flex;
        justify-content: space-between;
    }

    .prev, .next {
        height: 100%;

        &.disabled {
            opacity: .2;
        }
    }

    .prev {
        transform: rotate(180deg);
    }
</style>