<script lang="ts">
	import './animations.scss';

	import LinuxGuiTopBar from './LinuxGUITopBar.svelte';
	import LinuxDesktopIcon from './LinuxDesktopIcon.svelte';
	import LinuxTerminalGUI from './LinuxTerminalGUI.svelte';
	import TrashDesktopIcon from '$lib/images/flat-remix/user-trash.svg'
	import PDFFileIcon from '$lib/images/PDF_file_icon.png'
	import HomeIcon from '$lib/images/flat-remix/folder_home.svg'
	import { goto } from '$app/navigation';

	let floatTopAnimation = 'float-top-in';
	let floatLeftAnimation = 'float-left-in';
	let fadeAnimation = 'fade-in';
	let zoomAnimation = '';

	let storedPosition = {
		left: 0,
		top: 0,
		height: 0,
		width: 0
	};

	let left = 336;
	let top = 115;
	let height = 700;
	let width = 1125;

	let offsetLeft = 0;
	let offsetTop = 0;

	let fullscreen = false;
	let returnHome = false;

	function returnToMainPage() {
		floatTopAnimation = 'float-top-out';
		floatLeftAnimation = 'float-left-out';
		fadeAnimation = 'fade-out';
		toggleTerminal(true);

		returnHome = true;

		setTimeout(() => {
			goto('/');
		}, 600);
	}

	let moving = false;

	function onMouseDown(e: any) {
		moving = true;

		offsetLeft = e.detail.layerX;
		offsetTop = e.detail.layerY;
	}

	function onMouseMove(e: any) {
		if (moving) {
			if (fullscreen) {
				fullscreenToggle();

				offsetLeft = (e.detail.pageX / window.innerWidth) * width;
			}

			left = e.detail.pageX - offsetLeft;
			top = e.detail.pageY - offsetTop;

			if (top < -20) top = -20;
		}
	}

	function onMouseUp() {
		moving = false;

		if (top < 0) fullscreenToggle();
	}

	function fullscreenToggle() {
		if (fullscreen) {
			height = storedPosition.height;
			width = storedPosition.width;
			top = storedPosition.top;
			left = storedPosition.left;

			fullscreen = false;
		} else {
			storedPosition.height = height;
			storedPosition.width = width;
			storedPosition.top = top;
			storedPosition.left = left;

			left = 0;
			width = window.innerWidth;
			top = 36;
			height = window.innerHeight - 35;

			fullscreen = true;
		}
	}

	let terminal_holder: Element;

	function toggleTerminal(hide?: boolean) {
		if (returnHome) return;

		if (hide == null) {
			hide = zoomAnimation == 'zoom-out' ? true : false;
		}

		if (hide) {
			zoomAnimation = 'zoom-in';
			setTimeout(() => {
				terminal_holder.setAttribute('hidden', '');
			}, 200);
		} else {
			zoomAnimation = 'zoom-out';
			try {
				terminal_holder.removeAttribute('hidden');
			} catch (e) {}
		}
	}

	setTimeout(() => {
		toggleTerminal(false);
	}, 1500);
</script>

<svelte:head>
	<title>Ryan Thornton</title>
	<meta name="description" content="Interactive Resume" />
</svelte:head>

<section>
	<div class="background {fadeAnimation}" />
	<div class={floatTopAnimation}>
		<LinuxGuiTopBar
			on:exit={returnToMainPage}
			on:desktop-button={() => {
				toggleTerminal(true);
			}}
			on:terminal-button={() => {
				toggleTerminal();
			}}
		/>
	</div>
	<div class={floatLeftAnimation}>
		<LinuxDesktopIcon
			fileName="Trash"
			imagePath={TrashDesktopIcon}
		/>
		<LinuxDesktopIcon
			fileName="PDF Resume"
			imagePath={PDFFileIcon}
			on:dblclick={() => {
				window.open('/Ryan%20Thornton%20Resume.pdf', '_blank')?.focus();
			}}
		/>
		<LinuxDesktopIcon
			fileName="Home"
			imagePath={HomeIcon}
			on:dblclick={returnToMainPage}
		/>
	</div>
	<div
		hidden
		bind:this={terminal_holder}
		class="floating {zoomAnimation}"
		style="left:{left}px;top:{top}px;height:{height}px;width:{width}px"
	>
		<LinuxTerminalGUI
			on:maximize={fullscreenToggle}
			on:minimize={() => toggleTerminal(true)}
			on:mousedown={onMouseDown}
			on:mousemove={onMouseMove}
			on:mouseup={onMouseUp}
			on:doubleclick={fullscreenToggle}
		/>
	</div>
</section>

<style>
	section {
		min-height: 100vh;
		margin: 0;
	}

	.background {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		z-index: 0;
		width: 100%;
		height: 100%;
		z-index: -1;
		background: url('$lib/images/Logo.png') center;
		background-repeat: no-repeat;
	}

	.floating {
		position: absolute;
		width: 1000px;
		height: 700px;
		resize: both;
		overflow: hidden;
	}
</style>
