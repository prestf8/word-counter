<template>
    <div class="stats-display">
        <div class="stats-display__container">
            <p class="stats__words"><span class="bold">Words:</span> {{getWordLength}}</p>
            <p class="stats__chars"><span class="bold">Characters:</span> {{getCharLength}}</p>
            <p class="stats__sentences"><span class="bold">Sentences:</span> {{getSentenceNumber}}</p>
            <p class="stats__paragraphs"><span class="bold">Paragraphs:</span> {{getParagraphNumber}}</p>
            <p class="stats__reading-time"><span class="bold">Reading Time:</span> {{getReadingTime}}s</p> 
            <p class="stats__speaking-time"><span class="bold">Speaking Time:</span> {{getSpeakingTime}}s</p>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";


@Component
export default class StatsDisplay extends Vue {
    @Prop() content!: string;

    public withWhiteSpaceContent() {
        console.log("withWhiteSpaceContent");
    }


    // Word Count
    get getWordLength() {
        const wordReg = /\b\w+\b/g;

        return this.content.split(wordReg).length-1;
    }

    // Character Count
    get getCharLength() {
        return this.content.length;
    }

    // Sentence Count
    get getSentenceNumber() {
        // \w[.?!](\s|$)
        const sentenceReg = /\w[?."!](\s?|$)/g;
        const matches = this.content.split(sentenceReg)!;

        return (matches.length-1)/2;
    }

    // Paragraph Count
    get getParagraphNumber() {
        if (this.content.length == 0) {
            return 0;
        }

        return this.content.replace(/\n$/gm, '').split(/\n/).length;
    }

    // Reading Time
    get getReadingTime() {
        return Math.round((this.content.length / 1100) * 60);
    }

    // Speaking Time
    get getSpeakingTime() {
        return Math.round((this.content.length / 750) * 60);
    }
}
</script>

<style lang="scss">
    @import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');

    $darkgray: #2b2b2b;

    .bold {
        font-weight: bold;
    }

    .stats-display {
        text-align: left;
        height: 80%;
        width: 90%;
        font-family: 'Montserrat', sans-serif;
        color: $darkgray;
        // background: yellow;
    }

    .stats-display__container {
        width: 100%;
        padding: 1rem;
    }

    @media screen and (min-width: 900px) {
        .stats-display {
            display: flex;
        }

        .stats-display__container {
            // background: green;
            margin: auto;
        }
    }
</style>