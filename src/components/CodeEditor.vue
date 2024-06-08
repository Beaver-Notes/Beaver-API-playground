<template>
  <div>
    <textarea placeholder="Insert request body" class="w-full p-2 rounded bg-neutral-700 text-white outline-none focus:ring focus:ring-amber-400 focus:ring-opacity-80" ref="textarea"></textarea>
  </div>
</template>

<script>
import { EditorState } from "@codemirror/state";
import { EditorView, basicSetup } from "@codemirror/basic-setup";
import { javascript } from "@codemirror/lang-javascript";

export default {
  props: {
    value: String,
  },
  data() {
    return {
      code: this.value,
    };
  },
  watch: {
    value(newValue) {
      if (newValue !== this.code) {
        this.code = newValue;
        this.updateEditor(newValue);
      }
    },
  },
  methods: {
    updateEditor(value) {
      this.editorView.dispatch({
        changes: { from: 0, to: this.editorView.state.doc.length, insert: value },
      });
    },
  },
  mounted() {
    this.editorView = new EditorView({
      state: EditorState.create({
        doc: this.code,
        extensions: [basicSetup, javascript()],
      }),
      parent: this.$refs.textarea,
    });

    this.editorView.dispatch({
      changes: { from: 0, insert: this.code },
    });

    this.editorView.updateListener = update => {
      if (update.docChanged) {
        this.code = this.editorView.state.doc.toString();
        this.$emit("input", this.code);
      }
    };
  },
};
</script>

<style>
.cm-editor {
  height: auto;
}
</style>
