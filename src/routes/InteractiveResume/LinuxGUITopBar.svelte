<script lang="ts">
	import logoButton from '$lib/images/LogoButton.png';
	import desktopButton from '$lib/images/flat-remix/folder-desktop.svg';
	import gitFolderButton from '$lib/images/flat-remix/folder-git.svg';
	import chromeButton from '$lib/images/flat-remix/google-chrome.svg';
	import terminalButton from '$lib/images/flat-remix/utilities-terminal.svg';
	import shutoffButton from '$lib/images/flat-remix/system-shutdown-symbolic.svg';

	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function openInNewTab(url: string) {
		window.open(url, '_blank')?.focus();
	}
</script>

<div class="top-bar">
	<div class="top-bar-category">
		<button> <img alt="Home Button" src={logoButton} /> </button>
		<div class="vl" />
		<button
			on:click={() => {
				dispatch('desktop-button');
			}}
		>
			<img alt="Desktop" src={desktopButton} />
		</button>
		<button on:click={() => openInNewTab('https://github.com/Dolphay1/')}>
			<img alt="Git Explorer" src={gitFolderButton} />
		</button>
		<button on:click={() => openInNewTab('https://google.com')}>
			<img alt="Google Chrome" src={chromeButton} />
		</button>
		<button on:click={() => dispatch('terminal-button')}>
			<img alt="Terminal" src={terminalButton} />
		</button>
		<div class="vl" />
	</div>
	<div class="top-bar-category align-right">
		<button on:click={() => dispatch('exit')}>
			<img alt="Terminal" src={shutoffButton} />
		</button>
	</div>
</div>

<style>
	div.top-bar {
		height: 35px;
		background-color: #23242c4d;
		display: flexbox;
		box-shadow: 0px 0px 20px 0px #23242c;

		-webkit-user-select: none;
		-khtml-user-select: none;
		-moz-user-select: none;
		-o-user-select: none;
		user-select: none;
	}

	div.top-bar-category {
		height: 100%;
		width: fit-content;
		display: inline-flex;
		align-items: center;
	}

	div.align-right {
		float: right;
	}

	div.top-bar img {
		padding: 0%;
		height: 100%;
		transition:
			-webkit-filter 0.3s,
			filter 0.3s;
		background-color: transparent;
		border-color: transparent;
	}

	div.top-bar button {
		height: 100%;
		margin: 0px 5px 0px 5px;
		padding: 0px 0px;
		background-color: transparent;
		border-color: transparent;
		border-width: 0px;
	}

	div.top-bar img:hover {
		-webkit-filter: brightness(150%);
		filter: brightness(150%);
	}

	div.vl {
		border-left: 2px solid #8d8f91;
		height: 85%;
		display: inline-block;
		vertical-align: center;
		margin-right: 3px;
		margin-left: 3px;
	}
</style>
