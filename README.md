# organization-manager-website
Our midterms project: a website that manages organizations

## ITS Organizations — Frontend (README)

A. **Overview**

Single-page frontend to browse student organizations at ITS with search, filters, featured toggle, responsive cards, details modal, and a separate detail view (in the same HTML file) that shows Members (with role tags) and Upcoming Events. Includes a simple Create Organization modal to demo CRUD.

Stack: Vanilla HTML + Tailwind (CDN) + a bit of JS.
No build step. No external assets. Works in any static server or VS Code Live Server.

**Files & Structure**

Recommended structure (no assets folder needed):

```
EF234301_WebPro_MID_5025241005_Nuha Usama Okbah_5025241014_Almira Nayla Felisitha_5025241082_Isabel Hayaaulia Ismail/
├─ Login/
│  └─ Its Orgs – Auth Page (frontend).html          # login / signup UI (frontend only)
└─ Directory Page/
   └─ Its Orgs – Directory Page (frontend).html               # directory + details modal + detail view (single file)
```


**B. Explanation**

The canvas already contains:

Its Orgs – Auth Page (frontend).html (login)
``` html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>ITS Orgs – Welcome</title>

  <!-- Google Font for aesthetic heading -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@500;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">

  <!-- Tailwind CSS CDN (no build step) -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            display: ['Playfair Display', 'serif'],
            sans: ['Inter', 'system-ui', 'sans-serif']
          },
          colors: {
            brand: {
              50:'#f5f7ff', 100:'#eef2ff', 200:'#e0e7ff', 300:'#c7d2fe',
              400:'#a5b4fc', 500:'#818cf8', 600:'#6366f1', 700:'#4f46e5', 800:'#4338ca', 900:'#3730a3'
            }
          }
        }
      }
    }
  </script>

  <style>
    /* subtle animated background */
    .bg-grid {
      background-image:
        radial-gradient(circle at 1px 1px, rgba(99,102,241,.15) 1px, transparent 0);
      background-size: 24px 24px;
    }
  </style>
</head>
<body class="min-h-screen bg-white text-slate-800 font-sans">
  <div class="absolute inset-0 bg-gradient-to-b from-brand-50 via-white to-white pointer-events-none"></div>
  <div class="relative min-h-screen grid place-items-center px-4 bg-grid">
    <div class="w-full max-w-md">
      <!-- Card -->
      <div class="relative rounded-3xl shadow-xl ring-1 ring-black/5 overflow-hidden bg-white/80 backdrop-blur">
        <!-- Welcome header -->
        <div class="px-8 pt-8">
          <h1 class="font-display text-4xl text-slate-900 tracking-tight text-center">Welcome</h1>
          <p class="mt-2 text-center text-sm text-slate-500">Sign in or create your account to view organizations in ITS</p>
        </div>

        <!-- Tabs -->
        <div class="mt-6 px-2">
          <div class="mx-6 grid grid-cols-2 rounded-full bg-slate-100 p-1">
            <button id="tab-signin" class="tab-btn rounded-full py-2 text-sm font-medium transition" data-target="panel-signin">Sign in</button>
            <button id="tab-signup" class="tab-btn rounded-full py-2 text-sm font-medium transition" data-target="panel-signup">Sign up</button>
          </div>
        </div>

        <!-- Panels -->
        <div class="p-8">
          <!-- Sign in form -->
          <form id="panel-signin" class="tab-panel space-y-4" method="post" action="/signin" autocomplete="on" novalidate>
            <!-- Uncomment this inside Blade: @csrf -->
            <div>
              <label class="block text-sm font-medium text-slate-700">Username</label>
              <input name="username" type="text" required class="mt-1 w-full rounded-xl border-slate-200 focus:border-brand-500 focus:ring-brand-500" placeholder="yourusername" />
            </div>
            <div>
              <label class="block text-sm font-medium text-slate-700">Password</label>
              <div class="relative">
                <input id="signin-password" name="password" type="password" required class="mt-1 w-full rounded-xl border-slate-200 focus:border-brand-500 focus:ring-brand-500 pr-12" placeholder="••••••••" />
                <button type="button" class="absolute inset-y-0 right-0 px-3 text-sm text-slate-600" data-toggle-pass="signin-password">Show</button>
              </div>
            </div>
            <div class="flex items-center justify-start pt-2">
              <label class="inline-flex items-center gap-2 text-sm text-slate-600">
                <input type="checkbox" name="remember" class="rounded border-slate-300 text-brand-600 focus:ring-brand-500"> Remember me
              </label>
            </div>
            <button type="submit" class="w-full rounded-xl bg-brand-600 hover:bg-brand-700 active:bg-brand-800 text-white font-medium py-2.5 transition">Sign in</button>
            <p class="text-center text-sm text-slate-500">No account? <a href="#" data-toggle="panel-signup" class="text-brand-700 hover:underline">Sign up</a></p>
          </form>

          <!-- Sign up form -->
          <form id="panel-signup" class="tab-panel hidden space-y-4" method="post" action="/signup" autocomplete="on" novalidate>
            <!-- Uncomment this inside Blade: @csrf -->
            <div>
              <label class="block text-sm font-medium text-slate-700">Username</label>
              <input name="username" type="text" required class="mt-1 w-full rounded-xl border-slate-200 focus:border-brand-500 focus:ring-brand-500" placeholder="choose a username" />
            </div>
            <div>
              <label class="block text-sm font-medium text-slate-700">Password</label>
            <div class="relative">
              <input id="signup-password" name="password" type="password" required minlength="6" class="mt-1 w-full rounded-xl border-slate-200 focus:border-brand-500 focus:ring-brand-500 pr-12" placeholder="create a password" />
              <button type="button" class="absolute inset-y-0 right-0 px-3 text-sm text-slate-600" data-toggle-pass="signup-password">Show</button>
            </div>
            </div>
            <!-- Optional: confirm password (can be removed if backend doesn't need it) -->
            <div>
              <label class="block text-sm font-medium text-slate-700">Confirm Password</label>
              <div class="relative">
                <input id="signup-confirm" type="password" required minlength="6" class="mt-1 w-full rounded-xl border-slate-200 focus:border-brand-500 focus:ring-brand-500 pr-12" placeholder="repeat your password" />
                <button type="button" class="absolute inset-y-0 right-0 px-3 text-sm text-slate-600" data-toggle-pass="signup-confirm">Show</button>
              </div>
              <p id="confirm-msg" class="mt-1 hidden text-xs text-rose-600">Passwords do not match</p>
            </div>
            <button type="submit" class="w-full rounded-xl bg-brand-600 hover:bg-brand-700 active:bg-brand-800 text-white font-medium py-2.5 transition">Create account</button>
            <p class="text-center text-sm text-slate-500">Already have an account? <a href="#" data-toggle="panel-signin" class="text-brand-700 hover:underline">Sign in</a></p>
          </form>
        </div>

        <!-- Footer hint -->
        <div class="px-8 pb-6">
          <p class="text-center text-xs text-slate-400">By continuing you agree to our Terms & Privacy Policy</p>
        </div>
      </div>

      <!-- Tiny status toast (replace with real flash messages) -->
      <div id="toast" class="invisible opacity-0 transition-opacity fixed left-1/2 -translate-x-1/2 bottom-6 bg-slate-900 text-white text-sm px-4 py-2 rounded-full shadow-lg"></div>
    </div>
  </div>

  <script>
    // Tabs logic
    const tabBtns = document.querySelectorAll('.tab-btn');
    const panels = document.querySelectorAll('.tab-panel');

    function activateTab(targetId){
      panels.forEach(p => p.id === targetId ? p.classList.remove('hidden') : p.classList.add('hidden'));
      tabBtns.forEach(btn => {
        const active = btn.dataset.target === targetId;
        btn.classList.toggle('bg-white', active);
        btn.classList.toggle('shadow', active);
        btn.classList.toggle('text-slate-900', active);
        btn.classList.toggle('text-slate-500', !active);
      });
    }

    // default: show Sign in
    activateTab('panel-signin');

    tabBtns.forEach(btn => btn.addEventListener('click', () => activateTab(btn.dataset.target)));
    document.querySelectorAll('[data-toggle]')
      .forEach(a => a.addEventListener('click', e => { e.preventDefault(); activateTab(a.dataset.toggle) }))

    // Password visibility toggles
    document.querySelectorAll('[data-toggle-pass]').forEach(btn => {
      btn.addEventListener('click', () => {
        const id = btn.getAttribute('data-toggle-pass');
        const input = document.getElementById(id);
        if (!input) return;
        const isPassword = input.getAttribute('type') === 'password';
        input.setAttribute('type', isPassword ? 'text' : 'password');
        btn.textContent = isPassword ? 'Hide' : 'Show';
      });
    });

    // Confirm password check
    const signupForm = document.getElementById('panel-signup');
    const confirmInput = document.getElementById('signup-confirm');
    const confirmMsg = document.getElementById('confirm-msg');
    if (signupForm) {
      signupForm.addEventListener('submit', (e) => {
        const pass = signupForm.querySelector('input[name="password"]').value;
        const confirm = confirmInput.value;
        if (pass !== confirm) {
          e.preventDefault();
          confirmMsg.classList.remove('hidden');
          confirmInput.classList.add('ring-1','ring-rose-500');
          showToast('Passwords must match');
        }
      });
      confirmInput.addEventListener('input', () => {
        confirmMsg.classList.add('hidden');
        confirmInput.classList.remove('ring-1','ring-rose-500');
      })
    }

    // Simple toast for demo
    function showToast(msg){
      const t = document.getElementById('toast');
      t.textContent = msg; t.classList.remove('invisible');
      requestAnimationFrame(()=>{ t.classList.remove('opacity-0'); t.classList.add('opacity-100'); });
      setTimeout(()=>{ t.classList.add('opacity-0'); setTimeout(()=> t.classList.add('invisible'), 300); }, 1800);
    }
  </script>
</body>
</html>

```
Its Orgs – Directory Page (frontend).html  (single-file directory + detail)

``` html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>ITS Organizations</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = { theme:{ extend:{
      fontFamily:{ display:['Playfair Display','serif'], sans:['Inter','system-ui','sans-serif'] },
      colors:{ brand:{50:'#f5f7ff',100:'#eef2ff',200:'#e0e7ff',300:'#c7d2fe',400:'#a5b4fc',500:'#818cf8',600:'#6366f1',700:'#4f46e5',800:'#4338ca',900:'#3730a3'} }
    }}};
  </script>
  <style>
    .bg-grid{background-image:radial-gradient(circle at 1px 1px, rgba(99,102,241,.14) 1px, transparent 0);background-size:22px 22px}
    .clamp-2{display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden}
    .page{display:none}
    .page.active{display:block}
  </style>
</head>
<body class="min-h-screen bg-white text-slate-800 font-sans">
  <div class="absolute inset-0 bg-gradient-to-b from-brand-50 via-white to-white pointer-events-none"></div>

  <!-- DIRECTORY PAGE -->
  <main id="page-directory" class="page active relative mx-auto max-w-6xl px-4 pb-24 pt-10 bg-grid">
    <header class="flex flex-col items-center gap-3 text-center">
      <h1 class="font-display text-4xl tracking-tight text-slate-900">ORGANIZATION</h1>
      <p class="text-slate-500 text-sm">Browse and discover organizations at ITS. Search, filter, and feature your favorites.</p>
    </header>

    <section class="mt-8 grid gap-3 md:grid-cols-[1fr_auto] md:items-center">
      <div class="flex items-center gap-2">
        <div class="relative flex-1">
          <input id="search" type="text" placeholder="Search organizations…" class="w-full rounded-2xl border border-slate-200 bg-white/90 px-4 py-2.5 pr-10 shadow-sm focus:border-brand-500 focus:ring-brand-500" />
          <svg class="pointer-events-none absolute right-3 top-1/2 -translate-y-1/2 h-5 w-5 text-slate-400" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="m21 21-3.5-3.5"/></svg>
        </div>
        <button id="btn-filter" class="rounded-2xl border border-slate-200 bg-white px-4 py-2.5 text-sm font-medium shadow-sm hover:bg-slate-50">Filter ▾</button>
      </div>
      <label class="inline-flex items-center justify-end gap-2 text-sm text-slate-600">
        <input id="toggle-featured" type="checkbox" class="rounded border-slate-300 text-brand-600 focus:ring-brand-600" />
        ★ Featured only
      </label>
    </section>

    <div class="mt-4"><div id="quick-tags" class="flex flex-wrap gap-2"></div></div>

    <div id="filter-card" class="hidden mt-3 rounded-2xl border border-slate-200 bg-white p-4 shadow-lg">
      <p class="mb-2 text-xs uppercase tracking-wide text-slate-400">Filter by categories</p>
      <div id="filters" class="grid grid-cols-2 gap-2 sm:grid-cols-3 md:grid-cols-4"></div>
      <div class="mt-3 flex items-center justify-between">
        <button id="filter-clear" class="text-sm text-slate-500 hover:underline">Clear</button>
        <button id="filter-apply" class="rounded-xl bg-brand-600 px-4 py-2 text-sm font-medium text-white hover:bg-brand-700">Apply</button>
      </div>
    </div>

    <section id="grid" class="mt-8 grid grid-cols-1 gap-5 sm:grid-cols-2 lg:grid-cols-3"></section>

    <nav class="mt-10 flex items-center justify-between">
      <button id="prev" class="rounded-xl border border-slate-200 bg-white px-3 py-2 text-sm shadow-sm disabled:opacity-40">◀ Prev</button>
      <div id="pages" class="flex items-center gap-1"></div>
      <button id="next" class="rounded-xl border border-slate-200 bg-white px-3 py-2 text-sm shadow-sm disabled:opacity-40">Next ▶</button>
    </nav>

    <section class="mt-8 grid grid-cols-1 gap-3 sm:grid-cols-3">
      <a href="#" class="rounded-2xl border border-slate-200 bg-white p-4 text-center shadow hover:bg-slate-50" onclick="location.reload()">Home</a>
      <a href="#" id="go-yours" class="rounded-2xl border border-slate-200 bg-white p-4 text-center shadow hover:bg-slate-50">Your Organization</a>
      <button id="open-create" class="rounded-2xl border border-slate-200 bg-white p-4 text-center shadow hover:bg-slate-50">Create Organization</button>
    </section>
  </main>

  <!-- DETAILS MODAL (over directory) -->
  <div id="details-modal" class="invisible fixed inset-0 z-50 grid place-items-center bg-black/40 opacity-0 transition">
    <div class="w-full max-w-xl rounded-3xl bg-white p-6 shadow-xl">
      <div class="flex items-start justify-between gap-4">
        <div>
          <h2 id="details-title" class="font-display text-2xl"></h2>
          <p id="details-desc" class="mt-1 text-slate-600"></p>
        </div>
        <button id="details-close" class="rounded-full p-1 text-slate-500 hover:bg-slate-100">✕</button>
      </div>
      <div class="mt-6 flex flex-wrap items-center gap-3">
        <a id="details-view" href="#" class="rounded-xl bg-brand-600 px-4 py-2 text-sm font-medium text-white hover:bg-brand-700">View ▶</a>
        <button id="details-leave" class="rounded-xl border border-rose-200 bg-rose-50 px-4 py-2 text-sm text-rose-700 hover:bg-rose-100">Leave Organization</button>
        <button id="details-close2" class="rounded-xl border border-slate-200 bg-white px-4 py-2 text-sm">Close</button>
      </div>
    </div>
  </div>

  <!-- CREATE MODAL -->
  <div id="modal" class="invisible fixed inset-0 z-50 grid place-items-center bg-black/40 opacity-0 transition">
    <div class="w-full max-w-lg rounded-3xl bg-white p-6 shadow-xl">
      <div class="flex items-center justify-between">
        <h2 class="font-display text-2xl">Create Organization</h2>
        <button id="close-modal" class="rounded-full p-1 text-slate-500 hover:bg-slate-100">✕</button>
      </div>
      <form id="create-form" class="mt-4 space-y-3">
        <div>
          <label class="block text-sm font-medium">Name</label>
          <input name="name" required class="mt-1 w-full rounded-xl border border-slate-200 px-3 py-2 focus:border-brand-500 focus:ring-brand-500" />
        </div>
        <div>
          <label class="block text-sm font-medium">Description</label>
          <textarea name="desc" rows="3" class="mt-1 w-full rounded-xl border border-slate-200 px-3 py-2 focus:border-brand-500 focus:ring-brand-500" placeholder="Short description"></textarea>
        </div>
        <div>
          <label class="block text-sm font-medium">Categories</label>
          <div id="create-cats" class="mt-2 flex flex-wrap gap-2"></div>
        </div>
        <label class="inline-flex items-center gap-2 text-sm text-slate-600"><input name="featured" type="checkbox" class="rounded border-slate-300 text-brand-600 focus:ring-brand-600"> Mark as featured</label>
        <div class="pt-2 text-right">
          <button type="button" id="cancel-create" class="rounded-xl border border-slate-200 bg-white px-4 py-2 text-sm">Cancel</button>
          <button type="submit" class="ml-2 rounded-xl bg-brand-600 px-4 py-2 text-sm font-medium text-white hover:bg-brand-700">Save</button>
        </div>
      </form>
    </div>
  </div>

  <!-- DETAIL PAGE (same file, toggled view) -->
  <main id="page-detail" class="page relative mx-auto max-w-6xl px-4 pb-20 pt-8 bg-grid">
    <a href="#" id="back" class="text-sm text-slate-500 hover:underline">◀ Back to Directory</a>
    <header class="mt-3 flex items-start justify-between gap-4">
      <div class="max-w-3xl">
        <h1 id="org-title" class="font-display text-3xl text-slate-900"></h1>
        <p id="org-desc" class="mt-1 text-slate-600"></p>
        <button id="toggle-join" class="mt-3 rounded-xl border border-slate-200 bg-white px-4 py-2 text-sm shadow-sm hover:bg-slate-50">Join</button>
      </div>
      <button id="star" class="rounded-full border border-slate-200 bg-white px-3 py-1 text-sm">☆</button>
    </header>

    <section class="mt-6 grid gap-6 lg:grid-cols-[1fr_380px]">
      <div>
        <h2 class="mb-3 text-sm uppercase tracking-wide text-slate-400">Members</h2>
        <div id="members" class="grid grid-cols-1 gap-3 sm:grid-cols-2 md:grid-cols-3"></div>
        <div id="pagination" class="mt-3 flex items-center gap-2"></div>
      </div>
      <aside>
        <h2 class="mb-3 text-sm uppercase tracking-wide text-slate-400">Upcoming Events</h2>
        <div id="events" class="flex flex-col gap-3"></div>
      </aside>
    </section>
  </main>

  <script>
    // ====== Data from the PDF (extracted & normalized) — no external assets ======
    const CATEGORIES = ['Student Body','Religious','UKM – Arts','UKM – Sports','UKM – Martial Arts','UKM – Sci/Tech','Committee/Event','Others'];

    const NAMES = [
      'Student Executive Board (BEM ITS)',
      'Student Legislature (BLM ITS)',
      'Student Court (Mahkamah Mahasiswa ITS)',
      'Manarul Ilmi Mosque Community (Islam)',
      'Catholic Student Community St. Ignatius Loyola',
      'ITS Buddhist Development Team',
      'ITS Hindu Spiritual Development Team',
      'Photography Club (UKAFO)',
      'Rebana Music Club (Cinta Rebana)',
      'Music Club',
      'ITS Student Choir (PSM ITS)',
      'Dancing Club (traditional/modern dance, karawitan)',
      'Theater Club (Tiyang Alit Theatre)',
      'Victory Sepuluh Nopember Marching Corps (VSNMC)',
      'CLICK (Film & Cinematography Club)',
      'Siklus Club (outdoor & environmental)',
      'Scout Club (Pramuka ITS)',
      'Student Regiment (MENWA 802 ITS)',
      'PMR / First-Aid (KSR PMI) Club',
      'Astronomy Club',
      'Robotics Club',
      'Reasoning Clubs',
      'Maritime Challenge',
      'IFLS (Language & Culture Club)',
      'Journalism Club (LPM 1.0)',
      'Model United Nations Club (ITS MUN Club)',
      'Student Cooperative “Kopma Dr. Angka”',
      'Muaythai Club (ITS Muaythai Association/IMA)',
      'PSHT Club (Persaudaraan Setia Hati Terate)',
      'Merpati Putih Club',
      'Shorinji Kempo Club',
      'Taekwondo Club',
      'Karate-Do Club',
      'Kendo Club',
      'Basketball Club',
      'Football (Soccer) Club',
      'Bridge Club',
      'Badminton Club',
      'Tennis Club',
      'Billiards Club',
      'Chess Club',
      'Futsal Club',
      'Softball Club (“Stingray”)',
      'Archery Club (INARCO)',
      'Flag Football Club (“Seaborg”)',
      'Volleyball Club',
      'INI LHO ITS! (ILITS) – Open Campus',
      'GERIGI ITS (Generasi Integralistik)',
      'GEREX (Gerigi × UKM EXPO)',
      'UKM EXPO ITS',
      'MABA CUP ITS',
      'POMITS (Pekan Olahraga Mahasiswa ITS)',
      'ITS EXPO',
      'ITS Career Fair / ITS Job Fair (BKI)',
      'ITS PKM League (Liga PKM ITS)',
      'SCHEMATICS ITS',
      'CIVEX / Civil Expo ITS',
      'Computer & Electrical Engineering Expo',
      'Ramadan on Campus (RDK ITS)'
    ];

    function inferTags(name){
      const n = name.toLowerCase();
      const tags = [];
      if (/(bem|blm|mahkamah|student court|legislature)/.test(n)) tags.push('Student Body');
      if (/(mosque|islam|catholic|buddhist|hindu|ramadan)/.test(n)) tags.push('Religious');
      if (/(choir|music|photography|theater|dancing|marching|film|cinematography)/.test(n)) tags.push('UKM – Arts');
      if (/(robotics|astronomy|reasoning|computer|electrical|maritime)/.test(n)) tags.push('UKM – Sci/Tech');
      if (/(muaythai|psht|merpati|kempo|taekwondo|karate|kendo|silat)/.test(n)) tags.push('UKM – Martial Arts');
      if (/(basketball|football|soccer|badminton|tennis|billiards|chess|futsal|softball|archery|volleyball|flag football|bridge)/.test(n)) tags.push('UKM – Sports');
      if (/(expo|career|fair|gerigi|gerex|ilits|maba cup|pomits|league)/.test(n)) tags.push('Committee/Event');
      if (!tags.length) tags.push('Others');
      if (/(ukm)/.test(n) && !tags.some(t=>t.startsWith('UKM'))) tags.push('UKM – Arts');
      return tags;
    }
    function makeDesc(name){
      const n = name;
      if (/BEM|BLM|Court|Mahkamah/i.test(n)) return 'Campus-wide student body focusing on governance and policy.';
      if (/Community|Mosque|Catholic|Buddhist|Hindu|Ramadan/i.test(n)) return 'Religious/faith-based community activities and services.';
      if (/Choir|Music|Photography|Theater|Dancing|Marching|Film/i.test(n)) return 'Art & performance club with practices, showcases, and events.';
      if (/Muaythai|PSHT|Merpati|Kempo|Taekwondo|Karate|Kendo/i.test(n)) return 'Martial arts training, grading, and competitions.';
      if (/Basketball|Football|Soccer|Badminton|Tennis|Billiards|Chess|Futsal|Softball|Archery|Volleyball|Bridge/i.test(n)) return 'Sports club with regular practices and matches.';
      if (/Robotics|Astronomy|Reasoning|Computer|Electrical|Maritime/i.test(n)) return 'Science & technology activities, workshops, and competitions.';
      if (/EXPO|Career|Fair|GERIGI|GEREX|ILITS|MABA|POMITS|League|CIVEX/i.test(n)) return 'Institute-level committee/event series.';
      return 'Student organization/club with regular gatherings and programs.';
    }

    const ORGS = NAMES.map((name, idx)=>({ id: idx+1, name, desc: makeDesc(name), tags: inferTags(name), featured: Math.random()<0.2 }));

    // ====== Directory state & UI ======
    const grid = document.getElementById('grid');
    const searchInput = document.getElementById('search');
    const btnFilter = document.getElementById('btn-filter');
    const filterCard = document.getElementById('filter-card');
    const filtersWrap = document.getElementById('filters');
    const filterClear = document.getElementById('filter-clear');
    const filterApply = document.getElementById('filter-apply');
    const quickTags = document.getElementById('quick-tags');
    const toggleFeatured = document.getElementById('toggle-featured');
    const prevBtn = document.getElementById('prev');
    const nextBtn = document.getElementById('next');
    const pagesWrap = document.getElementById('pages');

    const modal = document.getElementById('modal');
    const closeModal = document.getElementById('close-modal');
    const cancelCreate = document.getElementById('cancel-create');
    const createForm = document.getElementById('create-form');
    const createCats = document.getElementById('create-cats');
    const openCreate = document.getElementById('open-create');

    const detailsModal = document.getElementById('details-modal');
    const detailsClose = document.getElementById('details-close');
    const detailsClose2 = document.getElementById('details-close2');
    const detailsTitle = document.getElementById('details-title');
    const detailsDesc = document.getElementById('details-desc');
    const detailsView = document.getElementById('details-view');
    const detailsLeave = document.getElementById('details-leave');

    const state = { page:1, perPage:9, search:'', selected:new Set(), featuredOnly:false, showOnlyYours:false };

    // Build filters and chips
    CATEGORIES.forEach(cat=>{
      const item=document.createElement('label');
      item.className='inline-flex items-center gap-2 rounded-xl border border-slate-200 px-3 py-2 text-sm';
      item.innerHTML=`<input type="checkbox" value="${cat}" class="rounded border-slate-300 text-brand-600 focus:ring-brand-600"> ${cat}`;
      filtersWrap.appendChild(item);
    });
    function chip(text,onClick,active){const a=document.createElement('button');a.type='button';a.textContent=text;a.className='rounded-full border border-slate-200 bg-white px-3 py-1 text-xs shadow-sm hover:bg-slate-50'; if(active) a.classList.add('ring-1','ring-brand-200'); a.addEventListener('click',onClick); return a;}
    quickTags.appendChild(chip('All',()=>{state.selected.clear();applyAndRender();},true));
    CATEGORIES.forEach(cat=>quickTags.appendChild(chip(cat,()=>{state.selected=new Set([cat]);applyAndRender();})));

    CATEGORIES.forEach(cat=>{const label=document.createElement('label');label.className='inline-flex items-center gap-2 rounded-full border border-slate-200 px-3 py-1 text-sm';label.innerHTML=`<input type="checkbox" value="${cat}" class="rounded border-slate-300 text-brand-600 focus:ring-brand-600"> ${cat}`;createCats.appendChild(label)});

    function getFiltered(){
      let list=[...ORGS];
      if(state.search){const q=state.search.toLowerCase();list=list.filter(o=>o.name.toLowerCase().includes(q)||(o.desc||'').toLowerCase().includes(q)||(o.tags||[]).some(t=>t.toLowerCase().includes(q)));}
      if(state.selected.size) list=list.filter(o=>o.tags.some(t=>state.selected.has(t)));
      if(state.featuredOnly) list=list.filter(o=>o.featured);
      if(state.showOnlyYours) list=list.filter(o=>o.owner==='you');
      return list;
    }
    function cardHTML(o){
      const tags=(o.tags||[]).map(t=>`<span class="inline-flex items-center rounded-full border border-slate-200 px-2 py-0.5 text-xs">${t}</span>`).join(' ');
      return `<article class=\"relative rounded-3xl border border-slate-200 bg-white p-4 shadow-sm hover:shadow-md\">
        <button title=\"Toggle featured\" data-star=\"${o.id}\" class=\"absolute right-3 top-3 rounded-full border border-slate-200 bg-white px-2 py-0.5 text-xs\">${o.featured?'★':'☆'}<\/button>
        <h3 class=\"text-lg font-semibold text-slate-900\">${o.name}<\/h3>
        <div class=\"mt-2 flex flex-wrap gap-1\">${tags}<\/div>
        <p class=\"mt-3 text-sm text-slate-600 clamp-2\">${o.desc||''}<\/p>
        <div class=\"mt-4 flex items-center justify-between\">
          <button class=\"rounded-xl border border-slate-200 bg-white px-3 py-1.5 text-sm hover:bg-slate-50\" data-view=\"${o.id}\">View details<\/button>
          ${o.owner==='you'?'<span class=\"text-xs text-slate-400\">owned by you<\/span>':''}
        <\/div>
      <\/article>`;
    }
    function render(){
      const list=getFiltered();
      const totalPages=Math.max(1,Math.ceil(list.length/state.perPage));
      if(state.page>totalPages) state.page=totalPages;
      const start=(state.page-1)*state.perPage; const pageItems=list.slice(start,start+state.perPage);
      grid.innerHTML=pageItems.map(cardHTML).join('');
      prevBtn.disabled=state.page===1; nextBtn.disabled=state.page===totalPages; pagesWrap.innerHTML='';
      for(let i=1;i<=totalPages;i++){const b=document.createElement('button'); b.textContent=i; b.className='h-9 w-9 rounded-lg border border-slate-200 bg-white text-sm shadow-sm hover:bg-slate-50'; if(i===state.page) b.classList.add('bg-brand-600','text-white','border-brand-600'); b.addEventListener('click',()=>{state.page=i;render();}); pagesWrap.appendChild(b);} }
    function applyAndRender(){state.page=1;render();}

    // Events
    document.addEventListener('click', (e)=>{
      const star=e.target.closest('[data-star]'); if(star){const id=Number(star.getAttribute('data-star')); const idx=ORGS.findIndex(o=>o.id===id); if(idx>-1){ORGS[idx].featured=!ORGS[idx].featured; render();}}
      const view=e.target.closest('[data-view]'); if(view){const id=Number(view.getAttribute('data-view')); const org=ORGS.find(o=>o.id===id); openDetails(org);} });
    searchInput.addEventListener('input', e=>{state.search=e.target.value.trim(); applyAndRender();});
    toggleFeatured.addEventListener('change', e=>{state.featuredOnly=e.target.checked; applyAndRender();});
    btnFilter.addEventListener('click', ()=>filterCard.classList.toggle('hidden'));
    filterClear.addEventListener('click', ()=>{state.selected.clear();[...filtersWrap.querySelectorAll('input[type=checkbox]')].forEach(cb=>cb.checked=false);});
    filterApply.addEventListener('click', ()=>{state.selected.clear();[...filtersWrap.querySelectorAll('input[type=checkbox]')].forEach(cb=>{if(cb.checked) state.selected.add(cb.value);}); filterCard.classList.add('hidden'); applyAndRender();});
    prevBtn.addEventListener('click', ()=>{if(state.page>1){state.page--; render();}});
    nextBtn.addEventListener('click', ()=>{state.page++; render();});

    // Create modal
    function show(el){el.classList.remove('invisible'); el.classList.remove('opacity-0');}
    function hide(el){el.classList.add('opacity-0'); setTimeout(()=>el.classList.add('invisible'),150);}
    openCreate.addEventListener('click', ()=>show(modal));
    closeModal.addEventListener('click', ()=>hide(modal));
    cancelCreate.addEventListener('click', ()=>hide(modal));
    createForm.addEventListener('submit', (e)=>{e.preventDefault(); const f=new FormData(createForm); const name=(f.get('name')||'').toString().trim(); if(!name) return; const desc=(f.get('desc')||'').toString(); const featured=!!f.get('featured'); const tags=[...createCats.querySelectorAll('input:checked')].map(i=>i.value); ORGS.push({id:Date.now(), name, desc:desc||'Student-created organization.', tags: tags.length?tags:['Others'], featured, owner:'you'}); createForm.reset();[...createCats.querySelectorAll('input')].forEach(i=>i.checked=false); hide(modal); applyAndRender();});

    // Details modal -> Detail page
    function openDetails(org){ if(!org) return; detailsTitle.textContent=org.name; detailsDesc.textContent=org.desc||''; detailsView.onclick=(e)=>{ e.preventDefault(); goDetail(org.id); hide(detailsModal);}; detailsLeave.onclick=()=>{ const idx=ORGS.findIndex(o=>o.id===org.id); if(idx>-1) ORGS.splice(idx,1); hide(detailsModal); render(); }; show(detailsModal); }
    detailsClose.addEventListener('click', ()=>hide(detailsModal));
    detailsClose2.addEventListener('click', ()=>hide(detailsModal));

    // ====== Detail page logic (same file) ======
    const pageDir=document.getElementById('page-directory');
    const pageDet=document.getElementById('page-detail');
    const back=document.getElementById('back');
    const orgTitle=document.getElementById('org-title');
    const orgDesc=document.getElementById('org-desc');
    const starBtn=document.getElementById('star');
    const joinBtn=document.getElementById('toggle-join');
    const membersWrap=document.getElementById('members');
    const pager=document.getElementById('pagination');
    const eventsWrap=document.getElementById('events');

    const first=['Ayu','Dika','Rama','Salsa','Bima','Nadia','Rizky','Naufal','Daffa','Fajar','Icha','Adit','Siti','Bagas','Kevin','Lala','Dewi','Rani','Iqbal','Gita'];
    const last=['Pratama','Putri','Saputra','Wibowo','Santoso','Wijaya','Permata','Ananda','Hidayat','Nugroho','Cahyani','Pangestu','Rahma','Septian','Maulana','Wardani'];
    function randName(){ return first[Math.floor(Math.random()*first.length)]+' '+last[Math.floor(Math.random()*last.length)]; }

    function silverTag(){return '<span class="inline-flex items-center rounded-full border border-slate-200 bg-slate-100 px-2 py-0.5 text-xs text-slate-600">Members</span>';}
    function yellowTag(){return '<span class="inline-flex items-center rounded-full border border-yellow-200 bg-yellow-100 px-2 py-0.5 text-xs text-yellow-800">Leader</span>';}

    const detailState={ page:1, perPage:9, currentId:null, roster:[], joined:false };

    function goDetail(id){ detailState.currentId=id; const org=ORGS.find(o=>o.id===id); if(!org) return; pageDir.classList.remove('active'); pageDir.style.display='none'; pageDet.classList.add('active'); pageDet.style.display='block'; orgTitle.textContent=org.name; orgDesc.textContent=org.desc||''; starBtn.textContent=org.featured?'★':'☆'; detailState.roster=Array.from({length:18},()=>({name:randName(), role:'member'})); detailState.roster[Math.floor(Math.random()*detailState.roster.length)]={name:randName(), role:'leader'}; renderMembers(); renderEvents(org); }
    function backToDir(){ pageDet.classList.remove('active'); pageDet.style.display='none'; pageDir.classList.add('active'); pageDir.style.display='block'; render(); }
    back.addEventListener('click', (e)=>{e.preventDefault(); backToDir();});

    function memberCard(m){ return `<article class=\"rounded-2xl border border-slate-200 bg-white p-3 shadow-sm\"><div class=\"font-medium\">${m.name}<\/div><div class=\"mt-2\">${m.role==='leader'?yellowTag():silverTag()}<\/div></article>`; }
    function renderMembers(){ const total=Math.max(1,Math.ceil(detailState.roster.length/detailState.perPage)); if(detailState.page>total) detailState.page=total; const s=(detailState.page-1)*detailState.perPage; const items=detailState.roster.slice(s,s+detailState.perPage); membersWrap.innerHTML=items.map(memberCard).join(''); pager.innerHTML=''; for(let i=1;i<=total;i++){ const b=document.createElement('button'); b.textContent=i; b.className='h-9 w-9 rounded-lg border border-slate-200 bg-white text-sm shadow-sm hover:bg-slate-50'; if(i===detailState.page) b.classList.add('bg-brand-600','text-white','border-brand-600'); b.addEventListener('click',()=>{detailState.page=i; renderMembers();}); pager.appendChild(b);} }

    function renderEvents(org){ const base=[{date:'12 Nov', title:'Open House / Q&A', where:'Main Hall'},{date:'18 Nov', title:'Weekly Activity', where:'Campus Area'},{date:'05 Dec', title:'Showcase / Tournament', where:'Auditorium / GOR'}]; eventsWrap.innerHTML=base.map(e=>`<article class=\"rounded-2xl border border-slate-200 bg-white p-4 shadow-sm\"><div class=\"text-sm text-slate-500\">${e.date}<\/div><div class=\"mt-1 font-medium\">${org.name}: ${e.title}<\/div><div class=\"text-sm text-slate-600\">Location: ${e.where}<\/div></article>`).join(''); }

    starBtn.addEventListener('click', ()=>{ const org=ORGS.find(o=>o.id===detailState.currentId); if(!org) return; org.featured=!org.featured; starBtn.textContent=org.featured?'★':'☆'; });
    joinBtn.addEventListener('click', ()=>{ detailState.joined=!detailState.joined; joinBtn.textContent=detailState.joined?'Leave':'Join'; });

    // Initial paint
    render();
  </script>
</body>
</html>
```
