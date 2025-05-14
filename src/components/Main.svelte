<script>
    import Step from "./Step.svelte";
    import { onMount } from 'svelte';

    let bgEl;
    let ticking = false;
    let textContainer; // Referensi ke elemen teks "About Me"
    let benefitsContainer; // Referensi ke elemen benefits
    let isVisible = false; // Status apakah elemen terlihat
    let benefitRefs = [];
    let visibleItems = [];
    let profileImage;

    let introText = "Hi! I'm a web developer";
    let displayedText = [];
    let index = 0;

    const wordColors = {
        "Hi!": "#ffffff",
        "I'm": "#5b21b6",
        "a": "#ffffff",
        "web": "#5b21b6",
        "developer": "#5b21b6"
    };

    const words = introText.split(" ");
    let charMap = [];

    for (const word of words) {
        for (let i = 0; i < word.length; i++) {
            charMap.push({
                char: word[i],
                color: wordColors[word],
                isHighlighted: word === "web" || word === "developer"
            });
        }
        // Tambahkan spasi antar kata, kecuali untuk kata terakhir
        charMap.push({
            char: " ", // Spasi antar kata
            color: "#ffffff",
            isHighlighted: false
        });
    }
    // Hapus spasi terakhir jika ada
    if (charMap[charMap.length - 1]?.char === " ") {
        charMap.pop();
    }

    onMount(() => {
        const typingInterval = setInterval(() => {
            if (index < charMap.length) {
                displayedText = [...displayedText, charMap[index]];
                index++;
            } else {
                clearInterval(typingInterval);
            }
        }, 100);

        const handleScroll = () => {
            if (!ticking) {
                window.requestAnimationFrame(() => {
                    const scrollStart = 100;
                    const scrollRange = 400;

                    let scrollY = window.scrollY;
                    let progress = Math.min(Math.max(scrollY - scrollStart, 0), scrollRange) / scrollRange;

                    let offset = (1 - progress) * 100;

                    if (bgEl) {
                        bgEl.style.transform = `translateY(${offset}%)`;
                    }

                    if (profileImage) {
                        profileImage.style.setProperty('--scroll', scrollY);
                    }

                    ticking = false;
                });
                ticking = true;
            }
        };

        window.addEventListener('scroll', handleScroll);

        const observer = new IntersectionObserver(
            (entries) => {
                entries.forEach((entry) => {
                    if (entry.target === textContainer) {
                        isVisible = entry.isIntersecting; // Perbarui status
                    } else {
                        const index = benefitRefs.indexOf(entry.target);
                        if (index !== -1) {
                            visibleItems[index] = entry.isIntersecting;
                        }
                    }
                });
            },
            { threshold: 0.5 } // Elemen dianggap terlihat jika 50% masuk viewport
        );

        if (textContainer) observer.observe(textContainer);
        benefitRefs.forEach((el) => el && observer.observe(el));

        return () => {
            window.removeEventListener('scroll');
            observer.disconnect();
        };
    });

    let steps = [
        {
            name: "Smoljames Store",
            icon: "fa-solid fa-cart-shopping",
            description:
                "Smoljames Store is a a merchanising store created with Next.js, Commerce.js & Stripe! Commerce.js is a CMS for managing project and Stripe is used for all transaction handling.",
        },
        {
            name: "Utlimate Todos",
            icon: "fa-solid fa-list-check",
            description:
                "Test for model robustness with customized test profiles and receive exhaustive reporting on the performance and potential vulnerabilities of your model.",
        },
        {
            name: "Pokédex",
            icon: "fa-solid fa-diagram-project",
            description:
                "Augment your datasets with our filters and dataset manipulations to ensure your models are trained on the highest quality datasets (coming soon).",
        },
    ];

    let benefits = [
        {
            metric: "10x",
            name: "Self-Taught & Passionate",
            description:
                "I'm a self-taught web developer who started from scratch using free online resources. I quickly fell in love with the creative process and problem-solving involved in web development—starting with HTML, CSS, and JavaScript, and gradually expanding into frameworks, backend development, and more.",
        },
        {
            name: "Full-Stack Capabilities",
            description:
                "I focus on more than just visuals—I build seamless and functional web experiences. With hands-on experience in both frontend and backend technologies, I develop websites and apps that are fast, secure, and user-friendly from end to end.",
        },
        {
            name: "Creative & Solution-Oriented",
            description:
                "Every project is an opportunity to solve real-world problems through design and code. I thrive whether working independently or collaborating with a team, always aiming to deliver custom digital solutions—from sleek landing pages to robust web platforms.",
        },
    ];

    let tabs = [
       
    ];
</script>

<style>
    .source-sans-font {
        font-family: 'Source Sans 3', sans-serif;
    }

    .fade-in {
        opacity: 0; /* Awalnya tidak terlihat *
        transform: translateY(20px); /* Mulai dari bawah */
        transition: opacity 0.6s ease-out, transform 0.6s ease-out;
    }

    .fade-in.visible {
        opacity: 1; /* Teks menjadi terlihat */
        transform: translateY(0); /* Geser ke posisi normal */
    }

    .slide-in {
        opacity: 0;
        transform: translateX(-50px); /* Mulai dari posisi geser ke kiri */
        transition: opacity 0.6s ease-out, transform 0.6s ease-out;
    }

    .slide-in.visible {
        opacity: 1;
        transform: translateX(0); /* Geser ke posisi normal */
    }

    .slide-in-right {
        opacity: 0;
        transform: translateX(50px); /* Mulai dari posisi geser ke kanan */
        transition: opacity 0.6s ease-out, transform 0.6s ease-out;
    }

    .slide-in-right.visible {
        opacity: 1;
        transform: translateX(0); /* Geser ke posisi normal */
    }

    /* Efek turun dari atas ke bawah */
    .slide-down {
        opacity: 0;
        transform: translateY(-100px); /* Mulai dari atas */
        transition: opacity 1s ease-out, transform 0.6s ease-out;
    }

    .slide-down.visible {
        opacity: 1;
        transform: translateY(0); /* Geser ke posisi normal */
    }

    /* Smooth scrolling untuk navigasi */
    html {
        scroll-behavior: smooth;
    }

    /* Efek fade untuk teks */
    .fade-text {
        opacity: 0; /* Awalnya tidak terlihat */
        transition: opacity 1s ease-in-out; /* Animasi fade */
    }

    .fade-text.visible {
        opacity: 1; /* Teks menjadi terlihat */
    }

    .typing span {
        opacity: 0;
        transform: translateY(10px);
        display: inline-block;
        animation: fadeIn 0.5s ease forwards;
    }

    @keyframes fadeIn {
        from {
            opacity: 0;
            transform: translateY(10px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }
</style>

<main>
    <div class="aurora-background"></div>
    <div class="flex flex-col flex-1 relative">
        <section
            id="introPage"
            class="grid grid-cols-1 lg:grid-cols-2 gap-6 py-8 sm:py-8 max-w-screen-xl w-full mx-auto px-4"
        >
            <div
                class="flex flex-col items-center lg:items-start justify-center text-center lg:text-left gap-8 md:gap-6 w-full"
            >
                <h2
                    class="font-semibold typing flex flex-wrap justify-center lg:justify-start gap-1 text-[clamp(2rem,6vw,4.5rem)] leading-tight"
                    style="line-height: 1.2;"
                >
                    {#each displayedText as item}
                        <span style="color: {item.color};">{item.char}</span>
                    {/each}
                </h2>

               <p class="text-base sm:text-lg md:text-xl" style="font-family: 'Roboto Slab', serif;">
                    I build <span style="color: #7c3aed;">websites</span> that are visually appealing, fast, user-friendly, and fully optimized for all devices. Each project is crafted with <span style="color: #7c3aed;">attention to detail</span> to deliver both <span style="color: #7c3aed;">aesthetic</span> and <span style="color: #7c3aed;">functional excellence.</span>
                </p>
            </div>

            <div
                class="relative shadow-2xl grid place-items-center w-[350px] h-[500px] mx-auto"
                style="border-radius: 16px;"
                bind:this={profileImage}
            >
                <img
                    bind:this={profileImage}
                    src={"images/profile.png"}
                    alt="Zetane Engine"
                    class="object-cover z-[2] max-h-[70vh] profile-image"
                    style="border-radius: 16px;"
                    loading="lazy"
                />
            </div>
        </section>

        <section>
            <section
                id="about"
                class="relative pt-[150px] pb-20 lg:pb-32 flex flex-col gap-16 sm:gap-20 md:gap-24"
            >
                <div
                    bind:this={bgEl}
                    class="fixed top-0 left-0 w-full h-[950px] z-10 pointer-events-none bg-parallax"
                ></div>

                <div class="relative z-10">
                    <!-- Konten lainnya -->
                </div>
            </section>

            <section class="relative z-20 py-16 text-center text-white">
                <div
                    bind:this={textContainer}
                    class="flex flex-col gap-2 text-center relative before:absolute before:top-0 before:left-0 before:w-2/3 before:h-1.5 before:bg-violet-700 after:absolute after:bottom-0 after:right-0 after:w-2/3 after:h-1.5 after:bg-violet-700 py-2 slide-down fade-in {isVisible ? 'visible' : ''}" 
                >
                    <h6 class="text-large sm:text-xl md:text-2xl">
                        Want to know more?
                    </h6>
                    <h3 class="font-semibold text-3xl sm:text-4xl md:text-5xl">
                        A bit <span class="poppins text-violet-400">about</span> me.
                    </h3>
                </div>
            
                <div
                    bind:this={benefitsContainer}
                    class="flex flex-col gap-16 w-full mx-auto max-w-[800px] mt-32"
                >
                  {#each benefits as benefit, index}
    <div
        bind:this={benefitRefs[index]}
        class={`benefit-item relative p-6 sm:p-8 rounded-2xl bg-white/5 backdrop-blur-lg border border-white/10
                transition-all duration-700 ease-in-out transform hover:scale-[1.02] group
                ${visibleItems[index] ? 'visible' : ''} delay-${index}`}
    >
        <!-- Vertical line -->
        <div class="absolute left-0 top-0 h-full w-1 bg-gradient-to-b from-violet-500 to-transparent rounded-full"></div>

        <!-- Number -->
        <p class="text-4xl sm:text-5xl font-bold text-violet-500 poppins mb-4">
            0{index + 1}
        </p>

        <!-- Title -->
        <h3 class="text-2xl sm:text-3xl md:text-4xl font-semibold text-white mb-2 group-hover:text-violet-400 transition-colors duration-300">
            {benefit.name}
        </h3>

        <!-- Description -->
        <p class="text-gray-300 text-base sm:text-lg leading-relaxed">
            {benefit.description}
        </p>
    </div>
{/each}

                </div>
            </section>
        </section>
        <section class="flex gap-4">
            {#each tabs as tab, index}
                <a
                    href={tab.link}
                    class="duration-200 hover:text-violet-400"
                    target={index === 2 ? "_blank" : ""}
                    aria-label={`Navigate to ${tab.name}`}
                >
                    <p>{tab.name}</p>
                </a>
            {/each}
        </section>

    </div>
</main>
