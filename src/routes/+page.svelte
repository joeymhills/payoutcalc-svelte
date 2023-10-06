<style>

  h1 {
    text-align: center;
  }
  input {
    width: 50%;
    margin: 0 auto;
    padding: 10px;
    display: block;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid black;
  }
  button {
    width: 50%;
    margin: 0 auto;
    display: block;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid black;
  }
  p {
    text-align: center;
  }
</style>
<script lang='ts'>
type Payment = {
    payer: string,
    reciever: string,
    amount: number
}
let link: string = ""
let payments: Payment[] = []

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

<h1>Welcome to the PokerNow.club Payout Calculator</h1>

<input type="text" placeholder="Game link" bind:value={link} />
<button on:click={getInfo(link)}>Submit</button>

<div>
  {#if payments != undefined}
    {#each payments as payment}
        <p>{payment.payer} pays {payment.reciever} {payment.amount} dollars</p>
    {/each}
  {/if}
</div>
