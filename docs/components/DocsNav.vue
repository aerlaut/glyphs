<template lang="pug">
  aside.flex.flex-col.px-4.sticky.self-start.py-4(style='top: 72px')
    template(v-for='(docs, category, index) in categories')
      h3.text-grey-900.uppercase.text-xs.tracking-wider.mt-4.mb-3 {{ category }}
      div.flex.flex-col.w-full(style='min-width: 16rem')
        template(v-for='doc in docs')
          nuxt-link.px-2.py-1.rounded-lg.capitalize(
            :to='doc.disabled ? $route.path : doc.path',
            @click.native='$store.commit("menu/toggle", false)',
            :class=`{
              'opacity-50 pointer-events-none': doc.disabled,
              'text-gray-800 dark_text-gray-300 bg-gray-300 dark_bg-gray-700': $route.path === doc.path,
              'text-gray-600 hover_text-gray-800 focus_text-gray-800': !doc.disabled && $route.path !== doc.path && $colorMode.value === 'light',
              'text-gray-500 hover_text-gray-300 focus_text-gray-300': !doc.disabled && $route.path !== doc.path && $colorMode.value === 'dark',
            }`
          ) {{ doc.title }}
            template(v-if='doc.disabled')
              span.text-xs.ml-2.text-gray-600.dark_text-gray-500 (Coming Soon)
</template>

<script>
export default {
  computed: {
    categories () {
      return this.$store.state.docs.categories
    }
  }
}
</script>
