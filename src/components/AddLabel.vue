<template>
  <div class="bg-black-800 py-10">
    <h2 class="text-2xl py-2 text-white font-bold">Add Label</h2>
    <div class="flex flex-col space-y-4 mx-8 md:mx-72">
      <div class="flex flex-col space-y-6">
        <div class="text-white flex items-start">
          <span class="bg-amber-500 rounded py-2 px-3 text-white font-bold"
            >1</span
          >
          <span class="ml-4 py-2"
            >Set the endpoint to http://localhost:3000/add-label</span
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
        <pre class="text-left text-white bg-neutral-700 rounded p-2">
{
  "id": "note-id",
  "labelId": "label-content"
}</pre
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
    "id": "note-id",
    "labelId": "label-content"
  }`,
      endpoint: "http://localhost:3000/add-label",
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
