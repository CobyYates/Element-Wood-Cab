<template>
  <div>
    <Hero v-if="page.fields.hero" :hero="page.fields.hero" />
    <v-row>
      <v-col class="text-center">
        <CopyBlock
          v-if="page.fields.copyBlock"
          :copy="page.fields.copyBlock.fields"
          copy-class="my-5"
        >
        </CopyBlock>
        <v-btn
          color="primary"
          large
          class="mx-auto mb-8"
          to="/inspiration-gallery"
        >
          Get Inspired
        </v-btn>
      </v-col>
    </v-row>
    <v-row v-if="page.fields.cabinetSwatch">
      <v-col cols="10" md="8" class="mx-auto">
        <CabinetSwatch
          v-for="swatch in page.fields.cabinetSwatch"
          :key="swatch.i"
          :swatch="swatch"
        />
      </v-col>
    </v-row>
    <v-row v-if="page.fields.galleryItems" class="mt-16">
      <v-col cols="10" md="8" class="mx-auto">
        <v-row>
          <v-col
            cols="12"
            md="6"
            v-for="image in page.fields.galleryItems"
            :key="image.i"
          >
            <GalleryItem :image="image.fields" />
          </v-col>
        </v-row>
      </v-col>
    </v-row>
    <v-row v-if="page.fields.flipbook">
      <v-col>
        <Book :pdf="page.fields.flipbook" />
      </v-col>
    </v-row>
  </div>
</template>

<script>
import { createClient } from "../plugins/contentful";
import Hero from "~/components/Hero.vue";
import CabinetSwatch from "~/components/CabinetSwatch.vue";
import GalleryItem from "~/components/GalleryItem.vue";
import Book from "../components/Book.vue";
import CopyBlock from "~/components/CopyBlock.vue";
const contentfulClient = createClient();

export default {
  name: "index",
  data() {
    return {};
  },
  asyncData({ env, params }) {
    return contentfulClient
      .getEntries({
        content_type: "page",
        "fields.slug": "home",
      })
      .then((page) => {
        return {
          page: page.items[0],
        };
      })
      .catch(console.error);
  },
  components: { Hero, CabinetSwatch, GalleryItem, Book, CopyBlock },
};
</script>

<style lang="scss">
h1,
h2,
h3,
h4,
p,
a,
.v-btn {
  font-family: "Tw Cen MT", sans-serif;
}
h2 {
  font-size: 1.6rem;
  font-weight: 600;
  color: rgb(110, 110, 113);
}
</style>
