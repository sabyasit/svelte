<script>
    import { page } from "$app/stores";
    import cart from "../../../store.js";

    let product = null;
    fetch(`https://fakestoreapi.com/products/${$page.params.id}`)
        .then((r) => r.json())
        .then((data) => {
            product = data;
        });

    let addCart = (product) => {
        cart.update((value) => {
            value.push({ ...product, count: 1 });
            return value;
        });
    };
</script>

<main>
    {#if product}
        <div class="product">
            <img class="img" src={product.image} alt={product.title} />
            <div class="details">
                <div><strong>{product.title}</strong></div>
                <div>{product.description}</div>
                <div><strong>Category</strong> - {product.category}</div>
                <div><strong>Ratting</strong> - {product.rating.rate}</div>
                <div><strong>Price</strong> - ₹{product.price}</div>
                <div>
                    <button on:click={() => addCart(product)}>Add</button>
                </div>
            </div>
        </div>
    {:else}
        <div class="loading">Loading...</div>
    {/if}
</main>

<style>
    main {
        display: flex;
        margin: 20px 10px;
    }
    .loading {
        margin: 0 auto;
        padding-top: 100px;
    }
    .product {
        display: flex;
        padding: 10px;
        gap: 10px;
        flex: 1;
        border: solid 1px rgb(227 227 227);
        box-shadow: rgba(50, 50, 93, 0.25) 0px 2px 5px -1px,
            rgba(0, 0, 0, 0.3) 0px 1px 3px -1px;
    }
    .product .img {
        width: 110px;
    }
    .product .details {
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: center;
        gap: 5px;
    }
</style>
