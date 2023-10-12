<style>
/* import fonts */
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');
body, h1, h2, p, ul, li {
    margin: 0;
    padding: 0;
}
header {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 80px;
    width: 100%;
    background-color: #333;
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
    flex-direction: column;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background-color: #333;
    font-family: roboto, sans-serif;
    color: #fff;
}

/* Style the heading with a lighter text color */
h1 {
    color: #fff;
    font-weight: normal;
    margin-bottom: 20px;
}
.intro {
    margin: 20px;
    font-size: 20px;
    color: #fff;
}

/* Style the input element with a dark background and light text color */
input {
    width: 375px;
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
    width: 375px;
    padding: 10px;
    margin: 10px;
    border: 1px solid #555;
    border-radius: 5px;
    font-size: 16px;
    background-color: #444;
    color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* Style the .payments class with dark background and light text color */
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
}

/* Style individual <p> tags within the .payments class */
.payments p {
    margin: 10px;
    font-size: 18px;
    color: #fff;
}
</style>

<script lang='ts'>
type Payment = {
    payer: string,
    reciever: string,
    amount: number
}
let link: string = ""
let payments: Payment[] = undefined

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
      .then(res => { return res.json()})
      .then(res => {
        console.log(res)
        payments = []
        res.forEach((payment: Payment) => {
          payer = payment.payer
          reciever = payment.reciever
          amount = (payment.amount /100 ).toFixed(2)
          payments.push({ payer, reciever, amount })
          payments = payments
        }).then(console.log(`payments: ${payments}`))
      }); 
  }
</script>

<head>
    <title>Payout Calculator</title>
</head>

<header>
    <h1>Payout Calculator</h1>
</header>

<body>
    <p class="intro">Enter your Pokernow.club game link below to get started</p>

    <input type="text" placeholder="Game link" bind:value={link} />
    <button on:click={getInfo(link)}>Submit</button>

    {#if payments != undefined} 
        <div class="payments">
            {#each payments as payment}
                <p>{payment.payer} pays {payment.reciever} {payment.amount} dollars</p>
            {/each}
        </div>
    {/if}
</body>

