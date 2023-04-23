<script>
  import "../global.css";
  import cart from "../store.js";
  import CartModal from '$lib/cart.svelte';
  import Cart_icon from '$lib/cart_icon.svelte';

  let cartCount = 0;
  let showCart = false;

  cart.subscribe((value) => {
    cartCount = value.length;
  });

  let openCart = () => {
    showCart = true;
  };

  let toggleCartModal = () => {
    showCart = !showCart;
  };

</script>

<header>
  <nav>
    <div>
      <a href="/">Home</a>
      <a href="/product">Product</a>
    </div>
    <div class="cart">
      <button on:click={openCart}><Cart_icon /> <br> {cartCount} Item</button>
    </div>
  </nav>
</header>
<CartModal {showCart} on:click={toggleCartModal} />
<slot />

<style>
  header {
    text-align: center;
    background: rgb(180, 179, 179);
    padding: 5px 20px;
  }
  nav {
    display: flex;
    align-items: center;
  }
  .cart {
    margin-left: auto;
  }
  a {
    margin-left: 15px;
    text-decoration: none;
    font-weight: 600;
    text-transform: uppercase;
  }
</style>
