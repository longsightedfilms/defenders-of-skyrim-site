<template>
  <div>
    <page-header
      :title="$t('pages.download')"
      :image="require('@/assets/images/backgrounds/download_mod.jpg')"
    />
    <div class="container">
      <div class="row">
        <div
          v-for="mod in mods"
          :key="mod._id"
          class="col-md-12 col-lg-8 col-hg-6"
        >
          <card-mod :mod="mod" />
        </div>
        <div class="col-24">
          <h2>{{ $t('changelog') }}</h2>
          <div
            v-for="(logsArray, title, index) in logs"
            :key="`${title}-${index}`"
            class="my-4"
          >
            <h3 class="heading-secondary heading-greek">
              <span>{{ $t(`mods.${title}`) }}</span>
            </h3>
            <card-changelog
              v-for="log in logsArray"
              :key="log._id"
              :version="log.version"
              :description="log.description"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'nuxt-property-decorator';
import CardMod from '@/components/Cards/CardMod.vue';
import CardChangelog from '@/components/Cards/CardChangelog.vue';
import type {
  IMod,
} from '@/types/types';

@Component({
  components: {
    CardMod,
    CardChangelog,
  },
  head() {
    return {
      title: this.$t('pages.download').toString(),
      meta: [
        {
          property: 'og:title',
          content: `${this.$t('pages.download')} - Defenders of Skyrim`,
          hid: 'og:title',
        },
      ],
    };
  },
})
export default class Download extends Vue {
  logs: any = {}

  mods: IMod[] = []

  async asyncData({ app }: { app: any }): Promise<any> {
    const data = await app.$getDownloads();
    return {
      logs: data.logs,
      mods: data.mods,
    };
  }
}
</script>
