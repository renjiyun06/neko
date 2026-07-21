<template>
  <div class="about" @click="toggle">
    <div class="window">
      <div class="brand">
        <strong>中恒科技</strong>
        <span>浏览器协作平台</span>
      </div>
      <p>安全、便捷地共享浏览器操作环境。</p>
    </div>
  </div>
</template>

<style lang="scss" scoped>
  .about {
    position: absolute;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba($color: $background-floating, $alpha: 0.8);

    .window {
      width: min(420px, calc(100vw - 40px));
      padding: 36px;
      border-radius: 12px;
      background: $background-secondary;
      box-shadow: $elevation-high;
    }

    .brand {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 8px;

      strong {
        font-size: 28px;
        letter-spacing: 0.08em;
      }

      span {
        color: $text-muted;
        font-size: 14px;
        letter-spacing: 0.12em;
      }
    }

    p {
      margin: 26px 0 0;
      padding-top: 20px;
      border-top: 1px solid $background-modifier-accent;
      color: $interactive-normal;
      line-height: 1.7;
    }
  }
</style>

<script lang="ts">
  import { Component, Vue } from 'vue-property-decorator'

  @Component({ name: 'neko-about' })
  export default class extends Vue {
    toggle(event: { target?: HTMLElement }) {
      if (event.target && event.target.classList.contains('about')) {
        this.$accessor.client.toggleAbout()
      }
    }
  }
</script>
