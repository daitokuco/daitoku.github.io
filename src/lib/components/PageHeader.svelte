<script>
    import Toggler from "@/components/Toggler.svelte";
    import { onMount } from "svelte";

    export let compact = false;

    let isDarkMode = false; // default to light mode

    function setTheme(isDarkMode) {
        if (typeof window !== "undefined") {
            const rootStyle = document.documentElement.style;
            const computedStyle = getComputedStyle(document.documentElement);

            if (isDarkMode) {
                rootStyle.setProperty("--txtPrimaryColor", "#ffffff");
                rootStyle.setProperty("--txtHintColor", "#cfcfcf");
                rootStyle.setProperty("--txtDisabledColor", "#c2c2c2");
                rootStyle.setProperty("--primaryColor", "#ffffff");
                rootStyle.setProperty("--bodyColor", "#121212");
                rootStyle.setProperty("--baseColor", "#121212");
                rootStyle.setProperty("--baseAlt1Color", "#424242");
                rootStyle.setProperty("--baseAlt2Color", "##6e6e6e");
                rootStyle.setProperty("--baseAlt3Color", "#737373");
                rootStyle.setProperty("--baseAlt4Color", "#8b8b8b");
            } else {
                rootStyle.setProperty("--txtPrimaryColor", computedStyle.getPropertyValue("--light-txtPrimaryColor").trim());
                rootStyle.setProperty("--txtHintColor", computedStyle.getPropertyValue("--light-txtHintColor").trim());
                rootStyle.setProperty("--txtDisabledColor", computedStyle.getPropertyValue("--light-txtDisabledColor").trim());
                rootStyle.setProperty("--primaryColor", computedStyle.getPropertyValue("--light-primaryColor").trim());
                rootStyle.setProperty("--bodyColor", computedStyle.getPropertyValue("--light-bodyColor").trim());
                rootStyle.setProperty("--baseColor", computedStyle.getPropertyValue("--light-baseColor").trim());
                rootStyle.setProperty("--baseAlt1Color", computedStyle.getPropertyValue("--light-baseAlt1Color").trim());
                rootStyle.setProperty("--baseAlt2Color", computedStyle.getPropertyValue("--light-baseAlt2Color").trim());
                rootStyle.setProperty("--baseAlt3Color", computedStyle.getPropertyValue("--light-baseAlt3Color").trim());
                rootStyle.setProperty("--baseAlt4Color", computedStyle.getPropertyValue("--light-baseAlt4Color").trim());
            }
        }
    }

    // Toggles between light and dark mode
    function toggleMode() {
        isDarkMode = !isDarkMode;
        localStorage.setItem("darkMode", isDarkMode.toString()); // Store mode in localStorage
        window.requestAnimationFrame(() =>  {
            setTheme(isDarkMode);
        });
        
    }

    onMount(() => {
        if (typeof window !== "undefined") {
            isDarkMode = localStorage.getItem("darkMode") === "true"; // Get mode from localStorage
        }
        setTheme(isDarkMode); // Call the function on initial load to apply the preferred theme
    });
</script>

<header class="page-header">
    <div class="wrapper wrapper-lg">
        <a href="/" class:logo-sm={compact}>
            <img src="/images/logo.png" alt="Daitoku logo" width="200" height="82" />
        </a>

        <div class="flex-fill" />

        <slot />
        
        <button type="button" class="btn btn-circle btn-secondary responsive-menu-btn" title="Menu">
            <span />
            <i class="ri-menu-3-fill" />
            <Toggler class="dropdown dropdown-lg dropdown-right dropdown-nowrap responsive-menu-dropdown">
                <a href="/about" class="dropdown-item">About Us</a>
                <a href="/services" class="dropdown-item">Services</a>
                <a href="/team" class="dropdown-item">Our Team</a>
                <button type="button" class="dropdown-item" on:click={toggleMode} title="dark">
                    {#if isDarkMode}
                        <!-- If in dark mode, display the 'Sun' icon -->
                        <i class="ri-sun-line" />
                    {:else}
                        <!-- If in light mode, display the 'Moon' icon -->
                        <i class="ri-moon-line" />
                    {/if}
                    Theme
                </button>
                <!--- add here dark mode butto nas well,  need to change the colours for certain elements to always be black/white regardless of mode-->
            </Toggler>
        </button>

        <nav class="main-menu">
            <a href="/about" class="btn btn-secondary">About Us</a>
            <a href="/services" class="btn btn-secondary">Services</a>
            <a href="/team" class="btn btn-secondary">Our Team</a>
            <button type="button" class="btn btn-circle btn-secondary" on:click={toggleMode} title="dark">
                {#if isDarkMode}
                    <!-- If in dark mode, display the 'Sun' icon -->
                    <i class="ri-sun-line" />
                {:else}
                    <!-- If in light mode, display the 'Moon' icon -->
                    <i class="ri-moon-line" />
                {/if}
            </button>
        </nav>
    </div>
</header>
