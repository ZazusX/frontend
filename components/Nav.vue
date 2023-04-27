<template>
  <nav>
    <ul>
      <li v-for="(page, index) in data" :key="page.id">
        <a
          :href="convert(page.attributes.Title)"
          :title="page.attributes.Title"
          :class="{ active: index == 0 }"
        >
          {{ page.attributes.Title }}
        </a>
        <ul v-if="page.attributes.content_pages.data.length > 0">
          <li
            v-for="(subpage, index) in page.attributes.content_pages.data"
            :key="index"
          >
            <a :href="subpage.attributes.Title">
              {{ subpage.attributes.Title }}
            </a>
          </li>
        </ul>
        <ul v-if="page.attributes.profile_pages.data.length > 0">
          <li
            v-for="(profilepage, index) in page.attributes.profile_pages.data"
            :key="index"
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
  id: string;
  attributes: {
    Title: string;
    content_pages: {
      data: Array<Content>;
    };
    sub_pages: {
      data: any;
    };
    profile_pages: {
      data: Array<Profile>;
    };
  };
}

interface Content {
  id: string;
  attributes: {
    Title: string;
  };
}

interface Profile {
  id: string;
  attributes: {
    Name: string;
  };
}

interface Pages extends Array<Page> {}

export default defineComponent({
  name: "Nav",
  props: {
    data: {
      type: [] as Page[],
    },
  },
  data() {
    return {};
  },
  methods: {
    convert(str: string) {
      if (str !== undefined) {
        str = str.replace(/^\s+|\s+$/g, "");
        str = str.toLowerCase();
        str = str
          .replace(/[^a-z0-9 -]/g, "")
          .replace(/\s+/g, "-")
          .replace(/-+/g, "-");
        return str;
      }
    },
  },
});
</script>
