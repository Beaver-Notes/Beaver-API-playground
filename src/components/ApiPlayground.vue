<template>
    <div>
      <h1>Beaver Notes API Playground</h1>
      <div>
        <label for="requestType">Select Request Type:</label>
        <select v-model="selectedRequestType" @change="updateRequestBody">
          <option v-for="(body, type) in requestBodies" :key="type" :value="type">{{ type }}</option>
        </select>
      </div>
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
        selectedRequestType: 'POST /request-auth',
        requestBody: '',
        endpoint: '',
        token: '',
        response: '',
        requestBodies: {
          'POST /request-auth': `{
    "id": "Your ID",
    "platform": "Platform",
    "auth": ["note:add", "note:delete", "label:add"]
  }`,
          'GET /confirm-auth': `{
    "id": "Your ID",
    "platform": "Platform"
  }`,
          'POST /add-note': `{
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
          'POST /delete-note': `{
    "id": "note-id"
  }`,
          'POST /add-label': `{
    "id": "note-id",
    "labelId": "label-content"
  }`
        },
      };
    },
    methods: {
      updateRequestBody() {
        this.requestBody = this.requestBodies[this.selectedRequestType];
        const [method, url] = this.selectedRequestType.split(' ');
        this.endpoint = `http://localhost:3000${url}`;
      },
      async sendRequest() {
        try {
          const requestData = JSON.parse(this.requestBody);
          const [method, url] = this.selectedRequestType.split(' ');
          const config = {
            method: method.toLowerCase(),
            url: this.endpoint,
            headers: {
              Authorization: `Bearer ${this.token}`,
              'Content-Type': 'application/json',
            },
            data: method === 'GET' ? null : requestData,
          };
          const res = await axios(config);
          this.response = JSON.stringify(res.data, null, 2);
        } catch (error) {
          this.response = `Error: ${error.message}`;
        }
      },
    },
    mounted() {
      this.updateRequestBody();
    },
  };
  </script>
  