<script>
    let title = "";
    let description = "";
    let result;

	async function doPost () {
		const res = await fetch('http://localhost:59697/api/todo', {
			method: 'POST',
			headers: {
			'Content-Type': 'application/json'
			},
			body: JSON.stringify({title,description})
		})
	}

    async function getResult() {
        let response = await fetch(`http://localhost:59697/api/todo`);
        let res = await response.json();
        return res;
    }

    async function submitHandler(e) {
		await doPost ();
        result = getResult();
    }
</script>

<div class="container mt-5">
    <div class="row">
        <div class="col-md"></div>
        <div class="col-md-8 text-center">
            <h1 class="display-4">ToDo</h1>
            <form class="form-inline" on:submit|preventDefault={submitHandler}>
                <input class="w-75 form-control" type="text" bind:value={title}>
                <input class="w-75 form-control" type="text" bind:value={description}>
                <button class="w-25 btn btn-dark">Ekle</button>
            </form> 

            {#if result===undefined}
 
            <p>no result</p>

            {:else}

            {#await result}

            <p>Loading...</p>

            {:then value}

			<ul>
				{#each value as item}
					<li>{item.title} - {item.description}</li>
				{/each}
			</ul>

            {:catch error}

            {error.message}

            {/await}

            {/if}
        </div>
        <div class="col-md"></div>
    </div>
</div>