<!-- JavaScript Section -->
<script>
  // General Artists Details

  let artist = "";
  let artistArray = [];
  let pageId = 1;
  let searchWord = "";
  let res;
  async function getArtData() {
    if (searchWord == "") {
      res = await fetch(`https://api.artic.edu/api/v1/artworks?page=${pageId}`);
    } else {
      res = await fetch(
        `https://api.artic.edu/api/v1/artworks/search?q=${searchWord}&page=${pageId}`
      );
    }
    const data = await res.json();
    artistArray = [];

    if (res.ok) {
      console.log(data);
      data.data.map((item) => {
        artistArray.push({
          id: item.id,
          api_link: item.api_link,
          title: item.title,
          artist_id: item.artist_id,
          artist_title: item.artist_title,
          image_id: item.image_id,
          artist_display: item.artist_display,
          place_of_origin: item.place_of_origin,
          alt_text: item.thumbnail.alt_text
        });
      });
      artist = JSON.stringify(artistArray);
    } else {
      throw new Error(data);
    }
    return artistArray;
  }

  function handleClick() {
    getArtData();
  }

  // Individual Art Details

  import ArtistCard from "./ArtistCard.svelte";

  let artistId;

  async function getArtistData() {
    const res = await fetch(
      `https://api.artic.edu/api/v1/artworks/${artistId}`
    );
    const data = await res.json();
    console.log(data)

    if (res.ok) {
      return data;
    } else {
      throw new Error(data);
    }
  }

  let promise;
  async function handleGetArtistData() {
    let promise1 = await getArtistData();
    promise = promise1.data;
  }

  // Get Art Image Section

  let artId;
  let artImg = "";
  function handleGetArtId() {
    artImg = `https://www.artic.edu/iiif/2/${artId}/full/843,/0/default.jpg`;
  }

  function handleMessage(event) {
    artId = event.detail.text;
    handleGetArtId();
  }
  
  function handleMessage2(event) {
    artistId = event.detail.text;
    handleGetArtistData();
  }

  let w;
  let h;
  let size = 400;

  import Navbar from "./Navbar.svelte";
</script>

<!--  - - - - - - - - - - -  - - - - HTML SECTION - - - -  - - - - - - -  - - - -  -->
<Navbar />

<main>
  <img class="header-img" src="https://api.artic.edu/docs/assets/logo.svg" alt="artic" /><h2 class="title">Art Institute of Chicago API</h2>
  <h5 class="header-text">
    This application will fetch data from the Art Institute of Chicago API and
    display the results.
  </h5>

  <!-- General Artists HTML Section  -->

  <div class="container">
    <section class="section">
      <form on:submit={handleClick}>
      <div>
        <input
          class="pageInput"
          type="text"
          name="pageId"
          bind:value={pageId}
        />
        <input
          class="artistInput"
          type="text"
          name="searchWord"
          bind:value={searchWord}
          placeholder="search word"
        />
      </div>
      <button type="submit" on:click={handleClick}> Get Art Data </button>
    </form>
    
      <div class="details">
        {#each artistArray as { id, api_link, title, artist_id, artist_title, image_id, artist_display, place_of_origin, 
          alt_text, credit_line, date_display, category_titles, classification_titles, medium_display }, index}
          
        
        <ArtistCard
            index = {index}
            id = {id}
            api_link = {api_link}
            title = {title}
            artist_id = {artist_id}
            artist_title = {artist_title}
            image_id = {image_id}
            artist_display = {artist_display}
            place_of_origin = {place_of_origin}
            alt_text = {alt_text}
            credit_line = {credit_line}
            date_display = {date_display}
            category_titles = {category_titles}
            classification_titles = {classification_titles}
            medium_display = {medium_display}
            on:message={handleMessage}
            on:message2={handleMessage2}
            showLink = true
          />
        {/each}
      </div>
    </section>

    <!--  ------------ --------Individual Artists HTML Section  ----------- ---------------->

    <section class="section">
      <input class="artistInput" type="text" name="artistId" bind:value={artistId} />
      <button on:click={handleGetArtistData}> Get Single Art Data </button>
      <div class="details">
        {#await promise}
          <p>...waiting</p>
        {:then response}
          {#if promise !== undefined}
          <ArtistCard
            id = {response.id}
            api_link = {response.api_link}
            title = {response.title}
            artist_id = {response.artist_id}
            artist_title = {response.artist_title}
            image_id = {response.image_id}
            artist_display = {response.artist_display}
            place_of_origin = {response.place_of_origin}
            alt_text = {response.alt_text}
            credit_line = {response.credit_line}
            date_display = {response.date_display}
            category_titles = {response.category_titles}
            classification_titles = {response.classification_titles}
            medium_display = {response.medium_display}
            on:message={handleMessage}
            on:message2={handleMessage2}
            showLink = false
        />
          {/if}
        {:catch error}
          <p style="color: red">{error.message}</p>
        {/await}
      </div>
    </section>

    <!-- ----------- ----------- Image Section ------------- -------------- -->

    <section class="section">
        <input type="range" min="200" max="1500" bind:value={size} />
        <input class="artInput" type="text" name="artId" bind:value={artId} />
      <button on:click={handleGetArtId}> Get Art Image </button>
      <img class="artImg" src={artImg} alt="" width={size} />
    </section>
  </div>
</main>
<div class="bgimg-1 w3-display-container w3-opacity-min" id="home">
  <div class="w3-display-middle" style="white-space:nowrap;">
    <span class="w3-center w3-padding-large w3-black w3-xlarge w3-wide w3-animate-opacity">Art Institute of Chicago API</span>
  </div>
  
</div>
<p class="footer-sign">By Kyle Larson, Jan. 2023 - Svelte Project</p>
<style>
  main {
    padding-top: 20px;
  }
  .container {
    display: flex;
    justify-content: space-around;
    align-items: flex-start;
  }

  .section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .artImg {
  }

  .pageInput {
    width: 30px;
  }

  .artistInput {
    width: 100px;
  }

  .artInput {
    width: 300px;
  }
  .header-img {
    margin-left: 10px;
  }

  .title {
    display: inline-block;
    margin-left: 10px;
  }

  .header-text {
    margin-left: 10px;
    margin-bottom: 20px;;
  }

  button {
    background-color: rgb(53, 53, 71);
    color:rgb(249, 249, 249);
    border-radius: 5px;
    padding: 3px 6px;
    margin: 5px;
    border: none;
  }

  .footer-sign {
    text-align: center;
    margin: 0 0 10px 0;
  }
</style>
