<template>
  <div class="notifications">
    <component
      v-for="item in items" :key="item.id"
      :is="item.component"
      v-bind="item.props"
      @close="removeItem(item.id)"
    />
  </div>
</template>

<script>
import Notification from './Notification'

export default {
  data () {
    return {
      id: 1,
      items: [],
    }
  },

  methods: {
    success (text, duration) {
      this.addItem({
        duration,
        component: Notification,
        props: { text, icon: 'success', color: 'success' }
      })
    },

    error (text, duration) {
      this.addItem({
        duration,
        component: Notification,
        props: { text, icon: 'error', color: 'danger' }
      })
    },

    addItem (context) {
      const id = this.id++
      const { items } = this

      items.push({
        id,
        props: context.props,
        component: context.component
      })

      setTimeout(() => {
        this.removeItem(id)
      }, context.duration || 5000)
    },

    removeItem (id) {
      this.items.splice(this.items.findIndex(item => item.id === id), 1)
    }
  }
}
</script>
