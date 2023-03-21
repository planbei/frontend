<script>
    import user from "../../user";
    import {onMount} from 'svelte';
    import Cookies from "js-cookie";


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
            user.update(val => val = null)
        }
    })
</script>


{#if $user?.role === 'admin'}
    <h1>Welcome {$user?.username}</h1>
{:else if $user?.role === undefined}
    <h1>You are not even logged in please leave</h1>
    <a href="/">Go back to safety</a>
{:else if $user?.role !== 'admin'}
    <h1>You are not an admin</h1>

{/if}