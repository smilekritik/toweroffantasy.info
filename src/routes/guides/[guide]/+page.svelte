<script>
    import SectionNavigation from "$lib/components/SectionNavigation.svelte";
    import Ad from "$lib/components/Ad.svelte";
    export let data;
</script>

<svelte:head>
    <title>{data.title} | Tower of Fantasy Index</title>
    <meta name="description" content={data.description} />
    <meta property="og:title" content={data.title} />
    <meta property="og:description" content={data.description} />
    <meta property="og:image" content={`/images/mia.webp`} />
    <meta name="theme-color" content="#377dcb" />
</svelte:head>

<SectionNavigation />

<h1>{data.title}</h1>
<strong>By {data.author}</strong>
<p>
    Last updated {new Date(data.date).toLocaleDateString("en-US", {
        month: "short",
        day: "numeric",
        year: "numeric",
    })}
</p>

<Ad unit="Article" refreshOnNav />

<article id="npArticle" class="main-layout full-bleed">
    <svelte:component class="hey" this={data.content} />
</article>