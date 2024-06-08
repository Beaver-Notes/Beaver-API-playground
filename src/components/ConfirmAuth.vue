<template>
  <div class="bg-black-800 py-10">
    <h2 class="text-2xl py-2 text-white font-bold">Confirm Authentication</h2>
    <div class="flex flex-col space-y-4 mx-8 sm:mx-72">
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
      <button class="p-2 bg-amber-500 text-white rounded" @click="sendRequest">Send Request</button>
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
    "id": "Your ID",
    "platform": "Platform"
  }`,
      endpoint: "http://localhost:3000/confirm-auth",
      token: "",
      response: "",
    };
  },
  methods: {
    async sendRequest() {
      try {
        const requestData = JSON.parse(this.requestBody);
        const res = await axios.get(this.endpoint, {
          headers: {
            Authorization: `Bearer ${this.token}`,
            "Content-Type": "application/json",
          },
          data: requestData,
        });
        this.response = JSON.stringify(res.data, null, 2);
      } catch (error) {
        this.response = `Error: ${error.message}`;
      }
    },
  },
};
</script>
