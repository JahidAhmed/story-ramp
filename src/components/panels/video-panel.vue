<template>
    <div class="video-container self-start justify-center flex flex-col h-full align-middle py-5 w-full">
        <!-- YouTube video -->
        <template v-if="config.videoType === 'YouTube'">
            <iframe
                :src="config.src"
                :width="config.width ? config.width : '100%'"
                :height="config.height ? config.height : '500px'"
                style="position: relative; left: 50%; transform: translateX(-50%)"
                allowfullscreen
            ></iframe>
        </template>

        <!-- video with local/external source -->
        <template v-if="config.videoType === 'local' || config.videoType === 'external'">
            <video
                :title="config.title"
                :poster="config.thumbnailUrl"
                :width="config.width ? config.width : '100%'"
                :height="config.height ? config.height : '500px'"
                style="position: relative; left: 50%; transform: translateX(-50%)"
                controls
            >
                <source :type="fileType" :src="config.src" />
                <!-- add captions  -->
                <track
                    kind="captions"
                    :src="config.caption"
                    :srclang="lang"
                    :label="langs[lang]"
                    v-if="config.caption"
                />
            </video>
            <!-- Optional: can also provide timestamps with <button data-cuepoint="[time]"> here -->
        </template>

        <!-- title of the video with transcript -->
        <figcaption>
            <p>
                {{ config.title }}
                <span v-if="config.transcript">(<a :href="config.transcript" target="_NEW"></a>)</span>
            </p>
        </figcaption>
    </div>
</template>

<script lang="ts">
import { VideoPanel } from '@storylines/definitions';
import { Component, Vue, Prop } from 'vue-property-decorator';

// TODO ITEMS:
// - test accessibility (mainly transcripts)
// - add video dimensions support (height and width config options), current video panel taking way too much space (make sure
// the panel only takes the amount of space that is required even if no config dimensions are specified)
// - check mobile resolution (especially after the dimensions are fixed)
// - add/test full screen functionality
// - test local video file (add it to the assets folder or something)
// * most important - make it work for dynamic panels since video player may not be initialized at runtime
@Component({})
export default class VideoPanelV extends Vue {
    @Prop() config!: VideoPanel;

    lang = 'en';
    langs: Record<string, string> = {
        en: 'English',
        fr: 'French'
    };
    fileType = '';

    // NOTE: add data-wb-mltmd='{"shareUrl": "url"}' to figure element for providing share link
    created(): void {
        this.lang = this.$route.params.lang ? this.$route.params.lang : 'en';

        // find file type extension for non-YT videos
        if (this.config.videoType === 'external' || this.config.videoType === 'local') {
            const fileName = this.config.src.substring(this.config.src.lastIndexOf('/') + 1);
            const ext = fileName.split('.').pop();
            this.fileType = `video/${ext}`;
        }
    }
}
</script>

<style lang="scss">
@media screen and (max-width: 640px) {
    .video-container {
        max-width: 100vw;
        max-height: 38vh;
        background-color: white;
    }
}
</style>
