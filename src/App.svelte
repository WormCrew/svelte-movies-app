<script>
	import Router from 'svelte-spa-router';
	import routes from './routes.js';
	import Header from './components/Header.svelte'
	import Genres from './components/Genres.svelte'
</script>

<main>
	<Header />
	<Genres />
	<Router {routes} />
</main>

<style>
main {
	overflow-y:hidden;
}
</style>
