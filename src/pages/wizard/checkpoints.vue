<template>
  <div class="wizard-project">
    <PageHeader>
      <h2 class="title">
        <span class="icon"><font-awesome-icon icon="sync-alt" /></span>
        <span>Updater</span>
      </h2>

      <h3 class="subtitle">
        {{ $checkpoints.name }}
      </h3>

      <template #right>
        <button class="button" @click="$dreampower.openAppFolder()">
          <span class="icon"><font-awesome-icon icon="folder-open" /></span>
          <span>{{ $dreampower.name }} Folder</span>
        </button>
      </template>
    </PageHeader>

    <div class="project__content">
      <div v-if="!requirements.power.checkpoints" class="notification notification--warning">
        <span class="icon"><font-awesome-icon icon="info-circle" /></span>
        <span>It is necessary to download and install this component to continue.</span>
      </div>

      <div v-else class="notification">
        Installed version: <strong>{{ $checkpoints.version }}</strong>
      </div>

      <div v-if="updater.error" class="notification notification--danger">
        <h5>CONNECTION ERROR!</h5>
        <span>A problem has occurred when trying to get the information from Github, please make sure you have a stable internet connection and restart the application.</span>
        <br><br>

        <pre>
<span v-if="updater.errorResponse">{{ updater.errorResponse }}</span>
{{ updater.error.stack }}
</pre>
      </div>

      <!-- Updater -->
      <AppBox>
        <ProjectUpdate project="checkpoints" />
      </AppBox>

      <!-- Settings -->
      <AppBox title="Settings.">
        <!-- Download protocol. -->
        <MenuItem
          v-if="updater.hasTorrentUrls || updater.hasIPFSUrls"
          label="Download protocol."
          tooltip="- **Any:** Use all protocols if necessary.

- **HTTP:** Fastest and most reliable for most connections.

- **Torrent & IPFS:** Download the file from other computers with the option to cancel at any time and resume later. More reliable for unstable and low speed connections. May require a few minutes of preparation before starting the download.">
          <template #description>
            <span class="item__description">Select the protocol that will be used to download the file.</span>
          </template>

          <select v-model="updater.downloadMethod" class="input">
            <option :value="0">
              Any
            </option>

            <option :value="1">
              HTTP
            </option>

            <option v-if="updater.hasTorrentUrls" :value="3">
              Torrent
            </option>

            <option v-if="updater.hasIPFSUrls" :value="2">
              IPFS
            </option>
          </select>
        </MenuItem>
      </AppBox>

      <hr>

      <PageHeader>
        <h2 class="title">
          <span class="icon"><font-awesome-icon icon="book" /></span>
          <span>Changelog</span>
        </h2>
      </PageHeader>

      <!-- Changelog -->
      <ProjectChangelog project="checkpoints" :limit="1" />
    </div>
  </div>
</template>

<script>
import { requirements } from '~/modules/system'
import { checkpoints } from '~/modules/projects'

export default {
  layout: 'wizard',

  middleware({ redirect, route }) {
    if (!route.query.forced) {
      if (requirements.power.checkpoints) {
        redirect('/wizard/waifu')
      }
    }
  },

  data: () => ({
    requirements,
  }),

  computed: {
    updater() {
      return checkpoints.updater
    },
  },
}
</script>

<style lang="scss" scoped>
</style>
