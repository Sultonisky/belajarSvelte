<script>
  // export let message = "Hello, This is default value of modal";
  import { createEventDispatcher } from "svelte";
  export let openModal = false;
  const dispatch = createEventDispatcher();

  function closeModal() {
    dispatch("close");
  }
</script>

{#if openModal}
  <div class="backdrop" on:click={closeModal}>
    <div class="modal" on:click|stopPropagation>
      <button class="closeBtn" on:click={closeModal} aria-label="Tutup">
        <span class="iconify" data-icon="mdi:close" style="font-size: 1.5rem;"></span>
      </button>
      <slot />
    </div>
  </div>
{/if}

<style>
  .backdrop {
    width: 100vw;
    height: 100vh;
    position: fixed;
    background: rgba(30, 41, 59, 0.25);
    z-index: 100;
    top: 0;
    left: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    animation: fadeIn 0.2s;
    cursor: pointer;
  }
  .modal {
    position: relative;
    padding: 2rem 1.5rem 1.5rem 1.5rem;
    border-radius: 18px;
    max-width: 420px;
    width: 95vw;
    margin: 0 auto;
    text-align: center;
    background: #fff;
    box-shadow: 0 8px 32px 0 rgba(30, 41, 59, 0.18);
    animation: popIn 0.22s cubic-bezier(.4,2,.6,1) both;
    cursor: auto;
  }
  .closeBtn {
    position: absolute;
    top: 1rem;
    right: 1rem;
    background: transparent;
    border: none;
    cursor: pointer;
    padding: 0.2rem;
    border-radius: 50%;
    transition: background 0.15s;
    z-index: 2;
  }
  .closeBtn:hover {
    background: #f3f4f6;
  }
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }
  @keyframes popIn {
    0% { transform: scale(0.95); opacity: 0; }
    100% { transform: scale(1); opacity: 1; }
  }
</style>
