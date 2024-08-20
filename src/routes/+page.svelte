<script>
  import ThreeItemGrid from '$components/ThreeItemGrid.svelte';
  import Carousel from '$components/Carousel.svelte';

  /** @type {import('./$types').PageData} */
  export let data;

  $: clothesCollection = data.products[0]?.node?.products?.edges;
  $: featuredCollection = data.products[1]?.node?.products?.edges;
</script>

<svelte:head>
  <title>Home – SvelteKit Commerce</title>
</svelte:head>

<main>
  <section>
    <div class="lg:h-[60vh]">
      <ThreeItemGrid products={featuredCollection} />
    </div>
  </section>
  <section>
    <div class="flex flex-col px-8 py-20 text-dark bg-light lg:flex-row lg:items-center">
      <div
        class="flex-none mb-4 mr-8 text-3xl font-bold text-left md:text-4xl lg:mb-0 lg:w-1/3 lg:text-right lg:text-6xl"
      >
        Jelajahi Dunia Bersama <strong class="text-bmrOrange"> BMR Tour And Travel </strong>
      </div>
      <div>
        <div class="lg:text-2xl">
          BMR Tour adalah mitra perjalanan terpercaya Anda untuk menjelajahi keindahan Bali. Kami
          menawarkan berbagai paket wisata menarik, mulai dari liburan santai di pantai hingga
          petualangan seru di alam. Dengan pengalaman kami, kami berkomitmen memberikan pengalaman
          liburan yang tak terlupakan bagi setiap pelanggan.
        </div>
        <button class="mt-4 font-bold text-bmrMain hover:text-bmrOrange lg:text-2xl">
          Baca Lebih Lanjut
        </button>
      </div>
    </div>
  </section>
  <section>
    <Carousel items={clothesCollection} />
  </section>
</main>
