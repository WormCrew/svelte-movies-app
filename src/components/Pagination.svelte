<script>
  export let total_pages
  // export let current_page
  import { current_page } from './store.js'
  

  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();
  
  let pages=[]
  const LEFT_PAGE = 'LEFT';
  const RIGHT_PAGE = 'RIGHT';
  const totalNumbers = 9
  const totalBlocks = 11

  function rangex(size, startAt = 0) {
    return [...Array(size).keys()].map(i => i + startAt);
  }

  function range(from, to) {
    let i = from;
    const range = []
    while (i <= to) {
      range.push(i)
      i += 1
    }
    return range
  }

  function fetchPageNumbers() {
    if (total_pages > totalBlocks) {
      let startPage = (Math.max(2, $current_page - 3))
      let endPage = Math.min(total_pages - 1, $current_page + 3);
      pages = range(startPage, endPage);

      const hasLeftSpill = startPage > 2;
      const hasRightSpill = (total_pages - endPage) > 1;
      const spillOffset = totalNumbers - (pages.length + 1);

      if (hasLeftSpill && !hasRightSpill) {
          const extraPages = range(startPage - spillOffset, startPage - 1);
          pages = [LEFT_PAGE, ...extraPages, ...pages]
        } else if (!hasLeftSpill && hasRightSpill) {
          const extraPages = range(endPage + 1, endPage + spillOffset);
          pages = [...pages, ...extraPages, RIGHT_PAGE]
        } else if (hasLeftSpill && hasRightSpill)  {
          pages = [LEFT_PAGE, ...pages, RIGHT_PAGE]
      }
      pages = [1,...pages,total_pages]
    } else {
    pages = range(1, total_pages)
  }
}
  function handleClick(page) {
    if (page !== $current_page) {
      $current_page = page
      dispatch('change', page);
    }
  }
  
  $: fetchPageNumbers($current_page)

</script>

{#if (total_pages > 1) }
  <section class= 'pagination'>
    {#each pages as page}
      {#if (page === LEFT_PAGE)}
        <a class="page-link" href="" aria-label="Previous" on:click|preventDefault ="{() => handleClick($current_page -1)}">
          <i class="fa fa-angle-left"></i>
        </a>
      {:else if (page === RIGHT_PAGE)}
        <a class="page-link" href="" aria-label="Next" on:click|preventDefault ="{() => handleClick($current_page + 1)}">
          <i class="fa fa-angle-right"></i>
        </a>
      {:else}
        <a class="page-link { $current_page === page ? 'active' : ''}" href="" on:click|preventDefault ="{() => handleClick(page) }">{ page }</a>
      {/if}
    {/each}
  </section>
{/if}

<style>
  i {
    font-size: 14px;
  }

  .page-link {
		background-color:var(--secondary-colour);
		border-radius:30px;
		border: 2px solid var(--border-colour);
		margin: 2px 10px;
		text-decoration: none;
		font-weight: 600;
		padding-bottom: 2px;
		padding-left: 37px;
		padding-right: 37px;
		padding-top: 1px;
		display: flex;
		justify-content: center;
		transition: all .4s ease-in-out;
  }  
  .page-link:hover {
    background-color: rgb(155, 89, 218);
		color: black;
	}
  .active {
    font-weight: 900;
    background-color: rgb(100, 6, 6);
    scale: 1.1;
  }

	.pagination {
		background-color: var(--primary-colour);
		max-width: 1250px;
		margin: 5px auto;
		padding: 2px;
    display: flex;
    flex: wrap;
    justify-content: center;
		font-size: 12px;
		border-radius: 15px;
	}
</style>