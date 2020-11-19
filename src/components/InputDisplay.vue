<template>
    <div class="input-display">
        <div class="input-display__input-container">
            <div class="input-display__controls">
                <button class="input-display__clear" @click="clear">Clear</button>
                <button class="input-display__undo" @click="undo">Undo</button>
                <button class="input-display__redo" @click="redo">Redo</button>
                <button class="input-display__without-whitespace" :class="{active: ws, nonactive: !ws}" @click="sendToggleWhiteSpace">Whitespace</button>
            </div>
            <textarea class="input-display__textarea" v-model="content" @input="sendContent" ref="textarea" />
        </div>
        <div class="input-display__stats-display">
            <StatsDisplay :content="content"></StatsDisplay>
        </div>
    </div>
</template>



<script lang = "ts">
import { Component, Vue, Emit, Prop, Watch } from "vue-property-decorator";
import StatsDisplay from './StatsDisplay.vue';

@Component({
    components: {
        StatsDisplay,
    }
})
export default class InputDisplay extends Vue {
    @Prop() ws!: boolean;

    private content = "";

    private undoStages: string[] = [];
    private redoStages: string[] = [];

    // get textarea(): Vue & { focusInput: () => boolean } {
    //     return this.$refs.textarea as Vue & { focusInput: () => boolean }
    // }

    @Watch("ws")
    whiteSpaceChange() {
        if (!this.ws) {
            const whitespaceRemover = /\s{2,}/g;
            this.content = this.content.replace(whitespaceRemover, '');
        } 
        // this.$refs.textarea.focusInput();
        (this.$refs.textarea as Vue & {focus: () => boolean}).focus();

        this.sendContent(this.content);
    }



    @Emit("send-content")
    sendContent(content: string) {
        if (!this.ws) {
            const whitespaceRemover = /\s{2,}/g;
            this.content = this.content.replace(whitespaceRemover, '');
        } 

        this.undoStages.push(this.content);

        return this.content;
    }

    public undo() {
        if (this.undoStages.length > 0) {
            const undoMaterial = this.undoStages.pop()!;
            this.content = this.undoStages[this.undoStages.length-1];

            this.redoStages.push(undoMaterial);
        }
    }

    public redo() {
        if (this.redoStages.length > 0) {
            this.content = this.redoStages[this.redoStages.length-1];
            const redoMaterial = this.redoStages.pop()!;

            this.undoStages.push(redoMaterial);
        }
    } 

    public clear() {
        this.content ="";
        this.$emit("clear-content");
    }

    public sendToggleWhiteSpace() {
        this.$emit("send-whitespace")
    }
     
}
</script>

<style lang="scss">

$darkgray: #212121;

%inputDisplayButton {
    padding: 0.5rem;
    border-radius: 5px;
    border: 3px solid $darkgray;
    margin: 4px;
    outline: none;
    font-size: 0.8rem;
    color: white;
}

.nonactive {
    background-color: coral;
}


.active {
    background-color: green;
}

.input-display {
    text-align: center;

    padding: 0.8rem;

    display: grid;
    grid-template-columns: 100%;
    grid-template-rows: auto;
}

.input-display__input-container {
    display: flex;
    flex-flow: column nowrap;
    align-items: center;
}

.input-display__controls {
    display: flex;
    justify-content: center;
    flex-flow: row wrap;
}

.input-display__redo, .input-display__undo, .input-display__clear {
    background-color: coral;
}

.input-display__controls > button {
    @extend %inputDisplayButton;
    transition: all .2s;
    
}

.input-display__controls > button:hover {
    // background: lightcoral;
    cursor: pointer;
    opacity: 0.8;
}

.input-display__textarea {
    display: inline-block;
    width: 100%;
    height: 300px;
    padding: 0.5rem;
    outline: none;
    border-radius: 5px;
    border: 5px solid $darkgray;
}

.input-display__stats-display {
    // background: red;
    font-size: 1.7rem;
}

@media screen and (min-width: 400px) {
    %inputDisplayButton {
        padding: 0.9rem;
        font-size: 1rem;
    }
}

@media screen and (min-width: 600px) {
    %inputDisplayButton {
        padding: 1rem;
        font-size: 1.2rem;
    }

    .input-display__textarea {
        min-height: 400px;
    }

}

@media screen and (min-width: 900px) {
    %inputDisplayButton {
        padding: 1.2rem;
        font-size: 1.4rem;
    }

    .input-display {
        grid-template-columns: 60% 40%;
        grid-template-rows: auto;
    }

    .input-display__stats-display {
        font-size: 1.9rem;
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: 1rem;

    }

    .input-display__textarea {
        min-height: 450px;
        margin: 1rem;
        font-size: 1.2rem;
    }
}

@media screen and (min-width: 1200px) {
    .input-display__stats-display {
        font-size: 2.2rem;
    }
}

@media screen and (min-width: 1700px) {
    %inputDisplayButton {
        padding: 1.3rem;
        margin: 0.5rem;
        font-size: 1.6rem;
    }

    .input-display__textarea {
        font-size: 1.4rem;
    }

    .input-display__stats-display {
        font-size: 3rem;
    }

    .input-display__textarea {
        min-height: 560px;
    }
}





</style>