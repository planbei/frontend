<script>
    import {onMount} from 'svelte';
    import user from '../user';
    import Cookies from 'js-cookie';
    import {goto} from "$app/navigation";

    let truc = "coucou"
    $: isLoggedIn = $user !== null;
    $: truc2 = truc

    onMount( async() => {
        const userLoggedInStatus = async() => {
            const result = await fetch('http://localhost:3000/users/me',{
                method: 'GET',
                credentials:'omit',
                headers: {
                    'Accept': 'application/json',
                    'content-type': 'application/json',
                    'Authorization': 'bearer ' + await Cookies.get("jwt")
                }
            })

            return result
        }
        try {
            const result = await userLoggedInStatus();
            const returnedData = await result.json();
            user.update(val => val = {...returnedData})
        } catch (e) {
            await Cookies.remove("jwt",  { path: '', domain: 'localhost' })
            user.update(val => val = null)
        }
    })

    const redirectToConnection = async () => {
        let href = isLoggedIn ? '/locations' : 'redirect';
        await goto(href, {noScroll: false, replaceState: true});
    } ;

</script>

<div class="content_wrapper">
    <h1>Welcome to Secure-Web-Frontend</h1>
    {#if isLoggedIn}
        <h2>Thank you {$user.username} for logging in!</h2>
    {/if}
        <h3><a href="/" on:click|preventDefault={redirectToConnection}>Click here to consult the filming locations in Paris</a></h3>
</div>

{Cookies.get('jwt')}

<style>
    h1, h2, h3{
        color: #FFF;
    }
    .content_wrapper{
        margin-top: 4em;
        text-align: center;
    }
</style>