<script>
import {Client} from "@heroiclabs/nakama-js"
import { onMount } from 'svelte';

var email = "test@toekomst.com"
var password = "3bc8f72e95a9"
var client = new Client("defaultkey", "127.0.0.1", 7350);



const create = true;

onMount(async () => {
})


var onClick = async()=> {
const session = await client.authenticateEmail(email, password, create, "mycustomusername");
console.info("Successfully authenticated:", session);



}


</script>



<form>
 <label for="email">Email</label>
<input type="text" id="email" name="email" bind:value={email}>
<label for="password">password<label>
<input type="password" id="password" name="password" bind:value={password}>
<button on:click={onClick}>login/register</button>
</form>




