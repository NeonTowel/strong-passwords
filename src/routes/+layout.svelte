<script lang="ts">
  import "../app.css";

  let { children } = $props();
</script>

<svelte:head>
  <title>Galactic Passwords</title>
</svelte:head>

<div
  class="bg-gradient-to-br from-rose-pine-overlay to-rose-pine-base text-rose-pine-text min-h-screen"
>
  {@render children()}
</div>
