<script>
    export let data
    let nome = data[0].origin

    let internet = []
    let voz = []
    let imagem = []
    let jfl = []
    let acesso = []

    let ips_off = []

    let card_class = "card w-auto h-full border border-black p-3"

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

    console.log(data)

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
        console.log(nome + " - Imagem")
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
        console.log(nome + " - Jfl")
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
        console.log(nome + " - Voz")
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
        console.log(nome + " - Internet")
      }
    }

    verifyInternet()

    // console.log(internet)
    // console.log(voz)
    // console.log(imagem)
    // console.log(jfl)
    // console.log(acesso)
</script>


<div class={card_class}>
  <div class="name uppercase font-bold text-2xl border-b-2">
    <i class="fa-solid fa-building mr-1"></i>
    {nome}
  </div>
  {#each ips_off as ip_off}
    <div class="text-lg font-bold">
      {ip_off}
    </div>
  {/each}
  <div class="flex w-full justify-around items-end">
    <div>
      <i class={internet_class}></i>
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

<style>
</style>