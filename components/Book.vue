<template>
  <div
    :class="$vuetify.breakpoint.mdAndUp ? 'pt-16' : ''"
    class="flipbook__container mx-auto"
  >
    <client-only>
      <flipbook
        :key="pdf.sys && pdf.sys.id"
        class="flipbook"
        v-model="value"
        :pages="pages"
        :click-to-zoom="true"
        v-slot="flipbook"
        v-if="pages.length > 1"
      >
        <div
          :class="`flipbook__icons-${desktop ? 'desktop' : 'mobile'}`"
          class="flipbook__icons"
        >
          <v-btn text @click="flipbook.flipLeft">
            <v-icon :left="desktop || false" :size="desktop ? 20 : 50">
              mdi-chevron-left
            </v-icon>
            <span v-if="desktop">Previous Page</span>
          </v-btn>
          <v-icon @click="flipbook.zoomOut">mdi-magnify-minus-outline</v-icon>
          <v-icon @click="flipbook.zoomIn">mdi-magnify-plus-outline</v-icon>
          <v-btn text @click="flipbook.flipRight">
            <span v-if="desktop">Next Page</span>
            <v-icon :right="desktop || false" :size="desktop ? 20 : 50">
              mdi-chevron-right
            </v-icon>
          </v-btn>
        </div>
      </flipbook>
      <v-card
        v-else
        width="90%"
        height="80vh"
        elevation="6"
        class="flipbook__message d-flex align-center justify-center mx-auto ma-10"
      >
        <h2>Coming Soon</h2>
      </v-card>
    </client-only>
  </div>
</template>

<script>
export default {
  data() {
    return {
      value: 0,
    };
  },
  props: {
    pdf: {
      type: Object,
      default: () => ({}),
    },
  },
  computed: {
    pages() {
      // Defensive: ensure structure exists
      const arr =
        this.pdf && this.pdf.fields && Array.isArray(this.pdf.fields.pdfPage)
          ? this.pdf.fields.pdfPage.map((e, i) => e.fields?.file?.url || null)
          : [];
      arr.unshift(null);
      // console.log("PDF Pages:", arr);
      return arr;
    },
    desktop() {
      return this.$vuetify.breakpoint.mdAndUp;
    },
  },
};
</script>

<style lang="scss" scoped>
.flipbook {
  &__container {
    max-width: 850px;
  }
  height: 85vh !important;
  position: relative;
  &__icons {
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    &-desktop {
      top: -30px;
    }
    &-mobile {
      top: 70px;
    }
  }
  &__message {
  }
}
</style>
