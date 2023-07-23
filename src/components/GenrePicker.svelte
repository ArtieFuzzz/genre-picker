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

      genre = genres[Math.floor(Math.random() * genres_length)]
    } catch {
      genre = 'ERROR: Could not fetch /genres.txt try refreshing'
    }
  })
</script>

<div
  class="flex flex-col items-center justify-center w-screen max-w-[21rem] md:max-w-xl"
>
  {#if !genre}
    <p class="text-[1rem] font-mono mt-0">Picking a genre...</p>
  {:else}
    <p class="text-[1rem] font-mono mt-0">
      The genre I have chosen is... <span class="font-extrabold">{genre}</span>
    </p>
    <button class="rounded-md" on:click={async () => pick()}>Re-pick</button>
  {/if}
</div>
