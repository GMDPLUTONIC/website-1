<script lang="ts">
    import { createEventDispatcher } from "svelte";

    import Button from "./Button.svelte";
    import Row from "./Row.svelte";

    export let perPage: number;
    export let total: number;
    export let page: number;
    export let pageCount: number;
    export let label: string;
    export let labelOne: string = label;
    export let disabled = false;

    $: max_page = Math.max(Math.ceil(total / perPage), 1);
    $: title = pageCount == 1 ? labelOne : label;

    const dispatch = createEventDispatcher<{ select: { page: number } }>();

    const gotoPage = (page: number) => {
        dispatch("select", { page });
    };
</script>

<nav>
    {#if pageCount === total}
        <p>Showing {pageCount} {title}</p>
    {:else}
        <p>Showing {pageCount} of {total} {title}</p>
    {/if}
    <Row>
        <Button
            on:click={async () => gotoPage(Math.max(page - 1, 1))}
            icon="left"
            design="dark-small"
            disabled={disabled || total === 0 || page === 1} />
        <span>Page {page} of {max_page}</span>
        <Button
            on:click={async () => gotoPage(Math.min(page + 1, max_page))}
            icon="right"
            design="dark-small"
            disabled={disabled || total === 0 || page === max_page} />
    </Row>
    
    <div style="justify-self: end;">
        <slot />
    </div>
</nav>

<style lang="scss">
    @use "$lib/styles/media-queries.scss" as *;

    nav {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
        gap: 1rem;

        @include gt-md {
            display: grid;
            grid-template-columns: 1fr max-content 1fr;
        }
    }
</style>
