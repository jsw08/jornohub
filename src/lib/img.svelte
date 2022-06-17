<script>
export let imgs = [{title:"No available images.",image:"https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftacm.com%2Fwp-content%2Fuploads%2F2018%2F01%2Fno-image-available.jpeg"}]
import { createEventDispatcher } from 'svelte';
import Gamepad from "../../node_modules/svelte-gamepad/src/Gamepad.svelte";
const dispatch = createEventDispatcher();

let cImgN = 0
let canclick = true
$: currentImg = imgs[cImgN]

const btn = (btn) => {
  if (canclick) {
    canclick = false
    if (btn && imgs[cImgN -1] !== undefined)
      cImgN--
    else if (!btn && imgs[cImgN +1] !== undefined)
      cImgN++; dispatch('more')
    setTimeout(() => {canclick = true}, 500)
  }
}

window.addEventListener("keypress", function (e) {
  if (e.key === ' ' || e.key === 'Spacebar') {
    // ' ' is standard, 'Spacebar' was used by IE9 and Firefox < 37
    e.preventDefault()
    btn(false)
  }
})
</script>

{#each imgs as i}
<img src={i.img} style:display="none" alt="Img prefetcher"/>
{/each}

<Gamepad
  gamepadIndex={0}
  on:DPadUp={() => {btn(false)}}
  on:DPadDown={() => {btn(true)}}
/>

<div>
  <a href={currentImg.postLink}>{currentImg.title}</a>
  <br>
  <img on:click={() => {location.assign(currentImg.postLink)}} src={currentImg.image} alt={currentImg.title}/>
  <button style:left="5px" on:click={() => {btn(true)}}>	
  &#8592;</button>
  <button style:right="5px"  on:click={() => btn(false)}>	
  &#8594;</button>
</div>

<style>
img {
  width: auto;
  height: auto;
  object-fit: contain;
  max-height: 80vh;
  max-width: 80vw;
  min-width: 30vw;
  min-height: 30vh;
  border-radius: 13px;
  cursor: pointer;
}
button {
  margin: 0;
  position: absolute;
  top: 50%;
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);
  padding: 0px 10px;
  margin-bottom: 1;
  font-size: 18pt;
  color:white;
}
</style>