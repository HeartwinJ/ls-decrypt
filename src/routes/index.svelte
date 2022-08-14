<script>
	import { onMount } from 'svelte';
	import CryptoJS from 'crypto-js';

	/** @type {String}*/
	let storageSecret;
	/** @type {String}*/
	let data;
	/** @type {String}*/
	let value;

	$: {
		if (storageSecret && data) {
			try {
				value = CryptoJS.RC4.decrypt(data, storageSecret).toString(CryptoJS.enc.Utf8);
			} catch (e) {}
		} else {
			value = '';
		}
	}

	function copyResult() {
		navigator.clipboard.writeText(value);
	}

	onMount(async () => {
		console.info(`🤖 LS Decrypt v${APP_VERSION}`);
	});
</script>

<div class="h-full w-full flex flex-col p-8 gap-8 text-neutral-100">
	<h1 class="text-5xl text-center md:text-left">
		<span class="font-bold">LS</span>
		<span class="font-thin">DECRYPT</span>
	</h1>
	<div class="w-full">
		<div class="mb-2">Storage Secret</div>
		<input
			type="text"
			class="bg-neutral-800 border border-neutral-700 rounded-lg py-2 px-4 w-full outline-none"
			bind:value={storageSecret}
		/>
	</div>
	<div class="grow flex flex-col md:flex-row gap-8">
		<div class="grow flex flex-col">
			<div class="mb-2">Encrypted Data</div>
			<textarea
				class="grow bg-neutral-800 border border-neutral-700 rounded-lg py-2 px-4 w-full resize-none outline-none"
				bind:value={data}
			/>
		</div>
		<div class="grow flex flex-col">
			<div class="mb-2 flex items-center justify-between">
				<span>Decrypted Value</span>
				<button
					class="text-neutral-400 bg-neutral-800 text-xs px-3 py-1 rounded-lg hover:text-neutral-300"
					on:click|preventDefault={copyResult}
				>
					&#10064; &nbsp; Copy
				</button>
			</div>
			<textarea
				class="grow bg-neutral-800 border border-neutral-700 rounded-lg py-2 px-4 w-full resize-none outline-none"
				bind:value
				readonly
			/>
		</div>
	</div>
</div>
