<script>
  import Img from './lib/img.svelte'
  import Start from './lib/start.svelte'
  import RedditImageFetcher from "reddit-image-fetcher";

  let chosen = false
  let startthings;
  $: imgs = []

  const nsfw = () => {
    let i = prompt("Enter nsfw code (yes ik this looks cheap, but i'm too lazy to make it look good lol).")
    if (i) 
      switch(i) {
        case "gonewild":
          load({"detail":{"type":"custom","sub":["nsfw","superhotteens"]}});
          break;
        case "pokeporn":
          load({"detail":{"type":"custom", "sub":["pokeporn"]}})
          break;
        default:
          alert("No vaild input.")
      }
  }; 

  const load = (e) => {
    startthings = e.detail;
    RedditImageFetcher.fetch({
      type: e.detail.type,
      total: 10,
      subreddit: e.detail.sub
    }).then(result => {
        console.log(result);
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
      imgs.push(result[0])
      imgs = imgs
    })
  }
</script>

{#if chosen}
  <Img imgs={imgs} on:more={more}/>
  <button on:click={() => {location.reload()}} style:position="absolute" style:left="5px" style:bottom="5px">Home</button>
{:else}
  <Start on:nsfw={nsfw} on:load={load}/>
{/if} 
