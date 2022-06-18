<script>
  import Img from './lib/img.svelte'
  import Start from './lib/start.svelte'
  import RedditImageFetcher from "reddit-image-fetcher";
  // ../node_modules/svelte-gamepad/src/Gamepad.svelte

  let chosen = false
  let startthings;
  $: imgs = []

  const nsfw = () => {
    let i = prompt("Enter nsfw code (yes ik this looks cheap, but i'm too lazy to make it look good lol).")
    if (i) 
      switch(i) {
        case "gonewild":
          load({"detail":{"type":"custom","sub":["nsfw","superhotteens","gonewild18"]}}, true);
          break;
        case "goneteens":
          load({"detail":{"type":"custom","sub":["superhotteens","gonewild18","legalteens","teenhdporn"]}}, true)
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
      total: 10,
      subreddit: e.detail.sub
    }).then(result => {
      imgs = result
      chosen=true
    });
  }

  const more = () => {
    RedditImageFetcher.fetch({
      type: startthings.type,
      total: 1,
      subreddit: startthings.sub
    }).then(result => {
      if (!imgs.includes(result[0])) {
        imgs.push(result[0])
        imgs = imgs
        if (nsfw) imgs[0] = {"title":"U BE HORNY","postLink":"about:blank","image":"https://c.tenor.com/G-hXJ8T-vqAAAAAC/tenor.gif"}
      } else {
        more()
      }
    })
  }
</script>

{#if chosen}
  <Img imgs={imgs} on:more={more}/>
  <button on:click={() => {location.reload()}} style:position="absolute" style:left="5px" style:bottom="5px">Home</button>
{:else}
  <Start on:nsfw={nsfw} on:load={load}/>
{/if} 
