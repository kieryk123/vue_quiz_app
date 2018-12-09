<template>
    <div class="progress-bar">
        <span class="progress-bar__text">{{ currentValue }} / {{ maximumValue }}</span>
        <div class="progress-bar__filler" :style="{width: calculatedWidth}"></div>
    </div>
</template>

<script>
export default {
    computed: {
        calculatedWidth() {
            return `${this.currentValue * 10}%`;
        }
    },

    props: ['currentValue', 'maximumValue']
}
</script>

<style lang="scss">
.progress-bar {
    position: relative;
    height: 40px;
    width: 600px;
    background-color: $color-tertiary;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: $shadow-primary;
    margin: 30px 15px;

    @include breakpoint(630px) {
        width: 100%;
    }

    &__text {
        position: absolute;
        z-index: 1;
        top: calc(50% - 20px / 2);
        left: calc(50% - 60px / 2);
        color: $color-primary;
        height: 20px;
        width: 60px;
        text-align: center;
    }

    &__filler {
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        width: 40%;
        background-color: $color-secondary;
        transition: .3s all linear;
    }
}
</style>
