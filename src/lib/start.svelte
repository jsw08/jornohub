<script>
import { createEventDispatcher } from 'svelte';
const dispatch = createEventDispatcher();

  const handleClick = (e,ee) => {
    switch (e) {
      case "memes":
        dispatch('load', ee);
    }
  } 
</script>

<div style:display="grid" style:grid-gaps="3px" style:font-size="20pt">
  <a href="#uClickedOmgWOW" on:click={() => {dispatch('load',{type:'memes'})}}>Memes</a>
  <a href="##uClickedOmgWOW" on:click={() => {dispatch('load', {type:'custom', sub:['eyeblech']})}}>Eyeblech</a>
  <a href="##uClickedOmgWOW" on:click={() => {dispatch('nsfw')}}>Custom NSFW</a>
</div>