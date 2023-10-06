
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
