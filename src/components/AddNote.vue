<template>
  <div class="bg-black-800 py-10">
    <h2 class="text-2xl py-2 text-white font-bold">Add Note</h2>
    <div class="flex flex-col space-y-4 mx-8 md:mx-72">
      <div class="flex flex-col space-y-6">
        <div class="text-white flex items-start">
          <span class="bg-amber-500 rounded py-2 px-3 text-white font-bold"
            >1</span
          >
          <span class="ml-4 py-2"
            >Set the endpoint to http://localhost:3000/add-note</span
          >
        </div>
        <div class="text-white flex items-start">
          <span class="bg-amber-500 rounded py-2 px-3 text-white font-bold"
            >2</span
          >
          <span class="ml-4 py-2"
            >Insert and modify the JSON request data into the code block</span
          >
        </div>
        <span class="py-2 bg-amber-400 text-white bg-opacity-80 rounded">
          Make sure to save the note ID somewhere safe in an actual integration.
          Use proper tiptap-formatted content to create a new note, since
          Markdown content won't be parsed automatically.
        </span>
        <pre class="text-left text-white bg-neutral-700 rounded p-2">
{
    "id": "test1",
    "title": "Sample Note",
    "content": {
        "type": "doc",
        "content": [
            {
                "type": "paragraph",
                "content": [
                    {
                        "type": "text",
                        "text": "This is a sample note content."
                    }
                ]
            }
        ]
    },
    "isBookmarked": false,
    "isArchived": false
}
</pre
        >
      </div>
      <code-editor v-model="requestBody"></code-editor>
      <input
        class="w-full p-2 rounded bg-neutral-700 text-white outline-none focus:ring focus:ring-amber-400 focus:ring-opacity-80"
        v-model="endpoint"
        placeholder="Endpoint URL"
      />
      <input
        class="w-full p-2 rounded bg-neutral-700 text-white outline-none focus:ring focus:ring-amber-400 focus:ring-opacity-80"
        v-model="token"
        placeholder="Bearer Token"
      />
      <button class="p-2 bg-amber-500 text-white rounded" @click="sendRequest">
        Send Request
      </button>
      <pre>{{ response }}</pre>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CodeEditor from "./CodeEditor.vue";

export default {
  components: { CodeEditor },
  data() {
    return {
      requestBody: `{
    "id": "note.id",
    "title": "Your Title",
    "content": {
      "type": "doc",
      "content": [
        {
          "type": "paragraph",
          "content": [
            {
              "type": "text",
              "text": "This is a sample note content."
            }
          ]
        }
      ]
    },
    "labels": [],
    "isBookmarked": false,
    "isArchived": false
  }`,
      endpoint: "http://localhost:3000/add-note",
      token: "",
      response: "",
    };
  },
  methods: {
    async sendRequest() {
      try {
        const requestData = JSON.parse(this.requestBody);
        const res = await axios.post(this.endpoint, requestData, {
          headers: {
            Authorization: `Bearer ${this.token}`,
            "Content-Type": "application/json",
          },
        });
        this.response = JSON.stringify(res.data, null, 2);
      } catch (error) {
        this.response = `Error: ${error.message}`;
      }
    },
  },
};
</script>
