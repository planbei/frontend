<script>
    import user from "../../user.js";
    import {onMount} from "svelte";
    import {goto} from "$app/navigation";
    import Login from "$lib/Login.svelte";

    $: isLoggedIn = $user !== null;

    onMount( async() => {
        if(isLoggedIn) await goto('/redirect', {noScroll: false, replaceState: true});
    })


</script>
{isLoggedIn}
<div class="content_wrapper">
    <h2>You must be logged in to consult the filming locations</h2>
    <h3>If you already have an account, you can sign up below</h3>
    <Login redirection="/locations"/>
    <h3><a href="../register">Click here if you want to create an account</a></h3>
</div>

<style>
    h1, h2, h3{
        color: #FFF;
    }
    .content_wrapper{
        margin-top: 4em;
        text-align: center;
    }
</style>