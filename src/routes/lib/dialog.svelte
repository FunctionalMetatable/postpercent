<script>
    import { highlight, dialogActive, name, activeTheme, themes, dropdownActive } from '../storage.js';
    import { onMount } from 'svelte';
    import { HEXtoRGB } from './hextorgb.js';
    import Dropdown from './dropdown.svelte';

    function toggleDialog() {$dialogActive ? dialogActive.set(false) : dialogActive.set(true)}
    function toggleName() {$name == "postpercent" ? name.set("shefwerld") : name.set("postpercent")}
    function toggleDropdown() {$dropdownActive ? dropdownActive.set(false) : dropdownActive.set(true)}

    let logoColor = "var(--accent)";
    let lightText = 0;
    $: if (logoColor != $highlight)   {
        logoColor = $highlight
        if (logoColor.length == 7)  {
            const contraster = [];
            contraster.push(HEXtoRGB(logoColor).r); contraster.push(HEXtoRGB(logoColor).g); contraster.push(HEXtoRGB(logoColor).b);
            const brightness = Math.round(((parseInt(contraster[0]) * 299) + (parseInt(contraster[1]) * 587) + (parseInt(contraster[2]) * 114)) / 1000);
            brightness > 150 ? lightText = 1 : lightText = 0;
        }
    }
    onMount(async() => {
        logoColor = "var(--accent)";
        lightText = 0;
    })
    $: hoverColor = `--hoverColor:${$highlight};`
</script>

<div id="overlay" style={$dialogActive ? "display: flex;" : "display: none;"}>
</div>
<ul class="dialog" style={$dialogActive ? `display: flex;` : `display: none;`}>
    <ul class="dialog-top" style="background-color: {logoColor}; color: {lightText == 1 ? `var(--background)`: `var(--text)`}; transition: background-color 1s;">
        <li>postpercent settings</li>
        <li class="button" on:click={toggleDialog} style={hoverColor}>Close</li>
    </ul>
    <ul>
        <li>Change Theme</li>
        <li class="button" on:click={toggleDropdown} style={hoverColor}>{$activeTheme}</li>
        <Dropdown type="theme" options={themes}></Dropdown>
    </ul>
    <ul>
        <li style="font-family: Comic Sans MS">Shefwerld is a better name</li>
        <li class="button" on:click={toggleName} style={hoverColor}>
            {#if $name == "postpercent"}
                No
            {:else}
                <span style="font-family: Comic Sans MS">Yes</span>
            {/if}
        </li>
    </ul>
</ul>
<style>
    #overlay    {
        position: fixed;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        background-color: var(--background);
        opacity: 0.8;
        z-index: 998;
        transition: background-color 1s;
    }
    .dialog  {
        position: fixed;
        display: flex;
        flex-direction: column;
        justify-content: center;
        list-style: none;
        top: 25%;
        left: calc(25% - 2.5px);
        width: 50%;
        background-color: var(--background);
        transition: background-color 1s;
        opacity: 1;
        z-index: 999;
        user-select: none;
    }
    ul  {
        display: flex;
        align-items: center;
        flex-direction: row;
        list-style: none;
        text-decoration: none;
        margin: 0;
        padding: 0;
        width: 100%;
        justify-content: space-between;
        padding: 5px 5px;
    }
    .dialog-top {
        margin-bottom: 15px;
    }
    @media only screen and (max-width: 720px)   {
        .dialog {
            width: calc(100% - 4em - 20px);
            left: calc(2em + 5px);
        }
    }
    .button {
        background-color: var(--background-hover);
        padding: 5px 5px;
    }
    .button:hover {
        background-color: var(--hoverColor);
        transition: background-color 0.25s;
    }
</style>