<template>
  <div class="page">
    <header>
      <a class="logo" href="#"></a>
      <button class="toggle">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          height="48"
          viewBox="0 96 960 960"
          width="48"
        >
          <path
            fill="currentColor"
            d="M120 816v-60h720v60H120Zm0-210v-60h720v60H120Zm0-210v-60h720v60H120Z"
          />
        </svg>
      </button>
    </header>
    <Navigation :data="pages" @navigate="navigate($event)" />
    <div v-for="(page, index) in pages" :key="index">
      <Contents
        v-if="page.attributes.content_pages.data.length > 0"
        :contents="page.attributes.content_pages.data"
        :activePage="active"
      />
    </div>
    <footer>
      <a href="#">IMPRESSUM</a> &nbsp; . &nbsp; <a href="#">AGB</a> &nbsp; .
      &nbsp; <a href="#">DATENSCHUTZERKLÄRUNG</a> &nbsp; . &nbsp;
      <a href="#">ENGLISCH</a> &nbsp; . &nbsp; <a href="#">SUCHE</a>
    </footer>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import Navigation from "~/components/Nav.vue";
import Contents from "~/components/Contents.vue";

interface Category {
  id: Number;
  attributes: {
    Title: Text;
  };
}
interface Profile {
  id: Number;
  attributes: {
    Name: Text;
    Profession: Text;
    CompanyMosition: Text;
    Phone: Text;
    Email: Text;
    Position: Text;
  };
}
interface Page {
  id: Number;
  attributes: {
    title: Text;
    content_pages: {
      data: Array<any>;
    };
    sub_pages: {
      data: Category[];
    };
    profile_pages: {
      data: Profile[];
    };
  };
}
interface Pages {
  data: Array<Page>;
}
export default defineComponent({
  name: "PageContent",
  data() {
    return {
      pages: [] as Page[],
      active: "",
    };
  },
  methods: {
    async fetchContents() {
      const contentsResponse = await axios.get<Pages>(
        "http://localhost:3033/api/root-pages?populate=*"
      );
      this.pages = contentsResponse.data.data;
    },
    navigate(page: string) {
      this.active = page;
    },
  },
  async mounted() {
    await this.fetchContents();
  },
  components: {
    Navigation,
    Contents,
  },
});
</script>
