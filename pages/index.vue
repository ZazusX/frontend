<template>
  <ul class="list-group">
    <li v-for="(content, index) in pages" :key="index" class="list-group-item">
      <h1>{{ content.attributes.title }}</h1>
      {{ content.attributes.description }}
      <hr />
      {{ content.attributes.tags }}
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
interface Content {
  id: number;
  attributes: {
    title: Text;
    description: Text;
    tags: Text;
  };
}
interface Contents {
  data: Content[];
}
export default defineComponent({
  name: "PageContent",
  data() {
    return {
      pages: [] as Content[],
      fetchingContents: false,
    };
  },
  methods: {
    async fetchContents() {
      const contentsResponse = await axios.get<Contents>(
        "http://localhost:3033/api/contents"
      );
      this.pages = contentsResponse.data.data;
    },
  },
  async mounted() {
    await this.fetchContents();
  },
});
</script>
