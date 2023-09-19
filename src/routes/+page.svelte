<script lang="ts">
	import * as mmdb from 'mmdb-lib';
	import { Buffer } from 'buffer';
	import { onMount } from 'svelte';
	import {
		IconAlertTriangleFilled,
		IconCircleCheckFilled,
		IconCloudUpload,
		IconDatabaseSearch
	} from '@tabler/icons-svelte';
	import MetadataItem from '$lib/components/MetadataItem.svelte';

	let inputFiles: FileList;
	let mmdbReader: mmdb.Reader<mmdb.Response> | null;

	let targets = '';
	let result = '';
	let fileReminder = false;
	let errorMsg = '';

	function parseFile(files: FileList) {
		result = '';
		errorMsg = '';
		mmdbReader = null;

		if (!files || files.length === 0) {
			fileReminder = true;
			return;
		}
		const reader = new FileReader();
		reader.onload = function (event: ProgressEvent<FileReader>) {
			result = '';
			const arrayBuffer = event.target!.result as ArrayBuffer;
			const buffer = Buffer.from(arrayBuffer);
			try {
				mmdbReader = new mmdb.Reader(buffer);
			} catch (e: unknown) {
				console.log(e);
				if (typeof e === 'string') {
					errorMsg = e.toUpperCase();
				} else if (e instanceof Error) {
					errorMsg = e.message;
				}
				fileReminder = true;
				return;
			}
			console.log(mmdbReader);
			fileReminder = false;
			lookup();
		};
		reader.readAsArrayBuffer(files[0]);
	}

	function lookup() {
		if (!mmdbReader) {
			fileReminder = true;
			return;
		}
		if (targets === '') {
			return;
		}
		const targetList = targets.split(',').map((v) => {
			return v.trim();
		});
		if (targetList.length < 1) {
			return;
		}

		let output = new Map<string, mmdb.Response | null>();
		targetList.forEach((target) => {
			const res = mmdbReader!.get(target);
			output.set(target, res);
		});

		result = JSON.stringify(Object.fromEntries(output), null, 4);
	}

	$: parseFile(inputFiles);

	onMount(() => {
		fileReminder = false;
	});
</script>

<div
	class="flex flex-col md:flex-row gap-7 w-full max-w-6xl pt-4 mx-auto px-4 pb-4 md:py-6 md:px-10 mt-5 rounded-md"
>
	<div class="w-full md:w-2/5">
		<form>
			<div class="flex items-center justify-center w-full">
				<label
					for="dropzone-file"
					class="flex flex-col items-center justify-center w-full h-32 border-2 border-gray-300 border-dashed rounded-lg cursor-pointer bg-gray-50 dark:hover:bg-bray-800 dark:bg-gray-700 hover:bg-gray-100 dark:border-gray-600 dark:hover:border-gray-500 dark:hover:bg-gray-600 {fileReminder
						? 'bg-red-50 border-red-500 text-red-900 placeholder-red-700 rounded-lg focus:ring-red-500  focus:border-red-500 hover:bg-red-100 dark:text-red-500 dark:placeholder-red-500 dark:border-red-800 dark:hover:bg-red-950'
						: ''}"
				>
					<div class="flex flex-col items-center justify-center pt-5 pb-6">
						<IconCloudUpload class="w-8 h-8 mb-4 text-gray-500 dark:text-gray-400" />
						<p class="mb-2 text-sm text-gray-500 dark:text-gray-400">
							<span class="font-semibold">Click to import</span> or drag and drop
						</p>
						<p class="text-xs text-gray-500 dark:text-gray-400">MMDB(GeoIP2, GeoLite2)</p>
					</div>
					<input id="dropzone-file" type="file" class="hidden" required bind:files={inputFiles} />
				</label>
			</div>
		</form>
		{#if errorMsg !== ''}
			<p class="flex items-center mt-2 text-sm text-red-600 dark:text-red-500">
				<IconAlertTriangleFilled class="w-4 mr-1" />
				<span class="font-medium">Parsing failed.</span>
				{errorMsg}
			</p>
		{/if}
		{#if mmdbReader}
			<p class="flex items-center mt-2 text-sm text-green-600 dark:text-green-500">
				<IconCircleCheckFilled class="w-4 mr-1" />
				Successfully parsed [<span class="font-medium">{inputFiles[0].name}</span>].
			</p>
		{/if}

		<p class="mt-4 font-medium text-gray-900 dark:text-gray-200">Metadata</p>
		<div class="mt-2 border-t border-gray-100">
			<dl class="divide-y divide-gray-100">
				<MetadataItem title="type" value={mmdbReader?.metadata.databaseType} />
				<MetadataItem title="major_version" value={mmdbReader?.metadata.binaryFormatMajorVersion} />
				<MetadataItem title="minor_version" value={mmdbReader?.metadata.binaryFormatMinorVersion} />
				<MetadataItem title="build_epoch" value={mmdbReader?.metadata.buildEpoch.toISOString()} />
				<MetadataItem title="ip_version" value={mmdbReader?.metadata.ipVersion} />
				<MetadataItem title="languages" value={mmdbReader?.metadata.languages.toString()} />
				<MetadataItem title="node_count" value={mmdbReader?.metadata.nodeCount} />
				<MetadataItem title="record_size" value={mmdbReader?.metadata.recordSize} />
				<MetadataItem title="search_tree_size" value={mmdbReader?.metadata.searchTreeSize} />
			</dl>
		</div>
	</div>

	<div class="flex flex-col w-full md:w-3/5">
		<div>
			<form>
				<label for="lookup" class="block mb-2 font-medium text-gray-900 dark:text-white sr-only"
					>Lookup</label
				>
				<div class="relative">
					<div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
						<IconDatabaseSearch class="w-5 text-gray-500 dark:text-gray-400" />
					</div>
					<input
						type="search"
						id="lookup"
						class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-t-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
						placeholder="1.1.1.1, 8.8.8.8, 1.2.3.4/8"
						required
						bind:value={targets}
					/>
					<button
						type="submit"
						class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
						on:click={lookup}>Lookup</button
					>
				</div>
			</form>
		</div>
		<div class="grow py-4 px-8 rounded-b-lg bg-gray-100 dark:bg-gray-800">
			<pre class="text-gray-900 dark:text-white break-words overflow-x-scroll">{result}</pre>
		</div>
	</div>
</div>
