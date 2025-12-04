<script lang="ts">
  let count = 1;
  let passwords: any[] = [];
  let loading = false;
  let error: string | null = null;
  let countdown = 0;

  async function generatePasswords() {
    if (countdown > 0) return;

    loading = true;
    error = null;

    try {
      const response = await fetch(`https://pw.bkend.app/v2/generate/${count}`);
      if (!response.ok) {
        throw new Error("Failed to fetch passwords");
      }
      const data = await response.json();
      passwords = data.results;
      startCountdown();
    } catch (e: any) {
      error = e.message;
    } finally {
      loading = false;
    }
  }

  function startCountdown() {
    countdown = 10;
    const interval = setInterval(() => {
      countdown -= 1;
      if (countdown <= 0) {
        clearInterval(interval);
      }
    }, 1000);
  }
</script>

<div class="flex items-center justify-center min-h-screen p-4">
  <div class="w-full max-w-4xl bg-rose-pine-base rounded-xl shadow-2xl p-8 m-4">
    <header class="mb-8 text-center">
      <h1 class="text-5xl font-bold neon-gradient-text">Galactic Passwords</h1>
    </header>

    <main class="w-full max-w-2xl mx-auto">
      <div class="flex items-center justify-center space-x-4 mb-8">
        <label for="password-count" class="text-lg text-rose-pine-subtle"
          >Number of passwords:</label
        >
        <select
          id="password-count"
          bind:value={count}
          class="bg-rose-pine-surface border border-rose-pine-highlight-high rounded-md p-2 text-rose-pine-text"
        >
          {#each Array.from({ length: 10 }, (_, i) => i + 1) as num}
            <option value={num}>{num}</option>
          {/each}
        </select>
        <button
          on:click={generatePasswords}
          disabled={loading || countdown > 0}
          class="px-6 py-2 rounded-md font-semibold
           bg-rose-pine-iris text-rose-pine-base
           hover:bg-rose-pine-love transition-colors duration-300
           disabled:bg-rose-pine-muted disabled:cursor-not-allowed"
        >
          {#if loading}
            <span>Generating...</span>
          {:else if countdown > 0}
            <span>Wait ({countdown}s)</span>
          {:else}
            <span>Generate</span>
          {/if}
        </button>
      </div>

      {#if error}
        <p class="text-center text-rose-pine-love">{error}</p>
      {/if}

      <div class="space-y-6">
        {#each passwords as item}
          <div
            class="bg-rose-pine-surface p-4 rounded-lg border border-rose-pine-highlight-med"
          >
            <p class="text-lg font-bold text-rose-pine-rose mb-1">
              {item.sentence}
            </p>
            <p class="text-sm text-rose-pine-subtle mb-2">— {item.source}</p>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div class="bg-rose-pine-overlay p-3 rounded">
                <p class="font-mono text-rose-pine-foam">
                  {item.passwords.noSpaces}
                </p>
              </div>
              <div class="bg-rose-pine-overlay p-3 rounded">
                <p class="font-mono text-rose-pine-foam">
                  {item.passwords.hyphenated}
                </p>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </main>
  </div>
</div>
