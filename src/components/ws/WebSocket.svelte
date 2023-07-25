<script>
  import Card from "../card/Card.svelte";
  const socket = new WebSocket("ws://localhost:8090");
  // const socket = new WebSocket("ws://192.168.3.28:8090");

  let device_list = []
  let update
  let payload = []
  let formatted_data = []

  let conds = [
    {
      devices: [
        { origin: "Acácia",name: "Internet", ip: "10.0.27.1"},
        { origin: "Acácia",name: "VoIp", ip: "10.0.27.3"},
        { origin: "Acácia",name: "Cam1", ip: "10.0.27.11"},
        { origin: "Acácia",name: "Cam2", ip: "10.0.27.210"},
        { origin: "Acácia",name: "ControlId1", ip: "10.0.27.31"},
        { origin: "Acácia",name: "ControlId2", ip: "10.0.27.32"},
      ]
    },
    {
      devices: [
        { origin: "Albatroz",name: "Internet", ip: "10.0.56.1"},
        { origin: "Albatroz",name: "VoIp", ip: "10.0.56.5"},
        { origin: "Albatroz",name: "Cam1", ip: "10.0.56.11"},
        { origin: "Albatroz",name: "Cam2", ip: "10.0.56.21"},
        { origin: "Albatroz",name: "ControlId1", ip: "10.0.56.31"},
        { origin: "Albatroz",name: "ControlId2", ip: "10.0.56.32"},
      ]
    },
  ]

  let count = 0
  for(let cond of conds){
    for(let device of cond.devices){
      payload[count] = device
      count++
    }
  }

// var conds2 = []
// for (let dev of payload.devices) {
//   for (let i in conds) {
//     for (let j in conds[i].devices) {
//       if (conds[i].devices[j].ip == dev.ip) {
//         conds[i].devices[j].online = dev.online;
//         break;
//       }
//     }
//   }
// }

// conds = conds2;

    socket.onopen = (event) => {
      console.log("Conexão estabelecida.");
      socket.send(JSON.stringify({devices: payload}))
    };

    socket.onmessage = (event) => {
        const data = JSON.parse(event.data);
        let index = 0
        if("status" in data){
          update = data
        console.log(data)
        }
        else if("devices" in  data){
          device_list = data.devices
          console.log(device_list)
          for(let i = 0; i < device_list.length; i++) {
            if(i > 0){
              if(device_list[i].origin != device_list[i-1].origin){
                formatted_data[index] = device_list[i].origin
                index++
              }
            }
            else{
              formatted_data[index] = device_list[i].origin
              index++
            }
          }
          console.log(formatted_data)
        }
    };
</script>

{#each device_list as data}
  <div>
    <Card {data}/>
  </div>
{/each}