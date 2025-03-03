<template lang="pug">
aside.global-site-nav(
  :class='{ "is-hide": !sidenav.isShow }',
  :data-is-show='sidenav.isShow'
)
  .backdrop(@click='sidenav.toggle(false)')
  .inner
    .list
      .group
        .title Navigation
        ul
          li
            router-link(to='/')
              icon
                Home
              | Home
          li
            router-link(to='/categories')
              icon
                Folder
              | Categories Index

      .group
        .title User
        ul
          li(v-if='user.profile')
            router-link(to='/profile')
              icon
                icon-user
              | {{ user.profile.name }} (you)
          li(v-if='user.profile')
            router-link(to='/favourite')
              icon
                Bookmark
              | My Favourite
          li
            router-link(to='/auth')
              icon
                Fingerprint
              | Authorization

      .group
        .title {{ PROJECT_NAME }}
        ul
          li
            router-link(to='/about')
              icon
                Heart
              | About us
</template>

<script setup lang="ts">
import { computed, onMounted, watch } from 'vue'
import { PROJECT_NAME } from '../config'
import {
  Home,
  Heart,
  User as IconUser,
  Fingerprint,
  Bookmark,
  Folder,
} from '@vicons/fa'
import { useRouter } from 'vue-router'
import { useUserStore } from '@/stores/user'
import { useSidenavStore } from '@/stores/sidenav'

const router = useRouter()
const user = useUserStore()
const sidenav = useSidenavStore()

router.afterEach(() => {
  sidenav.toggle(false)
})

onMounted(() => {
  document.addEventListener('keydown', ({ key }) => {
    if (key === 'Escape') {
      sidenav.toggle(false)
    }
  })
})

watch(
  computed(() => sidenav.isShow),
  (val) => {
    if (val) {
      document.body.classList.add('global-sidenav-is-show', 'lock-scroll')
    } else {
      document.body.classList.remove('global-sidenav-is-show', 'lock-scroll')
    }
  }
)
</script>

<style scoped lang="sass">
.global-side-nav
  z-index: 15

.backdrop
  position: fixed
  top: 0
  left: 0
  width: 100vw
  height: 100vh
  background-color: rgba(0, 0, 0, 0.1)
  z-index: 15

.inner
  position: fixed
  top: 0
  left: 0
  padding-top: 50px
  width: calc(1rem + 240px)
  max-width: 80vw
  height: 100vh
  background-color: #fff
  z-index: 16
  transition: all 0.5s

.list
  max-height: calc(100vh - 56px)
  overflow-x: auto

.group
  margin: 0.5rem 0

  .title
    user-select: none
    padding: 0 1.6rem
    margin: 1.6rem 0 0.4rem 0
    font-weight: 600
    font-size: 0.8rem
    color: #aaa

  ul
    margin: 0
    list-style: none
    padding-left: 0

    .xicon
      margin-right: 0.4rem

    li
      a
        padding: 0.8rem 1.6rem
        display: block
        color: #888
        &:hover
          background-color: rgba(0, 0, 0, 0.05)
        &:after
          display: none !important

        &.not-allowed
          cursor: not-allowed
          text-decoration: line-through

// Hidden state
.is-hide
  .inner
    left: -300px
  .backdrop
    display: none
</style>
