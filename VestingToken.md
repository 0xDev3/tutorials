# Creating and Vesting an ERC-20 Token

This will be a short tutorial on how to use Dev3 to create a token and a vesting portal for an ERC-20 token.

## Scenario

Imagine we have an ERC-20 token, which vests in three separate rounds:

* Founders/advisors
* Seed Sale
* Presale

Beyond this, our token has an unvested category for _Public Sale_

The distribution of tokens will go as follows:

* Founders - 20%, 2 years of linear vesting
* Seed Sale - 30%, 1 year of linear vesting
* Presale - 30%, 6 months of linear vesting
* Public Sale - 20%, no vesting

## Implementation

To achieve this, we will only be using the Dev3 smart contract templates, although - if you're an aspiring
smart contract developer - you might want to develop your own smart contracts and _import_ them into Dev3.

We will be creating this project on the Polygon _Mumbai Testnet_ as this project is going to require paying
gas, and Mumbai has a fantastic faucet service. If you want to develop on a different chain - Dev3 supports
10+ EVM networks.

### Creating your Dev3 Workspace

In order to get started, first you will need to create a Dev3 Workspace. Go to https://app.dev3.sh and log-into
the platform. 

Then, choose _Mumbai Testnet_ from the list of available networks.
<img width="936" alt="Screenshot 2022-12-17 at 15 06 11" src="https://user-images.githubusercontent.com/42938691/208245877-f3de55b8-8f38-4e57-9c94-288f99fd0b8f.png">

Click the 'New Mumbai Testnet Workspace' button on the right. If you don't have gas, you will be presented with notes on
how to get gas from the Mumbai Faucet. This might take up to a few minutes.

Once you have enough gas and the warning messages are gone, give your workspace a name and click 'Create New Workspace'. I will call mine
'Tutorial Workspace'.

After your workspace has been created, enter the dashboard by clicking the 'Open Workspace' button next to it. Now, you should be
presented with the Dev3 Dashboard screen.

<img width="1383" alt="Screenshot 2022-12-17 at 15 10 36" src="https://user-images.githubusercontent.com/42938691/208246057-03277430-6b8a-4592-a57b-8148e6787949.png">

If you made it to here - congrats! You've sucessfully created a Dev3 Workspace.

### Planning out the smart contract architecture

The next step of our project is planning out the smart contract architecture. Per our requirements, we should have four groups of 
addresses for the different token vesting properties. 

We will achieve this by using the `PaymentSplitter.sol` smart contract combined with the `VestingWallet.sol` smart contract. Both of 
them are available in Dev3 templates. For our token, we will use `ERC20-FixedSupply.sol`, which is also available.

The smart contract architecture is laid out in the following diagram.

![Untitled Diagram drawio(7)](https://user-images.githubusercontent.com/42938691/208246318-6ce5cabe-3603-4877-ab23-14eea9609c23.png)


### Deploying and setting up our contracts

We will deploy our smart contracts from the Dev3 Dashboard and set all the balances on the contracts from there also. Let's get started.

#### Deploying the token

Initially, we need to create our token. Select the 'Deploy from Template' button in the 'Smart Contracts' screen (change screens through the
left). Type 'ERC20' in the search field and select 'Fixed Supply Token'

<img width="1088" alt="Screenshot 2022-12-17 at 15 22 18" src="https://user-images.githubusercontent.com/42938691/208246563-2a75da33-cbfa-4a0b-8744-0a4e83db6003.png">

In the following screen, fill out the required information. 'Contract Alias' is the alias through which you will be able to access your contract in the
Dashboard and the SDK - think of it as a _human readable_ replacement for the contract address. I will fill our the information the following way

* **Contract Alias** - _token-x_
* **Token Name** - _Token X_
* **Token Symbol** - _TOKX_
* **Initial Supply** - 2 million tokens. Since we're passing values in _base units_ here, we need to add zeroes
 (think of it like cents and dollars, 5 dollars would be 500 cents). However, in the Fixed Supply Token, we have 18 zeroes behind
trailing our token amount. So we will put 2000000 (2 Million) and then add 18 zeroes (here they are for you to copy - 000000000000000000). The input
will be 2000000000000000000000000
* **Supply Owner** - The account which will initially hold all the tokens. You can set it to your own account by clicking the input field and clicking
'Set my address' followed by 'Confirm' in the modal window which appears.

After you have filled out all of the fields, click 'Create Contract Deployment Request'. This should re-route you to the 'Pending Requests' sceren.

<img width="1115" alt="Screenshot 2022-12-17 at 15 31 07" src="https://user-images.githubusercontent.com/42938691/208246948-7b672e14-abad-49dd-82c1-acc24523812b.png">

Click the 'Open Link' button and the Contract Deploy screen should be visible. On that screen, click the 'Deploy' button and wait for the contract to be deployed.

<img width="497" alt="Screenshot 2022-12-17 at 15 32 24" src="https://user-images.githubusercontent.com/42938691/208247011-41dab2c2-56d6-45f0-866a-b9f9dfcd35fc.png">

You should soon see a 'Success' screen. This means your token has been sucessfully deployed!

You can go to the 'My Contracts' tab in the 'Smart Contracts' screen and verify that your token has been deployed!

<img width="1071" alt="Screenshot 2022-12-17 at 15 39 39" src="https://user-images.githubusercontent.com/42938691/208247299-2297fa61-c5cd-4a96-b791-b45ec96d4a91.png">

### Defining our Addresses

Dev3 has a very useful utility feature called 'Address Book'. We use the address book to assign human readable aliases and metadata to any blockchain
address. Access the address book screen by clicking 'Address Book' in the sidebar on the left.

Since this is a tutorial project, you probably don't have real addresses to vest the tokens to, which is fine - the token is worthless anyways, so
no funds lost at the end :) But in order to test out the app later, you will need to have at least a few addresses which are actually controlled by you.

Ideally, you can use MetaMask to do this very quickly. If you don't have it - you can install it (https://metamask.io/). Once you have the 
extension up and running, open it (top right corner of you browser) and click the circle in the top right of the extension. Then, a menu will open, and
you can click 'Create Account'. 

You can use this feature to create a number of blockchain addresses. If you're not in the mood to do that - 
here is a list of valid blockchain addresses you can use.

* 0xCddF6308143Da5FF1BF47CD5745e2D963E3a09cA
* 0x672d7Ef3aBC2Ac3048fA694b41793C735613b506
* 0x13253bb16670735C121238C60843ac8dD410d2a8
* 0xDa824E0175F384337d38Fd87C2E13FaD8078fA26
* 0xa80E052086DCB5E0688D16774965d78D391323aA
* 0xB78d59Ce728A0dD037eA0B7122D32A00F21D35a6

In the address book, click the 'New Address Book Entry' button and add the addresses with the aliases for founders, seed investors, presale investors and
public investors. One investor can be in multiple rounds - which, thankfully, saves us the trouble of generating dozens of addresses. I'll create
the following Address Book entries.

* `founder-1`: 0xCddF6308143Da5FF1BF47CD5745e2D963E3a09cA
* `founder-2`: 0x672d7Ef3aBC2Ac3048fA694b41793C735613b506
* `founder-3`: 0x13253bb16670735C121238C60843ac8dD410d2a8

* `seed-1`: 0xDa824E0175F384337d38Fd87C2E13FaD8078fA26
* `seed-2`: 0xa80E052086DCB5E0688D16774965d78D391323aA
* `seed-3`: 0xB78d59Ce728A0dD037eA0B7122D32A00F21D35a6

* `presale-1`: 0xB78d59Ce728A0dD037eA0B7122D32A00F21D35a6
* `presale-2`: 0xDa824E0175F384337d38Fd87C2E13FaD8078fA26
* `presale-3`: 0xCddF6308143Da5FF1BF47CD5745e2D963E3a09cA

* `public-1`: 0xCddF6308143Da5FF1BF47CD5745e2D963E3a09cA
* `public-2`: 0x13253bb16670735C121238C60843ac8dD410d2a8
* `public-3`: 0xB78d59Ce728A0dD037eA0B7122D32A00F21D35a6

Obviously, in a real-world scenario, you would have much more holders for each category, but for this tutorial, we're keeping things simple with just 
three holders per category.

Add all your addresses to the address book and lets continue forward.

### Deploying Payment Splitters

Next, we will be deploying the `PaymentSplitter.sol` contract(s). This contract holds tokens and distributes it to multiple people at certain proportions 
called `shares`. We will need four instances of this contract for our four groups of token holders (Founders, Seed, Presale, Public). 

Click on 'Deploy from Template' again and search for the 'Payment Splitter' contract. Click the create button and start filling out
the deployment information. Again, the `Contract Alias` field is for you to access through the SDK so make it something easy to remember. I will call
this one `splitter-founders`.

The next field is an _array_ called _Shareholders_. This is an array of addresses which will receive the tokens which are sent to this contract. Since
we are now adding the founder addresses, we will be using the address book entries with the `founder-` prefix. One great utility Dev3 has here is our
'Smart Input' selector. Simply click the input field in the shareholders array input and a modal window will pop-up with all of your addresses.

Choose `founder-1` and click the plus button on the right of the input field. Do the same for the other two founders.

Once you have added the founders, you need to add their shares. You do this in the same order as you added founders. Lets say that the founder
one has 40% and other two founders have 30% each. In the `shares` array - add the number 40, click the plus button on the right, then add the number 30 
twice. Your screen should look like this:

<img width="727" alt="Screenshot 2022-12-17 at 16 18 15" src="https://user-images.githubusercontent.com/42938691/208248955-2c5a3e55-ada6-4e96-8f4d-2bac4d74b508.png">

Click 'Create Deployment Request' and then enter the deployment screen and deploy the contract.

Repeat this process for the rest of the vesting categories (Seed, Presale and Public). Choose the ratios of the investor stakes however you like.

#### Deploying Vesting Wallets

Now, we will be deploying the four `VestingWallet.sol` smart contracts, which will serve as the basis for our token vesting efforts.

Go to the 'Deploy from Template' screen again and search for 'Vesting Wallet'. Choose 'Vesting Wallet' from the list and click create. You
should know the deployment drill for now, so we won't go over it too much. Give your contract a memorable alias - I'll call mine `vesting-founders`.

The 'Account' field is the address which will be receiving the funds from the Vesting Wallet. This should be your PaymentSplitter contract. You don't
have the address of that contract in your address book, but this is a feature that will be coming very soon to Dev3, so if you're reading this tutorial
a bit later than it was written - you might have the contract address in your address book.

If you don't have it, go to 'My Contracts' tab and copy the address of the `splitter-founders` contract to your clipboard. Paste the address into the 
'Account' field. For the start date, set the time for 18 months in the past so we can test out the claiming portal functionality.

For the Duration field, our smart input can help again. Click on the input field and the smart input will open. As per the requirements, we'll set the 
founder vesting to two years.


Once you filled it out, click Create Deployment Request and deploy the contract like the others. Repeat this process for Presale and Seed Sale holders.
Public holders don't have vesting, so you don't have to do it for them.

