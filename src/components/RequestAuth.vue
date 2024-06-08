<template>
  <div class="bg-black-800 py-10">
    <h2 class="text-2xl py-2 text-white font-bold">Request Authentication</h2>
    <div class="flex flex-col space-y-4 mx-8 sm:mx-72">
      <div class="flex flex-col space-y-6">
        <span class="text-white">
          <span class="bg-amber-500 rounded py-2 px-3 text-white font-bold"
            >1</span
          >
          Set the endpoint to http://localhost:3000/request-auth
        </span>
        <span class="text-white">
          <span class="bg-amber-500 rounded py-2 px-3 text-white font-bold"
            >2</span
          >
          Insert the json request data into the codeblock
        </span>
        <pre>
            {
  "id": "user123",
  "platform": "web",
  "auth": ["note:add", "note:delete", "label:add"]
}
        </pre>
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
      <button @click="sendRequest" class="p-2 bg-amber-500 text-white rounded">
        Send Request
      </button>
    </div>

    <pre>{{ response }}</pre>
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
