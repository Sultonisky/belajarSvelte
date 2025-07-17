<script>
  let openModal = false;
</script>

{#if openModal}
  <div class="backdrop">
    <div class="modal">
      <p>Halo, World</p>
    </div>
  </div>
{/if}

<style>
  .backdrop {
    width: 100%;
    height: 100%;
    position: fixed;
    background: rgba(0, 0, 0, 0.8);
    z-index: 10;
    top: 0;
    left: 0;
    cursor: pointer;
  }

  .modal {
    padding: 10px;
    border-radius: 10px;
    max-width: 400px;
    margin: 10% auto;
    text-align: center;
    background: white;
  }
</style>
