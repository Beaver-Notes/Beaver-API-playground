<template>
    <div>
      <code-editor v-model="requestBody"></code-editor>
      <input v-model="endpoint" placeholder="Endpoint URL" />
      <input v-model="token" placeholder="Bearer Token" />
      <button @click="sendRequest">Send Request</button>
      <pre>{{ response }}</pre>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import CodeEditor from './CodeEditor.vue';
  
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
        endpoint: 'http://localhost:3000/add-note',
        token: '',
        response: '',
      };
    },
    methods: {
      async sendRequest() {
        try {
          const requestData = JSON.parse(this.requestBody);
          const res = await axios.post(this.endpoint, requestData, {
            headers: {
              Authorization: `Bearer ${this.token}`,
              'Content-Type': 'application/json',
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
  