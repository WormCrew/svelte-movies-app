<script>
  import { onMount } from  'svelte'
  
  export let movie_id
  const ApiKey = __myapp.env.API_KEY
  const PERSONS_API = `https://api.themoviedb.org/3/movie/${movie_id}/credits?api_key=${ApiKey}&language=en-US`
  const IMAGE_API = 'https://image.tmdb.org/t/p/w200/'
  let persons = []

  onMount(async () => {
    persons = await fetch(PERSONS_API)
		.then(x => x.json())
    .then(x => x.cast)
	})
</script>

{#if persons}
  <div class='container'>
    <h2>Top Billed Cast</h2>
    <div class='person-wrapper'>
      {#each persons as person}
      
        <div class='persons-card'>
          <a class='person-link'href=#/people/{person.id} key={person.id}>
          {#if person.profile_path}
            <img src={ IMAGE_API + person.profile_path } alt='profile'>
            {:else}
            <i class="icon fa fa-user-o fa-5x"></i>
            {/if}
          <h4 class='name'>{person.name}</h4> </a>
        </div>
     
      {/each}
    </div>
  </div>
{/if}

<style>
  .person-link {
    text-decoration: none;
    pointer-events: auto;
    cursor: pointer; 
  }  
  .persons-card:hover {
    transform: scale(1.1);
  }
  .name {
    color: var(--light-text);
    margin: 0;
    font-weight: 400;
    text-align:center;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .container {
    margin: 0 auto;
    width: 1250px;
    padding-bottom: 70px;
  }

  .person-wrapper {
    display: flex;
    overflow-y: hidden;
    position: relative;
    top: 0;
    left: 0;
  }

  /* .person-wrapper::after {
    content: '';
    width: 60px;
    height: 100%;
    position: absolute;
    top: 0;
    right: 0;
    background-image: linear-gradient(to right, rgba(255,255,255,0) 0%, #932432 100%);
    will-change: opacity;
  } */

  .persons-card {
    margin-bottom: 0.5rem;
    position: relative;
    background-color:var(--primary-colour);
    margin-right: 0.8rem;
    border-radius: 3px;
    transition: all .4s ease-in-out;
    cursor: pointer; 
  }

  img {
    width: 110px;
    border-radius: 3px;
  }

  .icon {
    display: inline-block;
    height: 165px;
    width: 110px;
    position: relative;
    top: 40px;
    left: 20px;
    z-index: 1;
  }
</style>

