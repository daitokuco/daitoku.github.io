<script>
    import "@/scss/landing.scss";
    import { onMount, tick } from "svelte";
    import { fly } from "svelte/transition";
    import tooltip from "@/actions/tooltip";
    import { sdk } from "@/stores/preferences";
    import CommonHelper from "@/utils/CommonHelper";
    import PageHeader from "@/components/PageHeader.svelte";
    import PageFooter from "@/components/PageFooter.svelte";
    import CodeBlock from "@/components/CodeBlock.svelte";

    // scroll reveal
    // ---------------------------------------------------------------
    const scrollTolerance = 0;
    const revealDelay = 150;
    let revealElems = [];
    let revealTimers = [];

    loadRevealElems();

    async function loadRevealElems() {
        if (typeof document === "undefined") {
            return;
        }
        await tick();
        clearRevealTimers();
        revealElems = Array.from(document.querySelectorAll(".scroll-reveal:not(.scroll-reached)") || []);
        scrollReveal();
    }

    function scrollReveal() {
        if (revealElems.length <= 0) {
            return;
        }

        let scrollTop = (window.scrollY || window.pageYOffset) << 0;
        let scrollViewport = scrollTop + window.innerHeight + (scrollTop > 0 ? scrollTolerance : 0);
        let toReveal = [];
        for (let i = 0; i < revealElems.length; i++) {
            if (scrollViewport >= revealElems[i].getBoundingClientRect().top + scrollTop) {
                toReveal.push(revealElems[i]);
            }
        }

        for (let i = 0; i < toReveal.length; i++) {
            CommonHelper.removeByValue(revealElems, toReveal[i]);
            revealTimers.push(
                setTimeout(() => {
                    if (toReveal[i]) {
                        toReveal[i]?.classList.add("scroll-reached");
                    }
                }, i * revealDelay)
            );
        }
    }

    function clearRevealTimers() {
        for (let i = revealTimers.length - 1; i >= 0; i--) {
            clearTimeout(revealTimers[i]);
        }
        revealTimers = [];
    }
    // ---

    function handleResize(e) {
        scrollReveal();
    }

    onMount(() => {
        document.body.classList.remove("loading");
        document.body.classList.add("loaded");

        document.addEventListener("scroll", scrollReveal, {
            capture: true,
            passive: true,
        });

        return () => {
            // detach event
            document.removeEventListener("scroll", scrollReveal, {
                capture: true,
                passive: true,
            });
        };
    });
</script>

<svelte:head>
    <title>Daitoku</title>
</svelte:head>

<svelte:window on:resize={handleResize} />

<div class="landing-hero">
    <PageHeader />

    <div class="wrapper wrapper-lg">
        <div class="hero-content">
            <div class="content-row">Unlocking Digital Potential</div>
            <div class="content-row responsive-hide">
                Empowering Business Success
            </div>
            <div class="content-row highlight">
                <strong>With Daitoku</strong>
            </div>
        </div>

    </div>
</div>

<div class="clearfix" />

<div class="wrapper wrapper-lg">
    <section class="landing-section m-t-45 scroll-reveal">
        <h2 class="landing-title"><strong>Welcome to  Daitoku</strong></h2>
    
        <div class="intro-text">
            <p>At <strong>Daitoku</strong>, we unlock your business's full potential through strategic technological solutions. Let's explore a new world of innovation together.</p>
        </div>
        
        <div class="service-preview">
            <h3><strong>Our Services</strong></h3>
            <img src="/images/services.png" alt="A collage of our services" style="width: 80%; height: auto; display: block; margin-left: auto; margin-right: auto;">
            <br>
            <p>We specialize in a range of fields to provide you with comprehensive technology solutions. <a href="/services">Learn more about our services</a>.</p>
        </div>
    
        <hr>
    
        <div class="about-preview">
            <h3><strong>About Us</strong></h3>
            <img src="/images/aboutlogo.png" alt="Daitoku logo" style="width: 80%; height: auto; display: block; margin-left: auto; margin-right: auto;">
            <br>
            <p>We are a dedicated team of professionals based in Ontario, Canada, passionate about driving success through technology. <a href="/about">Get to know us better</a>.</p>
            
        </div>
    
        <hr>
    
        <div class="team-preview">
            <h3>Meet Our Team</h3>
            <br>
            <p>Our team is our strength, comprised of dedicated, skilled, and innovative individuals. <a href="/team">Meet the people behind Daitoku</a>.</p>
            
        </div>
        
        <hr>
    
        <div class="contact-us">
            <h3>Contact Us</h3>
            <p>Ready to take your business to the next level? <a href="mailto:info@daitoku.ca">Get in touch with us</a> today!</p>
        </div>
    </section>
    
    

</div>

<PageFooter />
