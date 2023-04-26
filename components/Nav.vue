<template>
  <nav>
    <ul class="list-group">
      <li v-for="(page, index) in pages" :key="page.id">
        <a
          :href="convert(page.attributes.Title)"
          :class="{ active: index == 0 }"
        >
          {{ page.attributes.Title }}
        </a>
        <ul v-if="page.attributes.content_pages.data.length">
          <li
            v-for="(subpage, index) in page.attributes.content_pages.data"
            :key="index"
            class=""
          >
            <a :href="convert(subpage.attributes.Title)">
              {{ subpage.attributes.Title }}
            </a>
          </li>
        </ul>
        <ul v-if="page.attributes.profile_pages.data.length">
          <li
            v-for="(profilepage, index) in page.attributes.profile_pages.data"
            :key="index"
            class="list-group-item"
          >
            <a :href="convert(profilepage.attributes.Name)">{{
              profilepage.attributes.Name
            }}</a>
          </li>
        </ul>
      </li>
    </ul>
  </nav>
</template>
<script lang="ts">
import { defineComponent } from "vue";

interface Page {
  id: Number;
  attributes: {
    Title: Text;
    content_pages?: {
      data: any;
    };
    sub_pages?: {
      data: any;
    };
    profile_pages?: {
      data: any;
    };
  };
}
export default defineComponent({
  name: "Nav",
  props: {
    pages: null,
  },
  data() {
    return {};
  },
  methods: {
    convert(str: string) {
      str = str.replace(/^\s+|\s+$/g, "");
      str = str.toLowerCase();
      str = str
        .replace(/[^a-z0-9 -]/g, "")
        .replace(/\s+/g, "-")
        .replace(/-+/g, "-");
      return str;
    },
  },
});
</script>
