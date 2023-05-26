<script>
    export let item;
    export let removeTodoHandler;
    export let done;
    export let updateTodoHandler;
    let hover = false;
</script>

<div
    class="item {done ? 'strike' : ''}"
    on:mouseenter={() => (hover = true)}
    on:mouseleave={() => (hover = false)}
>
    <div>
        <input
            type="checkbox"
            name=""
            id=""
            bind:checked={done}
            on:change={() => updateTodoHandler(item)}
        />
        <span>{item}</span>
    </div>

    {#if hover}
        <button class="delete" on:click={() => removeTodoHandler(item)}
            >Remove
        </button>
    {/if}
</div>
