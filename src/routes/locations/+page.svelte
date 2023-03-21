<script>
    import user from "../../user.js";
    import {onMount} from "svelte";
    import {goto} from "$app/navigation";
    import Cookies from "js-cookie";

    let locations = [];

    let filmName = "" ; let filmDirectorName = ""; let filmProducerName = ""; let filmType = "" ; let address = "" ; let year = "" ;

    let hideTable = false;

    const invertVisibility = () => {
        const myDiv = document.getElementById("table");
        myDiv.hidden = !myDiv.hidden;
    }
    const modify = (id) => {
        const loadData = async() => {
            const data = await fetch('http://localhost:3000/locations/'+id,{
                method: 'GET',
                credentials:'omit',
                headers: {
                    'Accept': 'application/json',
                    'content-type': 'application/json',
                    'Authorization': 'bearer ' + await Cookies.get("jwt")
                }
            })
            let result = await data.json()
            return result;
        }

        let data = loadData()
        invertVisibility()
        filmName = data.filmName ; filmDirectorName = data.filmDirectorName ; filmProducerName = data.filmProducerName ; filmType = data.filmType ; address = data.address ; year = data.year ;

    }

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

        if(user === null) {
            await goto('/register', {noScroll: false, replaceState: true});
            return;
        }

        const loadData = async() => {
            const data = await fetch('http://localhost:3000/locations/',{
                method: 'GET',
                credentials:'omit',
                headers: {
                    'Accept': 'application/json',
                    'content-type': 'application/json',
                    'Authorization': 'bearer ' + await Cookies.get("jwt")
                }
            })
            let result = await data.json()
            return result;
        }

        locations = await loadData();

    })

</script>

<div class="content_wrapper">
    <h1>Filming locations in Paris :</h1>

    <br/>{$user?.role}

    <table id="table">
        <thead>
        <tr>
            <th scope="col">Film name</th>
            <th scope="col">Director</th>
            <th scope="col">Producer</th>
            <th scope="col">Film Type</th>
            <th scope="col">Address</th>
            <th scope="col">Year</th>
            <th scope="col">Modify</th>
            <th scope="col">Delete</th>
        </tr>

        </thead>
        <tbody>

        {#each locations as loc}
            <tr>
                <th scope="row">{loc.filmName}</th>
                <th scope="row">{loc.filmDirectorName}</th>
                <th scope="row">{loc.filmProducerName}</th>
                <th scope="row">{loc.filmType}</th>
                <th scope="row">{loc.address}</th>
                <th scope="row">{loc.year}</th>
                <td>
                    <button type="button" on:click={ e => { modify(loc._id) }}> Modify</button>
                </td>
                <td>
                    <button type="button" on:click={ e => { delete(loc._id) }}> Delete</button>
                </td>
            </tr>
        {/each}


        </tbody>

    </table>


</div>

<style>
    h1 {
        color: #FFF;
    }

    table,
    td {
        border: 1px solid #333;
    }

    thead {
        background-color: #333;
        color: #fff;
    }

    tbody {
        background-color: #333;
        color: #C0C0C0;
    }

    .content_wrapper{
        margin-top: 2em;
        text-align: center;
    }

</style>