<script>
import Prism from 'prismjs'

export default {
  functional: true,
  props: {
    code: {
      type: String,
      default: undefined
    },
    inline: {
      type: Boolean,
      default: false
    },
    lang: {
      type: String,
      default: 'js'
    }
  },
  render (h, ctx) {
    const code =
      ctx.props.code ||
      (ctx.children && ctx.children.length > 0 ? ctx.children[0].text : '')
    const inline = ctx.props.inline
    const language = ctx.props.lang
    const prismLanguage = Prism.languages[language]
    const className = `language-${language}`

    if (process.env.NODE_ENV === 'development' && !prismLanguage) {
      throw new Error(
        `Prism component for language "${language}" was not found, did you forget to register it? See all available ones: https://cdn.jsdelivr.net/npm/prismjs/components/`
      )
    }

    if (inline) {
      return h(
        'code',
        assign({}, ctx.data, {
          class: [ctx.data.class, className],
          domProps: assign({}, ctx.data.domProps, {
            innerHTML: Prism.highlight(code, prismLanguage)
          })
        })
      )
    }

    return h(
      'pre',
      assign({}, ctx.data, {
        class: [ctx.data.class, className]
      }),
      [
        h('code', {
          class: className,
          domProps: {
            innerHTML: Prism.highlight(code, prismLanguage)
          }
        })
      ]
    )
  }
}

function assign (obj) {
  for (let i = 1; i < arguments.length; i++) {
    // eslint-disable-next-line guard-for-in, prefer-rest-params
    for (const p in arguments[i]) {
      obj[p] = arguments[i][p]
    }
  }
  return obj
}
</script>
