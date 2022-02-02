<script>
    export let recettes = [];
    let query;

    async function submitForm() {
        const submit = await fetch('/recettes/search.json', {
            method: 'POST',
            headers: {
                'Accept': 'application/json, text/plain, */*',
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ query }),
        })
        const data = await submit.json()

        recettes = data
    }
</script>

<svelte:head>
    <title>Recettes</title>
</svelte:head>

<form on:submit|preventDefault={submitForm}>
    <label for="query">
        <span class="sr-only">Recherche sur marmiton.org</span>
    </label>
    <input
            id="query"
            aria-label="Recherche sur marmiton.org"
            type="search"
            name="query"
            placeholder="Recherche sur marmiton.org"
            required
            bind:value={query}
    />
    <input type="submit" value="Rechercher" />
</form>

<section class="recettes">
    {#each recettes as item, index}
        <article>
            <h2><a sveltekit:prefetch href="{item.url ?? `/recettes/${index}`}">{item.name}</a></h2>
            <h3>⏱ {item.totalTime} min 👨‍🍳 {['', 'Très Facile', 'Facile', 'Moyenne', 'Difficile'][item.difficulty || 0]} € {['', 'Bon marché', 'Moyen', 'Assez cher'][item.budget||0]} 😋 {item.people} Personnes</h3>
            {#if item.image}
                <img src={item.image} alt={item.name}>
            {/if}
        </article>
    {/each}
</section>