<script lang="ts">
  import Tree from "$lib/components/tree.svelte";

  let id = 1;
  let notes = []; // array to store all notes

  //svelte tick to do reactive
  const updateNotes = () => {
    notes = [...notes];
  };

  const generateNote = () => {
    let n = {
      id,
      title: "Note " + id,
      children: [],
    };
    id++;
    return n;
  };

  // add one note
  notes.push(generateNote());

  // simulate a request
  setTimeout(() => {
    notes.push(generateNote());
    updateNotes();
  }, 200);

  // on:click on note inside component, emit event to mainframe with note.id to find
  const find = (event) => {
    let { id } = event.detail; //get id from emited event
    let find = each(id, [notes]); // start the first loop
    if (find) {
      /*
        when find, will return note reference
        then you can make a request on database and push new notes on note.children
      */
      find.children.push(generateNote());
      updateNotes();
    } else {
      alert("note not found");
    }
  };

  //Each array and verify every note
  const each = (id: any, noteArray: any) => {
    let find: any = false;
    let stack = []; //store next chieldrens

    noteArray.forEach((children) => {
      //each noteArray array
      children.forEach((note) => {
        if (note.id == id) find = note;
        // push childrens to stack[]
        if (note.children.length) stack.push(note.children);
      });
    });
    if (find) return find;
    // if not found note, keep running on their children
    if (!find && stack.length) return each(id, stack);
    if (!find) return false;
  };
</script>

<div class="w-1/2 p-2 text-white">
  {#each notes as note}
    <Tree {note} on:find={find} />
  {/each}
</div>
