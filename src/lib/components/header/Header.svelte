<script lang="ts">
	export let disconnectWallet: any;
	import { connected, chainId, selectedAccount, defaultEvmStores } from 'svelte-web3';
	import { web3Modal } from '$lib/stores/main';
	import { browserSet } from '$lib/utils/requestUtils';
	import { errorToast } from '../toast/toastTheme';

	const connectWithWalletConnect = async () => {
		if (typeof window.ethereum === 'undefined') {
			errorToast('Please Install MetaMask');
			return;
		}
		try {
			const provider = await $web3Modal.connect();
			defaultEvmStores.setProvider(provider);
			browserSet('isWalletConnected', true);
		} catch (error) {
			errorToast('Could not Connect Wallet');
			console.log(error);
		}
	};

	// const disconnectWallet = async () => {
	// 	await defaultEvmStores.disconnect();
	// 	await $web3Modal.clearCachedProvider();
	// 	browserSet('isWalletConnected', false);
	// };

	$: $connected && console.log($selectedAccount);
</script>

<header>
	<div class="wrapper">
		<nav>
			<div class="left">
				<a href="/" class="logo"><span>D</span>auction</a>
				<div class="search-container">
					<input type="text" name="search" id="search" placeholder="Search by items or sellers" />
					<img src="/icons/search-icon.svg" alt="" />
				</div>
			</div>
			<ul>
				<li><a href="/explore">Explore</a></li>
				<li><a href="/user/biddings">My Bids</a></li>
				<li><a href="/user/create-auction">Create Auction</a></li>
				{#if !$connected || $selectedAccount === null}
					<li>
						<button
							class="btn-outline-primary connect-wallet-button"
							on:click={() => connectWithWalletConnect()}
						>
							<img src="/icons/connect-wallet-icon.svg" alt="" />
							<span>Connect Wallet</span>
						</button>
					</li>
				{:else}
					<!-- <li>
						<img style="width: 48px" src="/images/profile-images/nav-profile-pic.svg" alt="" />
					</li> -->
					<li class="account">
						<button class="btn-outline-primary wallet-connected-button" on:click={disconnectWallet}>
							<img class="nav-profile-pic" src="/icons/wallet-connected.svg" alt="" />
						</button>
						<span>{`${$selectedAccount?.slice(0, 6)}...${$selectedAccount?.slice(-4)}`}</span>
					</li>
					<!-- <li>
						<a href="#logout" >
							{`${$selectedAccount.slice(0, 6)}...${$selectedAccount.slice(-4)}`}
						</a>
					</li> -->
				{/if}
			</ul>
		</nav>
	</div>
</header>

<style>
	.account {
		display: flex;
		align-items: center;
		gap: 10px;
		color: var(--white-background);
		font-size: 24px;
		font-weight: 500;
	}
</style>
