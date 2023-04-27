<template>
  <div>
    <article v-for="(content, index) in contents"
      :key="content.attributes.Title" :ref="content.attributes.Title"
      :data-title="content.attributes.Title" v-if="activePage === content.attributes.Title">
      <main>
        <h1>{{ content.attributes.Title }} </h1>
        <div v-html="formatRte(content.attributes.Description)"></div>
      </main>
      <img :src="content.attributes.desktopImage?.data.attributes.url" alt="" class="background" />
      <aside></aside>
    </article>
  </div>
</template>
  <script lang="ts">
    import { defineComponent } from "vue";
    import { marked } from "marked";
    marked.use({
      gfm: true,
      breaks: true,
    });
  
    interface Content {
      id: string;
      attributes: {
        Title: string;
        Description: string;
        Tags: Text;
        mobileImage?: {
          data: any;
        };
        desktopImage?: {
          data: any;
        };
      };
    }
    interface Contents extends Array<Content> {}

    export default defineComponent({
      name: "Contents",
      props: {
        contents: {
          type: [] as Content[],
        },
        activePage: {
          type: String
        }
      },
      data() {
        return {
         
        };
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
        formatRte(str: string) {
          if(str !== undefined) {
            return marked(str);
          }
        }
      },      
    });
  </script>
</template>
