<script>

    import {goto} from '$app/navigation';
    import user from '../user';
    import Cookies from 'js-cookie';

    $: authenticated = $user !== null

    const logout = async() => {
        user.update(val => val = null);
        await Cookies.remove('jwt', { path: '', domain: 'localhost' })
        await goto('/',{noScrool: false, replaceState: true});
    }

</script>

<nav class='navbar'>

    <a class="nav_link" href="/">Home</a>
    {#if authenticated === false}
    <a class="nav_link" href="/login">Login</a>
    <a class="nav_link" href="/register">Register</a>
    {:else}
    <button on:click={logout}>Logout</button>
    {/if}

</nav>

<style>
    .navbar{
        display: flex;
        flex-direction: row;
        align-items: center;
        position: absolute;

    }
    .nav_link{
        text-decoration: none;
        font-size: 1.25em;
        color: #FC521A;
        margin: 0 1em;

    }
    .nav_link:hover{
        border-bottom: 2px solid #FC521A;
    }
</style>