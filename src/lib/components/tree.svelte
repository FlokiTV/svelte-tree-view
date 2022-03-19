<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import Children from "./children.svelte";
  export let note: any;
  export let deep = 0;
  const dispatch = createEventDispatcher();
  function find(id: any) {
    dispatch("find", {
      id: id,
    });
  }
</script>

<div
  style="margin-left:{deep}px"
  class="bg-black p-2 mt-0.5"
  on:click={() => {
    find(note.id);
  }}
>
  {note.title}
</div>
{#if note.children.length}
  {#each note.children as n}
    <Children
      on:find={(event) => {
        find(event.detail.id);
      }}
      note={n}
      deep={deep + 15}
    />
  {/each}
{/if}
