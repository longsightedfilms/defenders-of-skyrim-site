<template>
  <div>
    <page-header
      :title="currentMap.title"
    />
    <div class="container-fluid">
      <div class="row">
        <div class="col">
          <div style="height: 600px">
            <client-only>
              <l-map
                :zoom="mapConfig.zoom"
                :min-zoom="mapConfig.minZoom"
                :max-zoom="mapConfig.maxZoom"
              >
                <l-tile-layer
                  :url="mapConfig.url"
                  :tile-size="728"
                />
                <l-marker
                  v-for="marker in currentMap.meta.markers"
                  :key="marker.name"
                  :lat-lng="marker"
                >
                  <l-popup :content="marker.name" />
                </l-marker>
              </l-map>
            </client-only>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'nuxt-property-decorator';

@Component({
  head() {
    return {
      title: (this as SingleLocationMap).currentMap.title,
      meta: [
        {
          property: 'og:title',
          content: `${(this as SingleLocationMap).currentMap.title} - Defenders of Skyrim`,
          hid: 'og:title',
        },
      ],
    };
  },
})
export default class SingleLocationMap extends Vue {
  currentMap = {
    title: '',
    slug: '',
    meta: {
      bounds: [[-512, -512], [512, 512]],
      markers: [],
    },
  }

  mapConfig = {
    url: `https://maps-static.defendersofskyrim.com/${this.currentMap.slug}/{z}/{y}/{x}.jpg`,
    zoom: 1,
    maxZoom: 6,
    minZoom: 1,
  }

  async asyncData({ app, params }: { app: any, params: any }): Promise<any> {
    const currentMap = await app.$getSingleLocationMap(params.location);
    const mapConfig = {
      url: `https://maps-static.defendersofskyrim.com/${currentMap.slug}/{z}/{y}/{x}.jpg`,
      zoom: 1,
      maxZoom: 6,
      minZoom: 1,
    };

    return {
      currentMap,
      mapConfig,
    };
  }
}
</script>
