<script lang="ts">
	import '../app.css';
	import { ModeWatcher } from 'mode-watcher';

	import Logo from '$lib/assets/logo.png';
	interface Props {
		children?: import('svelte').Snippet;
	}

	let { children }: Props = $props();
</script>

<title>Metapulse</title>
<link rel="icon" href={Logo} />
<meta
	name="description"
	content="Metapulse is a service that generates meta tags for your website and social networks based on basic informations. Everything is done locally." />
<meta name="robots" content="index, follow" />

<meta property="og:type" content="website" />
<meta property="og:title" content="Metapulse" />
<meta
	property="og:description"
	content="Metapulse is a service that generates meta tags for your website and social networks based on basic informations. Everything is done locally." />
<meta property="og:url" content="https://metapulse.amine-rahmane.fr" />
<meta property="og:image" content="https://metapulse.amine-rahmane.fr/metapulse.png" />
<meta property="og:site_name" content="Metapule" />

<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="Metapulse" />
<meta
	name="twitter:description"
	content="Metapulse is a service that generates meta tags for your website and social networks based on basic informations. Everything is done locally." />
<meta name="twitter:url" content="https://metapulse.amine-rahmane.fr" />
<meta name="twitter:image" content="https://metapulse.amine-rahmane.fr/metapulse.png" />
<meta name="twitter:site" content="@dasporal" />

<ModeWatcher />
{@render children?.()}
