<template>
  <b-card
    class="mb-4 equal"
    bg-variant="skyrim"
    text-variant="white"
    no-body
  >
    <div class="aspect-ratio_16-9">
      <b-card-img-lazy
        v-if="mod.thumbnail !== ''"
        :src="$getAbsoluteImageURL(mod.thumbnail.path)"
        top
      />
      <b-img
        v-else
        class="card-img img-fluid"
        blank-color="#333333"
        blank
      />
    </div>

    <b-card-body>
      <b-card-title>{{ mod.title }}</b-card-title>
      <b-button
        :href="mod.link"
        :disabled="mod.link === ''"
        variant="skyrim"
        block
      >
        {{ $t('buttons.download', { version: mod.version }) }}
      </b-button>
    </b-card-body>
  </b-card>
</template>

<script lang="ts">
import { Vue, Component } from 'nuxt-property-decorator';

@Component({
  props: {
    mod: {
      type: Object,
      required: true,
    },
  },
})
export default class CardMod extends Vue {}
</script>
