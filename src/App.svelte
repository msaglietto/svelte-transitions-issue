<script>
	import { fade, fly } from 'svelte/transition';
	let visible = true;

  function workaround(node, params) {
     if (!node.hasOwnProperty('ownerDocument')) {
        Object.defineProperty(node, 'ownerDocument', { get: function() { return node.parentElement; } });
        node.parentElement.head = node.parentElement
     }
    return fly(node, params)
  }
</script>

<svelte:options tag="test-transitions" />

<div>
  <label>
    <input type="checkbox" bind:checked={visible}>
    visible
  </label>

  {#if visible}
    <p transition:fade="{{ duration: 3000 }}">
      Fades in and out not working
    </p>
  {/if}
  <br />
  {#if visible}
    <p transition:workaround="{{ duration: 3000 }}">
      Fly in and out working
    </p>
  {/if}
</div>
