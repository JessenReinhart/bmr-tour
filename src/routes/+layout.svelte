<script>
  import '../app.css';
  import Header from '$components/Header.svelte';
  import Footer from '$components/Footer.svelte';
  import ShoppingCart from '$components/ShoppingCart.svelte';
  import { getCartItems } from '../store';
  import { onMount } from 'svelte';
  import { createCart } from '$utils/shopify';
  import 'bytemd/dist/index.css'

  let cartId;
  let checkoutUrl;
  let cartCreatedAt;
  let cartItems = [];

  onMount(async () => {
    if (typeof window !== 'undefined') {
      cartId = JSON.parse(localStorage.getItem('cartId'));
      cartCreatedAt = JSON.parse(localStorage.getItem('cartCreatedAt'));
      checkoutUrl = JSON.parse(localStorage.getItem('cartUrl'));

      let currentDate = Date.now();
      let difference = currentDate - cartCreatedAt;
      let totalDays = Math.ceil(difference / (1000 * 3600 * 24));
      let cartIdExpired = totalDays > 6;
      if (cartId === 'undefined' || cartId === 'null' || cartIdExpired) {
        await callCreateCart();
      }
      await loadCart();
      document.addEventListener('keydown', (e) => {
        let keyCode = e.keyCode;
        if (keyCode === 27) {
          showCart = false;
        }
      });
    }
  });

  async function callCreateCart() {
    const cartRes = await createCart();

    if (typeof window !== 'undefined') {
      localStorage.setItem('cartCreatedAt', Date.now());
      localStorage.setItem('cartId', JSON.stringify(cartRes.body?.data?.cartCreate?.cart?.id));
      localStorage.setItem(
        'cartUrl',
        JSON.stringify(cartRes.body?.data?.cartCreate?.cart?.checkoutUrl)
      );
    }
  }

  async function loadCart() {
    const res = await getCartItems();
    cartItems = res?.body?.data?.cart?.lines?.edges;
  }

  let showCart = false;
  let loading = false;

  async function openCart() {
    await loadCart();
    showCart = true;
  }
  function hideCart() {
    showCart = false;
  }

  function getCheckoutUrl() {
    window.open(checkoutUrl, '_blank');
    loading = false;
  }

  async function addToCart(event) {
    await fetch('/cart.json', {
      method: 'PATCH',
      body: JSON.stringify({ cartId: cartId, variantId: event.detail.body })
    });
    // Wait for the API to finish before updating cart items
    await loadCart();
    loading = false;
  }

  async function removeProduct(event) {
    if (typeof window !== 'undefined') {
      cartId = JSON.parse(localStorage.getItem('cartId'));
    }
    await fetch('/cart.json', {
      method: 'PUT',
      body: JSON.stringify({
        cartId,
        lineId: event.detail.body.lineId,
        quantity: event.detail.body.quantity,
        variantId: event.detail.body.variantId
      })
    });
    await loadCart();
    loading = false;
  }
</script>

<main class={`${showCart ? 'h-screen' : 'min-h-screen'} text-white overflow-hidden`}>
  {#if showCart}
    <ShoppingCart
      items={cartItems}
      on:click={hideCart}
      on:removeProduct={removeProduct}
      on:addProduct={addToCart}
      on:getCheckoutUrl={getCheckoutUrl}
      bind:loading
    />
  {/if}
  <Header on:openCart={openCart} />
  <div class="min-h-screen overflow-scroll">
    <slot />
    <Footer />
  </div>
</main>
