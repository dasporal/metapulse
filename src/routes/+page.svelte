<script lang="ts">
	import { Button } from '$lib/components/ui/button';
	import * as Card from '$lib/components/ui/card';
	import { Input } from '$lib/components/ui/input';
	import { Label } from '$lib/components/ui/label';
	let title = '';
	let description = '';
	let url = '';
	let image = '';
	let siteName = '';
	let twitterHandle = '';

	let showMetaTags = false;

	function generateMetaTags() {
		showMetaTags = true;
	}

	function copyToClipboard() {
		const codeBlock = document.querySelector('#meta-tag-code') as HTMLPreElement;
		if (!codeBlock) return;

		navigator.clipboard.writeText(codeBlock.innerText).then(() => {
			alert('Meta tags copied to clipboard!');
		});
	}
</script>

<div class="container mx-auto p-4">
	<!-- Meta Tag Generation Form -->
	<form on:submit|preventDefault={generateMetaTags} class="space-y-4">
		<div class="space-y-2">
			<Label for="title">Title</Label>
			<Input bind:value={title} id="title" placeholder="Title" />
		</div>

		<div class="space-y-2">
			<Label for="description">Description</Label>
			<Input bind:value={description} id="description" placeholder="Description" />
		</div>

		<div class="space-y-2">
			<Label for="url">URL</Label>
			<Input bind:value={url} id="url" placeholder="URL" />
		</div>

		<div class="space-y-2">
			<Label for="image">Image URL</Label>
			<Input bind:value={image} id="image" placeholder="Image URL" />
		</div>

		<div class="space-y-2">
			<Label for="siteName">Site Name</Label>
			<Input bind:value={siteName} id="siteName" placeholder="Site Name" />
		</div>

		<div class="space-y-2">
			<Label for="twitterHandle">Twitter Handle</Label>
			<Input bind:value={twitterHandle} id="twitterHandle" placeholder="Twitter Handle" />
		</div>

		<Button type="submit" variant="default" class="mt-4">Generate Meta Tags</Button>
	</form>

	{#if showMetaTags}
		<Card.Root class="mt-6">
			<Card.Header>
				<Card.Title>Generated Meta Tags</Card.Title>
			</Card.Header>
			<Card.Content>
				<pre id="meta-tag-code" class="overflow-auto rounded-md bg-gray-800 p-4 text-white">
          <code>
          &lt;title&gt;{title}&lt;/title&gt;
          &lt;meta name="description" content="{description}" /&gt;
          &lt;meta property="og:title" content="{title}" /&gt;
          &lt;meta property="og:description" content="{description}" /&gt;
          &lt;meta property="og:url" content="{url}" /&gt;
          &lt;meta property="og:image" content="{image}" /&gt;
          &lt;meta property="og:site_name" content="{siteName}" /&gt;
          &lt;meta name="twitter:card" content="summary_large_image" /&gt;
          &lt;meta name="twitter:title" content="{title}" /&gt;
          &lt;meta name="twitter:description" content="{description}" /&gt;
          &lt;meta name="twitter:url" content="{url}" /&gt;
          &lt;meta name="twitter:image" content="{image}" /&gt;
          &lt;meta name="twitter:site" content="{twitterHandle}" /&gt;
          </code>
        </pre>
				<Button variant="secondary" class="mt-4" on:click={copyToClipboard}
					>Copy to Clipboard</Button
				>
			</Card.Content>
		</Card.Root>
	{/if}
</div>
