<script lang="ts">
  import { onMount } from 'svelte'

  let genre: string | null = null
  let genres: string[] | null = null
  let genres_length: number | null = null

  async function pick() {
    genre = genres![Math.floor(Math.random() * genres_length!)]
  }

  onMount(async () => {
    try {
      let req = await fetch('/genres.txt')

      if (!req.ok) throw null

      const res = await req.text()
      genres = res.split('\n')
      genres_length = genres!.length

      console.info(`Loaded ${length} genres`)

      genre = genres[Math.floor(Math.random() * genres_length)]
    } catch {
      genre = 'ERROR: Could not fetch /genres.txt try refreshing'
    }
  })
</script>

{#if !genre}
  <p class="text-xl font-mono">Picking a genre...</p>
{:else}
  <p class="text-xl font-mono">
    The genre I have chosen is... <span class="font-extrabold">{genre}</span>
  </p>
  <button class="rounded-md" on:click={async () => pick()}>Re-pick</button>
{/if}
