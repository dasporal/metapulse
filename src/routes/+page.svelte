<script lang="ts">
	import { Button } from '$lib/components/ui/button';
	import * as Card from '$lib/components/ui/card';
	import { Input } from '$lib/components/ui/input';
	import { Label } from '$lib/components/ui/label';
	import { Checkbox } from '$lib/components/ui/checkbox';
	import { writable } from 'svelte/store';

	import { onMount } from 'svelte';
	import toast, { Toaster } from 'svelte-french-toast';
	import ModeToggler from '$lib/components/ModeToggler.svelte';

	import Logo from '$lib/assets/logo.png';

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
				toast.success('Meta tags copied to clipboard!', {
					position: 'bottom-right'
				});
			} catch (err) {
				console.error('Failed to copy: ', err);
				toast.error('Failed to copy to clipboard. Please try again.', {
					position: 'bottom-right'
				});
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

<Toaster />
<main class="container mx-auto flex flex-col gap-12 p-4">
	<div class="fixed right-4 top-4"><ModeToggler /></div>
	<div class="space-y-4">
		<div class="flex flex-row items-center justify-center gap-4">
			<img src={Logo} alt="Metapulse logo" class="h-24" />
			<h1 class="scroll-m-20 text-center text-4xl font-extrabold tracking-tight lg:text-5xl">
				Metapulse
			</h1>
		</div>
		<p class="text-center text-xl text-muted-foreground">Write once, copy everywhere.</p>
	</div>
	<div>
		<form on:submit|preventDefault={generateMetaTags} class="space-y-8">
			<div class="space-y-4">
				<h2 class="text-2xl font-bold">Basic Information</h2>
				<div class="grid grid-cols-1 gap-4 md:grid-cols-2">
					<div class="space-y-2">
						<Label for="title">Title</Label>
						<p class="text-sm text-muted-foreground">
							The main title of your page, displayed in search results and browser tabs.
						</p>
						<Input bind:value={$formData.title} id="title" placeholder="Page Title" />
					</div>
					<div class="space-y-2">
						<Label for="description">Description</Label>
						<p class="text-sm text-muted-foreground">
							A brief summary of your page content, shown in search results.
						</p>
						<Input
							bind:value={$formData.description}
							id="description"
							placeholder="Page Description" />
					</div>
				</div>
				<div class="grid grid-cols-1 gap-4 md:grid-cols-2">
					<div class="space-y-2">
						<Label for="url">URL</Label>
						<p class="text-sm text-muted-foreground">
							The full URL of your page, including the protocol (http:// or https://).
						</p>
						<Input bind:value={$formData.url} id="url" placeholder="https://example.com" />
					</div>
					<div class="space-y-2">
						<Label for="image">Image URL</Label>
						<p class="text-sm text-muted-foreground">
							The URL of the main image associated with your page, used in social media previews.
						</p>
						<Input
							bind:value={$formData.image}
							id="image"
							placeholder="https://example.com/image.jpg" />
					</div>
				</div>
			</div>

			<div class="space-y-4">
				<h2 class="text-2xl font-bold">Site Information</h2>
				<div class="grid grid-cols-1 gap-4 md:grid-cols-2">
					<div class="space-y-2">
						<Label for="siteName">Site Name</Label>
						<p class="text-sm text-muted-foreground">
							The name of your website or brand, used in social media previews.
						</p>
						<Input bind:value={$formData.siteName} id="siteName" placeholder="Your Site Name" />
					</div>
					<div class="space-y-2">
						<Label for="twitterHandle">Twitter Handle</Label>
						<p class="text-sm text-muted-foreground">
							Your Twitter username, used for Twitter Card metadata.
						</p>
						<Input
							bind:value={$formData.twitterHandle}
							id="twitterHandle"
							placeholder="@yourtwitterhandle" />
					</div>
				</div>
			</div>

			<div class="space-y-4">
				<h2 class="text-2xl font-bold">Robot Settings</h2>
				<div class="flex flex-col gap-4">
					<div class="flex items-center space-x-2">
						<Checkbox bind:checked={$formData.robotsIndex} id="robotsIndex" />
						<Label for="robotsIndex">Allow indexing and let search engines index this page</Label>
					</div>
					<div class="flex items-center space-x-2">
						<Checkbox bind:checked={$formData.robotsFollow} id="robotsFollow" />
						<Label for="robotsFollow"
							>Allow following and let search engines follow links on this page.</Label>
					</div>
				</div>
			</div>

			<Button type="submit" variant="default" class="w-full">Generate Meta Tags</Button>
		</form>

		{#if showMetaTags}
			<Card.Root class="mt-8">
				<Card.Header>
					<Card.Title>Generated Meta Tags</Card.Title>
				</Card.Header>
				<Card.Content>
					<pre id="meta-tag-code" class="overflow-auto rounded-md bg-gray-800 p-4 text-white">
						<code>{generateMetaTagsHTML($formData)}</code>
					</pre>
					<Button variant="secondary" class="mt-4 w-full" on:click={copyToClipboard}>
						Copy to Clipboard
					</Button>
				</Card.Content>
			</Card.Root>
		{/if}
	</div>
</main>
