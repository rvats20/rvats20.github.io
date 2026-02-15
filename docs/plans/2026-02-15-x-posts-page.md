# X Posts Page Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add a separate `tweets.html` page displaying manually-curated X posts in a responsive card grid, matching the existing site design.

**Architecture:** New standalone HTML page (`tweets.html`) duplicating the nav/footer/theme system from `index.html`. Tweet data is hardcoded as HTML cards. `index.html` gets a nav link to the new page.

**Tech Stack:** Tailwind CSS CDN, Lucide Icons, Vanilla JS, static HTML (no build step)

---

### Task 1: Add "Tweets" nav link to `index.html`

**Files:**
- Modify: `index.html:205-213` (desktop nav)
- Modify: `index.html:236-244` (mobile menu)

**Step 1: Add "Tweets" link to desktop nav**

In `index.html`, find the desktop nav links block (around line 205-213). Add a "Tweets" link after "Blog":

```html
<!-- Find this line: -->
<a href="#blogs" class="text-sm hover:text-violet-500 transition-colors">Blog</a>

<!-- Add immediately after it: -->
<a href="tweets.html" class="text-sm hover:text-violet-500 transition-colors">Tweets</a>
```

**Step 2: Add "Tweets" link to mobile menu**

In `index.html`, find the mobile menu block (around line 236-244). Add a "Tweets" link after "Blog":

```html
<!-- Find this line: -->
<a href="#blogs" class="block py-2 text-sm hover:text-violet-500 transition-colors">Blog</a>

<!-- Add immediately after it: -->
<a href="tweets.html" class="block py-2 text-sm hover:text-violet-500 transition-colors">Tweets</a>
```

**Step 3: Verify in browser**

Open `index.html` in browser. Confirm:
- "Tweets" appears in desktop nav between "Blog" and "Education"
- "Tweets" appears in mobile hamburger menu
- Link points to `tweets.html` (will 404 for now, that's expected)

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add Tweets nav link to index.html"
```

---

### Task 2: Create `tweets.html` with page shell

**Files:**
- Create: `tweets.html`

**Step 1: Create `tweets.html` with head, nav, footer, and theme JS**

Create `tweets.html` at the project root with the full page shell. This includes:
- Same `<head>` as `index.html` (Tailwind CDN, Inter font, Lucide Icons, dark mode config) but with updated title/meta for the tweets page
- Same glassmorphism nav with "Tweets" link shown as active (using `text-violet-600 dark:text-violet-400`)
- Nav links point back to `index.html#section` (e.g., `index.html#about`) instead of `#about`
- Same footer (duplicated from `index.html`)
- Dark/light mode toggle JS + Lucide init
- Scroll-to-top button
- Empty `<main>` section as placeholder

```html
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tweets — Rahul Vats</title>
    <meta name="description" content="Latest posts from Rahul Vats on X — thoughts on AI, cloud infrastructure, and tech.">

    <meta property="og:title" content="Tweets — Rahul Vats">
    <meta property="og:description" content="Latest posts from Rahul Vats on X — thoughts on AI, cloud infrastructure, and tech.">
    <meta property="og:image" content="https://rvats20.github.io/assets/img/perfil.png">
    <meta property="og:url" content="https://rvats20.github.io/tweets.html">
    <meta property="og:type" content="website">

    <link rel="shortcut icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>⚡</text></svg>" type="image/svg+xml">

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">

    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: { sans: ['Inter', 'system-ui', 'sans-serif'] },
                    colors: {
                        dark: { base: '#0a0a0f', card: 'rgba(255,255,255,0.05)', surface: '#111118' },
                    },
                },
            },
        }
    </script>

    <script src="https://unpkg.com/lucide@latest"></script>

    <style>
        .glass-nav {
            backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
            background: rgba(255,255,255,0.8);
            border-bottom: 1px solid rgba(0,0,0,0.05);
        }
        .dark .glass-nav {
            background: rgba(10,10,15,0.85);
            border-bottom: 1px solid rgba(255,255,255,0.05);
        }
        .gradient-text {
            background: linear-gradient(135deg, #8b5cf6, #3b82f6);
            -webkit-background-clip: text; -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        .card-glow {
            transition: all 0.3s ease;
            border: 1px solid transparent;
        }
        .card-glow:hover {
            border-color: rgba(139,92,246,0.3);
            box-shadow: 0 0 30px rgba(139,92,246,0.1);
        }
        .reveal { opacity: 0; transform: translateY(30px); transition: opacity 0.6s ease, transform 0.6s ease; }
        .reveal.active { opacity: 1; transform: translateY(0); }
        html { scroll-behavior: smooth; }
    </style>
</head>

<body class="bg-slate-50 dark:bg-dark-base text-slate-800 dark:text-gray-200 font-sans transition-colors duration-300">

<!-- Navigation -->
<nav class="glass-nav fixed top-0 left-0 right-0 z-50 transition-all duration-300">
    <div class="max-w-6xl mx-auto px-4 sm:px-6 py-3 flex items-center justify-between">
        <a href="index.html" class="text-xl font-bold gradient-text">RV</a>

        <div class="hidden md:flex items-center gap-6">
            <a href="index.html#about" class="text-sm hover:text-violet-500 transition-colors">About</a>
            <a href="index.html#skills" class="text-sm hover:text-violet-500 transition-colors">Skills</a>
            <a href="index.html#experience" class="text-sm hover:text-violet-500 transition-colors">Experience</a>
            <a href="index.html#projects" class="text-sm hover:text-violet-500 transition-colors">Projects</a>
            <a href="index.html#opensource" class="text-sm hover:text-violet-500 transition-colors">Open Source</a>
            <a href="index.html#blogs" class="text-sm hover:text-violet-500 transition-colors">Blog</a>
            <a href="tweets.html" class="text-sm text-violet-600 dark:text-violet-400 transition-colors">Tweets</a>
            <a href="index.html#education" class="text-sm hover:text-violet-500 transition-colors">Education</a>
        </div>

        <div class="flex items-center gap-3">
            <button id="theme-toggle" class="p-2 rounded-lg hover:bg-slate-200 dark:hover:bg-gray-800 transition-colors" aria-label="Toggle theme">
                <i data-lucide="moon" class="w-5 h-5 dark:hidden"></i>
                <i data-lucide="sun" class="w-5 h-5 hidden dark:block"></i>
            </button>
            <button id="mobile-menu-btn" class="md:hidden p-2 rounded-lg hover:bg-slate-200 dark:hover:bg-gray-800 transition-colors" aria-label="Menu">
                <i data-lucide="menu" class="w-5 h-5"></i>
            </button>
        </div>
    </div>

    <div id="mobile-menu" class="hidden md:hidden px-4 pb-4 space-y-2">
        <a href="index.html#about" class="block py-2 text-sm hover:text-violet-500 transition-colors">About</a>
        <a href="index.html#skills" class="block py-2 text-sm hover:text-violet-500 transition-colors">Skills</a>
        <a href="index.html#experience" class="block py-2 text-sm hover:text-violet-500 transition-colors">Experience</a>
        <a href="index.html#projects" class="block py-2 text-sm hover:text-violet-500 transition-colors">Projects</a>
        <a href="index.html#opensource" class="block py-2 text-sm hover:text-violet-500 transition-colors">Open Source</a>
        <a href="index.html#blogs" class="block py-2 text-sm hover:text-violet-500 transition-colors">Blog</a>
        <a href="tweets.html" class="block py-2 text-sm text-violet-600 dark:text-violet-400 transition-colors">Tweets</a>
        <a href="index.html#education" class="block py-2 text-sm hover:text-violet-500 transition-colors">Education</a>
    </div>
</nav>

<!-- PAGE CONTENT GOES HERE (Task 3) -->
<main class="pt-20">
</main>

<!-- Footer -->
<footer class="py-12 border-t border-slate-200 dark:border-gray-800">
    <div class="max-w-6xl mx-auto px-4 sm:px-6">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-8">
            <div>
                <h3 class="text-lg font-bold gradient-text mb-3">Rahul Vats</h3>
                <p class="text-sm text-slate-600 dark:text-gray-400 leading-relaxed">
                    AI Solutions & Cloud Infrastructure Specialist. Building intelligent systems that scale.
                </p>
            </div>
            <div>
                <h4 class="font-semibold mb-3 text-sm">Quick Links</h4>
                <div class="grid grid-cols-2 gap-1">
                    <a href="index.html#about" class="text-sm text-slate-600 dark:text-gray-400 hover:text-violet-500 transition-colors py-0.5">About</a>
                    <a href="index.html#skills" class="text-sm text-slate-600 dark:text-gray-400 hover:text-violet-500 transition-colors py-0.5">Skills</a>
                    <a href="index.html#experience" class="text-sm text-slate-600 dark:text-gray-400 hover:text-violet-500 transition-colors py-0.5">Experience</a>
                    <a href="index.html#projects" class="text-sm text-slate-600 dark:text-gray-400 hover:text-violet-500 transition-colors py-0.5">Projects</a>
                    <a href="index.html#blogs" class="text-sm text-slate-600 dark:text-gray-400 hover:text-violet-500 transition-colors py-0.5">Blog</a>
                    <a href="index.html#education" class="text-sm text-slate-600 dark:text-gray-400 hover:text-violet-500 transition-colors py-0.5">Education</a>
                </div>
            </div>
            <div>
                <h4 class="font-semibold mb-3 text-sm">Contact</h4>
                <div class="space-y-2">
                    <a href="mailto:rahul.rkumar23@gmail.com" class="flex items-center gap-2 text-sm text-slate-600 dark:text-gray-400 hover:text-violet-500 transition-colors">
                        <i data-lucide="mail" class="w-4 h-4"></i> rahul.rkumar23@gmail.com
                    </a>
                    <p class="flex items-center gap-2 text-sm text-slate-600 dark:text-gray-400">
                        <i data-lucide="map-pin" class="w-4 h-4"></i> Noida, Sector-73, India
                    </p>
                </div>
                <div class="flex gap-3 mt-4">
                    <a href="https://www.linkedin.com/in/rahulvatts/" target="_blank" aria-label="LinkedIn" class="p-2 rounded-lg hover:bg-slate-200 dark:hover:bg-gray-800 transition-colors">
                        <i data-lucide="linkedin" class="w-4 h-4"></i>
                    </a>
                    <a href="https://github.com/rvats20" target="_blank" aria-label="GitHub" class="p-2 rounded-lg hover:bg-slate-200 dark:hover:bg-gray-800 transition-colors">
                        <i data-lucide="github" class="w-4 h-4"></i>
                    </a>
                    <a href="https://x.com/rahulx" target="_blank" aria-label="X" class="p-2 rounded-lg hover:bg-slate-200 dark:hover:bg-gray-800 transition-colors">
                        <svg class="w-4 h-4" viewBox="0 0 24 24" fill="currentColor"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
                    </a>
                </div>
            </div>
        </div>
        <div class="pt-6 border-t border-slate-200 dark:border-gray-800 text-center">
            <p class="text-xs text-slate-500 dark:text-gray-600">Designed by Rahul Vats</p>
        </div>
    </div>
</footer>

<!-- Scroll to top -->
<button id="scroll-top-btn" onclick="window.scrollTo({top:0,behavior:'smooth'})" class="fixed bottom-6 right-6 p-3 rounded-full bg-violet-600 text-white shadow-lg shadow-violet-500/25 opacity-0 translate-y-4 transition-all duration-300 hover:bg-violet-700 z-40" aria-label="Scroll to top">
    <i data-lucide="chevron-up" class="w-5 h-5"></i>
</button>

<script>
lucide.createIcons();

// Dark/Light mode
const html = document.documentElement;
const themeToggle = document.getElementById('theme-toggle');
const savedTheme = localStorage.getItem('theme');
if (savedTheme === 'light') { html.classList.remove('dark'); }
else { html.classList.add('dark'); }
themeToggle.addEventListener('click', () => {
    html.classList.toggle('dark');
    localStorage.setItem('theme', html.classList.contains('dark') ? 'dark' : 'light');
});

// Mobile menu
const mobileMenuBtn = document.getElementById('mobile-menu-btn');
const mobileMenu = document.getElementById('mobile-menu');
mobileMenuBtn.addEventListener('click', () => { mobileMenu.classList.toggle('hidden'); });
mobileMenu.querySelectorAll('a').forEach(a => { a.addEventListener('click', () => mobileMenu.classList.add('hidden')); });

// Scroll to top button
const scrollTopBtn = document.getElementById('scroll-top-btn');
window.addEventListener('scroll', () => {
    if (window.scrollY > 400) { scrollTopBtn.style.opacity = '1'; scrollTopBtn.style.transform = 'translateY(0)'; }
    else { scrollTopBtn.style.opacity = '0'; scrollTopBtn.style.transform = 'translateY(1rem)'; }
});

// Reveal animations
const revealElements = document.querySelectorAll('.reveal');
const revealObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => { if (entry.isIntersecting) { entry.target.classList.add('active'); } });
}, { threshold: 0.1, rootMargin: '0px 0px -50px 0px' });
revealElements.forEach(el => revealObserver.observe(el));
</script>
</body>
</html>
```

**Step 2: Verify in browser**

Open `tweets.html` in browser. Confirm:
- Nav renders with glassmorphism effect
- "Tweets" link is highlighted in violet (active state)
- Dark/light toggle works and syncs with `index.html` (same localStorage key)
- Footer renders correctly
- All nav links point to `index.html#section`
- "RV" logo links to `index.html`

**Step 3: Commit**

```bash
git add tweets.html
git commit -m "feat: add tweets.html page shell with nav, footer, theme"
```

---

### Task 3: Add page header section

**Files:**
- Modify: `tweets.html` (replace the empty `<main>` content)

**Step 1: Add the header section inside `<main>`**

Replace the empty `<main class="pt-20"></main>` in `tweets.html` with:

```html
<main class="pt-20">

<!-- Header -->
<section class="py-16 sm:py-20">
    <div class="max-w-6xl mx-auto px-4 sm:px-6 text-center">
        <h1 class="text-3xl sm:text-4xl font-bold mb-4 reveal">
            Latest from <span class="gradient-text">X</span>
        </h1>
        <p class="text-slate-600 dark:text-gray-400 mb-6 max-w-md mx-auto reveal">
            Thoughts on AI, cloud, and tech.
        </p>
        <a href="https://x.com/rahulx" target="_blank" class="inline-flex items-center gap-2 px-5 py-2.5 border border-slate-300 dark:border-gray-600 hover:border-violet-400 rounded-xl text-sm font-medium transition-all hover:shadow-lg reveal">
            <svg class="w-4 h-4" viewBox="0 0 24 24" fill="currentColor"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
            Follow @rahulx
        </a>
    </div>
</section>

<!-- TWEET CARDS GO HERE (Task 4) -->

</main>
```

**Step 2: Verify in browser**

Open `tweets.html`. Confirm:
- "Latest from X" title renders with "X" in gradient
- Subtitle text appears
- Follow button links to X profile, has hover effect
- Reveal animations trigger on scroll

**Step 3: Commit**

```bash
git add tweets.html
git commit -m "feat: add page header to tweets.html"
```

---

### Task 4: Add tweet card grid with placeholder content

**Files:**
- Modify: `tweets.html` (add tweet cards inside `<main>`, after the header section)

**Step 1: Add the tweet cards section**

Replace `<!-- TWEET CARDS GO HERE (Task 4) -->` with the tweet card grid. Each card has: avatar, name/handle, date, text with highlighted hashtags, and a "View on X" link.

```html
<!-- Tweets Grid -->
<section class="pb-20 sm:pb-28">
    <div class="max-w-6xl mx-auto px-4 sm:px-6">
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 reveal">

            <!-- Tweet 1 -->
            <div class="p-6 rounded-2xl bg-white dark:bg-dark-card border border-slate-200/50 dark:border-gray-800 card-glow">
                <div class="flex items-center gap-3 mb-4">
                    <img src="assets/img/perfil.png" alt="Rahul Vats" class="w-10 h-10 rounded-full object-cover">
                    <div>
                        <div class="text-sm font-semibold">Rahul Vats</div>
                        <div class="text-xs text-slate-500 dark:text-gray-500">@rahulx</div>
                    </div>
                    <svg class="w-4 h-4 ml-auto text-slate-400" viewBox="0 0 24 24" fill="currentColor"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
                </div>
                <p class="text-sm text-slate-700 dark:text-gray-300 leading-relaxed mb-4">
                    Building AI agents that actually work in production is a different game than demos. The gap between "cool prototype" and "reliable system" is where the real engineering happens. <span class="text-violet-600 dark:text-violet-400">#AIAgents</span> <span class="text-violet-600 dark:text-violet-400">#MLOps</span>
                </p>
                <div class="flex items-center justify-between">
                    <span class="text-xs text-slate-400 dark:text-gray-600">Feb 12, 2026</span>
                    <a href="https://x.com/rahulx" target="_blank" class="text-xs text-violet-600 dark:text-violet-400 hover:underline inline-flex items-center gap-1">
                        View on X <i data-lucide="arrow-up-right" class="w-3 h-3"></i>
                    </a>
                </div>
            </div>

            <!-- Tweet 2 -->
            <div class="p-6 rounded-2xl bg-white dark:bg-dark-card border border-slate-200/50 dark:border-gray-800 card-glow">
                <div class="flex items-center gap-3 mb-4">
                    <img src="assets/img/perfil.png" alt="Rahul Vats" class="w-10 h-10 rounded-full object-cover">
                    <div>
                        <div class="text-sm font-semibold">Rahul Vats</div>
                        <div class="text-xs text-slate-500 dark:text-gray-500">@rahulx</div>
                    </div>
                    <svg class="w-4 h-4 ml-auto text-slate-400" viewBox="0 0 24 24" fill="currentColor"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
                </div>
                <p class="text-sm text-slate-700 dark:text-gray-300 leading-relaxed mb-4">
                    Just migrated a legacy monitoring stack to Kubernetes. Went from 15-minute deploy cycles to under 2 minutes. The key was getting the health checks right from day one. <span class="text-violet-600 dark:text-violet-400">#Kubernetes</span> <span class="text-violet-600 dark:text-violet-400">#DevOps</span>
                </p>
                <div class="flex items-center justify-between">
                    <span class="text-xs text-slate-400 dark:text-gray-600">Feb 8, 2026</span>
                    <a href="https://x.com/rahulx" target="_blank" class="text-xs text-violet-600 dark:text-violet-400 hover:underline inline-flex items-center gap-1">
                        View on X <i data-lucide="arrow-up-right" class="w-3 h-3"></i>
                    </a>
                </div>
            </div>

            <!-- Tweet 3 -->
            <div class="p-6 rounded-2xl bg-white dark:bg-dark-card border border-slate-200/50 dark:border-gray-800 card-glow">
                <div class="flex items-center gap-3 mb-4">
                    <img src="assets/img/perfil.png" alt="Rahul Vats" class="w-10 h-10 rounded-full object-cover">
                    <div>
                        <div class="text-sm font-semibold">Rahul Vats</div>
                        <div class="text-xs text-slate-500 dark:text-gray-500">@rahulx</div>
                    </div>
                    <svg class="w-4 h-4 ml-auto text-slate-400" viewBox="0 0 24 24" fill="currentColor"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
                </div>
                <p class="text-sm text-slate-700 dark:text-gray-300 leading-relaxed mb-4">
                    RAG is not just "throw documents into a vector DB." Chunking strategy, embedding model choice, and retrieval ranking matter more than most tutorials let on. <span class="text-violet-600 dark:text-violet-400">#RAG</span> <span class="text-violet-600 dark:text-violet-400">#LLM</span>
                </p>
                <div class="flex items-center justify-between">
                    <span class="text-xs text-slate-400 dark:text-gray-600">Feb 3, 2026</span>
                    <a href="https://x.com/rahulx" target="_blank" class="text-xs text-violet-600 dark:text-violet-400 hover:underline inline-flex items-center gap-1">
                        View on X <i data-lucide="arrow-up-right" class="w-3 h-3"></i>
                    </a>
                </div>
            </div>

            <!-- Tweet 4 -->
            <div class="p-6 rounded-2xl bg-white dark:bg-dark-card border border-slate-200/50 dark:border-gray-800 card-glow">
                <div class="flex items-center gap-3 mb-4">
                    <img src="assets/img/perfil.png" alt="Rahul Vats" class="w-10 h-10 rounded-full object-cover">
                    <div>
                        <div class="text-sm font-semibold">Rahul Vats</div>
                        <div class="text-xs text-slate-500 dark:text-gray-500">@rahulx</div>
                    </div>
                    <svg class="w-4 h-4 ml-auto text-slate-400" viewBox="0 0 24 24" fill="currentColor"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
                </div>
                <p class="text-sm text-slate-700 dark:text-gray-300 leading-relaxed mb-4">
                    Terraform + AWS + a good tagging strategy = infrastructure you can actually reason about 6 months later. Future you will thank present you. <span class="text-violet-600 dark:text-violet-400">#Terraform</span> <span class="text-violet-600 dark:text-violet-400">#AWS</span>
                </p>
                <div class="flex items-center justify-between">
                    <span class="text-xs text-slate-400 dark:text-gray-600">Jan 28, 2026</span>
                    <a href="https://x.com/rahulx" target="_blank" class="text-xs text-violet-600 dark:text-violet-400 hover:underline inline-flex items-center gap-1">
                        View on X <i data-lucide="arrow-up-right" class="w-3 h-3"></i>
                    </a>
                </div>
            </div>

        </div>
    </div>
</section>
```

**Step 2: Verify in browser**

Open `tweets.html`. Confirm:
- 4 tweet cards render in a responsive grid (1 col mobile, 2 tablet, 3 desktop)
- Each card has avatar, name, handle, X logo, text with violet hashtags, date, "View on X" link
- Cards have glassmorphism hover glow effect
- Reveal animation triggers
- Dark/light mode styles both look correct

**Step 3: Commit**

```bash
git add tweets.html
git commit -m "feat: add tweet cards grid with placeholder content"
```

---

### Task 5: Final verification and cleanup

**Files:**
- Verify: `index.html`, `tweets.html`

**Step 1: Cross-page navigation check**

1. Open `index.html` in browser
2. Click "Tweets" in nav → should open `tweets.html`
3. On `tweets.html`, click "About" → should go to `index.html#about`
4. On `tweets.html`, click "RV" logo → should go to `index.html`
5. Toggle dark/light mode on `tweets.html` → switch to `index.html` → theme should persist

**Step 2: Mobile responsiveness check**

1. Open `tweets.html` on a narrow viewport (< 768px)
2. Hamburger menu should work
3. Tweet cards should stack in 1 column
4. Header should be readable

**Step 3: Final commit**

If any fixes were needed, commit them:

```bash
git add -A
git commit -m "fix: final tweaks for tweets page"
```

---

## How to add a real tweet later

Copy an existing card `<div>` and update:
1. The tweet text inside `<p>`
2. The date in the `<span>`
3. The `href` on "View on X" link to the actual tweet URL

Example tweet URL format: `https://x.com/rahulx/status/1234567890`
