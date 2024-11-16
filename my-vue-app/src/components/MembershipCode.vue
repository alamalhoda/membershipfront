<template>
  <v-app>
    <v-main>
      <v-container>
        <v-form @submit.prevent="sendCode">
          <v-text-field
              label="Enter Code"
              v-model="code"
              required
          ></v-text-field>
          <v-btn type="submit" color="primary">Send</v-btn>
        </v-form>
        <v-alert v-if="responseMessage" :type="responseType">
          {{ responseMessage }}
        </v-alert>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';
import { ref } from 'vue';

export default {
  name: 'CodeSender',
  setup() {
    const code = ref('');
    const responseMessage = ref('');
    const responseType = ref('info');


    const sendCode = async () => {
      try {
        const response = await axios.post('http://localhost:8000/membership/check-code/', { code: code.value });
        const { is_active } = response.data;
        responseMessage.value = is_active[1];
        responseType.value = is_active[0] ? 'success' : 'error';
        console.log(response.data);
      } catch (error) {
        responseMessage.value = 'Error: ' + (error.response ? error.response.data.message : error.message);
      }
    };

    return {
      code,
      responseMessage,
      responseType,
      sendCode
    };
  }
};
</script>

<style scoped>
/* استایل‌های دلخواه خود را اینجا اضافه کنید */
</style>