<script lang="ts">
    import Header from "./header/Header.svelte";
    import {fly} from "svelte/transition";
</script>

<div class="bg-image"></div>
<div class="bg-overlay"></div>

<div class="menu">
    <div in:fly|global={{duration: 180, y: -20}}>
        <Header/>
    </div>

    <div class="menu-wrapper">
        <slot/>
    </div>
</div>

<style lang="scss">
  .bg-image {
      position: fixed;
      inset: 0;
      background: url('backgrounds/background.png') center / cover no-repeat;
      transform: scale(1.04);
      z-index: -2;
  }

  .bg-overlay {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.55);
      backdrop-filter: blur(6px);
      z-index: -1;
  }

  .menu {
    padding: 40px;
    display: flex;
    flex-direction: column;
    // Not sized in vh, which not every browser scales with zoom
    position: fixed;
    inset: 0;
  }

  .menu-wrapper {
    flex: 1;
    display: flex;
    flex-direction: column;
    will-change: transform;
  }

  @media screen and (max-width: 1366px) {
    .menu {
      zoom: 0.8;
    }
  }

  @media screen and (max-width: 1200px) {
    .menu {
      zoom: 0.5;
    }
  }

  @media screen and (max-height: 1100px) {
    .menu {
      zoom: 0.8;
    }
  }

  @media screen and (max-height: 700px) {
    .menu {
      zoom: 0.5;
    }
  }

  @media screen and (max-height: 540px) {
    .menu {
      zoom: 0.4;
    }
  }
</style>
