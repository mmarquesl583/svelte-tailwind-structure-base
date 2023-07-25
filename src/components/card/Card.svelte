<script>
    export let origin
    export let device
    export let ip
    export let on
    let card_class

    if(on){
      card_class = "bg-green-500"
    }
</script>


<div class={card_class}> {origin} - {device} - {ip}</div>
