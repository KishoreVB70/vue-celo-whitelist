# BUILDING A CELO WHITELIST DAPP USING VUE.JS
A whitelist dapp built on the celo blockchain using Vue.js for giving users early access to any services(could be early access to an NFT collection, a marketplace etc).


## Table of Content
- Introduction
- Requirement
- Prerequisites
- Tech stack
- Build and deploy the smart contract
- Building the frontend using Vue.js
- Pushing to Github
- Delpoying to vercel


## Introduction
Celo was designed to enable a new universe of financial solutions accessible for mobile users, creating a global financial ecosystem where an end-user can onboard into the Celo ecosystem with just a mobile number. It offers the following key features
- Layer-1 protocol
- EVM compatible
- Proof-of-stake
- Carbon negative
- Mobile-first identity
- Ultra-light clients
- Localized stablecoins (cUSD, cEUR, cREAL)
- Gas payable in multiple currencies
For more information, click [here](https://docs.celo.org/general) to learn more about celo


## Requirements
- Writing the smart contract and deploying it on the celo blockchain using RemixIDE. Access should be given to the first 15 users for free who want to get in.
- There should be a website where people can go and enter into the whitelist. We will use Vue.js to build it and interact with our smart contract

## Prerequisites
- You can write code in Vue.js
- Have celo extension wallet installed and set up. If not, install [CeloExtensionWallet](https://chrome.google.com/webstore/detail/celoextensionwallet/kkilomkmpmkbdnfelcpgckmpcaemjcdh?hl=en) from Google Chrome store
- Nodejs installed on your machine.
- An IDE such as Vscode or Sublime text.
- Remix IDE.
- Command line or similar software installed.

Lets start building 🚀


## Build And Deploy The Smart Contract
Now it's time to create a Solidity smart contract.
You can use any editor you like to make the contract. However, for this part of the tutorial we recommend the online IDE [RemixIDE](https://remix.ethereum.org/)
### 1.  Go to Remix
![Remix IDE](https://github.com/ozo-vehe/vue-celo-whitelist/blob/master/remixIDE.png)

### 2.  Create a new solidity file in the contract folder of remixIDE, named Whitelist.sol

### 3.  Copy and paste the follwing code into the new solidity file created

#### 3.1 Specify the solidity version and add a license 
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;
```

#### 3.2 Define the contract
```solidity
contract Whitelist {
  // This is the contract's body, here you'll specify the logic for this contract.
}
```

#### 3.3 Inside the contract create the following variables
```solidity
// Max number of whitelisted addresses allowed to join the whitelist
uint256 public maxWhitelistedAddresses = 10;

// Create a mapping of whitelistedAddresses
// if an address is whitelisted, we would set it to true, it is false by default for all other addresses.
mapping(address => bool) public whitelistedAddresses;

// numAddressesWhitelisted would be used to keep track of how many addresses have been whitelisted
uint256 public numAddressesWhitelisted;
```

#### 3.4 Next, create the addAddressToWhitelist function
```solidity
/**
addAddressToWhitelist - This function adds the address of the sender to the whitelist
*/
function addAddressToWhitelist() public {
  // check if the user has already been whitelisted
  require(!whitelistedAddresses[msg.sender], "Sender has already been whitelisted");
 
  // check if the numAddressesWhitelisted < maxWhitelistedAddresses, if not then throw an error.
  require(numAddressesWhitelisted < maxWhitelistedAddresses, "More addresses cant be added, limit reached");
 
  // Add the address which called the function to the whitelistedAddress array
  whitelistedAddresses[msg.sender] = true;
 
  // Increase the number of whitelisted addresses
  numAddressesWhitelisted += 1;
}
```

#### 3.5 The final file should look like this
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

contract Whitelist {
  // This is the contract's body, here you'll specify the logic for this contract.
  // Max number of whitelisted addresses allowed to join the whitelist
  uint256 public maxWhitelistedAddresses = 10;

  // Create a mapping of whitelistedAddresses
  // if an address is whitelisted, we would set it to true, it is false by default for all other addresses.
  mapping(address => bool) public whitelistedAddresses;

  // numAddressesWhitelisted would be used to keep track of how many addresses have been whitelisted
  uint256 public numAddressesWhitelisted;
  
  /**
  addAddressToWhitelist - This function adds the address of the sender to the whitelist
  */
  function addAddressToWhitelist() public {
    // check if the user has already been whitelisted
    require(!whitelistedAddresses[msg.sender], "Sender has already been whitelisted");
 
    // check if the numAddressesWhitelisted < maxWhitelistedAddresses, if not then throw an error.
    require(numAddressesWhitelisted < maxWhitelistedAddresses, "More addresses cant be added, limit reached");
 
    // Add the address which called the function to the whitelistedAddress array
    whitelistedAddresses[msg.sender] = true;
 
    // Increase the number of whitelisted addresses
    numAddressesWhitelisted += 1;
  }
}
```

### 4.  Deploying the smart contract on the celo blockchain
Create a Celo wallet and deploy your contract to the Celo testnet alfajores.
If you don't have celo extension wallet installed, follow the prompt below to install and set up your celo extension wallet

#### 4.1. Install the [CeloExtensionWallet](https://chrome.google.com/webstore/detail/celoextensionwallet/kkilomkmpmkbdnfelcpgckmpcaemjcdh?hl=en) from the Google Chrome Store.
![](https://raw.githubusercontent.com/dacadeorg/celo-development-101/main/content/gifs/celo_install_celo_extension_wallet.gif)

#### 4.2. Create a wallet.
![](https://raw.githubusercontent.com/dacadeorg/celo-development-101/main/content/gifs/celo_create_wallet.gif)

#### 4.3. Get Celo token for the alfajores testnet from [https://celo.org/developers/faucet](https://celo.org/developers/faucet)
![](https://raw.githubusercontent.com/dacadeorg/celo-development-101/main/content/gifs/celo_get_token_from_faucet.gif)

#### 4.4. Install the Celo remix plugin and deploy your contract.
In this tutorial, we will be deploy our smart contract, Whitelist.sol as opposed to Marketplace.sol shown in below


![](https://raw.githubusercontent.com/dacadeorg/celo-development-101/main/content/gifs/celo_install_remix_plugin_and_deploy_contract.gif)
  
Great! You deployed your first contract on the Celo blockchain. Congratulations 🎉.

### 5.  Saving the smart contract abi and address
When you compile your contract in Remix, Remix also creates the ABI in the form of a JSON for your contract. Copy the JSON and save it.

![](https://github.com/ozo-vehe/vue-celo-whitelist/blob/master/tutorial_assets/contract_abi.png)


It also creates an address of the contract which you need in order to find your contract and interact with it. Copy the address and save it.

![](https://github.com/ozo-vehe/vue-celo-whitelist/blob/master/tutorial_assets/contract_address.png)


In the next tutorial, you will learn how to create a front-end that will make use of your contract.


## Building The Frontend With Vue
To develop the website we will be using Vue, a javascript framework used to build interactive websites. It offers the following;
- Approachable: it builds on top of standard HTML, CSS and JavaScript with intuitive API and world-class documentation.
- Performant: truly reactive, compiler-optimized rendering system that rarely requires manual optimization.
- Versatile: a rich, incrementally adoptable ecosystem that scales between a library and a full-featured framework.

#### 1.  To start with, we will use vite, the official Vue project scaffolding tool. 
Make sure you have an up-to-date version of Node.js installed, then run the following command in your command line terminal

```
npm create vite@latest vue-celo-whitelist -- --template vue
```
#### 2.  Navigate to the the project.

```
cd vue-celo-whitelist
```

#### 3.  Install the required dependencies.
```
npm install
```

#### 4.  Start up a local development server.
```
npm run dev
```
The project directory should look something like this

(https://github.com/ozo-vehe/vue-celo-whitelist/blob/master/remixIDE.png)


#### 5.  In the src folder, delete the components folder(as the App.vue file will be sufficient for this tutorial) and the vue.svg file in the assets folder. Replace this file with this image [whitelist.png](https://github.com/ozo-vehe/vue-celo-whitelist/blob/master/tutorial_images/whitelist.png).

#### 6.  Create a new file called contract.js and paste in the following code

```js
export const contractAbi = YOUR_CONTRACT_ABI;
export const contractAddress = YOUR_CONTRACT_ADDRESS;
```

Replace YOUR_CONTRACT_ABI with the ABI of your Whitelist Contract.
Replace YOUR_CONTRACT_ADDRESS with the address of the whitelist contract that you deployed.

#### 6.  In the App.vue file, replace the code with the following code

```vue
<script setup>
  import { ref, reactive } from "vue";

  // DATA/VARIABLES
  const isConnected = ref(false); // variable for holding the state of a wallet, if its connected or not
  const loading = ref(false); // variable to show or hide spinner animation
  const isWhitelisted = ref(false); // checks if an address has already been whitelisted or not

</script>

<template>
  <main>
    <nav>
      <button v-if="isConnected" disabled>Wallet Connected</button>
      <button v-else>
        <span v-if="loading" class="loader"></span>
        <span v-else>Connect Wallet</span>
      </button>
      <p v-if="isConnected">0.00 <span>cUSD</span></p>
      <p v-else>0.00 <span>cUSD</span></p>
    </nav>

    <div class="whitelist">
      <div class="whitelistHeader">
        <h1>Welcome to <span>WhiteListedChain</span></h1>
        <p>
          Secure, transparent access to decentralized networks. 
          Click the button below to get early access.
        </p>
        <div>
          <p>0 have already joined the Whitelist</p>
          <p v-if="isWhitelisted" class="whitelisted">Thanks for joining the WhiteListedChain's whitelist</p>
          <button v-else>Get Early Access Pass</button>
        </div>
      </div>

      <div class="whitelistImage">
        <img src="./assets/whitelist.png" />
      </div>
    </div>
  </main>
</template>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');
  main {
    font-family: 'Montserrat';
    font-size: 16px;
    background-color: #fff;
  }
  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 5%;
  }
  button {
    display: flex;
    align-items: center;
    justify-content: center;
    border: none;
    min-width: 150px;
    height: 45px;
    overflow: hidden;
    background-color: #e096e0;
    color: #fff;
    padding: 13px 30px;
    border-radius: 8px;
    font-size: 1rem;
    cursor: pointer;
  }
  button:disabled {
    cursor: default;
    background-color: #e096e085;
  }
  button .loader {
    display: inline-block;
    border: thin solid #e096e0;
    width: 15px;
    height: 15px;
    border-radius: 50%;
    border-left: 2px solid #fff;
    animation: spinner 0.3s linear infinite;
  }
  @keyframes spinner {
    from { transform: rotate(0deg);}
    to { transform: rotate(360deg);}
  }

  .whitelist {
    display: flex;
    min-height: calc(100vh - 130px);
    justify-content: center;
    gap: 10px 30px;
    align-items: center;
    padding: 10px 5%;
  }
  .whitelist h1 {
    font-size: 4rem;
  }
  .whitelist h1 span {
    color: #e096e0;
  }

  .whitelist .whitelistHeader {
    width: 600px;
  }

  .whitelisted {
    color: #e096e0;
  }

  .whitelist .whitelistImage {
    width: 400px;
    height: 300px;
  }
  .whitelistImage img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
</style>

```

#### 7.  Go to the styles.css file in the src folder and replace the code with the following
```css
body {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}
```


Your project should look like this


![image](https://github.com/ozo-vehe/vue-celo-whitelist/blob/master/tutorial_images/image1.png)


#### 8.  We will now install a few packages needed to interact with our smart contract deployed on the celo blockchain. Run the following command on the terminal

```
npm install web3 @celo/contractkit
```
```
npm install vite-plugin-node-polyfills
```

After installing these packages, open vite.config.js file in the root directory and replace the code with the following code

```js
  import { defineConfig } from 'vite';
  import vue from '@vitejs/plugin-vue';
  import { nodePolyfills } from 'vite-plugin-node-polyfills';

  // https://vitejs.dev/config/
  export default defineConfig({
    plugins: [
      vue(),
      nodePolyfills({
        protocolImports: true,
      })
    ]
  })
  
```

#### 9. In the App.vue file, import the packages installed and add the following variables as they will be need for this tutorial

```vue
<script setup>
  import { ref, reactive } from "vue";
  
  import Web3 from "web3";
  import { newKitFromWeb3 } from "@celo/contractkit";
  import { contractAbi, contractAddress } from './contract';
  
  ...
  
  const ERC20_DECIMALS = 18;  // for balance conversion to a readable amount
  let kit = reactive(null);
  let cUSDBalance = ref(null);  // for holding the user's cUSD balance
  const numberOfWhitelistedAddresses = ref(0);  //  For storing the number of whitelisted addresses
  let contract = reactive(null);  //  to store a contract instance

</script>

```


#### 10. We will now write three functions, one to connect to the celo extension wallet, one to get the user's balance in cUSD and the last function to add a user to the whitelist.

##### 10.1. To connect to the celo extension wallet, we will create the connectWallet() function

```vue

<script setup>
  ...
 
  // Connect to the cele extension wallet
  const connectWallet = async() => {
    // Check if celo extension is installed else, user is prompted to install the extension
    if(window.celo) {
      try {
        loading.value = true;

        // Enable the celo extension to connect to an account created
        await window.celo.enable()
        
        // Using Web3 imported, create a new web3 instance passing in the window.celo as the provider
        const web3 = new Web3(window.celo)
        
        // assign the web3 instance to the variable kit using newKitFromWeb3 and passing the instance as an argument
        kit = newKitFromWeb3(web3);

        // get the connected address 
        const accounts = await kit.web3.eth.getAccounts()
        
        // assign the connected address to kit.deafulAccount
        kit.defaultAccount = accounts[0];

        // Call the getBalance function to get the users cUSD balance
        await getBalance();

        // Set isConnected to true and loading to false
        isConnected.value = true;
        loading.value = false;

        // Create a new contract instance using the contract info saved in contract.js file(contractAbi and contractAddress) and assing it to the reactive contract variable created earlier
        contract = new kit.web3.eth.Contract(contractAbi, contractAddress)

        // Get the number of whitelisted addresses and assign it to the numberOfWhiteListedAddresses ref variable
        numberOfWhitelistedAddresses.value =  await contract.methods.numAddressesWhitelisted().call();
        
        // Check if the connect account address has been whitelisted or not and assign it to the isWhitelisted ref variable
        isWhitelisted.value = await contract.methods.whitelistedAddresses(kit.defaultAccount).call();
      } catch (error) {
        console.log(error)
      }
    } else {
      alert("Celo Extension Walltet Not Installed");
    }
  }

</script>
```

#### 10.2.  To get user's balance in cUSD, we will create the getBalance() function

```vue
<script setup>
  ...
  
  // Get user balance in cUSD
  const getBalance = async() => {
    // Using the getTotalBalance function from kit, get the total balance and assign it to a totalBalance variable
    const totalBalance = await kit.getTotalBalance(kit.defaultAccount)
    
    // Using the ERC20_DECIMAL constant, convert the total balance to a readable amount set to 2 decimal places
    // assign the value to the cUSDBalance ref created earlier 
    cUSDBalance.value = totalBalance.cUSD.shiftedBy(-ERC20_DECIMALS).toFixed(2)
  }

</script>
```

#### 10.3.  To add a user's address to the whitelist, we will create the joinWhitelist() function

```vue
<script setup>

  ...
  
  // Add an account to the whitelist
  const joinWhitelist = async() => {
    // Checks if a wallet is connected, if not, alerts users to connect their wallet (the celo extension wallet)
    if(isConnected.value) {
      try {
        // Create a new contract instance with the HelloWorld contract info
        await contract.methods.addAddressToWhitelist().send({ from: kit.defaultAccount })
    
        // Get the number of whitelisted addresses and assign it to the numberOfWhitelistedAddresses ref variable
        numberOfWhitelistedAddresses.value =  await contract.methods.numAddressesWhitelisted().call();
        
        // Change the is whitlisted ref variable to true for this address
        isWhitelisted.value = true;
      } catch(error) {
        alert(error);
      }
    }
    else {
      alert("Connect Wallet (A Celo Extension Wallet)");
    }
  }
</script>
```

The final code for the App.vue file should look like this with the final changes made by replacing hard coded values with their respective

```vue
<script setup>
  import { ref, reactive } from "vue";
  import Web3 from "web3";
  import { newKitFromWeb3 } from "@celo/contractkit";
  import { contractAbi, contractAddress } from './contract';

  // DATA/VARIABLES
  const isConnected = ref(false);
  const ERC20_DECIMALS = 18;
  let kit = reactive(null);
  let cUSDBalance = ref(null);
  const loading = ref(false);
  const numberOfWhitelistedAddresses = ref(0);
  const isWhitelisted = ref(false);
  let contract = reactive(null);

  // METHODS/FUNCTIONS

  // Get user balance in cUSD
  const getBalance = async() => {
    // Using the getTotalBalance function from kit, get the total balance and assign it to a totalBalance variable
    const totalBalance = await kit.getTotalBalance(kit.defaultAccount)
    
    // Using the ERC20_DECIMAL constant, convert the total balance to a readable amount set to 2 decimal places
    // assign the value to the cUSDBalance ref created earlier 
    cUSDBalance.value = totalBalance.cUSD.shiftedBy(-ERC20_DECIMALS).toFixed(2)
  }

  // Connect to the cele extension wallet
  const connectWallet = async() => {
    // Check if celo extension is installed else, user is prompted to install the extension
    if(window.celo) {
      try {
        loading.value = true;

        // Enable the celo extension to connect to an account created
        await window.celo.enable()
        
        // Using Web3 imported, create a new web3 instance passing in the window.celo as the provider
        const web3 = new Web3(window.celo)
        
        // assign the web3 instance to the variable kit using newKitFromWeb3 and passing the instance as an argument
        kit = newKitFromWeb3(web3);

        // get the connected address 
        const accounts = await kit.web3.eth.getAccounts()
        
        // assign the connected address to kit.deafulAccount
        kit.defaultAccount = accounts[0];

        // Call the getBalance function to get the users cUSD balance
        await getBalance();

        // Set isConnected to true and loading to false
        isConnected.value = true;
        loading.value = false;

        // Create a new contract instance using the contract info saved in contract.js file(contractAbi and contractAddress) and assing it to the reactive contract variable created earlier
        contract = new kit.web3.eth.Contract(contractAbi, contractAddress)

        // Get the number of whitelisted addresses and assign it to the numberOfWhiteListedAddresses ref variable
        numberOfWhitelistedAddresses.value =  await contract.methods.numAddressesWhitelisted().call();
        
        // Check if the connect account address has been whitelisted or not and assign it to the isWhitelisted ref variable
        isWhitelisted.value = await contract.methods.whitelistedAddresses(kit.defaultAccount).call();
      } catch (error) {
        console.log(error)
      }
    } else {
      alert("Celo Extension Walltet Not Installed");
    }
  }
  
  // Add an account to the whitelist
  const joinWhitelist = async() => {
    // Checks if a wallet is connected, if not, alerts users to connect their wallet (the celo extension wallet)
    if(isConnected.value) {
      try {
        // Create a new contract instance with the HelloWorld contract info
        await contract.methods.addAddressToWhitelist().send({ from: kit.defaultAccount })
        
        // Get the number of whitelisted addresses and assign it to the numberOfWhitelistedAddresses ref variable
        numberOfWhitelistedAddresses.value =  await contract.methods.numAddressesWhitelisted().call();
        
        // Change the is whitlisted ref variable to true for this address
        isWhitelisted.value = true;
      } catch(error) {
        alert(error);
      }
    }
    else {
      alert("Connect Wallet (A Celo Extension Wallet)");
    }
  }
</script>

<template>
  <main>
    <nav>
      <button v-if="isConnected" disabled>Wallet Connected</button>
      <button v-else @click="connectWallet">
        <span v-if="loading" class="loader"></span>
        <span v-else>Connect Wallet</span>
      </button>
      <p v-if="isConnected">{{ cUSDBalance }} <span>cUSD</span></p>
      <p v-else>0.00 <span>cUSD</span></p>
    </nav>

    <div class="whitelist">
      <div class="whitelistHeader">
        <h1>Welcome to <span>WhiteListedChain</span></h1>
        <p>
          Secure, transparent access to decentralized networks. 
          Click the button below to get early access.
        </p>
        <div>
          <p>{{ numberOfWhitelistedAddresses }} have already joined the Whitelist</p>
          <p v-if="isWhitelisted" class="whitelisted">Thanks for joining the WhiteListedChain's whitelist</p>
          <button v-else @click="joinWhitelist">Get Early Access Pass</button>
        </div>
      </div>

      <div class="whitelistImage">
        <img src="./assets/whitelist.png" />
      </div>
    </div>
  </main>
</template>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');
  main {
    font-family: 'Montserrat';
    font-size: 16px;
    background-color: #fff;
  }
  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 5%;
  }
  button {
    display: flex;
    align-items: center;
    justify-content: center;
    border: none;
    min-width: 150px;
    height: 45px;
    overflow: hidden;
    background-color: #e096e0;
    color: #fff;
    padding: 13px 30px;
    border-radius: 8px;
    font-size: 1rem;
    cursor: pointer;
  }
  button:disabled {
    cursor: default;
    background-color: #e096e085;
  }
  button .loader {
    display: inline-block;
    border: thin solid #e096e0;
    width: 15px;
    height: 15px;
    border-radius: 50%;
    border-left: 2px solid #fff;
    animation: spinner 0.3s linear infinite;
  }
  @keyframes spinner {
    from { transform: rotate(0deg);}
    to { transform: rotate(360deg);}
  }

  .whitelist {
    display: flex;
    min-height: calc(100vh - 130px);
    justify-content: center;
    gap: 10px 30px;
    align-items: center;
    padding: 10px 5%;
  }
  .whitelist h1 {
    font-size: 4rem;
  }
  .whitelist h1 span {
    color: #e096e0;
  }

  .whitelist .whitelistHeader {
    width: 600px;
  }

  .whitelisted {
    color: #e096e0;
  }

  .whitelist .whitelistImage {
    width: 400px;
    height: 300px;
  }
  .whitelistImage img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
</style>

```



## Pushing Code To Github
We will show you how you can host your project on GitHub so other people can interact with it.
After testing your DApp and checking that everything behaves correctly, you can build your DApp in the command-line interface with the command.

```
npm run build
```

This will create a dist folder with all your application compressed into seperate files(html, css, javascript and assets used for your project. This folder contains the final project. Rename this folder to docs.

Upload your project to a new GitHub repository.
If needed, you can create a readme file for your project that explains your dapp and includes a link to your Dapp.


## Deploying To Vercel
We will now deploy your dApp, so that everyone can see your website and you can share it with everyone.

Follow these steps to deploy your DApp to vercel
1.  Go to Vercel and sign in with your GitHub.
2.  Then click on Add New button, select Project from the dropdown menu and then select your Whitelist DApp repo from the options giving.
3.  When configuring your new project, Vercel will allow you to customize your Root Directory
4.  Click Edit next to Root Directory and set it to dist
5.  Select the Framework as Vue.js
6.  Click Deploy

Now you can see your deployed website by going to your dashboard, selecting your project, and copying the URL beneath domains!

That’s it! Congratulations! You are done with the tutorial and have build a DApp using Vue and celo, pushed your code to Github and deployed it to vercel! 🎉


First set up your vue project(using vite or cli);
Develop the frontend without adding any functions for interactivity, variables initialized with dummy data and styles added for beautification
Go to the styles.css folder, delete all the styles defined there and add the following lines of css code. This is to prevent style errors from clashing css style rules

Install packages
  1.  celo contrack kit
  2.  web3 from web3.js using nmp install web3
  3.  install vite-plugin-node-polyfills, this is to prevent polyfills errors that usually arises
Import installed packages
Open vite.config.js file or vue.config.js file(if vue cli is used to set up the project)
paste the following code snippet
  import { defineConfig } from 'vite';
  import vue from '@vitejs/plugin-vue';
  import { nodePolyfills } from 'vite-plugin-node-polyfills';

  // https://vitejs.dev/config/
  export default defineConfig({
    plugins: [
      vue(),
      nodePolyfills({
        protocolImports: true,
      })
    ]
  })

Writing the functions
  1.  Connect wallet function (here the only function to be implemented is the connecet to wallet function first)
  2.  Create a contract instance using the contract details(that is, the contractAbi and the contractAddress) stored in contract.js file
  3.  Get balance function, call this function in the connectWallet function to get the users balance immediately
  4.  Join the whitelist function
  5.  Connect all the functions that is, add check in the connectWallet function to check if an address has already been whitelisted
