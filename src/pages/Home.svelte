<script>
import { onMount } from "svelte";
import PostForm from '../components/PostForm.svelte';

const apiBaseUrl = 'http://todo-backend-express.herokuapp.com';

let posts = [];

let editingPost = {
    title: '',
    url: null
}

onMount(async () =>{
    const res = await fetch(apiBaseUrl);
    posts = await res.json();
} )

const editPost = (post) => {
    editingPost = post;
}

const deletePost = (url) => {
   
        fetch(url, {
            method: 'DELETE',
            headers: {
                "Content-Type":"application/json",
            }
        })
        .then(res => res.json())
        .then(()=> {
            posts  = posts.filter(p => p.url !== url)
        })
}

const addPost = ({detail}) => {
    if(posts.find(p=>p.url===detail.url)){
        const index = posts.findIndex(p=> p.url == detail.url);
        let postsUpdated = posts;
        postsUpdated.splice(index, 1, detail)
        posts = postsUpdated;
        editingPost = {
    title: '',
    url: null
}
    }
    else{
        posts = [detail, ...posts]
        editingPost = {
    title: '',
    url: null
}
    }
}


</script>

<style>
    .delete-btn {
        color: red !important;
    }
</style>

<div class="container">
    <h2>
        Home
    </h2>

    <div class="row">
        <div class="col s12">
            <PostForm on:postCreated={addPost} editingPost={editingPost}/>
        </div>
    </div>

    <div class="row">
        { #if posts.length == 0}
          <h3>Loading Posts...</h3>
        { :else }
          {#each posts as post}
            <div class="col s6">
                <div class="card">
                    <div class="card-content">
                        <p class="card-title">
                            {post.title}
                        </p>
                    </div>
                    <div class="card-action">
                        <a href="#" on:click={()=> editPost(post)}>Edit</a>
                        <a href="#" class="delete-btn" on:click={()=> deletePost(post.url)}>Delete</a>
                    </div>
                </div>
            </div>
            {/each}
        {/if}
    </div>
</div>

