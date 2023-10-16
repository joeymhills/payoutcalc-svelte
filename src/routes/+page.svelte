<style>
/* import fonts */
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');

@keyframes fadeIn {
  from {
    opacity: 0; /* Start with 0 opacity */
  }
  to {
    opacity: 1; /* End with full opacity */
  }
}

header {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 55px;
    width: 100%;
    background-color: #222;
    box-shadow: 0 3px 4px rgba(0, 0, 0, 0.1);
    font-weight: 300;
    font-family: roboto, sans-serif;
    color: #fff;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1;
}
/* Set a dark background color for the body */
body {
    display: flex;
    margin-top: 55px;
    flex-direction: column;
    align-items: center;
    min-height: 100vh;
    background-color:#333;
    font-family: roboto, sans-serif;
    color: #fff;
    animation: fadeIn 0.4s ease-in forwards;
}

/* Style the heading with a lighter text color */
.intro {
    margin: 20px;
    font-size: 20px;
    color: #fff;
    text-align: center;
}

/* Style the input element with a dark background and light text color */
input {
    width: 360px;
    padding: 10px;
    margin: 10px;
    border: 1px solid #555;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    font-size: 16px;
    background-color: #444;
    color: #fff;
}
button {
    width: 330px;
    padding: 10px;
    margin: 10px;
    border: 1px solid #555;
    border-radius: 5px;
    font-size: 16px;
    background-color: #444;
    color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
button:hover {
    background-color: #555;
    color: #fff;
    cursor: pointer;
}

/* Style the .payments class with dark background and light text color with a fade in animation*/
.payments {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 20px;
    padding: 10px;
    border: 1px solid #555;
    border-radius: 5px;
    background-color: #444;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    color: #fff;
    opacity: 0; /* Start with 0 opacity */
    }

/* Style individual <p> tags within the .payments class */
.payments p {
    margin: 10px;
    font-size: 18px;
    color: #fff;
}
</style>

<script lang='ts'>

import { fade } from 'svelte/transition';
type Payment = {
    payer: string,
    reciever: string,
    amount: number
}
let link: string = ""
let payments: Payment[] = undefined
let error: string = undefined

  function getInfo(link:string) {
    let payer = ""
    let reciever = ""
    let amount = 0
    fetch(`https://go-payout-calc-production.up.railway.app/GetInfo`, {
            method: "POST",
            body: link,
            mode: "cors",
            headers: {
                "Content-Type": "text/plain"
            }})
      .then(res => {
      if (!res.ok) {
        error = " Error reaching server"
        payments = undefined
      }
      return res.json()})
      .then(res => {
        console.log(res)
        payments = []
        error = undefined
        res.forEach((payment: Payment) => {
          payer = payment.payer
          reciever = payment.reciever
          amount = (payment.amount /100 ).toFixed(2)
          payments.push({ payer, reciever, amount })
          payments = payments
        }).catch(error => {console.log("error is: ", error)}) 
        }); 
  }
</script>

<head>
    <title>Payout Calculator</title>
</head>

<header>
    <h2>Pokernow.club Payout Calculator</h2>
</header>

<body>
    <p class="intro">Enter your Pokernow.club game link below</p>

    <input type="text" placeholder="Game link" bind:value={link} />
    <button on:click={getInfo(link)}>Submit</button>

    {#if payments != undefined} 
        <div class="payments">
            {#each payments as payment}
                <p>{payment.payer} pays {payment.reciever} {payment.amount} dollars</p>
            {/each}
        </div>
    {/if}

    {#if error != undefined} 
        <div class="payments">
            {error}
        </div>
    {/if}

</body>
