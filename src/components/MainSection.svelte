<script>
	import MovieList from './MovieList.svelte'
	import Pagination from './Pagination.svelte'
	import MorePages from './MorePages.svelte'
	import {fade} from 'svelte/transition'
	import { onMount, afterUpdate, beforeUpdate, tick} from 'svelte'
	import { current_page } from './store.js'
	export let api_url
	$current_page = 1
	let movies =[]
	
	let total_pages = 0;
	let scroll = 0	

	onMount(async () => {
			getMovies(api_url + $current_page)
	})

	async function getMovies (API) {
		scroll = 0	
		const res = await fetch(API)
		const res_json = await res.json()
		movies = await res_json.results
		total_pages = res_json.total_pages
	}

 async function moreMovies (API) {
		const res = await fetch(API)
		const res_json = await res.json()
		const res_results = await res_json.results
		// await tick()
		scroll = (document.body.scrollHeight)
		movies = [...movies,...res_results ]
		
 }

 afterUpdate(() => {
		window.scrollTo({top: scroll, behavior: 'smooth'})
	});
</script>


{#if total_pages&&$current_page}
	<Pagination
		{total_pages}
		on:change="{(ev) => getMovies(api_url + ev.detail)}">
	</Pagination>
{/if}

<MovieList {movies}/>

{#if (total_pages-$current_page)}
	<MorePages 
    {total_pages}
    on:change="{(ev) => moreMovies(api_url + ev.detail)}">
	</MorePages>
{/if}


<style>

</style>