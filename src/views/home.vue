<style>
  .gist-item {
    padding: 10px 0;
    border-radius: 2px;
    color: #999;
    display: block;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    & sup {
      font-size: 12px;
    }
    & i {
      margin-right: 5px;
    }
    &:hover {
      cursor: pointer;
      color: #333;
    }
    &.active {
      color: rgba(234, 97, 0, 1);
    }
  }
</style>

<template>
  <section class="view-home">
    <div class="container">
      <div class="wrap">
        <div class="sidebar">
          <div class="sidebar-heading">
            <h2>
              {{ login ? 'Mine' : 'Public' }}
            </h2>
          </div>
          <div class="gist-list" v-if="gists.length > 0">
            <a v-link="{path: '/gist/' + gist.id}" class="gist-item" v-for="gist in gists">
              <i class="fa fa-file-o" v-if="gist.public"></i>
              <i class="octicon octicon-gist-secret" v-else></i>
              {{ gist.files | firstFileName }}
              <sup v-if="getFilesCount(gist.files) > 1">
                {{ getFilesCount(gist.files) }}
              </sup>
            </a>
          </div>
          <gist-pagination></gist-pagination>
          <div class="loading-state" v-if="loadingGists && gists.length === 0">
            <i class="fa fa-spinner fa-pulse fa-fw"></i>
          </div>
        </div>
        <div class="main">
          What should I put here?
        </div>
      </div>
    </div>
  </section>
</template>

<script>
  import {fetchUserGists} from 'actions'
  import gistPagination from 'components/gist-pagination'

  export default {
    vuex: {
      getters: {
        login: state => state.app.accessToken,
        loadingGists: state => state.user.loadingGists,
        gists: state => state.user.gists
      },
      actions: {
        fetchUserGists
      }
    },
    ready() {
      document.title = 'Vist'
      if (this.gists.length === 0) {
        this.fetchUserGists(1)
      }
    },
    methods: {
      getFilesCount(files) {
        return Object.keys(files).length
      }
    },
    filters: {
      firstFileName(files) {
        return Object.keys(files)[0]
      }
    },
    components: {
      gistPagination
    }
  }
</script>
