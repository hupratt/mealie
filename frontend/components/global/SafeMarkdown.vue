<template>
  <VueMarkdown :source="sanitizeMarkdown(source)"></VueMarkdown>
</template>

<script lang="ts">
// @ts-ignore vue-markdown has no types
import VueMarkdown from "@adapttive/vue-markdown";
import { defineComponent } from "@nuxtjs/composition-api";
import DOMPurify from "isomorphic-dompurify";

export default defineComponent({
  components: {
    VueMarkdown,
  },
  props: {
    source: {
      type: String,
      default: "",
    },
  },
  setup() {
    function sanitizeMarkdown(rawHtml: string | null | undefined): string {
      if (!rawHtml) {
        return "";
      }

      const sanitized = DOMPurify.sanitize(rawHtml, {
        // TODO: some more thought could be put into what is allowed and what isn't
        ALLOWED_TAGS: ["img", "div", "p", "iframe"],
        ADD_ATTR: ["src", "alt", "height", "width", "class", "allow", "title", "allowfullscreen", "frameborder", "scrolling"],
      });

      return sanitized;
    }

    return {
      sanitizeMarkdown,
    };
  },
});
</script>
