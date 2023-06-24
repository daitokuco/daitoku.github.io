<script>
    import { onMount } from "svelte";
    import tooltip from "@/actions/tooltip";

    // build seperately for very basic spam protection
    let supportEmail = "info";
    onMount(() => {
        setTimeout(() => {
            supportEmail += "@" + "daitoku.ca";
        }, 1000);
    });
</script>

<footer class="page-footer">
    <div class="wrapper wrapper-lg">
        <div class="primary-bar">
            <nav class="footer-col links">
                <a href="/about" class="link-item">About Us</a>
                <a href="/services" class="link-item">Services</a>
                <a href="/team" class="link-item">Our Team</a>
            </nav>

            <div class="footer-col socials">
                <div class="watermark-logo"><strong>Daitoku</strong></div>
                <a
                    href="mailto:{supportEmail}"
                    class="btn btn-secondary btn-circle btn-hint social-item"
                    aria-label="Contact support"
                    use:tooltip={{ text: "Contact " + supportEmail, position: "top" }}
                >
                    <i class="ri-mail-line" />
                </a>
            </div>
        </div>
        <div class="secondary-bar">
            <div class="terms">
                <span class="txt">
                    <strong>© {new Date().getFullYear()} Daitoku Corp.</strong>
                </span>
            </div>
            <div class="flex-fill" />
        </div>
    </div>
</footer>
