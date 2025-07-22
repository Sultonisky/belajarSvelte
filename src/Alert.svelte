<script>
  import { createEventDispatcher } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  export let message = '';
  export let type = 'success'; // success | info | error
  export let visible = false;

  const dispatch = createEventDispatcher();
  let timeoutId;
  $: if (visible) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      dispatch('close');
    }, 2500);
  }

  const icons = {
    success: 'mdi:check-circle',
    info: 'mdi:information',
    error: 'mdi:alert-circle',
  };
</script>

{#if visible}
  <div class="alert {type}"
    in:fly={{x: 40, duration: 200}}
    out:fade={{duration: 350}}>
    <span class="iconify alert-icon" data-icon={icons[type] || icons.info}></span>
    <span class="alert-message">{message}</span>
  </div>
{/if}

<style>
  .alert {
    position: fixed;
    top: 1.5rem;
    right: 1.5rem;
    min-width: 220px;
    max-width: 320px;
    z-index: 9999;
    display: flex;
    align-items: center;
    gap: 0.7rem;
    padding: 0.9em 1.3em;
    border-radius: 10px;
    font-size: 1.05rem;
    font-weight: 500;
    box-shadow: 0 4px 24px 0 #2563eb22;
    background: #fff;
    border-left: 6px solid #2563eb;
    color: #222;
    animation: fadeIn 0.2s;
    pointer-events: auto;
  }
  .alert.success {
    border-left-color: #22c55e;
  }
  .alert.info {
    border-left-color: #2563eb;
  }
  .alert.error {
    border-left-color: #ef4444;
  }
  .alert-icon {
    font-size: 1.6rem;
    flex-shrink: 0;
    color: inherit;
  }
  .alert.success .alert-icon {
    color: #22c55e;
  }
  .alert.info .alert-icon {
    color: #2563eb;
  }
  .alert.error .alert-icon {
    color: #ef4444;
  }
  .alert-message {
    flex: 1;
    word-break: break-word;
  }
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(-10px); }
    to { opacity: 1; transform: translateY(0); }
  }
</style>

<svelte:window on:beforeunload={() => clearTimeout(timeoutId)} /> 