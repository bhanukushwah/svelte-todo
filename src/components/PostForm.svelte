<form on:submit={onSubmit}>
    <div class="input-field">
        <label for="title">Title</label>
        <input type="text" bind:value={editingPost.title} />
    </div>
    <button type="submit" class="waves-effect waves-light btn"> {editingPost.url ? "Update" : "Add"}</button>
   
</form>

<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher()
    export let editingPost
    $: title = editingPost.title;
    const apiBaseUrl = 'http://todo-backend-express.herokuapp.com';

    async function onSubmit(event) {
        event.preventDefault();

        const newPost = {
            title: title,
            order: 10
        };

        let url = ''
        let method = ''
        if(editingPost.url){
            url = editingPost.url
            method = "PATCH"
        }
        else {
            url = apiBaseUrl
            method = "POST"
        }

        const res = await fetch(url, {
            method,
            body: JSON.stringify(newPost),
            headers: {
                "Content-Type":"application/json",
            }
        });
        const post = await res.json();
        dispatch('postCreated', post)
        title = ''
    }
</script>