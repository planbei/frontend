<script>
    import {goto} from '$app/navigation';
    import Cookies from 'js-cookie';

    let username = '';
    let password = '';
    let currentError = '';

    const login = () => {
        fetch('http://localhost:3000/users/login', {
            method: 'POST',
            credentials: 'omit',
            headers: {
                'Accept': 'application/json',
                'content-type': 'application/json',
            },
            body: JSON.stringify({
                username: username,
                password: password,
            })
        })
        .then( (res) => {
            return res.json()
        })
        .then( (data) => {
            Cookies.set('jwt',data.jwt, { expires: 0.007 });
        })
        .then( async() => {
            await goto('/', {noScroll: false, replaceState: true})
        })
        .catch( (error) => {
            console.log(error.track)
            currentError = error;
        })
    }
</script>



<form on:submit|preventDefault={login}>

    <div>
        <label for="username">Username : </label>
        <input type="text" id="username" bind:value={username}/>
    </div>

    <div>
        <label for="password">Password : </label>
        <input type="password" id="password" bind:value={password}/>
    </div>

    <button type="submit">Submit</button>

    <div>
        <small>{currentError}</small>
    </div>

</form>

<style>
    div{
        color: #FFF;
        margin-bottom: .25em;
    }
    small{
        color: #ff0000;
    }
</style>