<template>
  <nav
    class="navbar is-light is-fixed-top"
    :style="zindex"
    role="navigation"
    aria-label="main navigation"
  >
    <div class="navbar-brand">
      <navbar-item-link v-if="show_playlists" :to="{ name: 'playlists' }">
        <mdicon class="icon" name="music-box-multiple" size="16" />
      </navbar-item-link>
      <navbar-item-link v-if="show_music" :to="{ name: 'music' }">
        <mdicon class="icon" name="music" size="16" />
      </navbar-item-link>
      <navbar-item-link v-if="show_podcasts" :to="{ name: 'podcasts' }">
        <mdicon class="icon" name="microphone" size="16" />
      </navbar-item-link>
      <navbar-item-link v-if="show_audiobooks" :to="{ name: 'audiobooks' }">
        <mdicon class="icon" name="book-open-variant" size="16" />
      </navbar-item-link>
      <navbar-item-link v-if="show_radio" :to="{ name: 'radio' }">
        <mdicon class="icon" name="radio" size="16" />
      </navbar-item-link>
      <navbar-item-link v-if="show_files" :to="{ name: 'files' }">
        <mdicon class="icon" name="folder-open" size="16" />
      </navbar-item-link>
      <navbar-item-link v-if="show_search" :to="{ name: search_name }">
        <mdicon class="icon" name="magnify" size="16" />
      </navbar-item-link>
      <div
        class="navbar-burger"
        :class="{ 'is-active': show_burger_menu }"
        @click="show_burger_menu = !show_burger_menu"
      >
        <span />
        <span />
        <span />
      </div>
    </div>
    <div class="navbar-menu" :class="{ 'is-active': show_burger_menu }">
      <div class="navbar-start" />
      <div class="navbar-end">
        <!-- Burger menu entries -->
        <div
          class="navbar-item has-dropdown is-hoverable"
          :class="{ 'is-active': show_settings_menu }"
          @click="on_click_outside_settings"
        >
          <a class="navbar-item is-arrowless is-hidden-touch">
            <mdicon class="icon" name="menu" size="24" />
          </a>
          <div class="navbar-dropdown is-right">
            <navbar-item-link :to="{ name: 'playlists' }">
              <mdicon class="icon" name="music-box-multiple" size="16" />
              <b v-text="$t('navigation.playlists')" />
            </navbar-item-link>
            <navbar-item-link :to="{ name: 'music' }" exact>
              <mdicon class="icon" name="music" size="16" />
              <b v-text="$t('navigation.music')" />
            </navbar-item-link>
            <navbar-item-link :to="{ name: 'music-artists' }">
              <span class="pl-5" v-text="$t('navigation.artists')" />
            </navbar-item-link>
            <navbar-item-link :to="{ name: 'music-albums' }">
              <span class="pl-5" v-text="$t('navigation.albums')" />
            </navbar-item-link>
            <navbar-item-link :to="{ name: 'music-genres' }">
              <span class="pl-5" v-text="$t('navigation.genres')" />
            </navbar-item-link>
            <navbar-item-link
              v-if="spotify_enabled"
              :to="{ name: 'music-spotify' }"
            >
              <span class="pl-5" v-text="$t('navigation.spotify')" />
            </navbar-item-link>
            <navbar-item-link :to="{ name: 'podcasts' }">
              <mdicon class="icon" name="microphone" size="16" />
              <b v-text="$t('navigation.podcasts')" />
            </navbar-item-link>
            <navbar-item-link :to="{ name: 'audiobooks' }">
              <mdicon class="icon" name="book-open-variant" size="16" />
              <b v-text="$t('navigation.audiobooks')" />
            </navbar-item-link>
            <navbar-item-link :to="{ name: 'radio' }">
              <mdicon class="icon" name="radio" size="16" />
              <b v-text="$t('navigation.radio')" />
            </navbar-item-link>
            <navbar-item-link :to="{ name: 'files' }">
              <mdicon class="icon" name="folder-open" size="16" />
              <b v-text="$t('navigation.files')" />
            </navbar-item-link>
            <navbar-item-link :to="{ name: search_name }">
              <mdicon class="icon" name="magnify" size="16" />
              <b v-text="$t('navigation.search')" />
            </navbar-item-link>
            <hr class="my-3" />
            <navbar-item-link :to="{ name: 'settings-webinterface' }">{{
              $t('navigation.settings')
            }}</navbar-item-link>
            <a
              class="navbar-item"
              @click.stop.prevent="open_update_dialog()"
              v-text="$t('navigation.update-library')"
            />
            <navbar-item-link :to="{ name: 'about' }">{{
              $t('navigation.about')
            }}</navbar-item-link>
          </div>
        </div>
      </div>
    </div>
    <div
      v-show="show_settings_menu"
      class="is-overlay"
      @click="show_settings_menu = false"
    />
  </nav>
</template>

<script>
import * as types from '@/store/mutation_types'
import NavbarItemLink from '@/components/NavbarItemLink.vue'

export default {
  name: 'NavbarTop',
  components: { NavbarItemLink },

  data() {
    return {
      show_settings_menu: false
    }
  },

  computed: {
    search_name: {
      get() {
        return this.$store.state.search_source
      }
    },
    show_audiobooks() {
      return this.$store.getters.settings_option(
        'webinterface',
        'show_menu_item_audiobooks'
      ).value
    },
    show_burger_menu: {
      get() {
        return this.$store.state.show_burger_menu
      },
      set(value) {
        this.$store.commit(types.SHOW_BURGER_MENU, value)
      }
    },
    show_files() {
      return this.$store.getters.settings_option(
        'webinterface',
        'show_menu_item_files'
      ).value
    },
    show_music() {
      return this.$store.getters.settings_option(
        'webinterface',
        'show_menu_item_music'
      ).value
    },
    show_player_menu() {
      return this.$store.state.show_player_menu
    },
    show_playlists() {
      return this.$store.getters.settings_option(
        'webinterface',
        'show_menu_item_playlists'
      ).value
    },
    show_podcasts() {
      return this.$store.getters.settings_option(
        'webinterface',
        'show_menu_item_podcasts'
      ).value
    },
    show_radio() {
      return this.$store.getters.settings_option(
        'webinterface',
        'show_menu_item_radio'
      ).value
    },
    show_search() {
      return this.$store.getters.settings_option(
        'webinterface',
        'show_menu_item_search'
      ).value
    },
    show_update_dialog: {
      get() {
        return this.$store.state.show_update_dialog
      },
      set(value) {
        this.$store.commit(types.SHOW_UPDATE_DIALOG, value)
      }
    },
    spotify_enabled() {
      return this.$store.state.spotify.webapi_token_valid
    },
    zindex() {
      if (this.show_player_menu) {
        return 'z-index: 21'
      }
      return ''
    }
  },

  watch: {
    $route(to, from) {
      this.show_settings_menu = false
    }
  },

  methods: {
    on_click_outside_settings() {
      this.show_settings_menu = !this.show_settings_menu
    },
    open_update_dialog() {
      this.show_update_dialog = true
      this.show_settings_menu = false
      this.show_burger_menu = false
    }
  }
}
</script>

<style></style>
