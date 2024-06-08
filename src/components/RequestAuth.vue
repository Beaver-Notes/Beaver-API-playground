<template>
  <div class="bg-black-800 py-10">
    <h2 class="text-2xl py-2 text-white font-bold">Request Authentication</h2>
    <div class="flex flex-col space-y-4 mx-8 md:mx-72">
      <div class="flex flex-col space-y-6">
        <div class="text-white flex items-start">
          <span class="bg-amber-500 rounded py-2 px-3 text-white font-bold"
            >1</span
          >
          <span class="ml-4 py-2"
            >Set the endpoint to http://localhost:3000/request-auth</span
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
  "id": "user123",
  "platform": "web",
  "auth": ["note:add", "note:delete", "label:add"]
}       </pre
        >
        <div class="text-white flex items-start">
          <span class="bg-amber-500 rounded py-2 px-3 text-white font-bold"
            >3</span
          >
          <span class="ml-4 py-2"
            >Check Beaver Notes for a permission request</span
          >
        </div>
      </div>
      <span class="py-2 bg-amber-400 text-white bg-opacity-80 rounded">
        Make sure to save your token somewhere safe and encrypted in production.
      </span>
      <code-editor v-model="requestBody"></code-editor>
      <input
        class="w-full p-2 rounded bg-neutral-700 text-white outline-none focus:ring focus:ring-amber-400 focus:ring-opacity-80"
        v-model="endpoint"
        placeholder="Endpoint URL"
      />
      <button @click="sendRequest" class="p-2 bg-amber-500 text-white rounded">
        Send Request
      </button>
    </div>
    <pre class=" text-white p-2 rounded">{{ response }}</pre>
  </div>
</template>

<script>
import axios from "axios";
import { BeakerIcon, Cog6ToothIcon } from "@heroicons/vue/24/solid";
import CodeEditor from "./CodeEditor.vue";

export default {
  components: { CodeEditor, BeakerIcon, Cog6ToothIcon },
  data() {
    return {
      requestBody: `{
    "id": "Your ID",
    "platform": "Platform",
    "auth": ["note:add", "note:delete", "label:add"]
  }`,
      endpoint: "http://localhost:3000/request-auth",
      response: "",
    };
  },
  methods: {
    async sendRequest() {
      try {
        const requestData = JSON.parse(this.requestBody);
        const res = await axios.post(this.endpoint, requestData, {
          headers: {
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
