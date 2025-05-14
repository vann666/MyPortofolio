<script>
    import { onMount } from 'svelte';

    let elements = [];
    let observer;

    onMount(() => {
        observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                } else {
                    entry.target.classList.remove('visible');
                }
            });
        }, { threshold: 0.5 });

        elements.forEach(element => {
            observer.observe(element);
        });

        return () => {
            elements.forEach(element => {
                observer.unobserve(element);
            });
        };
    });
</script>

<style>
    .fade-in {
        opacity: 0;
        transform: translateY(20px);
        transition: opacity 0.6s ease-out, transform 0.6s ease-out;
    }

    .fade-in.visible {
        opacity: 1;
        transform: translateY(0);
    }
</style>

<div bind:this={elements} class="fade-in">
    <h2 class="text-4xl font-semibold">Scroll to see the animation!</h2>
    <p class="mt-4 text-lg">This content will fade in as you scroll down.</p>
</div>