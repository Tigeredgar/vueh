<template>
  <div>
    <div class="mb-5">
      <h1>External API</h1>
      <p>
        Call an external API by clicking the button below. This will call the
        external API using an access token, and the API will validate it using
        the API's audience value.
      </p>

      <button class="btn btn-primary mt-5" @click="callApi">Call API</button>
    </div>

    <div class="result-block-container">
      <div :class="['result-block', apiMessage ? 'show' : '']">
        <h6 class="muted">Result</h6>
        <pre v-highlightjs><code class="json">{{JSON.stringify(apiMessage, null, 2)}}</code></pre>
      </div>
    </div>
  </div>
</template>

<script>
import { useExternal } from "./../services/getExternal";
import { ref } from "@vue/reactivity";
export default {
  name: "Api",
  setup() {
    const { get } = useExternal();
    const apiMessage = ref();
    return {
      apiMessage,
      async callApi() {
        try {
          const data = await get();
          apiMessage.value = data;
        } catch (e) {
          apiMessage.value = `Error: the server responded with '${e.response.status}: ${e.response.statusText}'`;
        }
      },
    };
  },
};
</script>