<script>
    import Abilities from "$lib/components/simulacrum/Abilities.svelte";
    import Advancements from "$lib/components/simulacrum/Advancements.svelte";
    import Meta from "$lib/components/simulacrum/Meta.svelte";
    import UpgradeMaterials from "$lib/components/simulacrum/UpgradeMaterials.svelte";
    import WeaponEffects from "$lib/components/simulacrum/WeaponEffects.svelte";
    import WeaponHeader from "$lib/components/simulacrum/WeaponHeader.svelte";
    import SectionNavigation from "$lib/components/SectionNavigation.svelte";
    import OtherInfo from "$lib/components/simulacrum/OtherInfo.svelte";
    import AwakeningGifts from "$lib/components/simulacrum/AwakeningGifts.svelte";
    import AwakeningTraits from "$lib/components/simulacrum/AwakeningTraits.svelte";
    import UnreleasedWarning from "$lib/components/UnreleasedWarning.svelte";
    import Ad from "$lib/components/Ad.svelte";
    import _ from "lodash";

    export let data;

    $: globalData = data;
    $: chinaData = _.merge(_.cloneDeep(data), data.cnData);
    $: simulacrum = globalData;

    function getAvatarImg(simulacrum) {
        switch (simulacrum.name) {
            case "Nemesis":
                return `avatar_${simulacrum.cnName}`;
            case "Frigg":
                return `Avatar12`;
            case "Pepper":
                return `touxiang_susan`;
            case "Hilda":
                return `touxiang_hilda`;
            default:
                return `touxiang_${simulacrum.cnName}`;
        }
    }

    function getElementColor(element) {
        switch (element) {
            case "volt":
                return "#b769be";
            case "ice":
                return "#49a3d1";
            case "flame":
                return "#bb4033";
            case "physical":
                return "#d88c2a";
            case "altered":
                return "#b0ffc3";
        }
    }
</script>

<svelte:head>
    <title>{globalData.name} | Tower of Fantasy Index</title>
    <meta
        name="description"
        content={`Information about the ${simulacrum.rarity} simulacrum ${simulacrum.name}; weapon advancements and abilities, simulacrum traits, and other miscellaneous information.`}
    />
    <meta property="og:title" content={globalData.name} />
    <meta
        property="og:description"
        content={`Information about the ${globalData.rarity} simulacrum ${globalData.name}; weapon advancements and abilities, simulacrum traits, and other miscellaneous information.`}
    />
    <meta
        property="og:image"
        content={`/images/Icon/Avatar/${getAvatarImg(globalData)}.webp`}
    />
    <meta
        name="theme-color"
        content={getElementColor(globalData.weapon.element)}
    />
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</svelte:head>

<SectionNavigation
    links={[
        "weapon",
        "advancements",
        "skills",
        "meta",
        "awakening",
        "other info",
    ]}
/>

<h1>{simulacrum.name}</h1>
<span style="color: var(--text2)">
    {#if simulacrum.chinaOnly}
        <abbr title="China Exclusive" />
    {/if}
    {simulacrum.rarity} Simulacrum
</span>

{#if simulacrum.unreleased}
    <UnreleasedWarning />
{/if}

<h2 id="weapon">Weapon</h2>
<WeaponHeader weapon={simulacrum.weapon} />
<WeaponEffects weapon={simulacrum.weapon} rarity={simulacrum.rarity} />

<Ad unit="Banner1" />

<Advancements weapon={simulacrum.weapon} />

<Abilities weapon={simulacrum.weapon} />

<Ad unit="Banner2" />

{#if simulacrum.rarity === "SSR"}
    <UpgradeMaterials weapon={simulacrum.weapon} />
    <Meta {simulacrum} />
    <Ad unit="Banner3" />
{/if}

<h2 id="awakening">Awakening</h2>
<AwakeningTraits traits={simulacrum.traits} />

{#if !simulacrum.unreleased}
    <AwakeningGifts
        gifts={simulacrum.bestGifts}
        categories={simulacrum.giftTypes}
    />
{/if}

<h2 id="other-info">Other Info</h2>
{#if !simulacrum.unreleased}
    <OtherInfo {simulacrum} />
{:else}
    <p>There's nothing here yet!</p>
{/if}
