<template>
  <ul class="list-group">
    <li v-for="(content, index) in pages" :key="index" class="list-group-item">
      <h1>{{ content.attributes.title }}</h1>
      {{ content.attributes.description }}
      <hr />
      {{ content.attributes.tags }}
      <hr />
      <div class="" v-if="content.attributes.mobileImage">
        {{ content.attributes.mobileImage.data }}
      </div>
      <div class="" v-if="content.attributes.desktopImage">
        {{ content.attributes.desktopImage.data }}
      </div>
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
interface Content {
  id: Number;
  attributes: {
    title: Text;
    description: Text;
    tags: Text;
    mobileImage?: {
      data: any;
    };
    desktopImage?: {
      data: any;
    };
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
        "http://localhost:3033/api/contents?populate=*"
      );
      this.pages = contentsResponse.data.data;
    },
  },
  async mounted() {
    await this.fetchContents();
  },
});
</script>
