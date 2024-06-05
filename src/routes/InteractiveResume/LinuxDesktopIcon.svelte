<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	export let imagePath: string;
	export let fileName: string;

	let element: HTMLElement;

	const dispatch = createEventDispatcher();

	function onClick() {
		element.focus();
	}
</script>

<div
	role="none"
	tabindex="-1"
	class="desktop-icon"
	bind:this={element}
	on:click={() => {
		onClick();
		dispatch('click');
	}}
	on:dblclick={() => {
		onClick();
		dispatch('dblclick');
	}}
>
	<img draggable="false" class="file-image" alt="Desktop Icon" src={imagePath} />
	<div class="file-name">{fileName}</div>
</div>

<style>
	div.desktop-icon {
		width: 100px;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		margin-left: 50px;
		margin-top: 50px;

		-webkit-user-select: none;
		-khtml-user-select: none;
		-moz-user-select: none;
		-o-user-select: none;
		user-select: none;
	}

	div.desktop-icon:hover {
		background-color: hsla(182, 100%, 92%, 0.452);
	}

	div.desktop-icon:focus {
		background-color: hsla(181, 100%, 92%, 0.616);
	}

	img.file-image {
		height: 75px;
	}
	div.file-name {
		color: white;
		text-align: center;
		width: 100%;
		font-family: 'Lucida Sans Unicode';
	}
</style>
