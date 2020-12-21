<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <site-title :title="site.title"></site-title>

    </v-app-bar>

    <v-navigation-drawer app v-model="drawer">
      <site-menu :menu="site.menu"></site-menu>
    </v-navigation-drawer>
    <v-main>
      <router-view></router-view>
    </v-main>
    <site-footer :footer="site.footer"></site-footer>
  </v-app>
</template>

<script>
import SiteTitle from '@/views/site/Title'
import SiteFooter from '@/views/site/Footer'
import SiteMenu from '@/views/site/Menu'

export default {
  components: { SiteTitle, SiteFooter, SiteMenu },
  name: 'App',
  data: () => ({
    drawer: false,

    site: {
      menu: [
        {
          icon: 'mdi-home',
          title: 'home',
          subItems: [
            { title: 'Breakfast & brunch', to: '/' },
            { title: 'New American' },
            { title: 'Sushi' }
          ],
          to: '/'
        },
        {
          title: 'Dining',
          icon: 'mdi-silverware-fork-knife',
          active: true,
          subItems: [
            { title: 'Breakfast & brunch' },
            { title: 'New American' },
            { title: 'Sushi' }
          ]
        },
        {
          icon: 'mdi-school',
          subItems: [{ title: 'List Item' }],
          title: 'Education'
        },
        {
          icon: 'mdi-run',
          subItems: [{ title: 'List Item' }],
          title: 'Family'
        },
        {
          icon: 'mdi-bottle-tonic-plus',
          subItems: [{ title: 'List Item' }],
          title: 'Health'
        },
        {
          icon: 'mdi-content-cut',
          subItems: [{ title: 'List Item' }],
          title: 'Office'
        },
        {
          icon: 'mdi-tag',
          subItems: [{ title: 'List Item' }],
          title: 'Promotions'
        }
      ],
      title: '타이틀영역',
      footer: '하단영역'

    }
  }),
  created () {
    this.subscribe()
  },
  mounted () {
    console.log(this.$firebase)
  },
  methods: {
    subscribe () {
      this.$firebase.database().ref().child('site').on('value', (sn) => {
        const v = sn.val()
        console.log(v)
        if (!v) {
          this.$firebase.database().ref().child('site').set(this.site)
          console.log(this.site)
        }
      }, (e) => {
        console.log(e.message)
      })
    }
  }

}
</script>
