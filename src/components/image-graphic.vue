<template>
    <img
        :src="payload.src"
        :class="payload.class || 'px-10 my-8'"
        :style="payload.style"
        :alt="payload.alt"
        class="block bg-gray-200_ _max-w-full story-graphic"
    />
</template>

<script lang="ts">
import { ImageGraphic } from '@/story-config';
import { Component, Vue, Prop } from 'vue-property-decorator';

@Component
export default class ImageGraphicV extends Vue {
    @Prop() payload!: ImageGraphic['payload'];
}
</script>

<style lang="sass" scoped></style>
