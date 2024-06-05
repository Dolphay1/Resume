<script lang="ts">
	interface DelayData {
		text: string;
		delay: number;
		split?: boolean;
	}

	import { createEventDispatcher } from 'svelte';
	let dispatch = createEventDispatcher();

	let cursorBlink = true;
	let typable = false;
	let text = '';
	let command = '';
	let textToDisplay = '';
	let textArea: Element;

	const INTRO_DATA: DelayData[] = [
		{
			text: 'ryan.thornton@desktop: ',
			delay: 0
		},
		{
			text: '',
			delay: 500,
			split: false
		},
		{
			text: 'ssh ryan.thornton@resume\n',
			delay: 50,
			split: true
		},
		{
			text: 'Warning: You are accessing the resume of Ryan Thornton.\nThis resume should only be accessed by amazing people looking to hire him.\n\n',
			delay: 5,
			split: true
		},
		{
			text: 'Password: ',
			delay: 5,
			split: true
		},
		{
			text: '',
			delay: 500,
			split: false
		},
		{
			text: '************\n',
			delay: 100,
			split: true
		},
		{
			text: 'Resume> ',
			delay: 5,
			split: true
		},
		{
			text: '',
			delay: 200,
			split: false
		},
		{
			text: 'enable\n',
			delay: 100,
			split: true
		},
		{
			text: 'Resume# ',
			delay: 5,
			split: true
		},
		{
			text: 'show resume\n',
			delay: 100,
			split: true
		}
	];

	const PROMPT: DelayData[] = [
		{
			text: 'Resume# ',
			delay: 5,
			split: true
		}
	]

	const COMMANDS: any = {
		'show resume': [
			{ text: 'Ryan Thornton Operating System Software\n', delay: 5 },
			{ text: 'Compiled Mon 03-Jun-24 11:39 by ryan.thornton\n', delay: 5 },
			{ text: 'Image text-base: 0x600108A0, data-base: 0x60910000\n', delay: 5 },
			{ text: 'ROM: System Bootstrap, Version 1.1(7769) [lake 757]\n', delay: 5 },
			{ text: 'Router uptime is 4 minutes\n', delay: 5 },
			{ text: 'System restarted by cold start\n', delay: 5 },
			{ text: "Type 'show experience' to review Experience\n", delay: 5 },
			{ text: "Type 'show certifications' to review Certifications\n", delay: 5 },
			{ text: "Type 'show education' to review Education\n", delay: 5 },
			{ text: 'Resume# ', delay: 5 }
		],
		
		'show experience': [
			{ text: 'Start Date    Company        Title                       End Date   Years\n', delay: 5 },
			{ text: 'May1/23       Leidos         Transport Specialist   current       1\n', delay: 5 },
			{ text: 'Dec13/22      SAIC           IT Service Analyst     Apr28/23     0.4\n', delay: 5 },
			{ text: 'Jul27/20      Starbucks      Barista                    Nov11/22     2\n', delay: 5 },
			{ text: 'Resume# ', delay: 5 }
		],
		
		'show certifications': [
			{ text: 'Certification     :  Cisco Certified Network Associate\n', delay: 5 },
			{ text: 'Date Aquired    :  March 2022\n', delay: 5 },
			{ text: 'Date Exipres    :  April 2027\n', delay: 5 },
			{ text: 'Description      :  Shows the holder has a basic understanding of routing and switching.\n', delay: 5 },
			{ text: '\n', delay: 5 },
			{ text: 'Certification     :  Cisco Certified Specialist - Enterprise Core\n', delay: 5 },
			{ text: 'Date Aquired    :  April 2024\n', delay: 5 },
			{ text: 'Date Exipres    :  April 2027\n', delay: 5 },
			{ text: 'Description      :  Shows the holder has a professional understanding of routing and switching.\n', delay: 5 },
			{ text: '                           First exam required to aquire CCNP.\n', delay: 5 },
			{ text: '\n', delay: 5 },
			{ text: 'Certification     :  CompTIA Security+ CE\n', delay: 5 },
			{ text: 'Date Aquired    :  May 2022\n', delay: 5 },
			{ text: 'Date Exipres    :  May 2025\n', delay: 5 },
			{ text: 'Description      :  Shows the holder has an understanding of cyberthreats and how to avoid them.\n', delay: 5 },
			{ text: '\n', delay: 5 },
			{ text: 'Resume# ', delay: 5 }
		],
		
		'show education': [
			{ text: 'Degree     :  High School Diploma\n', delay: 5 },
			{ text: 'Date Aquired    :  June 2022\n', delay: 5 },
			{ text: 'School      :  Ocean Lakes High School\n', delay: 5 },
			{ text: '\n', delay: 5 },
			{ text: 'Resume# ', delay: 5 }
		]
	};

	function displayText(displayCursor?: boolean) {
		textToDisplay = text + (displayCursor ? '▂' : '');
	}

	function addTextToDisplay(displayTextData: string) {
		text += displayTextData;
		displayText(true);
		
		dispatch('text-displayed');
	}

	function writeDelay(textToWrite: string, delay: number, callback?: CallableFunction) {
		setTimeout(() => {
			addTextToDisplay(textToWrite)

			if (callback) callback();
		}, delay);
	}

	function serializeText(textToWrite: any, callback?: CallableFunction) {
		let innerCallback: CallableFunction = () => {};

		if (callback) innerCallback = callback;

		for (let i = textToWrite.length - 1; i > -1; i--) {
			let oldFunction = innerCallback;
			let data = textToWrite[i];

			if (data.split) {
				for (let k = data.text.length - 1; k > -1; k--) {
					let oldFunction2 = innerCallback;
					innerCallback = () => writeDelay(data.text[k], data.delay, oldFunction2);
				}
			} else {
				innerCallback = () => writeDelay(data.text, data.delay, oldFunction);
			}
		}

		innerCallback();
	}

	function runCommand(commandToRun: string) {
		typable = false;
		command = "";

		if (COMMANDS[commandToRun]) {
			serializeText(COMMANDS[commandToRun], () => {
				typable = true;
			});

			
			return;
		}

		if (commandToRun.length == 0) {
			serializeText(PROMPT);
			typable = true;
			return;
		}

		let commandSuccess = [];

		for (let i = 0; i < Object.keys(COMMANDS).length; i++) {
			let testCommand = Object.keys(COMMANDS)[i];

			if (testCommand.substring(0, commandToRun.length) == commandToRun) {
				commandSuccess.push(testCommand);
			}
		}

		if (commandSuccess.length == 1) {
			serializeText(COMMANDS[commandSuccess[0]], () => {
				typable = true;
			});
		} else if (commandSuccess.length == 0) {
			addTextToDisplay('Unknown command: ' + commandToRun + "\n")
			serializeText(PROMPT);
			typable = true;
		}
		else if(commandSuccess.length > 1) {
			addTextToDisplay('Amiguous command: ' + commandToRun + "\n")
			serializeText(PROMPT);
			typable = true;
		}
	}

	function setupTerminal() {
		text += '';

		serializeText(INTRO_DATA, () => {
			runCommand('show resume');
		});
	}

	setTimeout(() => {
		setupTerminal();

		setInterval(() => {
			displayText(cursorBlink);
			cursorBlink = !cursorBlink;
		}, 700);
	}, 1500);

	function onKeyDown(e: any) {
		if (
			typable &&
			/^([A-Za-z0-9 ]|Enter|Backspace)$/.test(e.key) &&
			document.activeElement === textArea
		) {
			let textToAdd = ""
			if (e.key == 'Backspace') {
				if (command.length > 0) {
					text = text.substring(0, text.length - 1);
					command = command.substring(0, command.length - 1);
				}
			} else if (e.key == 'Enter') {
				addTextToDisplay('\n');
				runCommand(command);

				return;
			} else {
				textToAdd = e.key;
				command += e.key;
			}

			addTextToDisplay(textToAdd);
		}
	}
</script>

<!-- svelte-ignore a11y-positive-tabindex -->
<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
<div class="terminal-text" tabindex="1" bind:this={textArea}>
	<pre>{textToDisplay}</pre>
</div>

<svelte:window on:keydown={onKeyDown} />

<style>
	pre {
		background-color: transparent;
		font-size: larger;
		font-family: 'Lucida Sans Unicode';
		color: white;
		border-color: transparent;
		padding: 0px 0px 0px 0px;
		margin: 0px 0px 0px 0px;
		box-shadow: 0px 0px 0px 0px;
		
	}
	.terminal-text {
		margin-left: 10px;
		font-size: larger;
		font-family: 'Lucida Sans Unicode';
		user-select: contain;
		white-space: pre-line;
		overflow-x: hidden;
	}

	.terminal-text:focus {
		outline: none;
	}
</style>
