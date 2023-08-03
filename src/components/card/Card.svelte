<!-- <script>
    export let data
    let nome = data[0].origin

    let internet = []
    let voz = []
    let imagem = []
    let jfl = []
    let acesso = []

    let ips_off = []

    let card_class = "card w-auto border border-black p-3 relative"

    let internet_class = "text-4xl fa-solid fa-wifi"
    let voz_class = "text-4xl fa-solid fa-phone-volume"
    let imagem_class = "text-4xl fa-solid fa-camera"
    let jfl_class = "text-4xl fa-solid fa-keyboard"
    let acesso_class = "text-4xl fa-solid fa-door-open"

    for(let i = 0; i < data.length; i++){
      let name = data[i].name
      let ip = data[i].ip
      let status = data[i].online
      let type

      let range = ip.split(".")[3]

      if(range == 1){
        type = 0 //internet
        internet.push({"device":name, "ip": ip, "status": status,"type": type})
      }
      else if(range < 11){
        type = 1 //voz
        voz.push({"device":name, "ip": ip, "status": status,"type": type})
      }
      else if(range < 21){
        type = 2 //imagem
        imagem.push({"device":name, "ip": ip, "status": status,"type": type})
      }
      else if(range < 31){
        type = 3 //jfl
        jfl.push({"device":name, "ip": ip, "status": status,"type": type})
      }
      else{
        type = 4 //controles de acesso
        acesso.push({"device":name, "ip": ip, "status": status,"type": type})
      }
    }

    // console.log(data)

    function verifyImage(){
      let on = true
      for(let i = 0; i < imagem.length; i++){
        if(!imagem[i].status){
          ips_off.push(imagem[i].ip)
          on = false
          break
        }
      }
      if(on){
        card_class += " bg-green-500 bg-opacity-30"
        imagem_class += " opacity-5"
      }
      else{
        card_class += " bg-red-300 bg-opacity-80"
        imagem_class += " text-red-700"
        on = true
        // console.log(nome + " - Imagem")
      }
    }

    function verifyAcess(){
      let on = true
      for(let i = 0; i < acesso.length; i++){
        if(!acesso[i].status){
          ips_off.push(acesso[i].ip)
          on = false
          break
        }
      }
      if(on){
        card_class += " bg-green-500 bg-opacity-30"
        acesso_class += " opacity-5"
      }
      else{
        card_class += " bg-red-300 bg-opacity-80"
        acesso_class += " text-red-700"
        on = true
        console.log(nome + " - Acesso")
      }
    }

    function verifyJfl(){
      let on = true
      for(let i = 0; i < jfl.length; i++){
        if(!jfl[i].status){
          ips_off.push(jfl[i].ip)
          on = false
          break
        }
      }
      if(on){
        card_class += " bg-green-500 bg-opacity-30"
        jfl_class += " opacity-5"
      }
      else{
        card_class += " bg-red-300 bg-opacity-80"
        jfl_class += " text-red-700"
        on = true
        // console.log(nome + " - Jfl")
      }
    }

    function verifyVoice(){
      let on = true
      for(let i = 0; i < voz.length; i++){
        if(!voz[i].status){
          ips_off.push(voz[i].ip)
          on = false
          break
        }
      }
      if(on){
        card_class += " bg-green-500 bg-opacity-30"
        voz_class += " opacity-5"
      }
      else{
        card_class += " bg-red-300 bg-opacity-80"
        voz_class += " text-red-700"
        on = true
        // console.log(nome + " - Voz")
      }
    }

    function verifyInternet(){
      let on = true
      for(let i = 0; i < internet.length; i++){
        if(!internet[i].status){
          ips_off.push(internet[i].ip)
          on = false
          break
        }
      }
      if(on){
        card_class += " bg-green-500 bg-opacity-30"
        internet_class += " opacity-5"
        verifyVoice()
        verifyImage()
        verifyJfl()
        verifyAcess()
      }
      else{
        card_class += " bg-red-300 bg-opacity-80"
        internet_class += " text-red-700"
        on = true
        voz_class += " opacity-5"
        imagem_class += " opacity-5"
        jfl_class += " opacity-5"
        acesso_class += " opacity-5"
        // console.log(nome + " - Internet")
      }
    }

    verifyInternet()
</script>


<div class={card_class}>
  <div class="name uppercase font-bold text-2xl border-b-2 items-start ml-2">
    <i class="fa-solid fa-building mr-1"></i>
    {nome}
  </div>
  <div class="items-end w-full h-auto">
    <div class="text-lg h-auto font-bold ml-2 mt-2 flex ">
      {#if ips_off.length === 1}
        <span>{ips_off[0]}</span>
      {:else}
        {#each ips_off as ip_off}
          {#if ip_off != ips_off[0]}
            <span> - </span>
          {/if}
          <span>{ip_off}</span>
        {/each}
      {/if}
    </div>
    <div class="flex w-full justify-around">
      <div class="relative">
        <i class={internet_class}></i>
        <!-- <span class="bg-red-500 text-white rounded-full absolute font-mono text-xs px-1 bottom-0 -ml-1 ">
          3
        </span>
      </div>
      <div>
        <i class={voz_class}></i>
      </div>
      <div>
        <i class={imagem_class}></i>
      </div>
      <div>
        <i class={jfl_class}></i>
      </div>
      <div>
        <i class={acesso_class}></i>
      </div>
    </div>
  </div>
</div> -->


<script>
  import { afterUpdate } from "svelte";

  export let data;

  let color;
  let ips_off = [];

  let types = {
    internet: {
      class:"text-4xl fa-solid fa-wifi",
      online: true,
      counter: 0
    },
    imagem: {
      class:"text-4xl fa-solid fa-camera",
      online: true,
      counter: 0
    },
    voz: {
      class:"text-4xl fa-solid fa-phone-volume",
      online: true,
      counter: 0
    },
    jfl: {
      class:"text-4xl fa-solid fa-keyboard",
      online: true,
      counter: 0
    },
    acesso: {
      class:"text-4xl fa-solid fa-door-open",
      online: true,
      counter: 0
    },
  }

  let icons = {

  }

  function getType(type){
    const get = types[type]
    get.counter++
    console.log(types)
    return get
  }

  function main() {
    let trigger = false
    let type

    for(let i = 0; i < data.devices.length; i++){
      if(data.devices[i].online == false){
        const name = data.devices[i].type
        type = getType(data.devices[i].type)
        ips_off.push(data.devices[i].ip)
        trigger = true
      }
    }
    color = trigger? "red" :"green"

    for (let t in types) {
      var found = false
      var internet = false
      for(let i = 0; i < data.devices.length; i++) {
        if(data.devices[i].type == "internet" && !data.devices[i].online){
          console.log(data.devices[i])
          internet = true
          types["internet"].online = false
        }
        else if (data.devices[i].type == t && !data.devices[i].online) {
          types[data.devices[i].type].online = false
          found = true
          break
        }
      }
      if (internet) break
      if (!found) types[t].online = true
    }
  }

  afterUpdate(()=>{
    main()
  })
</script>

<div class="card w-auto border border-black p-3 relative bg-{color}-500 opacity-70 ">
  <div class="name uppercase font-bold text-2xl border-b-2 items-start ml-2">
    <i class="fa-solid fa-building mr-1"></i>
    {data.name}
  </div>
  <div class="items-end w-full h-auto">
    <div class="text-lg h-auto font-bold ml-2 mt-2 flex ">
      {#if ips_off.length === 1}
        <span>{ips_off[0]}</span>
      {:else}
        {#each ips_off as ip_off}
          {#if ip_off != ips_off[0]}
            <span> - </span>
          {/if}
          <span>{ip_off}</span>
        {/each}
      {/if}
    </div>
    <div class="flex w-full justify-around">
      {#each Object.entries(types) as [name, content]}
        <div class="relative alpha-{content.online ? "1" : "2"}">
          <i class={content.class}></i>
        </div>
      {/each}
    </div>
  </div>
</div>
