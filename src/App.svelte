<script>
  import Img from './lib/img.svelte'
  import Start from './lib/start.svelte'
  import RedditImageFetcher from "reddit-image-fetcher";
  import { onMount } from 'svelte';
  // ../node_modules/svelte-gamepad/src/Gamepad.svelte

  let chosen = false
  let startthings;
  let cNsfw = false
  let cSpoil = false
  $: imgs = []

  const nsfw = () => {
    let i = prompt("Enter nsfw code (yes ik this looks cheap, but i'm too lazy to make it look good lol).")
    if (i) 
      switch(i) {
        case "gonewild":
          load({"detail":{"type":"custom","sub":["nsfw","superhotteens","gonewild18"]}}, true);
          break;
        case "tudes":
          load({"detail":{"type":"custom","sub":["superhotteens","gonewild18","legalteens","teenhdporn","perfectbody","smallcutie","adorablenudes"]}}, true)
          break;
        case "pokeporn":
          load({"detail":{"type":"custom", "sub":["pokeporn"]}}, true)
          break;
        default:
          alert("No vaild input.")
      }
  }; 

  const load = (e, nsfw=false) => {
    startthings = e.detail;
    RedditImageFetcher.fetch({
      type: e.detail.type,
      total: 2,
      subreddit: e.detail.sub
    }).then(result => {
      imgs = result
      chosen=true
      if (cNsfw) imgs[0] = {"title":"BONK, U HAVE NSFW ENABLED","postLink":"about:blank","image":"https://c.tenor.com/G-hXJ8T-vqAAAAAC/tenor.gif"}
      more()
    });
  }

  const more = () => {
    RedditImageFetcher.fetch({
      type: startthings.type,
      total: 2,
      subreddit: startthings.sub
      // subreddit: [startthings.sub[Math.floor(Math.random() * startthings.sub.length)]]
    }).then(result => {
      if (!imgs.includes(result[0])) {
        imgs.push(result[0])
        imgs = imgs
      } else {
        more()
      }
    })
  }
</script>
<div style:left="5px" style:top="5px" style:position="absolute">
  <input bind:checked={cNsfw} id="c" type="checkbox">
  <label for="c">nsfw</label>
</div>
<div style:right="5px" style:top="5px" style:position="absolute">
  <label for="cc">spoilers (WIP)</label>
  <input disabled bind:checked={cSpoil} id="cc" type="checkbox">
</div>
{#if chosen}
  <Img imgs={imgs} nsfw={cNsfw}  spoil={cSpoil} on:more={more}/>
  <button on:click={() => {location.reload()}} style:position="absolute" style:left="5px" style:bottom="5px">Home</button>
{:else}
  <Start on:nsfw={nsfw} on:load={load}/>
{/if} 
