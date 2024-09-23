<script lang="ts">
	import { Button } from '$lib/components/ui/button';
	import * as Card from '$lib/components/ui/card';
	import { Input } from '$lib/components/ui/input';
	import { Label } from '$lib/components/ui/label';
	import { Checkbox } from '$lib/components/ui/checkbox';
	import { writable } from 'svelte/store';

	// Use Svelte stores for reactive state management
	const formData = writable({
		title: '',
		description: '',
		url: '',
		image: '',
		siteName: '',
		twitterHandle: '',
		robotsIndex: true,
		robotsFollow: true
	});

	let showMetaTags = false;

	function generateMetaTags() {
		showMetaTags = true;
	}

	async function copyToClipboard() {
		const codeBlock = document.querySelector('#meta-tag-code');
		if (codeBlock) {
			try {
				await navigator.clipboard.writeText(codeBlock.textContent || '');
				alert('Meta tags copied to clipboard!');
			} catch (err) {
				console.error('Failed to copy: ', err);
				alert('Failed to copy to clipboard. Please try again.');
			}
		}
	}

	// Function to generate robots content
	$: robotsContent = `${$formData.robotsIndex ? 'index' : 'noindex'}, ${$formData.robotsFollow ? 'follow' : 'nofollow'}`;

	// Function to generate meta tags HTML
	function generateMetaTagsHTML(data) {
		return `
      <title>${data.title}</title>
      <meta name="description" content="${data.description}" />
      <meta name="robots" content="${robotsContent}" />

      <meta property="og:type" content="website" />
      <meta property="og:title" content="${data.title}" />
      <meta property="og:description" content="${data.description}" />
      <meta property="og:url" content="${data.url}" />
      <meta property="og:image" content="${data.image}" />
      <meta property="og:site_name" content="${data.siteName}" />

     
      <meta name="twitter:card" content="summary_large_image" />
      <meta name="twitter:title" content="${data.title}" />
      <meta name="twitter:description" content="${data.description}" />
      <meta name="twitter:url" content="${data.url}" />
      <meta name="twitter:image" content="${data.image}" />
      <meta name="twitter:site" content="${data.twitterHandle}" />
     
      <script type="application/ld+json">
      {
        "@context": "https://schema.org",
        "@type": "WebPage",
        "name": "${data.title}",
        "description": "${data.description}",
        "image": "${data.image}",
        "url": "${data.url}"
      }
      <\/script>
        `.trim();
	}
</script>

<div class="container mx-auto p-4">
	<form on:submit|preventDefault={generateMetaTags} class="space-y-4">
		{#each Object.entries($formData) as [key, value]}
			{#if key === 'robotsIndex' || key === 'robotsFollow'}
				<div class="flex items-center space-x-2">
					<Checkbox bind:checked={$formData[key]} id={key} />
					<Label for={key}>
						{key === 'robotsIndex' ? 'Allow indexing' : 'Allow following'}
					</Label>
				</div>
			{:else if typeof value === 'boolean'}
				<div class="flex items-center space-x-2">
					<Checkbox bind:checked={$formData[key]} id={key} />
					<Label for={key}>
						{key.replace(/([A-Z])/g, ' $1').replace(/^./, (str) => str.toUpperCase())}
					</Label>
				</div>
			{:else}
				<div class="space-y-2">
					<Label for={key}>
						{key.replace(/([A-Z])/g, ' $1').replace(/^./, (str) => str.toUpperCase())}
					</Label>
					<Input
						bind:value={$formData[key]}
						id={key}
						placeholder={key.replace(/([A-Z])/g, ' $1').replace(/^./, (str) => str.toUpperCase())}
					/>
				</div>
			{/if}
		{/each}
		<Button type="submit" variant="default" class="mt-4">Generate Meta Tags</Button>
	</form>

	{#if showMetaTags}
		<Card.Root class="mt-6">
			<Card.Header>
				<Card.Title>Generated Meta Tags</Card.Title>
			</Card.Header>
			<Card.Content>
				<pre id="meta-tag-code" class="overflow-auto rounded-md bg-gray-800 p-4 text-white">
          <code>{generateMetaTagsHTML($formData)}</code>
        </pre>
				<Button variant="secondary" class="mt-4" on:click={copyToClipboard}>
					Copy to Clipboard
				</Button>
			</Card.Content>
		</Card.Root>
	{/if}
</div>
