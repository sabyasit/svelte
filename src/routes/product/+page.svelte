<script>
    import cart from "../../store.js";

    let products = [];

    fetch("https://fakestoreapi.com/products")
        .then((r) => r.json())
        .then((data) => {
            let items = [];
            let category = Array.from(new Set(data.map((x) => x.category)));
            category.forEach((x, index) => {
                items.push({
                    id: index,
                    category: x,
                    items: data.filter((y) => y.category === x),
                });
            });
            products = items;
        });

    let addCart = (product) => {
        cart.update((value) => {
            if (value.filter((x) => x.id === product.id).length === 0) {
                value.push({ ...product, count: 1 });
            }
            return value;
        });
    };
</script>

<main>
    {#if products.length > 0}
        {#each products as item, index1 (item.id)}
            <div class="category">{item.category}</div>
            <div class="main">
                {#each item.items as p, index (p.id)}
                    <div class="product">
                        <div class="title">{p.title}</div>
                        <div class="img">
                            <img src={p.image} alt={p.title} />
                        </div>
                        <div class="details">
                            <div>₹{p.price}</div>
                            <div class="add">
                                <a href="./product/{p.id}">View</a>
                                <button on:click={() => addCart(p)}>Add</button>
                            </div>
                        </div>
                    </div>
                {/each}
            </div>
        {/each}
    {:else}
        <div class="loading">Loading...</div>
    {/if}
</main>

<style>
    main {
        display: flex;
        flex-direction: column;
        margin-bottom: 15px;
    }
    .category {
        padding: 25px 10px 5px 10px;
        text-transform: uppercase;
        font-size: larger;
        font-weight: 700;
    }

    .main {
        display: flex;
        gap: 20px;
        flex-wrap: wrap;
        margin: 0px 10px;
        justify-content: flex-start;
    }
    .loading {
        margin: 0 auto;
        padding-top: 100px;
    }
    .product {
        display: flex;
        flex-direction: column;
        width: 320px;
        padding: 10px;
        border: solid 1px rgb(227 227 227);
        box-shadow: rgba(50, 50, 93, 0.25) 0px 2px 5px -1px,
            rgba(0, 0, 0, 0.3) 0px 1px 3px -1px;
    }
    .product .title {
        font-weight: 600;
    }
    .product .img {
        margin-top: 10px;
        text-align: center;
    }
    .product .img img {
        width: 110px;
    }
    .product .details {
        margin-top: auto;
        display: flex;
    }
    .product .details .add {
        margin-left: auto;
    }
    .product .details .add a {
        text-decoration: none;
        font-size: 14px;
        padding-right: 10px;
        color: blue;
        font-weight: 600;
    }
</style>
