<template>
  <div class="btn-group">
    <slot></slot>
    <slot name="dropdown-menu"></slot>
  </div>
</template>
<script>
  import EventListener from './utils/EventListener'
  export default {
    methods: {
      toggleDropdown(e) {
        e.preventDefault()
        this.$el.classList.toggle('open')
      }
    },
    ready() {
      const el = this.$el
      const toggle = el.querySelector('[data-toggle="dropdown"]')
      if (toggle)
      {
        toggle.style.borderRadius = '4px'
        toggle.addEventListener('click', this.toggleDropdown)
      }
      this._closeEvent = EventListener.listen(window, 'click', (e)=> {
        if (!el.contains(e.target) || e.target.nodeName.toLowerCase() == 'a') el.classList.remove('open')
      })
      const targets = document.querySelectorAll('.btn-dropdown .dropdown-menu a')
      for( let i = 0; i < targets.length; i++) {
        targets[i].addEventListener('click', e => {
          el.classList.remove('open')
        })
      }
    },
    beforeDestroy() {
      if (this._closeEvent) this._closeEvent.remove()
    }
  }
</script>
