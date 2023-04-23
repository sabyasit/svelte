<script>
    import cart from "../store.js";

    export let showCart = false;

    let totalPrice = 0;
    let cartItem = [];
    cart.subscribe((value) => {
        totalPrice = 0;
        cartItem = value;
        cartItem.forEach((item) => {
            totalPrice = totalPrice + (item.count * item.price);
        })
    });
    let deleteItem = (item) => {
        cart.update((value) => {
            value = value.filter((x) => x.id != item.id);
            return value;
        });
    };
    let addItemCounter = (item) => {
        cart.update((value) => {
            value.find((x) => x.id === item.id).count += 1;
            return value;
        });
    };
    let removeItemCounter = (item) => {
        if(item.count === 1) {
            return;
        }
        cart.update((value) => {
            value.find((x) => x.id === item.id).count -= 1;
            return value;
        });
    };
</script>

<main>
    {#if showCart}
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <div class="backdrop" on:click|self>
            <div class="modal">
                <div class="title">Cart</div>
                <div class="body">
                    {#if cartItem.length > 0}
                        {#each cartItem as item, index (item.id)}
                            <div class="bodyItem">
                                <div>
                                    <img src={item.image} alt={item.title} />
                                </div>
                                <div>{item.title}</div>
                                <div>
                                    <button on:click={() => removeItemCounter(item)}>-</button>
                                    {item.count}
                                    <button on:click={() => addItemCounter(item)}>+</button>
                                </div>
                                <div>
                                    ₹{(item.count * item.price).toFixed(2)}
                                </div>
                                <div>
                                    <button on:click={() => deleteItem(item)}
                                        >Delete</button
                                    >
                                </div>
                            </div>
                        {/each}
                    {:else}
                        <div>No Item</div>
                    {/if}
                </div>
                <div class="footer">₹{totalPrice.toFixed(2)}</div>
            </div>
        </div>
    {/if}
</main>

<style>
    .backdrop {
        top: 0px;
        bottom: 0px;
        left: 0px;
        right: 0px;
        position: fixed;
        background: rgba(0, 0, 0, 0.8);
    }
    .modal {
        border-radius: 10px;
        max-width: 700px;
        margin: 100px auto;
        max-height: calc(100vh - 200px);
        background: white;
        display: flex;
        flex-direction: column;
    }
    .modal .title {
        border-bottom: solid 1px gray;
        padding: 10px;
        font-size: 20px;
        font-weight: 600;
    }
    .modal .footer {
        padding: 10px;
        border-top: solid 1px gray;
        text-align: right;
        font-size: 20px;
        font-weight: 600;
        color: green;
    }

    .modal .body {
        padding: 10px;
        flex: 1;
        overflow-y: scroll;
    }

    .modal .body .bodyItem {
        display: flex;
        gap: 10px;
        padding: 10px 0px;
    }

    .modal .body .bodyItem img {
        width: 50px;
    }
    .modal .body .bodyItem div {
        flex: 1;
        text-align: center;
    }
</style>
