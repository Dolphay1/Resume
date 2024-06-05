<script lang="ts">
	import LinuxTerminalText from './LinuxTerminalText.svelte';
	import minimizeButton from '$lib/images/flat-remix/window-minimize.svg';
	import maximizeButton from '$lib/images/flat-remix/window-maximize.svg';

	import terminalButton from '$lib/images/flat-remix/utilities-terminal.svg';
	import { Svrollbar } from 'svrollbar';

	import { createEventDispatcher } from 'svelte';
	import { text } from '@sveltejs/kit';

	const dispatch = createEventDispatcher();

	let contents: Element;
	let viewport: Element;

	function textDisplayed() {
		viewport.scrollTop = viewport.scrollHeight;
	}
</script>

<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
<!-- svelte-ignore a11y-role-has-required-aria-props -->
<div class="gui">
	<div class="title-bar">
		<div
			class="flex-1"
			on:mousedown={(e) => dispatch('mousedown', e)}
			on:dblclick={() => dispatch('doubleclick')}
			role="heading"
			aria-label="header"
		>
			<img draggable="false" src={terminalButton} class="icon" />
		</div>
		<div
			class="title-text"
			on:mousedown={(e) => dispatch('mousedown', e)}
			on:dblclick={() => dispatch('doubleclick')}
			role="heading"
			aria-label="header"
		>
			ryan.thornton@resume: #
		</div>
		<div class="flex-1">
			<div
				class="header"
				on:mousedown={(e) => dispatch('mousedown', e)}
				on:dblclick={() => dispatch('doubleclick')}
				role="heading"
				aria-label="header"
			/>
			<button
				on:click={() => {
					dispatch('minimize');
				}}
			>
				<img src={minimizeButton} draggable="false" alt="Minimize" />
			</button>
			<button
				on:click={() => {
					dispatch('maximize');
				}}
			>
				<img src={maximizeButton} draggable="false" alt="Minimize" />
			</button>
		</div>
	</div>

	<div class="action-bar">
		<div>File</div>
		<div>Actions</div>
		<div>Edit</div>
		<div>View</div>
		<div>Help</div>
	</div>

	<hr />

	<div bind:this={viewport} class="terminal">
		<Svrollbar {viewport} {contents} alwaysVisible={true} />
		<div bind:this={contents}>
			<LinuxTerminalText on:text-displayed={textDisplayed}/>
		</div>
	</div>
</div>

<svelte:window
	on:mouseup={() => dispatch('mouseup')}
	on:mousemove={(e) => {
		dispatch('mousemove', e);
	}}
/>

<style>
	.gui button {
		height: 100%;
		margin: 0px 5px 0px 5px;
		padding: 0px 0px;
		background-color: transparent;
		border-color: transparent;
		border-width: 0px;
		float: right;
	}

	.header {
		float: left;
		width: calc(100% - 100px);
		height: 100%;
	}

	.gui button img {
		height: 100%;
		border-radius: 10px;
		transition: background-color 0.2s;
	}

	.gui button img:hover {
		-webkit-filter: brightness(150%);
		filter: brightness(150%);
		background-color: gray;
	}

	.terminal {
		color: white;
		overflow: scroll;
		height: calc(100% - 80px);
		width: 100%;

		-ms-overflow-style: none;
		scrollbar-width: none;
	}

	.gui {
		background-color: #2c2f3df5;
		border-radius: 1%;
		width: 100%;
		height: 100%;
	}

	.title-bar {
		display: flex;
		align-items: center;
		justify-items: center;
		height: 40px;
		white-space: nowrap;
		overflow: hidden;
		user-select: none;
	}

	.action-bar {
		display: flex;
		font-family: 'Lucida Sans Unicode';
		color: white;
		font-weight: bold;
		user-select: none;
	}

	.action-bar div {
		padding-left: 5px;
		padding-right: 5px;
		margin-left: 5px;
		margin-right: 5px;
		border-radius: 10%;
		transition: background-color 0.2s;
	}

	.action-bar div:hover {
		background-color: gray;
	}

	.flex-1 {
		flex: 1;
		height: 100%;
	}

	.icon {
		height: 30px;
		margin-left: 5px;
		margin-top: 5px;
	}

	.title-text {
		color: white;
		font-weight: bold;
		width: 100%;
		height: 100%;
		text-align: center;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-content: center;
		flex: 1;
	}
</style>

