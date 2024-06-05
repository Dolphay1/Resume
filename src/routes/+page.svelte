<script lang="ts">
	import { goto } from '$app/navigation';

	let fadeClass = 'fade-in';

	function onClickButton(link: string) {
		fadeClass = 'fade-out';
		setTimeout(() => {
			goto(link);
		}, 600);
	}

	function downloadPDFResume() {}
</script>

<svelte:head>
	<title>Ryan Thornton</title>
	<meta name="description" content="Ryan Thornton's Resume Website" />
</svelte:head>

<section>
	<h1 class="screen-title {fadeClass}">Ryan Thornton</h1>
	<div class="button-holder">
		<button
			on:click={() => onClickButton('/InteractiveResume')}
			class="button large-button {fadeClass}">Interactive Resume</button
		>
		<!-- <div class="margin-big">
			<button on:click={() => onClickButton('/WebResume')} class="button small-button {fadeClass}"
				>Web Resume</button
			>
			<button on:click={downloadPDFResume} class="button small-button {fadeClass}"
				>PDF Resume</button
			>
		</div> -->
	</div>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	@property --color-bg-0 {
		syntax: '<color>';
		inherits: false;
		initial-value: rgb(75, 86, 95);
	}

	@property --color-bg-1 {
		syntax: '<color>';
		inherits: false;
		initial-value: hsl(200, 4%, 17%);
	}

	@property --color-bg-2 {
		syntax: '<color>';
		inherits: false;
		initial-value: hsl(0, 0%, 0%);
	}

	@keyframes bg-change {
		from {
			--color-bg-0: black;
			--color-bg-1: black;
			--color-bg-2: black;
		}
		to {
		}
	}

	.fade-in {
		animation-name: fade-in;
		animation-duration: 5s;
		animation-fill-mode: forwards;
		animation-timing-function: ease;
	}

	.fade-out {
		animation-name: fade-out;
		animation-duration: 0.5s;
		animation-fill-mode: forwards;
		animation-timing-function: ease;
	}

	div.margin-big {
		margin-top: 10px;
	}

	div.button-holder {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	button {
		border: 1px outset;
		border-radius: 3px;
		text-decoration: none;
		transition:
			background-color 0.5s,
			color 0.5s;
		color: red;
		border-color: red;
		cursor: pointer;
		background-color: transparent;
	}

	button:hover {
		color: white;
		background-color: red;
	}

	button.small-button {
		padding: 25px 85px;
		display: inline-block;
	}

	button.large-button {
		padding: 30px 200px;
	}

	.screen-title {
		font-size: 150px;
		background: linear-gradient(180deg, rgb(65, 255, 255) 30%, rgb(51, 51, 51) 100%);
		-webkit-background-clip: text;
		background-clip: text;
		-webkit-text-fill-color: transparent;
		font-family: Cambria;
		user-select: none;
	}
	@supports not (-moz-animation:alternate) {
		:global(body) {
			animation-name: bg-change;
			animation-duration: 6s;
			animation-fill-mode: forwards;
			animation-timing-function: ease;
			background-image: radial-gradient(
					50% 50% at 50% 50%,
					rgba(14, 12, 12, 0.75) 0%,
					rgba(255, 255, 255, 0) 100%
				),
				linear-gradient(180deg, var(--color-bg-0) 0%, var(--color-bg-1) 15%, var(--color-bg-2) 50%);
		}
	}
</style>
