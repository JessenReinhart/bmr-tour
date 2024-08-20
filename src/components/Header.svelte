<script>
  import { page } from '$app/stores';
  import Icons from '$components/Icons.svelte';
  import { cartQuantity } from '../store';
  import SearchBar from '$components/SearchBar.svelte';
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  $: currentRoute = $page.url.pathname;

  let showMenu = false;

  let tabs = [
    { name: 'Semua Paket', path: '/search' },
    { name: 'Paket Bali', path: '/search/featured' },
    { name: 'Travel Packages', path: '/search/clothes' }
  ];
  function openCart() {
    showMenu = false;
    dispatch('openCart', true);
  }
</script>

<nav class="flex items-center border-b bg-light text-bmrMain border-zinc-700 p-4 lg:px-6">
  <div class="flex w-1/3 items-center">
    <div class="mr-4" class:active={currentRoute === '/'}>
      <a href="/" data-sveltekit-prefetch class="">
        <img src="/bmr-logo.png" alt="BMR Logo" class="h-20" />
      </a>
    </div>
    <div class="hidden lg:flex">
      {#each tabs as tab, i (tab.name)}
        <div class:active={currentRoute === tab.path}>
          <a
            data-sveltekit-prefetch
            href={tab.path}
            class={`hover:opacity-100 px-2 py-1 text-bmrMain hover:text-bmrOrange font-bold rounded-lg ${
              currentRoute === tab.path ? 'opacity-100' : 'opacity-75'
            }`}>{tab.name}</a
          >
        </div>
      {/each}
    </div>
  </div>
  <div class="hidden w-1/3 lg:block">
    <SearchBar />
  </div>
</nav>
