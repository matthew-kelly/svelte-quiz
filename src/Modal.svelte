<script>
  import { fly, fade } from "svelte/transition";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
</script>

<div class="modal" transition:fade>
  <div class="modal-content" transition:fly={{ y: -100 }}>
    <button on:click={() => dispatch("close")}>Close</button>
    <slot />
  </div>
</div>

<style lang="scss">
  .modal {
    position: fixed;
    z-index: 10;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.8);

    .modal-content {
      background: white;
      padding: 20px;
      margin: 5rem;
      border-radius: 15px;
    }
  }
</style>
