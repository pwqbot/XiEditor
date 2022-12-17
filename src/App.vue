<template>
    <h1>XiEditor 6</h1>
    <div>
        <codemirror id="editor" v-model="code" placeholder="Code goes here..."
            :style="{ height: '500px', width: '600px' }" :autofocus="true" :indent-with-tab="true" :tab-size="2"
            :extensions="extensions" @ready="handleReady" @change="log('change', $event)" @focus="log('focus', $event)"
            @blur="log('blur', $event)" />
    </div>
    <div>
    <!--a button to compile code  -->
    <button @click="compile">Compile</button>

    </div>
</template>

<script lang="ts">
import { defineComponent, ref, shallowRef } from 'vue'
import { Codemirror } from 'vue-codemirror'
import { EditorView } from "@codemirror/view"
import { oneDarkTheme } from "@codemirror/theme-one-dark";
import { StreamLanguage, syntaxHighlighting, defaultHighlightStyle } from "@codemirror/language"
import { haskell } from "@codemirror/legacy-modes/mode/haskell"
import { materialLight } from '@ddietr/codemirror-themes/material-light'
import { materialDark } from '@ddietr/codemirror-themes/material-dark'
import { solarizedLight } from '@ddietr/codemirror-themes/solarized-light'
import { solarizedDark } from '@ddietr/codemirror-themes/solarized-dark'
import { dracula } from '@ddietr/codemirror-themes/dracula'
import { githubLight } from '@ddietr/codemirror-themes/github-light'
import { githubDark } from '@ddietr/codemirror-themes/github-dark'
import { aura } from '@ddietr/codemirror-themes/aura'
import { tokyoNight } from '@ddietr/codemirror-themes/tokyo-night'
import { tokyoNightStorm } from '@ddietr/codemirror-themes/tokyo-night-storm'
import { tokyoNightDay } from '@ddietr/codemirror-themes/tokyo-night-day'

export default {
    setup() {
        const code = ref(`console.log('Hello, world!')`)
        const extensions = [githubLight,
            StreamLanguage.define(haskell),
            syntaxHighlighting(defaultHighlightStyle),
        ]

        // Codemirror EditorView instance ref
        const view = shallowRef()
        const handleReady = (payload: { view: any; }) => {
            view.value = payload.view
        }

        // Status is available at all times via Codemirror EditorView
        const getCodemirrorStates = () => {
            const state = view.value.state
            const ranges = state.selection.ranges
            const selected = ranges.reduce((r: any, range: { to: any; from: number; }) => r + range.to - range.from, 0)
            const cursor = ranges[0].anchor
            const length = state.doc.length
            const lines = state.doc.lines
            // more state info ...
            // return ...
        }

        return {
            code,
            extensions,
            handleReady,
            log: console.log
        }
    },
    components: {
        Codemirror
    },
    name: 'app',
    data() {
        return {
            cmOption:
            {
                tabSize: 4,
                extensions: [
                    EditorView.lineWrapping,
                    EditorView.updateListener.of(update => {
                        console.log("update", update)
                    }),
                    oneDarkTheme,
                ]
            }
        }
    },
    methods: {
        compile() {
            console.log("compile")
        }
    },
    mounted() {
    }
}
</script>

<style>
.CodeMirror .CodeMirror-cursor {
    border-left: 1px solid #819090;
}

header {
    line-height: 1.5;
}

.logo {
    display: block;
    margin: 0 auto 2rem;
}

#editor {
    height: 600px;
    width: 1000px;
}

@media (min-width: 1024px) {
    header {
        display: flex;
        place-items: center;
        padding-right: calc(var(--section-gap) / 2);
    }

    .logo {
        margin: 0 2rem 0 0;
    }

    header .wrapper {
        display: flex;
        place-items: flex-start;
        flex-wrap: wrap;
    }
}
</style>
