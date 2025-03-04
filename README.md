## ECDSA Node

This project is an example of using a client and server to facilitate transfers between different addresses. Since there is just a single server on the back-end handling transfers, this is clearly very centralized. We won't worry about distributed consensus for this project.

However, something that we would like to incoporate is Public Key Cryptography. By using Elliptic Curve Digital Signatures we can make it so the server only allows transfers that have been signed for by the person who owns the associated address.

### Video instructions
For an overview of this project as well as getting started instructions, check out the following video:

https://www.loom.com/share/0d3c74890b8e44a5918c4cacb3f646c4

## Setup Instructions
 
### Client

The client folder contains a [react app](https://reactjs.org/) using [vite](https://vitejs.dev/). To get started, follow these steps:

1. Open up a terminal in the `/client` folder
2. Run `npm install` to install all the depedencies
3. Run `npm run dev` to start the application 
4. Now you should be able to visit the app at http://localhost:5173/

### Server

The server folder contains a node.js server using [express](https://expressjs.com/). To run the server, follow these steps:

1. Open a terminal within the `/server` folder 
2. Run `npm install` to install all the depedencies 
3. Run `node index` to start the server

_Hint_ - > Run `npm i -g nodemon` and then run `nodemon index` instead of `node index` to automatically restart the server on any changes!

The application should connect to the default server port (3042) automatically!

## 🏁 Your Goal: ECDSA

Only read this section **AFTER** you've followed the **Setup Instructions** above!

This project begins with a client that is allowed to transfer any funds from any account to another account. That's not very secure. By applying digital signatures we can require that only the user with the appropriate private key can create a signature that will allow them to move funds from one account to the other. Then, the server can verify the signature to move funds from one account to another.

Incorporate public key cryptography so transfers can only be completed with a valid signature
The person sending the transaction should have to verify that they own the private key corresponding to the address that is sending funds

> 🤔 While you're working through this project consider the security implications of your implementation decisions. What if someone intercepted a valid signature, would they be able to replay that transfer by sending it back to the server?

## Recommended Approach To Building This Project

There are many ways to approach this project. The goal is to create a client-server webapp that safely validates transaction intents, using public key cryptograhpy, between accounts. Below is a phased approach that clearly details out a roadmap to solving this goal:

1. **Phase 1**
- You have successfully git cloned this project onto your local machine
- You installed all dependencies by running `npm i` both in the `/client` and in the `/server` folders
- You have a website running on http://localhost:5173/ by running `npm run dev` in the `/client` folder
- You have a server process running by running `nodemon index` in the `/server` folder (remember to run `npm i -g nodemon` prior to this)
- A balance displays on the `Wallet Address` input box when you type in "0x1", "0x2" and "0x2"
- When you type in "0x1" (or any of the other accounts listed in the `server/index.js` file, you can also send an amount to any other account (using the right-hand column); this action withdraws whatever amount you send from the first account too. You should see these changes in real time, especially if you are using `nodemon` to run your server process
- Even if you reload the page on http://localhost:5173/, the balance changes you've previously made still remain - this is because it is your server actually keeping track of balances, not your client (ie. your front-end)

If all of these are complete, move on to **Phase 2**! ⬇️

2. **Phase 2**

At this point, our app security is not very good. If we deploy this app now, anyone can access any balance and make changes. This means that Alice (or really.. anyone!) can type in "0x2" and transfer an amount, even if that account is not actually her account! We need to find a way to assign ownership of accounts. 

Let's incorporate some of the cryptography we've learned in the previous lessons to build a half-baked solution; we will use [Ethereum Cryptography library](https://www.npmjs.com/package/ethereum-cryptography/v/1.2.0).

> Please use v1.2.0 of the Ethereum Cryptography library!

Start a new terminal tab and run `npm i ethereum-cryptography@1.2.0` - this will pull down functions to cryptographically sign and verify data.

> Remember, you must run `npm i ethereum-cryptography@1.2.0` in BOTH the `/client` and the `/server` folder!

In **Phase 2**, your job is to implement private keys so that when a user interacts with your application, the ONLY way they are allowed to move funds is if they provide the **private key** of the account they want to move funds from.

The key change is to change the `balances` object in the `/server/index.js` file to use **real public keys**.

You can do this programmatically (by editting the `/server/index.js` file) or using a script with the following functions:

```js
const secp = require("ethereum-cryptography/secp256k1");
const { toHex } = require("ethereum-cryptography/utils");

const privateKey = secp.utils.randomPrivateKey();

console.log('private key: ', toHex(privateKey));

const publicKey = secp.getPublicKey(privateKey);

console.log('public key', toHex(publicKey));
```

The script above will create a brand new random private key, and then get its equivalent public key, each time you run it.

Now you have the foundation to implement public key cryptography into your project!

To pass **Phase 2**:

1. You have replaced "0x1", "0x2" and "0x3" in the `server/index.js` file to actual public keys generated by using the [Ethereum Cryptograhpy library](https://www.npmjs.com/package/ethereum-cryptography/v/1.2.0).

> Extra credit: Make your accounts look like Ethereum addresses! (ie. instead of the long public key hexadecimal format, use the "0x" + 20 hex characters format of Ethereum - this is a fun challenge to get right!)

2. 









