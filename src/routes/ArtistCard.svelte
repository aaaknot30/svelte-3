<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let id;
  export let api_link;
  export let title;
  export let artist_id;
  export let artist_title;
  export let image_id;
  export let artist_display;
  export let place_of_origin;
  export let showLink;


  function generateImageTag() {
    //console.log(image_id)
    dispatch("message", {
      text: image_id
    });
  }

  function generateArtistId() {
    //console.log(image_id)
    dispatch("message2", {
      text: id
    });
  }


</script>

<div>
  {#if showLink == "true"}
  <p><a href="/" on:click={generateArtistId}>{api_link}</a></p>
  {/if}
  {#if title !== undefined}<p>{title}</p>{/if}
  {#if artist_title !== undefined}<p>{artist_title}</p>{/if}
  {#if image_id !== undefined}<p><a href="/" on:click={generateImageTag}>{image_id}</a></p>{/if}
  {#if artist_display !== undefined}<p>{artist_display}</p>{/if}
  {#if place_of_origin !== undefined}<p>{place_of_origin}</p>{/if}
</div>

<style>
  div {
    background-color: rgb(183, 206, 249);
    border: 1px solid green;
    border-radius: 12px;
    margin: 15px;
    width: 375px;
    text-align: center;
    box-shadow: 2px 2px 2px #333;
    color: #111;
  }

  p {
    margin: 4px;
  }

</style>
