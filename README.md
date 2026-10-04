<!DOCTYPE html>
<html lang="en" class="scroll-smooth" data-theme="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ShoeDealFinder — Sneaker Price Tracker & Drop Engine</title>
  
  <meta name="description" content="Track sneaker discounts, compare retailer prices, and discover all-time-low deals across Indian sneaker stores." />
  <meta property="og:title" content="ShoeDealFinder — Sneaker Price Tracker" />
  <meta property="og:description" content="Never pay retail markups. Compare VegNonVeg, Superkicks, Myntra, and official flagships." />
  <meta property="og:type" content="website" />
  <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>👟</text></svg>" />

  <!-- Cartoonish + Chunky Streetwear Typography: Fredoka + Nunito + JetBrains Mono -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@500;600;700&family=Nunito:wght@500;600;700;800;900&family=JetBrains+Mono:wght@700;800&display=swap" rel="stylesheet">

  <!-- Immediate Theme Guard -->
  <script>
    try {
      const savedTheme = localStorage.getItem('sdf_theme') || (window.matchMedia('(prefers-color-scheme: light)').matches ? 'light' : 'dark');
      document.documentElement.dataset.theme = savedTheme;
    } catch (e) {}
  </script>

  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: ['class', '[data-theme="dark"]'],
      theme: {
        extend: {
          fontFamily: {
            display: ['Fredoka', 'sans-serif'],
            sans: ['Nunito', 'sans-serif'],
            mono: ['JetBrains Mono', 'monospace'],
          },
          colors: {
            appBg: 'rgb(var(--bg-app-rgb) / <alpha-value>)',
            appCard: 'rgb(var(--bg-card-rgb) / <alpha-value>)',
            appSurface: 'rgb(var(--bg-surface-rgb) / <alpha-value>)',
            appBorder: 'rgb(var(--border-rgb) / <alpha-value>)',
            appBorderHover: 'rgb(var(--border-hover-rgb) / <alpha-value>)',
            appText: 'rgb(var(--text-rgb) / <alpha-value>)',
            appMuted: 'rgb(var(--muted-rgb) / <alpha-value>)',
            accent: 'rgb(var(--accent-rgb) / <alpha-value>)',
            accentHover: 'rgb(var(--accent-hover-rgb) / <alpha-value>)',
            accentText: 'var(--accent-text)',
            trendDrop: '#10b981',
            trendUp: '#ef4444'
          }
        }
      }
    };
  </script>

  <!-- Pinned Runtime Scripts -->
  <script src="https://unpkg.com/react@18.2.0/umd/react.production.min.js"></script>
  <script src="https://unpkg.com/react-dom@18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://unpkg.com/@babel/standalone@7.24.0/babel.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.2/dist/chart.umd.min.js"></script>

  <style>
    :root[data-theme="dark"] {
      --bg-app-rgb: 10 9 8;
      --bg-card-rgb: 20 18 15;
      --bg-surface-rgb: 30 28 23;
      --border-rgb: 48 44 36;
      --border-hover-rgb: 75 70 58;
      --text-rgb: 250 250 250;
      --muted-rgb: 161 161 170;
      --accent-rgb: 212 255 58;
      --accent-hover-rgb: 195 240 40;
      --accent-text: #0a0908;
      --pedestal: radial-gradient(circle at 50% 65%, #2d2920 0%, #14120e 75%);
    }

    :root[data-theme="light"] {
      --bg-app-rgb: 247 245 238;
      --bg-card-rgb: 255 255 255;
      --bg-surface-rgb: 238 233 221;
      --border-rgb: 220 213 198;
      --border-hover-rgb: 185 177 160;
      --text-rgb: 20 19 16;
      --muted-rgb: 108 104 95;
      --accent-rgb: 217 58 11;
      --accent-hover-rgb: 190 48 8;
      --accent-text: #ffffff;
      --pedestal: radial-gradient(circle at 50% 65%, #f1ebe0 0%, #ffffff 80%);
    }

    html, body {
      margin: 0;
      padding: 0;
      min-height: 100vh;
      background-color: rgb(var(--bg-app-rgb));
      color: rgb(var(--text-rgb));
      overflow-x: clip;
    }

    .tabular-nums { font-variant-numeric: tabular-nums; }
    .spotlight-pedestal { background: var(--pedestal); }

    @keyframes riseEntrance {
      from { opacity: 0; transform: translateY(18px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .card-animate-entrance {
      animation: riseEntrance 0.45s cubic-bezier(0.16, 1, 0.3, 1) both;
    }

    .card-3d-wrap { perspective: 1000px; }
    .card-3d-inner {
      will-change: transform;
      transform-style: preserve-3d;
      transition: box-shadow 0.25s ease, border-color 0.25s ease;
    }

    @keyframes pulseAura {
      0%, 100% { box-shadow: 0 0 0 0 rgba(var(--accent-rgb), 0.5); }
      50% { box-shadow: 0 0 0 6px rgba(var(--accent-rgb), 0); }
    }
    .pulse-glow { animation: pulseAura 2.2s infinite ease-in-out; }

    @keyframes drawSparkline { to { stroke-dashoffset: 0; } }
    .sparkline-path {
      stroke-dasharray: 200;
      stroke-dashoffset: 200;
      animation: drawSparkline 0.9s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }

    @keyframes heartPop {
      0% { transform: scale(1); }
      50% { transform: scale(1.35); }
      100% { transform: scale(1); }
    }
    .heart-bounce { animation: heartPop 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); }

    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    @keyframes fadeOut { from { opacity: 1; } to { opacity: 0; } }
    @keyframes scaleIn { from { opacity: 0; transform: scale(0.95) translateY(12px); } to { opacity: 1; transform: scale(1) translateY(0); } }
    @keyframes scaleOut { from { opacity: 1; transform: scale(1) translateY(0); } to { opacity: 0; transform: scale(0.95) translateY(12px); } }

    .modal-enter-backdrop { animation: fadeIn 0.2s ease-out forwards; }
    .modal-exit-backdrop { animation: fadeOut 0.2s ease-in forwards; }
    .modal-enter-panel { animation: scaleIn 0.25s cubic-bezier(0.16, 1, 0.3, 1) forwards; }
    .modal-exit-panel { animation: scaleOut 0.2s cubic-bezier(0.16, 1, 0.3, 1) forwards; }

    @keyframes slideUpDock { from { transform: translate(-50%, 100%); } to { transform: translate(-50%, 0); } }
    .animate-tray { animation: slideUpDock 0.3s cubic-bezier(0.16, 1, 0.3, 1) forwards; }

    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

    @media (prefers-reduced-motion: reduce) {
      *, ::before, ::after {
        animation-duration: 0.001ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.001ms !important;
      }
      .card-3d-inner { transform: none !important; }
    }
  </style>
</head>
<body class="selection:bg-accent selection:text-accentText antialiased">
  <div id="root"></div>

  <script type="text/babel">
    const { useState, useMemo, useEffect, useRef, useCallback } = React;

    // --- Inline Icons ---
    const IconSearch = () => <svg className="w-4 h-4 shrink-0" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.3-4.3"/></svg>;
    const IconHeart = ({ filled }) => <svg className={`w-4 h-4 transition-colors ${filled ? 'fill-accent text-accent' : 'text-appMuted hover:text-appText'}`} stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"/></svg>;
    const IconBell = () => <svg className="w-4 h-4 shrink-0" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><path d="M6 8a6 6 0 0 1 12 0c0 7 3 9 3 9H3s3-2 3-9"/><path d="M10.3 21a1.94 1.94 0 0 0 3.4 0"/></svg>;
    const IconExternal = () => <svg className="w-3.5 h-3.5 shrink-0" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg>;
    const IconClose = () => <svg className="w-4 h-4 shrink-0" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>;
    const IconCheck = () => <svg className="w-4 h-4 text-accent shrink-0" fill="none" stroke="currentColor" strokeWidth="3" viewBox="0 0 24 24"><polyline points="20 6 9 17 4 12"/></svg>;
    const IconSun = () => <svg className="w-4 h-4 shrink-0" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><circle cx="12" cy="12" r="4"/><path d="M12 2v2"/><path d="M12 20v2"/><path d="m4.93 4.93 1.41 1.41"/><path d="m17.66 17.66 1.41 1.41"/><path d="M2 12h2"/><path d="M20 12h2"/><path d="m6.34 17.66-1.41 1.41"/><path d="m19.07 4.93-1.41 1.41"/></svg>;
    const IconMoon = () => <svg className="w-4 h-4 shrink-0" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><path d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z"/></svg>;
    const IconCompare = () => <svg className="w-4 h-4 shrink-0" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><path d="m16 3 4 4-4 4"/><path d="M20 7H4"/><path d="m8 21-4-4 4-4"/><path d="M4 17h16"/></svg>;
    const IconShare = () => <svg className="w-4 h-4 shrink-0" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><circle cx="18" cy="5" r="3"/><circle cx="6" cy="12" r="3"/><circle cx="18" cy="19" r="3"/><line x1="8.59" y1="13.51" x2="15.42" y2="17.49"/><line x1="15.41" y1="6.51" x2="8.59" y2="10.49"/></svg>;
    const IconFilter = () => <svg className="w-4 h-4 shrink-0" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><polygon points="22 3 2 3 10 12.46 10 19 14 21 14 12.46 22 3"/></svg>;

    // --- Full 24-Shoe Catalog ---
    const RAW_SHOES = [
      {
        id: 'adidas-samba-og',
        sku: 'B75807',
        name: 'Adidas Originals Samba OG',
        brand: 'Adidas',
        category: 'Terrace Classic',
        gender: 'Unisex',
        colorway: 'Cloud White / Core Black / Gum',
        imageUrl: 'https://images.unsplash.com/photo-1582588678413-dbf45f4823e9?w=700&auto=format&fit=crop&q=80',
        mrp: 10999,
        lastChecked: '25m ago',
        description: 'Born on the pitch, adopted by streets globally. Full-grain leather with gritty suede T-toe overlays and vintage gum sole.',
        retailers: [
          { storeName: 'Superkicks', logo: '⚡', price: 8499, originalPrice: 10999, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://superkicks.in/search?q=samba+og' },
          { storeName: 'VegNonVeg', logo: '👟', price: 9499, originalPrice: 10999, sizes: ['UK 8', 'UK 9'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=samba+og' },
          { storeName: 'Adidas IN', logo: '👟', price: 10999, originalPrice: 10999, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9', 'UK 10', 'UK 11'], inStock: true, searchUrl: 'https://adidas.co.in/search?q=samba+og' }
        ],
        history: [10999, 9999, 10499, 8999, 8499]
      },
      {
        id: 'nike-dunk-low-panda',
        sku: 'DD1391-100',
        name: 'Nike Dunk Low Retro "Panda"',
        brand: 'Nike',
        category: 'Basketball Heritage',
        gender: 'Men',
        colorway: 'White / Black',
        imageUrl: 'https://images.unsplash.com/photo-1595950653106-6c9ebd614d3a?w=700&auto=format&fit=crop&q=80',
        mrp: 8695,
        lastChecked: '1h ago',
        description: 'Crisp monochrome leather overlays with signature 80s court proportions. Padded low-cut collar for all-day comfort.',
        retailers: [
          { storeName: 'Myntra', logo: '🛍️', price: 7495, originalPrice: 8695, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://myntra.com/nike-dunk' },
          { storeName: 'Superkicks', logo: '⚡', price: 8295, originalPrice: 8695, sizes: ['UK 8', 'UK 9'], inStock: true, searchUrl: 'https://superkicks.in/search?q=dunk+low' },
          { storeName: 'Nike IN', logo: '✔️', price: 8695, originalPrice: 8695, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10', 'UK 11'], inStock: true, searchUrl: 'https://nike.com/in' }
        ],
        history: [8695, 8695, 8295, 8495, 7495]
      },
      {
        id: 'asics-gel-kayano-14',
        sku: '1201A019-108',
        name: 'Asics GEL-KAYANO 14 Tech Runner',
        brand: 'Asics',
        category: 'Y2K Tech Running',
        gender: 'Unisex',
        colorway: 'Cream / Pure Silver / Metallic',
        imageUrl: 'https://images.unsplash.com/photo-1584735935682-2f2b69dff9d2?w=700&auto=format&fit=crop&q=80',
        mrp: 13999,
        lastChecked: '2h ago',
        description: 'Archival late 2000s performance aesthetic upgraded with signature GEL cushioning and TRUSSTIC support system.',
        retailers: [
          { storeName: 'VegNonVeg', logo: '👟', price: 11499, originalPrice: 13999, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=gel+kayano' },
          { storeName: 'Superkicks', logo: '⚡', price: 12499, originalPrice: 13999, sizes: ['UK 6', 'UK 7', 'UK 8'], inStock: true, searchUrl: 'https://superkicks.in/search?q=kayano+14' },
          { storeName: 'Asics IN', logo: '🅰️', price: 13999, originalPrice: 13999, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://asics.co.in' }
        ],
        history: [13999, 12999, 13499, 12499, 11499]
      },
      {
        id: 'air-jordan-1-low-grey',
        sku: '553558-053',
        name: 'Air Jordan 1 Low "Wolf Grey"',
        brand: 'Air Jordan',
        category: 'Court Lifestyle',
        gender: 'Unisex',
        colorway: 'Wolf Grey / White / Photon Dust',
        imageUrl: 'https://images.unsplash.com/photo-1552346154-21d32810aba3?w=700&auto=format&fit=crop&q=80',
        mrp: 8995,
        lastChecked: '30m ago',
        description: 'Timeless Peter Moore design with clean tonal leather, stitched wings insignia, and encapsulated heel Air unit.',
        retailers: [
          { storeName: 'Myntra', logo: '🛍️', price: 7895, originalPrice: 8995, sizes: ['UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://myntra.com/air-jordan-1' },
          { storeName: 'Superkicks', logo: '⚡', price: 8495, originalPrice: 8995, sizes: ['UK 7', 'UK 8'], inStock: true, searchUrl: 'https://superkicks.in/search?q=jordan+1+low' },
          { storeName: 'Nike IN', logo: '✔️', price: 8995, originalPrice: 8995, sizes: ['UK 8', 'UK 9', 'UK 10', 'UK 11'], inStock: true, searchUrl: 'https://nike.com/in' }
        ],
        history: [8995, 8495, 8495, 8295, 7895]
      },
      {
        id: 'new-balance-530-silver',
        sku: 'MR530SG',
        name: 'New Balance 530 Mesh Runner',
        brand: 'New Balance',
        category: 'Retro Lifestyle',
        gender: 'Unisex',
        colorway: 'White / Silver Metallic / Navy',
        imageUrl: 'https://images.unsplash.com/photo-1539185441755-769473a23570?w=700&auto=format&fit=crop&q=80',
        mrp: 9999,
        lastChecked: '3h ago',
        description: 'Authentic 90s vintage runner with high-breathability mesh, synthetic curve accents, and ABZORB shock absorption.',
        retailers: [
          { storeName: 'Myntra', logo: '🛍️', price: 6999, originalPrice: 9999, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://myntra.com/new-balance-530' },
          { storeName: 'Ajio', logo: '🅰️', price: 7999, originalPrice: 9999, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://ajio.com/s/new-balance' },
          { storeName: 'Superkicks', logo: '⚡', price: 8499, originalPrice: 9999, sizes: ['UK 8', 'UK 9'], inStock: true, searchUrl: 'https://superkicks.in/search?q=nb+530' }
        ],
        history: [9999, 8999, 9499, 7999, 6999]
      },
      {
        id: 'puma-palermo-archive',
        sku: '396463-03',
        name: 'Puma Palermo Terrace Suede',
        brand: 'Puma',
        category: 'Terrace Classic',
        gender: 'Unisex',
        colorway: 'Archive Green / Vapor Gray / Gum',
        imageUrl: 'https://images.unsplash.com/photo-1608231387042-66d1773070a5?w=700&auto=format&fit=crop&q=80',
        mrp: 6999,
        lastChecked: '1h ago',
        description: 'Straight from the 1980s football terrace archives, featuring signature T-toe suede construction and gum sole.',
        retailers: [
          { storeName: 'Myntra', logo: '🛍️', price: 3499, originalPrice: 6999, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://myntra.com/puma-palermo' },
          { storeName: 'Ajio', logo: '🅰️', price: 3849, originalPrice: 6999, sizes: ['UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://ajio.com/s/puma-palermo' },
          { storeName: 'Puma Official', logo: '🐆', price: 4199, originalPrice: 6999, sizes: ['UK 6', 'UK 7', 'UK 8'], inStock: true, searchUrl: 'https://in.puma.com' }
        ],
        history: [6999, 5499, 4999, 4199, 3499]
      },
      {
        id: 'nike-air-force-1-07',
        sku: 'CW2288-111',
        name: 'Nike Air Force 1 \'07 Triple White',
        brand: 'Nike',
        category: 'Iconic Streetwear',
        gender: 'Men',
        colorway: 'White / White / White',
        imageUrl: 'https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=700&auto=format&fit=crop&q=80',
        mrp: 8195,
        lastChecked: '15m ago',
        description: 'The foundation of modern sneaker culture. Stitched leather overlays, pivot-circle tread, and springy Nike Air cushioning.',
        retailers: [
          { storeName: 'Myntra', logo: '🛍️', price: 6495, originalPrice: 8195, sizes: ['UK 7', 'UK 7.5', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://myntra.com/air-force-1' },
          { storeName: 'VegNonVeg', logo: '👟', price: 7495, originalPrice: 8195, sizes: ['UK 8', 'UK 9'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=af1' },
          { storeName: 'Nike IN', logo: '✔️', price: 8195, originalPrice: 8195, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9', 'UK 10', 'UK 11'], inStock: true, searchUrl: 'https://nike.com/in' }
        ],
        history: [8195, 7895, 7895, 7195, 6495]
      },
      {
        id: 'reebok-stride-runner',
        sku: '100033144',
        name: 'Reebok STRIDE Breathable Runner',
        brand: 'Reebok',
        category: 'Athletic Running',
        gender: 'Men',
        colorway: 'Cloud White / Vector Navy / Pure Grey',
        imageUrl: 'https://images.unsplash.com/photo-1515955656352-a1fa3ffcd111?w=700&auto=format&fit=crop&q=80',
        mrp: 1874,
        lastChecked: '2h ago',
        description: 'Featherlight performance runner with high-ventilation sandwich mesh and responsive EVA foam midsole.',
        retailers: [
          { storeName: 'Flipkart', logo: '🛒', price: 890, originalPrice: 1874, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://flipkart.com/search?q=reebok+stride' },
          { storeName: 'Amazon IN', logo: '📦', price: 999, originalPrice: 1874, sizes: ['UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://amazon.in/s?k=reebok+stride' }
        ],
        history: [1874, 1499, 1299, 999, 890]
      },
      {
        id: 'red-tape-athleisure',
        sku: 'RSO3491-BLK',
        name: 'Red Tape Athleisure Sport Runner',
        brand: 'Red Tape',
        category: 'Athletic Running',
        gender: 'Women',
        colorway: 'Rose Gold / Pitch Black / White',
        imageUrl: 'https://images.unsplash.com/photo-1560769629-975ec94e6a86?w=700&auto=format&fit=crop&q=80',
        mrp: 1699,
        lastChecked: '3h ago',
        description: 'Engineered knit slip-on upper with memory foam cushioned footbed for lightweight jogging and gym training.',
        retailers: [
          { storeName: 'Amazon IN', logo: '📦', price: 1088, originalPrice: 1699, sizes: ['UK 4', 'UK 5', 'UK 6', 'UK 7'], inStock: true, searchUrl: 'https://amazon.in/s?k=red+tape+sneakers' },
          { storeName: 'Flipkart', logo: '🛒', price: 1199, originalPrice: 1699, sizes: ['UK 5', 'UK 6', 'UK 7'], inStock: true, searchUrl: 'https://flipkart.com/search?q=red+tape' }
        ],
        history: [1699, 1499, 1299, 1149, 1088]
      },
      {
        id: 'fila-melani-metallic',
        sku: '5RM01234',
        name: 'Fila Melani Metallic Court Sneakers',
        brand: 'Fila',
        category: 'Court Heritage',
        gender: 'Women',
        colorway: 'Metallic Silver / Prism White',
        imageUrl: 'https://images.unsplash.com/photo-1525966222134-fcfa99b8ae77?w=700&auto=format&fit=crop&q=80',
        mrp: 7999,
        lastChecked: '1h ago',
        description: 'Low-top retro tennis silhouette with metallic leather panels, padded collar, and durable cupsole.',
        retailers: [
          { storeName: 'Ajio', logo: '🅰️', price: 3919, originalPrice: 7999, sizes: ['UK 4', 'UK 5', 'UK 6', 'UK 7'], inStock: true, searchUrl: 'https://ajio.com/s/fila-melani' },
          { storeName: 'Myntra', logo: '🛍️', price: 4399, originalPrice: 7999, sizes: ['UK 5', 'UK 6'], inStock: true, searchUrl: 'https://myntra.com/fila-melani' }
        ],
        history: [7999, 5999, 4999, 4399, 3919]
      },
      {
        id: 'converse-chuck-70',
        sku: '162058C',
        name: 'Converse Chuck 70 Canvas High-Top',
        brand: 'Converse',
        category: 'Court Heritage',
        gender: 'Unisex',
        colorway: 'Parchment / Egret / Black',
        imageUrl: 'https://images.unsplash.com/photo-1607522370275-f14206abe5d3?w=700&auto=format&fit=crop&q=80',
        mrp: 5999,
        lastChecked: '2h ago',
        description: 'Enhanced 1970s icon with 12oz organic canvas, winged tongue stitching, and cushioned OrthoLite insole.',
        retailers: [
          { storeName: 'VegNonVeg', logo: '👟', price: 4549, originalPrice: 5999, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=chuck+70' },
          { storeName: 'Superkicks', logo: '⚡', price: 4999, originalPrice: 5999, sizes: ['UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://superkicks.in/search?q=chuck+70' }
        ],
        history: [5999, 5299, 4999, 4799, 4549]
      },
      {
        id: 'reebok-club-c-85',
        sku: 'GX3683',
        name: 'Reebok Club C 85 Vintage Court',
        brand: 'Reebok',
        category: 'Court Heritage',
        gender: 'Unisex',
        colorway: 'Chalk / Alabaster / Glen Green',
        imageUrl: 'https://images.unsplash.com/photo-1515955656352-a1fa3ffcd111?w=700&auto=format&fit=crop&q=80',
        mrp: 6599,
        lastChecked: '4h ago',
        description: 'Heritage 1985 tennis court sneaker built from soft garment leather with terry cloth lining and archival branding.',
        retailers: [
          { storeName: 'Flipkart', logo: '🛒', price: 2999, originalPrice: 6599, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://flipkart.com/search?q=club+c' },
          { storeName: 'Myntra', logo: '🛍️', price: 3499, originalPrice: 6599, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://myntra.com/reebok-club-c' }
        ],
        history: [6599, 5499, 4499, 3499, 2999]
      },
      {
        id: 'onitsuka-tiger-mexico-66',
        sku: 'THL202-0146',
        name: 'Onitsuka Tiger MEXICO 66 Heritage',
        brand: 'Onitsuka Tiger',
        category: 'Heritage Court',
        gender: 'Unisex',
        colorway: 'White / Tricolor Blue & Red',
        imageUrl: 'https://images.unsplash.com/photo-1525966222134-fcfa99b8ae77?w=700&auto=format&fit=crop&q=80',
        mrp: 10500,
        lastChecked: '50m ago',
        description: 'The legendary runner created for 1968 Olympic trials, revamped with premium calf leather and timeless tiger stripes.',
        retailers: [
          { storeName: 'Tata CLiQ Luxury', logo: '💎', price: 8990, originalPrice: 10500, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://tatacliq.com/search?q=mexico+66' },
          { storeName: 'Onitsuka IN', logo: '🐯', price: 10500, originalPrice: 10500, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://onitsukatiger.com' }
        ],
        history: [10500, 9990, 9490, 9190, 8990]
      },
      {
        id: 'asics-gel-kayano-12',
        sku: '1203A599-020',
        name: 'Asics GEL-KAYANO 12.1',
        brand: 'Asics',
        category: 'Y2K Tech Running',
        gender: 'Unisex',
        colorway: 'Grey / Cream / Metallic Silver',
        imageUrl: 'https://images.unsplash.com/photo-1584735935682-2f2b69dff9d2?w=700&auto=format&fit=crop&q=80',
        mrp: 13499,
        lastChecked: '40m ago',
        description: 'Archival 2000s running shoe aesthetics with dual GEL technology cushioning for maximum daily comfort.',
        retailers: [
          { storeName: 'VegNonVeg', logo: '👟', price: 11999, originalPrice: 13499, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=kayano' },
          { storeName: 'Superkicks', logo: '⚡', price: 12499, originalPrice: 13499, sizes: ['UK 6', 'UK 7', 'UK 8'], inStock: true, searchUrl: 'https://superkicks.in/search?q=kayano' }
        ],
        history: [13499, 13499, 12899, 12499, 11999]
      },
      {
        id: 'nike-vomero-5',
        sku: 'FB9149-001',
        name: 'Nike Zoom Vomero 5 "Cobblestone"',
        brand: 'Nike',
        category: 'Y2K Tech Running',
        gender: 'Unisex',
        colorway: 'Cobblestone / Light Bone / Oatmeal',
        imageUrl: 'https://images.unsplash.com/photo-1515955656352-a1fa3ffcd111?w=700&auto=format&fit=crop&q=80',
        mrp: 14995,
        lastChecked: '1h ago',
        description: 'Multi-layered tech runner featuring synthetic leather, plastic ribbing cage, and responsive dual Zoom Air units.',
        retailers: [
          { storeName: 'Myntra', logo: '🛍️', price: 12795, originalPrice: 14995, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://myntra.com/vomero-5' },
          { storeName: 'Superkicks', logo: '⚡', price: 13495, originalPrice: 14995, sizes: ['UK 8', 'UK 9'], inStock: true, searchUrl: 'https://superkicks.in/search?q=vomero' }
        ],
        history: [14995, 14495, 13995, 13295, 12795]
      },
      {
        id: 'adidas-campus-00s',
        sku: 'HQ8708',
        name: 'Adidas Originals Campus 00s',
        brand: 'Adidas',
        category: 'Skate Lifestyle',
        gender: 'Unisex',
        colorway: 'Core Black / Cloud White / Off White',
        imageUrl: 'https://images.unsplash.com/photo-1607522370275-f14206abe5d3?w=700&auto=format&fit=crop&q=80',
        mrp: 8999,
        lastChecked: '2h ago',
        description: 'Puffy proportions and wide laces inspired by 2000s skate culture, crafted with soft suede and oversized stripes.',
        retailers: [
          { storeName: 'Superkicks', logo: '⚡', price: 7199, originalPrice: 8999, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://superkicks.in/search?q=campus+00s' },
          { storeName: 'VegNonVeg', logo: '👟', price: 7999, originalPrice: 8999, sizes: ['UK 8', 'UK 9'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=campus' }
        ],
        history: [8999, 8499, 7999, 7599, 7199]
      },
      {
        id: 'adidas-gazelle-indoor',
        sku: 'H06122',
        name: 'Adidas Gazelle Indoor',
        brand: 'Adidas',
        category: 'Terrace Classic',
        gender: 'Unisex',
        colorway: 'Blue Bird / Cloud White / Gum',
        imageUrl: 'https://images.unsplash.com/photo-1539185441755-769473a23570?w=700&auto=format&fit=crop&q=80',
        mrp: 10999,
        lastChecked: '3h ago',
        description: 'Translucent gum rubber outsole wrapping around plush suede upper for an effortless indoor football aesthetic.',
        retailers: [
          { storeName: 'Superkicks', logo: '⚡', price: 8249, originalPrice: 10999, sizes: ['UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://superkicks.in/search?q=gazelle+indoor' },
          { storeName: 'Myntra', logo: '🛍️', price: 8799, originalPrice: 10999, sizes: ['UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://myntra.com/gazelle' }
        ],
        history: [10999, 10299, 9499, 8799, 8249]
      },
      {
        id: 'new-balance-2002r',
        sku: 'M2002RDA',
        name: 'New Balance 2002R Rain Cloud',
        brand: 'New Balance',
        category: 'Y2K Tech Running',
        gender: 'Unisex',
        colorway: 'Rain Cloud / Magnet / Grey',
        imageUrl: 'https://images.unsplash.com/photo-1552346154-21d32810aba3?w=700&auto=format&fit=crop&q=80',
        mrp: 14999,
        lastChecked: '1h ago',
        description: 'Deconstructed suede overlays with jagged raw edges and full N-ergy and ABZORB heel cushioning.',
        retailers: [
          { storeName: 'Superkicks', logo: '⚡', price: 11999, originalPrice: 14999, sizes: ['UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://superkicks.in/search?q=2002r' },
          { storeName: 'VegNonVeg', logo: '👟', price: 12999, originalPrice: 14999, sizes: ['UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=2002r' }
        ],
        history: [14999, 14499, 13499, 12499, 11999]
      },
      {
        id: 'new-balance-9060',
        sku: 'U9060ECA',
        name: 'New Balance 9060 "Sea Salt"',
        brand: 'New Balance',
        category: 'Futuristic Runner',
        gender: 'Unisex',
        colorway: 'Sea Salt / Surf / Grey',
        imageUrl: 'https://images.unsplash.com/photo-1584735935682-2f2b69dff9d2?w=700&auto=format&fit=crop&q=80',
        mrp: 15999,
        lastChecked: '4h ago',
        description: 'Exaggerated wavy sculpting with visible sway bars and dual-density ABZORB and SBS cushioning.',
        retailers: [
          { storeName: 'Superkicks', logo: '⚡', price: 13499, originalPrice: 15999, sizes: ['UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://superkicks.in/search?q=9060' },
          { storeName: 'VegNonVeg', logo: '👟', price: 14299, originalPrice: 15999, sizes: ['UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=9060' }
        ],
        history: [15999, 15499, 14799, 13999, 13499]
      },
      {
        id: 'puma-speedcat-og',
        sku: '398846-01',
        name: 'Puma Speedcat OG Motorsport',
        brand: 'Puma',
        category: 'Motorsport Court',
        gender: 'Unisex',
        colorway: 'Black / Puma White',
        imageUrl: 'https://images.unsplash.com/photo-1608231387042-66d1773070a5?w=700&auto=format&fit=crop&q=80',
        mrp: 8999,
        lastChecked: '45m ago',
        description: 'Low-profile F1 racing shoe design crafted from rich suede with rounded driver heel and embroidered cat.',
        retailers: [
          { storeName: 'Puma Official', logo: '🐆', price: 6299, originalPrice: 8999, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://in.puma.com' },
          { storeName: 'Myntra', logo: '🛍️', price: 6999, originalPrice: 8999, sizes: ['UK 7', 'UK 8', 'UK 9'], inStock: true, searchUrl: 'https://myntra.com/speedcat' }
        ],
        history: [8999, 7999, 7499, 6899, 6299]
      },
      {
        id: 'adidas-adilette-22',
        sku: 'HP6522',
        name: 'Adidas Originals ADILETTE 22 Slides',
        brand: 'Adidas',
        category: 'Slides',
        gender: 'Unisex',
        colorway: 'Desert Sand / St Desert Sand',
        imageUrl: 'https://images.unsplash.com/photo-1603808033192-082d6919d3e1?w=700&auto=format&fit=crop&q=80',
        mrp: 5999,
        lastChecked: '1h ago',
        description: 'Futuristic 3D topographic construction engineered with sugarcane-based EVA for ultra-soft comfort.',
        retailers: [
          { storeName: 'Ajio', logo: '🅰️', price: 3299, originalPrice: 5999, sizes: ['UK 6', 'UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://ajio.com/s/adilette' },
          { storeName: 'Myntra', logo: '🛍️', price: 3499, originalPrice: 5999, sizes: ['UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://myntra.com/adilette' }
        ],
        history: [5999, 4799, 3999, 3499, 3299]
      },
      {
        id: 'nike-air-max-tl-25',
        sku: 'FZ4110-001',
        name: 'Nike AIR MAX TL 2.5 Total Air',
        brand: 'Nike',
        category: 'Iconic Streetwear',
        gender: 'Men',
        colorway: 'Black / Metallic Silver / University Red',
        imageUrl: 'https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=700&auto=format&fit=crop&q=80',
        mrp: 16995,
        lastChecked: '3h ago',
        description: 'Full-length visible Max Air cushioning redesigned with breathable mesh and futuristic molded overlays.',
        retailers: [
          { storeName: 'Ajio Luxe', logo: '💎', price: 15296, originalPrice: 16995, sizes: ['UK 7.5', 'UK 8.5', 'UK 9.5', 'UK 10.5'], inStock: true, searchUrl: 'https://ajio.com/s/air-max' },
          { storeName: 'VegNonVeg', logo: '👟', price: 16995, originalPrice: 16995, sizes: ['UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=air+max' }
        ],
        history: [16995, 16995, 15999, 15600, 15296]
      },
      {
        id: 'asics-gt-2160',
        sku: '1203A275-103',
        name: 'Asics GT-2160 Retro Runner',
        brand: 'Asics',
        category: 'Y2K Tech Running',
        gender: 'Unisex',
        colorway: 'Oatmeal / Shamrock Green / Silver',
        imageUrl: 'https://images.unsplash.com/photo-1584735935682-2f2b69dff9d2?w=700&auto=format&fit=crop&q=80',
        mrp: 10999,
        lastChecked: '55m ago',
        description: 'Tribute to technical design language from the GT-2000 series featuring wavy forefoot sculpting and segmented sole.',
        retailers: [
          { storeName: 'VegNonVeg', logo: '👟', price: 8999, originalPrice: 10999, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=gt-2160' },
          { storeName: 'Superkicks', logo: '⚡', price: 9499, originalPrice: 10999, sizes: ['UK 8', 'UK 9'], inStock: true, searchUrl: 'https://superkicks.in/search?q=gt-2160' }
        ],
        history: [10999, 10499, 9999, 9499, 8999]
      },
      {
        id: 'air-jordan-4-retro',
        sku: 'FV5029-141',
        name: 'Air Jordan 4 Retro "Industrial Blue"',
        brand: 'Air Jordan',
        category: 'Basketball Heritage',
        gender: 'Men',
        colorway: 'Off-White / Military Blue / Neutral Grey',
        imageUrl: 'https://images.unsplash.com/photo-1552346154-21d32810aba3?w=700&auto=format&fit=crop&q=80',
        mrp: 18995,
        lastChecked: '20m ago',
        description: 'Faithful recreation of the 1989 Tinker Hatfield masterpiece with original Nike Air heel branding and mesh quarters.',
        retailers: [
          { storeName: 'Superkicks', logo: '⚡', price: 16995, originalPrice: 18995, sizes: ['UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://superkicks.in/search?q=jordan+4' },
          { storeName: 'VegNonVeg', logo: '👟', price: 17495, originalPrice: 18995, sizes: ['UK 7', 'UK 8', 'UK 9', 'UK 10'], inStock: true, searchUrl: 'https://vegnonveg.com/search?q=jordan+4' }
        ],
        history: [18995, 18995, 17995, 17495, 16995]
      }
    ];

    // Compute derived values
    const PROCESSED_SHOES = RAW_SHOES.map((shoe) => {
      const prices = shoe.retailers.map((r) => r.price);
      const lowestPrice = Math.min(...prices);
      const discountPercent = Math.round((1 - lowestPrice / shoe.mrp) * 100);
      const savingsRupees = shoe.mrp - lowestPrice;

      const retailers = shoe.retailers.map((r) => ({
        ...r,
        isLowest: r.price === lowestPrice
      }));

      const historyCleaned = [...shoe.history];
      historyCleaned[historyCleaned.length - 1] = lowestPrice;

      const prevPrice = historyCleaned.length > 1 ? historyCleaned[historyCleaned.length - 2] : historyCleaned[0];
      const dropAmount = prevPrice - lowestPrice;
      const isSixtyDayLow = lowestPrice <= Math.min(...historyCleaned.slice(0, -1));

      const avgPrice = Math.round(historyCleaned.reduce((a, b) => a + b, 0) / historyCleaned.length);
      const belowAvg = avgPrice - lowestPrice;

      return {
        ...shoe,
        lowestPrice,
        discountPercent,
        savingsRupees,
        retailers,
        history: historyCleaned,
        dropAmount,
        isSixtyDayLow,
        avgPrice,
        belowAvg
      };
    });

    const inr = (val) => `₹${Number(val).toLocaleString('en-IN')}`;

    function generateHistoryLabels(count) {
      const labels = [];
      const now = new Date();
      for (let i = count - 1; i >= 0; i--) {
        const d = new Date(now.getTime() - i * 14 * 24 * 60 * 60 * 1000);
        labels.push(d.toLocaleDateString('en-IN', { month: 'short', day: 'numeric' }));
      }
      return labels;
    }

    function SneakerImg({ src, alt, className = "" }) {
      const [failed, setFailed] = useState(false);
      if (failed) {
        return (
          <div className={`w-full h-full flex flex-col items-center justify-center p-3 text-appMuted ${className}`}>
            <span className="text-3xl mb-1">👟</span>
            <span className="text-xs font-display font-semibold text-center opacity-70">{alt}</span>
          </div>
        );
      }
      return (
        <img
          src={src}
          alt={alt}
          loading="lazy"
          decoding="async"
          onError={() => setFailed(true)}
          className={`w-full h-full object-contain filter drop-shadow-[0_14px_18px_rgba(0,0,0,0.55)] transition-transform duration-500 ${className}`}
        />
      );
    }

    // --- 3D Parallax Card ---
    function SneakerCard({ shoe, isFirstMount, isSaved, isComparing, onCardClick, onToggleWatchlist, onToggleCompare, onOpenAlert, idx }) {
      const cardWrapRef = useRef(null);
      const innerRef = useRef(null);
      const rafRef = useRef(null);

      const handleMouseMove = (e) => {
        if (!cardWrapRef.current || !innerRef.current) return;
        if (!window.matchMedia('(hover: hover) and (pointer: fine)').matches) return;
        if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;

        if (rafRef.current) cancelAnimationFrame(rafRef.current);
        const rect = cardWrapRef.current.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;
        const rotX = ((y - rect.height / 2) / (rect.height / 2)) * -6;
        const rotY = ((x - rect.width / 2) / (rect.width / 2)) * 6;

        rafRef.current = requestAnimationFrame(() => {
          if (innerRef.current) {
            innerRef.current.style.transform = `perspective(1000px) rotateX(${rotX.toFixed(2)}deg) rotateY(${rotY.toFixed(2)}deg) translateY(-4px)`;
          }
        });
      };

      const handleMouseLeave = () => {
        if (rafRef.current) cancelAnimationFrame(rafRef.current);
        if (innerRef.current) {
          innerRef.current.style.transform = 'perspective(1000px) rotateX(0deg) rotateY(0deg) translateY(0px)';
        }
      };

      const lowestOffer = shoe.retailers.find((r) => r.isLowest) || shoe.retailers[0];
      const trendColor = shoe.dropAmount > 0 ? '#10b981' : shoe.dropAmount < 0 ? '#ef4444' : '#a1a1aa';

      return (
        <div
          ref={cardWrapRef}
          onMouseMove={handleMouseMove}
          onMouseLeave={handleMouseLeave}
          style={{ animationDelay: isFirstMount ? `${idx * 40}ms` : '0ms' }}
          className={`card-3d-wrap group relative rounded-3xl ${isFirstMount ? 'card-animate-entrance' : ''}`}
        >
          <div
            ref={innerRef}
            className="card-3d-inner bg-appCard border border-appBorder hover:border-appBorderHover rounded-3xl overflow-hidden flex flex-col justify-between h-full relative"
          >
            <div>
              <div className="relative aspect-[4/3] p-4 spotlight-pedestal overflow-hidden">
                <SneakerImg src={shoe.imageUrl} alt={shoe.name} className="group-hover:scale-105" />

                <div className="absolute top-4 left-4 z-10 flex flex-col gap-1 items-start pointer-events-none">
                  {shoe.isSixtyDayLow ? (
                    <span className="text-xs font-display font-bold px-2.5 py-0.5 rounded-xl bg-accent text-accentText shadow-sm">
                      ⚡ 60-DAY LOW
                    </span>
                  ) : (
                    <span className="text-xs font-display font-bold px-2 py-0.5 rounded-xl bg-appSurface text-appText border border-appBorder">
                      {shoe.discountPercent}% OFF
                    </span>
                  )}
                </div>

                <div className="absolute top-4 right-4 z-20 flex items-center gap-1.5">
                  <button
                    type="button"
                    onClick={() => onToggleCompare(shoe.id)}
                    title={isComparing ? "Remove comparison" : "Compare"}
                    aria-label={`Compare ${shoe.name}`}
                    className={`p-2 rounded-xl backdrop-blur-md border transition ${
                      isComparing ? 'bg-accent text-accentText border-accent font-bold' : 'bg-appCard/80 text-appMuted hover:text-appText border-appBorder'
                    }`}
                  >
                    <IconCompare />
                  </button>

                  <button
                    type="button"
                    onClick={() => onToggleWatchlist(shoe.id)}
                    aria-label={`Save ${shoe.name} to watchlist`}
                    className={`p-2 rounded-xl backdrop-blur-md border transition ${
                      isSaved ? 'bg-accent/15 border-accent/40 text-accent heart-bounce' : 'bg-appCard/80 border-appBorder text-appMuted hover:text-appText'
                    }`}
                  >
                    <IconHeart filled={isSaved} />
                  </button>
                </div>
              </div>

              <div className="p-5 space-y-2">
                <div className="flex items-center justify-between text-xs text-appMuted">
                  <span className="font-display font-bold uppercase tracking-wider">{shoe.brand}</span>
                  <span className="text-xs font-semibold">{shoe.gender}</span>
                </div>

                <h3 className="font-display font-bold text-base text-appText line-clamp-1 group-hover:text-accent transition-colors">
                  <button
                    type="button"
                    onClick={() => onCardClick(shoe)}
                    className="text-left after:absolute after:inset-0 after:z-0 focus:outline-none focus-visible:ring-2 focus-visible:ring-accent rounded-xl"
                  >
                    {shoe.name}
                  </button>
                </h3>

                <div className="pt-2 flex items-end justify-between">
                  <div>
                    <div className="flex items-baseline gap-2">
                      <span className="text-2xl font-mono font-extrabold text-accent tabular-nums">
                        {inr(shoe.lowestPrice)}
                      </span>
                      <span className="text-xs font-mono text-appMuted line-through">
                        {inr(shoe.mrp)}
                      </span>
                    </div>

                    <div className="flex items-center gap-2 mt-1">
                      <span className="text-xs font-sans font-bold text-appMuted">
                        Save <strong className="text-appText font-mono">{inr(shoe.savingsRupees)}</strong>
                      </span>
                      {shoe.dropAmount > 0 && (
                        <span className="text-xs font-mono text-trendDrop font-bold flex items-center">
                          <span className="mr-0.5">↓</span> {inr(shoe.dropAmount)} vs 2 wks ago
                        </span>
                      )}
                    </div>
                  </div>

                  <div className="w-20 h-7 overflow-visible">
                    <svg width="80" height="26" viewBox="0 0 80 26" className="overflow-visible" aria-hidden="true">
                      {(() => {
                        const min = Math.min(...shoe.history);
                        const max = Math.max(...shoe.history);
                        const range = max - min || 1;
                        const points = shoe.history.map((val, i) => {
                          const x = (i / (shoe.history.length - 1)) * 76 + 2;
                          const y = 22 - ((val - min) / range) * 18 + 2;
                          return `${x.toFixed(1)},${y.toFixed(1)}`;
                        }).join(' ');
                        return (
                          <polyline
                            fill="none"
                            stroke={trendColor}
                            strokeWidth="2.2"
                            strokeLinecap="round"
                            strokeLinejoin="round"
                            points={points}
                            className="sparkline-path"
                          />
                        );
                      })()}
                    </svg>
                  </div>
                </div>

                <div className="pt-3 border-t border-appBorder text-xs text-appMuted flex items-center justify-between">
                  <span>Best at <strong className="text-appText">{lowestOffer.storeName}</strong></span>
                  <span className="font-mono text-xs opacity-75">{shoe.lastChecked}</span>
                </div>
              </div>
            </div>

            <div className="p-5 pt-0 grid grid-cols-2 gap-2 relative z-10">
              <button
                type="button"
                onClick={() => onCardClick(shoe)}
                className="w-full py-2.5 rounded-xl text-xs font-display font-bold bg-appSurface hover:bg-appBorder border border-appBorder text-appText transition"
              >
                Stores ({shoe.retailers.length})
              </button>
              <button
                type="button"
                onClick={() => onOpenAlert(shoe)}
                className="w-full py-2.5 rounded-xl text-xs font-display font-bold bg-accent/10 hover:bg-accent/20 border border-accent/20 text-accent transition flex items-center justify-center gap-1.5"
              >
                <IconBell /> Drop Alert
              </button>
            </div>
          </div>
        </div>
      );
    }

    // --- Modal Host ---
    function ModalManager({ activeModal, onCloseModal, onSwitchModal, watchlist, onToggleWatchlist, onTriggerToast }) {
      const [isClosing, setIsClosing] = useState(false);
      const closeBtnRef = useRef(null);
      const modalRef = useRef(null);
      const lastActiveRef = useRef(null);

      const triggerClose = useCallback(() => {
        setIsClosing(true);
        setTimeout(() => {
          setIsClosing(false);
          onCloseModal();
        }, 200);
      }, [onCloseModal]);

      useEffect(() => {
        if (!activeModal) return;
        lastActiveRef.current = document.activeElement;

        const prevOverflow = document.body.style.overflow;
        document.body.style.overflow = 'hidden';
        setTimeout(() => closeBtnRef.current?.focus(), 60);

        const handleKeyDown = (e) => {
          if (e.key === 'Escape') triggerClose();
          if (e.key === 'Tab' && modalRef.current) {
            const focusables = modalRef.current.querySelectorAll('button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])');
            if (!focusables.length) return;
            const first = focusables[0];
            const last = focusables[focusables.length - 1];

            if (e.shiftKey && document.activeElement === first) {
              last.focus();
              e.preventDefault();
            } else if (!e.shiftKey && document.activeElement === last) {
              first.focus();
              e.preventDefault();
            }
          }
        };

        window.addEventListener('keydown', handleKeyDown);
        return () => {
          document.body.style.overflow = prevOverflow;
          window.removeEventListener('keydown', handleKeyDown);
          lastActiveRef.current?.focus?.();
        };
      }, [activeModal, triggerClose]);

      if (!activeModal) return null;
      const { type, shoe, compareItems } = activeModal;

      return (
        <div
          onClick={(e) => e.target === e.currentTarget && triggerClose()}
          className={`fixed inset-0 z-50 flex items-end sm:items-center justify-center p-0 sm:p-6 bg-black/80 backdrop-blur-md ${
            isClosing ? 'modal-exit-backdrop' : 'modal-enter-backdrop'
          }`}
          role="dialog"
          aria-modal="true"
          aria-labelledby="modal-title"
        >
          <div
            ref={modalRef}
            className={`relative w-full max-w-3xl max-h-[92dvh] overflow-y-auto bg-appCard border border-appBorder rounded-t-3xl sm:rounded-3xl p-5 sm:p-8 text-appText shadow-2xl ${
              isClosing ? 'modal-exit-panel' : 'modal-enter-panel'
            }`}
          >
            <div className="w-12 h-1.5 rounded-full bg-appBorder mx-auto mb-4 sm:hidden"></div>

            <button
              ref={closeBtnRef}
              onClick={triggerClose}
              aria-label="Close dialog"
              className="absolute top-5 right-5 p-2 rounded-xl bg-appSurface hover:bg-appBorder text-appMuted hover:text-appText transition border border-appBorder z-20"
            >
              <IconClose />
            </button>

            {type === 'detail' && shoe && (
              <DetailModalContent
                shoe={shoe}
                isSaved={watchlist.includes(shoe.id)}
                onToggleWatchlist={onToggleWatchlist}
                onOpenAlert={() => onSwitchModal({ type: 'alert', shoe })}
                onClose={triggerClose}
                onTriggerToast={onTriggerToast}
              />
            )}

            {type === 'alert' && shoe && (
              <AlertModalContent
                shoe={shoe}
                onClose={triggerClose}
                onTriggerToast={onTriggerToast}
              />
            )}

            {type === 'compare' && compareItems && (
              <CompareModalContent
                shoes={compareItems}
                onClose={triggerClose}
              />
            )}
          </div>
        </div>
      );
    }

    // --- Sub-View: Shoe Detail ---
    function DetailModalContent({ shoe, isSaved, onToggleWatchlist, onOpenAlert, onTriggerToast }) {
      const [selectedSize, setSelectedSize] = useState('All');
      const [chartRange, setChartRange] = useState('60d');
      const canvasRef = useRef(null);
      const chartInstance = useRef(null);

      const allSizesForShoe = useMemo(() => {
        const set = new Set();
        shoe.retailers.forEach((r) => r.sizes.forEach((s) => set.add(s)));
        return Array.from(set).sort((a, b) => parseFloat(a.replace(/[^\d.]/g, '')) - parseFloat(b.replace(/[^\d.]/g, '')));
      }, [shoe]);

      const handleShare = async () => {
        const url = new URL(window.location.href);
        url.searchParams.set('shoe', shoe.id);
        const link = url.toString();

        try {
          if (navigator.clipboard && window.isSecureContext) {
            await navigator.clipboard.writeText(link);
            onTriggerToast('Shareable link copied to clipboard!');
          } else if (navigator.share) {
            await navigator.share({ title: shoe.name, url: link });
          } else {
            prompt('Copy this link:', link);
          }
        } catch (err) {
          prompt('Copy this link:', link);
        }
      };

      useEffect(() => {
        if (!canvasRef.current) return;
        if (chartInstance.current) chartInstance.current.destroy();

        const historySlice = chartRange === '30d' ? shoe.history.slice(-3) : shoe.history;
        const labels = generateHistoryLabels(historySlice.length);
        const isReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

        const computedStyles = getComputedStyle(document.documentElement);
        const accentRgb = computedStyles.getPropertyValue('--accent-rgb').trim() || '212 255 58';
        const strokeColor = `rgb(${accentRgb})`;
        const fillColor = `rgba(${accentRgb.replace(/ /g, ', ')}, 0.15)`;

        const ctx = canvasRef.current.getContext('2d');
        chartInstance.current = new Chart(ctx, {
          type: 'line',
          data: {
            labels: labels,
            datasets: [
              {
                label: 'Lowest Price',
                data: historySlice,
                borderColor: strokeColor,
                borderWidth: 3,
                backgroundColor: fillColor,
                fill: true,
                tension: 0.35,
                pointRadius: 4,
                pointBackgroundColor: strokeColor
              },
              {
                label: 'Original MRP',
                data: historySlice.map(() => shoe.mrp),
                borderColor: 'rgba(161, 161, 170, 0.4)',
                borderWidth: 1.5,
                borderDash: [5, 5],
                pointRadius: 0,
                fill: false
              }
            ]
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            animation: isReduced ? false : { duration: 600, easing: 'easeOutQuart' },
            plugins: {
              legend: { display: false },
              tooltip: {
                backgroundColor: '#14120e',
                titleFont: { family: 'Fredoka', size: 13 },
                bodyFont: { family: 'JetBrains Mono', size: 12 },
                callbacks: { label: (c) => ` ${inr(c.parsed.y)}` }
              }
            },
            scales: {
              x: { grid: { display: false }, ticks: { color: '#a1a1aa', font: { family: 'Nunito', size: 12 } } },
              y: { grid: { color: 'rgba(128,128,128,0.1)' }, ticks: { color: '#a1a1aa', font: { family: 'JetBrains Mono', size: 12 }, callback: (v) => inr(v) } }
            }
          }
        });

        return () => { if (chartInstance.current) chartInstance.current.destroy(); };
      }, [shoe, chartRange]);

      const lowestOffer = shoe.retailers.find((r) => r.isLowest) || shoe.retailers[0];
      const storesWithSelectedSize = selectedSize === 'All' ? shoe.retailers : shoe.retailers.filter((r) => r.sizes.includes(selectedSize));

      return (
        <div className="space-y-6">
          <div className="flex items-center gap-2.5 pr-10">
            <span className="text-xs uppercase font-display font-bold px-3 py-1 rounded-xl bg-accent/15 text-accent border border-accent/25">
              {shoe.brand}
            </span>
            <span className="text-xs font-mono text-appMuted">SKU: {shoe.sku}</span>
            <button
              type="button"
              onClick={handleShare}
              className="text-xs font-display font-bold text-appMuted hover:text-appText flex items-center gap-1.5 ml-auto p-2 rounded-xl bg-appSurface border border-appBorder"
            >
              <IconShare /> Share
            </button>
            <button
              type="button"
              onClick={() => onToggleWatchlist(shoe.id)}
              className="text-xs font-display font-bold p-2 rounded-xl bg-appSurface border border-appBorder"
            >
              <IconHeart filled={isSaved} />
            </button>
          </div>

          <div className="grid grid-cols-1 md:grid-cols-2 gap-6 items-center">
            <div className="h-64 sm:h-72 w-full rounded-3xl overflow-hidden spotlight-pedestal p-5 border border-appBorder flex items-center justify-center">
              <SneakerImg src={shoe.imageUrl} alt={shoe.name} />
            </div>

            <div className="space-y-3.5">
              <div>
                <h2 id="modal-title" className="text-2xl sm:text-3xl font-display font-bold text-appText tracking-tight">
                  {shoe.name}
                </h2>
                <p className="text-xs text-appMuted mt-1 font-semibold">{shoe.colorway} • {shoe.gender} • {shoe.category}</p>
              </div>

              <div className="p-4 rounded-2xl bg-appSurface border border-appBorder">
                <span className="text-xs font-display font-bold text-appMuted uppercase tracking-wider block mb-1">
                  Lowest Curated Price
                </span>
                <div className="flex items-baseline gap-2.5">
                  <span className="text-3xl font-mono font-extrabold text-accent tabular-nums">
                    {inr(shoe.lowestPrice)}
                  </span>
                  <span className="text-sm font-mono text-appMuted line-through">
                    {inr(shoe.mrp)}
                  </span>
                  <span className="text-xs font-display font-bold px-2 py-0.5 rounded-lg bg-accent text-accentText">
                    Save {inr(shoe.savingsRupees)}
                  </span>
                </div>
              </div>

              <div className="p-2.5 rounded-xl bg-appCard border border-appBorder text-xs flex items-center gap-2">
                <span className="text-base">🎯</span>
                <span className="font-semibold text-appText">
                  {shoe.belowAvg > 0
                    ? `Great deal: ${inr(shoe.belowAvg)} below its 60-day typical average.`
                    : 'Stable deal: Pricing has held steady over recent drop cycles.'}
                </span>
              </div>

              <div className="flex gap-2">
                <a
                  href={lowestOffer.searchUrl}
                  target="_blank"
                  rel="noreferrer"
                  className="flex-1 py-3 px-4 rounded-xl font-display font-bold text-xs bg-accent hover:bg-accentHover text-accentText transition shadow-md flex items-center justify-center gap-1.5"
                >
                  View at {lowestOffer.storeName}
                  <IconExternal />
                </a>
                <button
                  type="button"
                  onClick={onOpenAlert}
                  className="py-3 px-4 rounded-xl font-display font-bold text-xs bg-appSurface hover:bg-appBorder text-appText border border-appBorder transition flex items-center gap-1.5"
                >
                  <IconBell /> Drop Alert
                </button>
              </div>

              <p className="text-xs text-appMuted leading-relaxed pt-1">
                {shoe.description}
              </p>
            </div>
          </div>

          <div className="border-t border-appBorder pt-5">
            <div className="flex items-center justify-between mb-3">
              <span className="text-xs font-display font-bold text-appText uppercase tracking-wider">
                Filter Availability by Size (UK):
              </span>
              <span className="text-xs font-mono text-accent font-bold">
                {storesWithSelectedSize.length} of {shoe.retailers.length} stores have stock
              </span>
            </div>

            <div className="flex flex-wrap gap-2">
              <button
                type="button"
                onClick={() => setSelectedSize('All')}
                className={`px-3 py-1.5 rounded-xl text-xs font-display font-bold border transition ${
                  selectedSize === 'All' ? 'bg-accent text-accentText border-accent' : 'bg-appSurface text-appText border-appBorder'
                }`}
              >
                All Sizes
              </button>
              {allSizesForShoe.map((sz) => (
                <button
                  key={sz}
                  type="button"
                  onClick={() => setSelectedSize(sz)}
                  className={`px-3 py-1.5 rounded-xl text-xs font-mono font-bold border transition ${
                    selectedSize === sz
                      ? 'bg-accent text-accentText border-accent'
                      : 'bg-appSurface text-appText border-appBorder hover:border-appBorderHover'
                  }`}
                >
                  {sz}
                </button>
              ))}
            </div>
          </div>

          <div className="border-t border-appBorder pt-5">
            <h3 className="text-sm font-display font-bold text-appText mb-3">Retailer Stock & Deal Comparison</h3>
            <div className="overflow-x-auto rounded-2xl border border-appBorder bg-appSurface">
              <table className="w-full text-left text-xs">
                <thead className="bg-appCard text-appMuted uppercase font-display text-xs border-b border-appBorder">
                  <tr>
                    <th className="py-3 px-4">Store</th>
                    <th className="py-3 px-4">Price</th>
                    <th className="py-3 px-4">Sizes In Stock</th>
                    <th className="py-3 px-4 text-right">Store Search</th>
                  </tr>
                </thead>
                <tbody className="divide-y divide-appBorder">
                  {shoe.retailers.map((r, i) => (
                    <tr key={i} className={`transition ${selectedSize !== 'All' && !r.sizes.includes(selectedSize) ? 'opacity-35 bg-appCard/40' : r.isLowest ? 'bg-accent/5' : 'hover:bg-appCard'}`}>
                      <td className="py-3 px-4">
                        <div className="flex items-center gap-2 font-medium text-appText">
                          <span className="p-1 rounded bg-appCard border border-appBorder">{r.logo}</span>
                          <span>{r.storeName}</span>
                          {r.isLowest && <span className="text-[11px] font-display font-bold px-1.5 py-0.5 rounded bg-accent/20 text-accent border border-accent/30">BEST DEAL</span>}
                        </div>
                      </td>
                      <td className="py-3 px-4 font-mono font-bold text-appText text-sm">{inr(r.price)}</td>
                      <td className="py-3 px-4">
                        <div className="flex flex-wrap gap-1">
                          {r.sizes.map((s) => (
                            <span key={s} className={`px-1.5 py-0.5 rounded text-xs font-mono border ${selectedSize === s ? 'bg-accent text-accentText border-accent font-bold' : 'bg-appCard border-appBorder text-appMuted'}`}>
                              {s}
                            </span>
                          ))}
                        </div>
                      </td>
                      <td className="py-3 px-4 text-right">
                        <a href={r.searchUrl} target="_blank" rel="noreferrer" className="inline-flex items-center gap-1 px-3 py-1 rounded-xl bg-appCard hover:bg-appBorder text-accent border border-appBorder font-bold text-xs transition">
                          Find Pair <IconExternal />
                        </a>
                      </td>
                    </tr>
                  ))}
                </tbody>
              </table>
            </div>
          </div>

          <div className="border-t border-appBorder pt-5">
            <div className="flex items-center justify-between mb-3">
              <div>
                <h3 className="text-sm font-display font-bold text-appText">Price Trajectory vs Original MRP</h3>
                <p className="text-xs text-appMuted">Grey dashed line represents initial launch price ({inr(shoe.mrp)})</p>
              </div>
              <div className="flex items-center gap-1.5">
                <button type="button" onClick={() => setChartRange('30d')} className={`px-2.5 py-1 rounded-lg text-xs font-mono font-bold border transition ${chartRange === '30d' ? 'bg-accent text-accentText border-accent' : 'bg-appSurface text-appMuted border-appBorder'}`}>30D</button>
                <button type="button" onClick={() => setChartRange('60d')} className={`px-2.5 py-1 rounded-lg text-xs font-mono font-bold border transition ${chartRange === '60d' ? 'bg-accent text-accentText border-accent' : 'bg-appSurface text-appMuted border-appBorder'}`}>60D</button>
              </div>
            </div>
            <div className="h-44 w-full bg-appSurface rounded-2xl border border-appBorder p-3">
              <canvas ref={canvasRef}></canvas>
            </div>
          </div>
        </div>
      );
    }

    // --- Sub-View: Alert Form ---
    function AlertModalContent({ shoe, onClose, onTriggerToast }) {
      const [email, setEmail] = useState('');
      const [targetPrice, setTargetPrice] = useState(Math.round(shoe.lowestPrice * 0.9));
      const [error, setError] = useState('');

      const handleSubmit = (e) => {
        e.preventDefault();
        if (!targetPrice || targetPrice <= 0) {
          setError('Please provide a valid target price.');
          return;
        }
        if (targetPrice >= shoe.lowestPrice) {
          setError(`Target price must be lower than the current lowest deal (${inr(shoe.lowestPrice)})`);
          return;
        }

        try {
          const storedAlerts = JSON.parse(localStorage.getItem('sdf_alerts')) || [];
          storedAlerts.push({
            id: Date.now(),
            sku: shoe.sku,
            shoeName: shoe.name,
            targetPrice,
            email: email.trim() || 'local',
            date: new Date().toLocaleDateString('en-IN')
          });
          localStorage.setItem('sdf_alerts', JSON.stringify(storedAlerts));
        } catch (err) {}

        setError('');
        onTriggerToast(`Alert set for ${shoe.name} at ${inr(targetPrice)}. Saved to your browser.`);
        onClose();
      };

      return (
        <div className="space-y-4">
          <div className="w-10 h-10 rounded-2xl bg-accent/15 text-accent border border-accent/25 flex items-center justify-center">
            <IconBell />
          </div>

          <div>
            <h3 id="modal-title" className="text-xl font-display font-bold text-appText">
              Create Price Drop Notification
            </h3>
            <p className="text-xs text-appMuted mt-1">
              Notify me when <strong>{shoe.name}</strong> dips below my threshold.
            </p>
          </div>

          <div className="p-3.5 rounded-2xl bg-appSurface border border-appBorder flex items-center gap-3">
            <div className="w-12 h-12 rounded-xl bg-appCard p-1 shrink-0">
              <SneakerImg src={shoe.imageUrl} alt="" />
            </div>
            <div>
              <p className="text-xs font-bold text-appText truncate">{shoe.name}</p>
              <p className="text-xs text-appMuted font-semibold">
                Current Lowest: <span className="font-mono text-accent font-bold">{inr(shoe.lowestPrice)}</span>
              </p>
            </div>
          </div>

          <form onSubmit={handleSubmit} className="space-y-3.5 pt-2">
            <div>
              <label className="block text-xs font-display font-bold text-appText mb-1">Target Price (₹)</label>
              <input
                type="number"
                min="1"
                max={shoe.lowestPrice - 1}
                value={targetPrice}
                onChange={(e) => setTargetPrice(Number(e.target.value))}
                className="w-full px-3.5 py-2.5 rounded-xl bg-appSurface border border-appBorder text-sm font-mono text-accent focus:outline-none focus:border-accent"
                required
              />
              {error && <p className="text-xs text-trendUp mt-1 font-semibold">{error}</p>}
            </div>

            <div>
              <label className="block text-xs font-display font-bold text-appText mb-1">Email Address (Optional)</label>
              <input
                type="email"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
                placeholder="sneakerhead@example.com"
                className="w-full px-3.5 py-2.5 rounded-xl bg-appSurface border border-appBorder text-xs text-appText placeholder:text-appMuted focus:outline-none focus:border-accent"
              />
            </div>

            <p className="text-xs text-appMuted leading-relaxed">
              * Saved locally to your browser's drop dashboard. Push and webhook email integrations are simulated for sample demonstration.
            </p>

            <button
              type="submit"
              className="w-full py-3 rounded-xl font-display font-bold text-xs bg-accent hover:bg-accentHover text-accentText transition shadow-md"
            >
              Confirm Price Alert
            </button>
          </form>
        </div>
      );
    }

    // --- Sub-View: Aligned Side-by-Side Comparison Matrix ---
    function CompareModalContent({ shoes }) {
      const bestPrice = Math.min(...shoes.map((s) => s.lowestPrice));
      const bestDiscount = Math.max(...shoes.map((s) => s.discountPercent));
      const bestSavings = Math.max(...shoes.map((s) => s.savingsRupees));

      return (
        <div className="space-y-5">
          <div className="pr-10">
            <h3 id="modal-title" className="text-xl font-display font-bold text-appText">
              Side-by-Side Deal Matrix ({shoes.length} Kicks)
            </h3>
            <p className="text-xs text-appMuted">Comparative specs, price advantages, and retail coverage</p>
          </div>

          <div className="overflow-x-auto rounded-2xl border border-appBorder bg-appSurface">
            <table className="w-full text-xs text-left">
              <thead>
                <tr className="border-b border-appBorder bg-appCard">
                  <th className="p-3 text-appMuted font-display font-bold uppercase w-32">Metric</th>
                  {shoes.map((s) => (
                    <th key={s.id} className="p-3 min-w-[200px] font-display font-bold text-appText">
                      <div className="w-20 h-16 mx-auto mb-2 spotlight-pedestal rounded-xl p-1">
                        <SneakerImg src={s.imageUrl} alt="" />
                      </div>
                      <span className="block text-center line-clamp-1">{s.name}</span>
                    </th>
                  ))}
                </tr>
              </thead>
              <tbody className="divide-y divide-appBorder">
                <tr>
                  <td className="p-3 font-semibold text-appMuted">Brand</td>
                  {shoes.map((s) => <td key={s.id} className="p-3 text-center font-display font-bold">{s.brand}</td>)}
                </tr>
                <tr>
                  <td className="p-3 font-semibold text-appMuted">Lowest Deal</td>
                  {shoes.map((s) => (
                    <td key={s.id} className={`p-3 text-center font-mono font-bold text-sm ${s.lowestPrice === bestPrice ? 'text-accent bg-accent/5' : 'text-appText'}`}>
                      {inr(s.lowestPrice)}
                      {s.lowestPrice === bestPrice && <span className="block text-[11px] font-display text-accent font-bold">★ CHEAPEST</span>}
                    </td>
                  ))}
                </tr>
                <tr>
                  <td className="p-3 font-semibold text-appMuted">Total Savings</td>
                  {shoes.map((s) => (
                    <td key={s.id} className={`p-3 text-center font-mono font-bold ${s.savingsRupees === bestSavings ? 'text-accent' : 'text-appText'}`}>
                      Save {inr(s.savingsRupees)}
                      {s.savingsRupees === bestSavings && <span className="block text-[11px] font-display text-accent font-bold">★ BIGGEST SAVE</span>}
                    </td>
                  ))}
                </tr>
                <tr>
                  <td className="p-3 font-semibold text-appMuted">Original MRP</td>
                  {shoes.map((s) => <td key={s.id} className="p-3 text-center font-mono text-appMuted line-through">{inr(s.mrp)}</td>)}
                </tr>
                <tr>
                  <td className="p-3 font-semibold text-appMuted">Discount Rate</td>
                  {shoes.map((s) => (
                    <td key={s.id} className={`p-3 text-center font-display font-bold ${s.discountPercent === bestDiscount ? 'text-accent' : ''}`}>
                      {s.discountPercent}% OFF
                    </td>
                  ))}
                </tr>
                <tr>
                  <td className="p-3 font-semibold text-appMuted">Gender / Fit</td>
                  {shoes.map((s) => <td key={s.id} className="p-3 text-center font-semibold">{s.gender}</td>)}
                </tr>
                <tr>
                  <td className="p-3 font-semibold text-appMuted">Category</td>
                  {shoes.map((s) => <td key={s.id} className="p-3 text-center font-semibold">{s.category}</td>)}
                </tr>
                <tr>
                  <td className="p-3 font-semibold text-appMuted">Stores Tracked</td>
                  {shoes.map((s) => <td key={s.id} className="p-3 text-center font-mono">{s.retailers.length} Stores</td>)}
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      );
    }

    // --- Main App Root ---
    function App() {
      const [theme, setTheme] = useState(() => document.documentElement.dataset.theme || 'dark');

      const toggleTheme = () => {
        const next = theme === 'dark' ? 'light' : 'dark';
        setTheme(next);
        document.documentElement.dataset.theme = next;
        try { localStorage.setItem('sdf_theme', next); } catch (e) {}
      };

      // URL parameters read synchronously on first render
      const initialParams = useMemo(() => new URLSearchParams(window.location.search), []);
      const [search, setSearch] = useState(() => initialParams.get('q') || '');
      const [selectedBrand, setSelectedBrand] = useState(() => initialParams.get('brand') || 'All');
      const [selectedGender, setSelectedGender] = useState(() => initialParams.get('gender') || 'All');
      const [selectedSize, setSelectedSize] = useState(() => initialParams.get('size') || 'All');
      const [quickBucket, setQuickBucket] = useState(() => initialParams.get('bucket') || 'All');
      // Default to "savings" (Biggest Rupee Savings)
      const [sortBy, setSortBy] = useState(() => initialParams.get('sort') || 'savings');

      const [activeModal, setActiveModal] = useState(null);
      const [toastMsg, setToastMsg] = useState(null);
      const [compareList, setCompareList] = useState([]);
      const [mobileFilterOpen, setMobileFilterOpen] = useState(false);
      const [firstLoad, setFirstLoad] = useState(true);

      const searchInputRef = useRef(null);

      useEffect(() => {
        const timer = setTimeout(() => setFirstLoad(false), 900);
        return () => clearTimeout(timer);
      }, []);

      useEffect(() => {
        if (!toastMsg) return;
        const timer = setTimeout(() => setToastMsg(null), 3200);
        return () => clearTimeout(timer);
      }, [toastMsg]);

      useEffect(() => {
        const onGlobalKey = (e) => {
          if (e.key === '/' && document.activeElement !== searchInputRef.current && !['INPUT', 'TEXTAREA'].includes(document.activeElement.tagName)) {
            e.preventDefault();
            searchInputRef.current?.focus();
          }
        };
        window.addEventListener('keydown', onGlobalKey);
        return () => window.removeEventListener('keydown', onGlobalKey);
      }, []);

      const [watchlist, setWatchlist] = useState(() => {
        try { return JSON.parse(localStorage.getItem('sdf_favs')) || []; }
        catch { return []; }
      });

      useEffect(() => {
        try { localStorage.setItem('sdf_favs', JSON.stringify(watchlist)); } catch (e) {}
      }, [watchlist]);

      const toggleWatchlist = useCallback((id) => {
        const isRemoving = watchlist.includes(id);
        setWatchlist((prev) => (isRemoving ? prev.filter((item) => item !== id) : [...prev, id]));
        setToastMsg(isRemoving ? 'Removed from Watchlist' : 'Saved to Watchlist');
      }, [watchlist]);

      const toggleCompare = useCallback((id) => {
        setCompareList((prev) => {
          if (prev.includes(id)) return prev.filter((item) => item !== id);
          if (prev.length >= 3) {
            setToastMsg('Maximum 3 shoes can be compared side-by-side.');
            return prev;
          }
          return [...prev, id];
        });
      }, []);

      useEffect(() => {
        const shoeParam = initialParams.get('shoe');
        if (shoeParam) {
          const target = PROCESSED_SHOES.find((s) => s.id === shoeParam);
          if (target) setActiveModal({ type: 'detail', shoe: target });
        }
      }, [initialParams]);

      useEffect(() => {
        const url = new URL(window.location.href);
        if (search) url.searchParams.set('q', search); else url.searchParams.delete('q');
        if (selectedBrand !== 'All') url.searchParams.set('brand', selectedBrand); else url.searchParams.delete('brand');
        if (selectedGender !== 'All') url.searchParams.set('gender', selectedGender); else url.searchParams.delete('gender');
        if (selectedSize !== 'All') url.searchParams.set('size', selectedSize); else url.searchParams.delete('size');
        if (quickBucket !== 'All') url.searchParams.set('bucket', quickBucket); else url.searchParams.delete('bucket');
        if (sortBy !== 'savings') url.searchParams.set('sort', sortBy); else url.searchParams.delete('sort');

        if (activeModal?.shoe) url.searchParams.set('shoe', activeModal.shoe.id);
        else url.searchParams.delete('shoe');

        window.history.replaceState({}, '', url.toString());
      }, [search, selectedBrand, selectedGender, selectedSize, quickBucket, sortBy, activeModal]);

      const applyFilterWithTransition = (updateFn) => {
        if (document.startViewTransition && !window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
          document.startViewTransition(() => {
            ReactDOM.flushSync(updateFn);
          });
        } else {
          updateFn();
        }
      };

      // ==========================================
      // GENDER & BIGGEST SAVINGS & SORT BUG FIXES
      // ==========================================
      const filteredShoes = useMemo(() => {
        return PROCESSED_SHOES.filter((s) => {
          const q = search.toLowerCase().trim();
          const matchSearch = !q || s.name.toLowerCase().includes(q) || s.brand.toLowerCase().includes(q) || s.sku.toLowerCase().includes(q) || s.colorway.toLowerCase().includes(q) || s.category.toLowerCase().includes(q);
          const matchBrand = selectedBrand === 'All' || s.brand === selectedBrand;

          // GENDER BUG FIX:
          // In sneaker retail, Unisex shoes are suitable for BOTH Men and Women!
          // Selecting Men includes Men's + Unisex. Selecting Women includes Women's + Unisex.
          // Selecting Unisex shows purely unisex shoes.
          let matchGender = true;
          if (selectedGender === 'Men') {
            matchGender = s.gender === 'Men' || s.gender === 'Unisex';
          } else if (selectedGender === 'Women') {
            matchGender = s.gender === 'Women' || s.gender === 'Unisex';
          } else if (selectedGender === 'Unisex') {
            matchGender = s.gender === 'Unisex';
          }

          const matchSize = selectedSize === 'All' || s.retailers.some((r) => r.sizes.includes(selectedSize));

          // BIGGEST SAVINGS BUCKET FIX:
          let matchBucket = true;
          if (quickBucket === '50_off') matchBucket = s.discountPercent >= 50;
          if (quickBucket === 'savings_2500') matchBucket = s.savingsRupees >= 2500; // Rupee Savings >= 2500
          if (quickBucket === 'under_2500') matchBucket = s.lowestPrice <= 2500;
          if (quickBucket === 'under_5000') matchBucket = s.lowestPrice <= 5000;
          if (quickBucket === 'under_10000') matchBucket = s.lowestPrice <= 10000;
          if (quickBucket === 'saved') matchBucket = watchlist.includes(s.id);

          return matchSearch && matchBrand && matchGender && matchSize && matchBucket;
        }).sort((a, b) => {
          // SORT BUG FIX:
          // 'savings' sorts strictly by highest rupee savings (e.g. ₹4,080 > ₹3,600 > ₹3,000)
          if (sortBy === 'savings') {
            return (b.savingsRupees - a.savingsRupees) || (b.discountPercent - a.discountPercent) || (a.lowestPrice - b.lowestPrice);
          }
          // 'discount' sorts strictly by highest percentage (e.g. 55% > 50% > 45%)
          if (sortBy === 'discount') {
            return (b.discountPercent - a.discountPercent) || (b.savingsRupees - a.savingsRupees) || (a.lowestPrice - b.lowestPrice);
          }
          if (sortBy === 'priceLow') {
            return (a.lowestPrice - b.lowestPrice) || (b.savingsRupees - a.savingsRupees);
          }
          if (sortBy === 'priceHigh') {
            return (b.lowestPrice - a.lowestPrice) || (b.savingsRupees - a.savingsRupees);
          }
          if (sortBy === 'recentDrop') {
            return (b.dropAmount - a.dropAmount) || (b.savingsRupees - a.savingsRupees);
          }
          return 0;
        });
      }, [search, selectedBrand, selectedGender, selectedSize, quickBucket, sortBy, watchlist]);

      const availableBrands = useMemo(() => ['All', ...Array.from(new Set(PROCESSED_SHOES.map((s) => s.brand)))], []);
      const availableSizes = useMemo(() => {
        const set = new Set();
        PROCESSED_SHOES.forEach((s) => s.retailers.forEach((r) => r.sizes.forEach((sz) => set.add(sz))));
        return ['All', ...Array.from(set).sort((a, b) => parseFloat(a.replace(/[^\d.]/g, '')) - parseFloat(b.replace(/[^\d.]/g, '')))];
      }, []);
      const genders = ['All', 'Men', 'Women', 'Unisex'];

      const bucketLabels = {
        'All': 'All Deals',
        'savings_2500': '💰 ₹2,500+ Savings',
        '50_off': '🔥 50%+ Off',
        'under_2500': '⚡ Under ₹2,500',
        'under_5000': '🏷️ Under ₹5,000',
        'under_10000': '⭐ Under ₹10,000',
        'saved': '❤️ Watchlist'
      };

      const dealOfTheDay = useMemo(() => [...PROCESSED_SHOES].sort((a, b) => b.savingsRupees - a.savingsRupees)[0], []);

      return (
        <div className="flex flex-col min-h-screen">
          {/* Top Ticker */}
          <div className="bg-appCard border-b border-appBorder py-1.5 px-4 text-center text-xs font-mono text-appMuted flex items-center justify-center gap-3">
            <span><strong className="text-accent font-bold">LIVE INDEX:</strong> {PROCESSED_SHOES.length} kicks across Indian stores</span>
            <span className="hidden sm:inline">•</span>
            <span className="hidden sm:inline">Press <kbd className="px-1.5 py-0.5 rounded bg-appSurface border border-appBorder font-mono">/</kbd> to search</span>
          </div>

          {/* Navigation Bar */}
          <header className="sticky top-0 z-40 bg-appBg/90 backdrop-blur-xl border-b border-appBorder">
            <div className="max-w-7xl mx-auto px-4 sm:px-6 h-16 flex items-center justify-between gap-3">
              <div className="flex items-center gap-2.5 shrink-0">
                <div className="w-9 h-9 rounded-2xl bg-accent text-accentText font-display font-bold flex items-center justify-center text-base shadow-sm">
                  SF
                </div>
                <div className="flex flex-col">
                  <span className="font-display font-bold text-lg tracking-tight text-appText leading-none">
                    ShoeDealFinder
                  </span>
                  <span className="text-xs font-mono text-appMuted uppercase">Sneaker Tracker</span>
                </div>
              </div>

              {/* Central Search Bar */}
              <div className="flex-1 max-w-md mx-2">
                <div className="relative w-full">
                  <div className="absolute left-3.5 top-1/2 -translate-y-1/2 text-appMuted">
                    <IconSearch />
                  </div>
                  <input
                    ref={searchInputRef}
                    type="text"
                    value={search}
                    onChange={(e) => setSearch(e.target.value)}
                    placeholder="Search Samba, Kayano, Dunk, SKU... (/)"
                    aria-label="Search sneakers by model, brand, colorway, or SKU"
                    className="w-full pl-9 pr-8 py-2 rounded-2xl bg-appCard border border-appBorder text-xs text-appText placeholder:text-appMuted focus:outline-none focus:border-accent transition font-semibold"
                  />
                  {search && (
                    <button
                      onClick={() => setSearch('')}
                      className="absolute right-2.5 top-1/2 -translate-y-1/2 text-appMuted hover:text-appText"
                    >
                      <IconClose />
                    </button>
                  )}
                </div>
              </div>

              {/* Theme & Watchlist Quick Controls */}
              <div className="flex items-center gap-2 shrink-0">
                <button
                  type="button"
                  onClick={toggleTheme}
                  title="Toggle Theme"
                  aria-label="Toggle between dark and light paper theme"
                  className="p-2 rounded-2xl bg-appCard border border-appBorder text-appMuted hover:text-appText transition"
                >
                  {theme === 'dark' ? <IconSun /> : <IconMoon />}
                </button>

                <button
                  type="button"
                  onClick={() => setQuickBucket(quickBucket === 'saved' ? 'All' : 'saved')}
                  aria-label={`View watchlist with ${watchlist.length} items`}
                  className={`flex items-center gap-1.5 px-3 py-2 rounded-2xl text-xs font-display font-bold border transition ${
                    quickBucket === 'saved' ? 'bg-accent text-accentText border-accent' : 'bg-appCard text-appText border-appBorder hover:bg-appSurface'
                  }`}
                >
                  <IconHeart filled={quickBucket === 'saved'} />
                  <span className="hidden sm:inline">Watchlist</span>
                  <span className="font-mono text-xs px-1.5 py-0.5 rounded-lg bg-appSurface border border-appBorder">
                    {watchlist.length}
                  </span>
                </button>
              </div>
            </div>
          </header>

          {/* Hero Banner */}
          <section className="py-10 sm:py-14 px-4 sm:px-6 border-b border-appBorder relative overflow-hidden bg-appBg">
            <div className="max-w-7xl mx-auto grid grid-cols-1 lg:grid-cols-12 gap-8 items-center">
              <div className="lg:col-span-7 space-y-4 text-center lg:text-left">
                <div className="inline-flex items-center gap-2 px-3 py-1 rounded-full text-xs font-mono font-bold bg-accent/15 border border-accent/25 text-accent">
                  <span>●</span> {PROCESSED_SHOES.length} Curated Sneaker Pairs Tracked
                </div>

                <h1 className="text-3xl sm:text-5xl font-display font-bold text-appText tracking-tight leading-tight">
                  Pay The True Price. <br />
                  <span className="text-accent underline decoration-accent/40 decoration-4">Skip Retail Markups.</span>
                </h1>

                <p className="text-appMuted text-xs sm:text-sm max-w-lg mx-auto lg:mx-0 leading-relaxed font-semibold">
                  Sample deal engine scanning Indian sneaker boutiques & official brand stores.
                </p>

                {/* Quick Buckets */}
                <div className="flex flex-wrap justify-center lg:justify-start gap-2 pt-2">
                  {Object.entries(bucketLabels).map(([key, label]) => (
                    <button
                      key={key}
                      type="button"
                      onClick={() => applyFilterWithTransition(() => setQuickBucket(key))}
                      className={`px-3.5 py-1.5 rounded-2xl text-xs font-display font-bold border transition-all ${
                        quickBucket === key ? 'bg-accent text-accentText border-accent shadow-md' : 'bg-appCard text-appText border-appBorder hover:bg-appSurface'
                      }`}
                    >
                      {label}
                    </button>
                  ))}
                </div>
              </div>

              {/* Deal of the Day Spotlight Button */}
              {dealOfTheDay && (
                <div className="lg:col-span-5">
                  <button
                    type="button"
                    onClick={() => setActiveModal({ type: 'detail', shoe: dealOfTheDay })}
                    aria-label={`Featured steal of the day: ${dealOfTheDay.name}`}
                    className="w-full text-left p-5 rounded-3xl bg-appCard border border-accent/40 hover:border-accent transition shadow-2xl relative group focus:outline-none focus-visible:ring-2 focus-visible:ring-accent"
                  >
                    <div className="flex items-center justify-between mb-2">
                      <span className="text-xs font-display font-bold px-2 py-0.5 rounded-lg bg-accent text-accentText">
                        FEATURED STEAL OF THE DAY
                      </span>
                      <span className="text-xs font-mono text-appMuted">SKU: {dealOfTheDay.sku}</span>
                    </div>

                    <div className="grid grid-cols-2 gap-4 items-center mt-3">
                      <div className="aspect-[4/3] rounded-2xl spotlight-pedestal p-2 overflow-hidden flex items-center justify-center">
                        <SneakerImg src={dealOfTheDay.imageUrl} alt={dealOfTheDay.name} className="group-hover:scale-105" />
                      </div>
                      <div className="space-y-1">
                        <span className="text-xs font-display font-bold uppercase text-appMuted">{dealOfTheDay.brand}</span>
                        <h4 className="text-sm font-display font-bold text-appText line-clamp-2">{dealOfTheDay.name}</h4>
                        <div className="flex items-baseline gap-2 pt-1">
                          <span className="text-xl font-mono font-extrabold text-accent">{inr(dealOfTheDay.lowestPrice)}</span>
                          <span className="text-xs font-mono text-appMuted line-through">{inr(dealOfTheDay.mrp)}</span>
                        </div>
                        <span className="text-xs font-sans font-bold text-trendDrop block">Save {inr(dealOfTheDay.savingsRupees)} ({dealOfTheDay.discountPercent}% OFF)</span>
                      </div>
                    </div>
                  </button>
                </div>
              )}
            </div>
          </section>

          {/* Sticky Filtering Bar with Gender & Sort Fix */}
          <div className="sticky top-16 z-30 bg-appBg/95 backdrop-blur-md border-b border-appBorder py-3.5 px-4 sm:px-6 shadow-sm">
            <div className="max-w-7xl mx-auto flex items-center justify-between gap-3">
              {/* Desktop Horizontal Brand Scrollbar */}
              <div className="hidden md:flex items-center gap-1.5 overflow-x-auto no-scrollbar py-0.5">
                <span className="text-xs font-display font-bold text-appMuted uppercase tracking-wider mr-1 shrink-0">Brand:</span>
                {availableBrands.map((b) => (
                  <button
                    key={b}
                    type="button"
                    onClick={() => applyFilterWithTransition(() => setSelectedBrand(b))}
                    className={`px-3 py-1 rounded-xl text-xs font-display font-bold border shrink-0 transition ${
                      selectedBrand === b ? 'bg-accent text-accentText border-accent' : 'bg-appCard text-appText border-appBorder hover:bg-appSurface'
                    }`}
                  >
                    {b}
                  </button>
                ))}
              </div>

              {/* Mobile Filter Button */}
              <div className="md:hidden flex items-center gap-2">
                <button
                  type="button"
                  onClick={() => setMobileFilterOpen(true)}
                  className="px-3.5 py-2 rounded-2xl bg-appCard border border-appBorder text-xs font-display font-bold text-appText flex items-center gap-1.5"
                >
                  <IconFilter /> Filters
                  {((selectedBrand !== 'All' ? 1 : 0) + (selectedGender !== 'All' ? 1 : 0) + (selectedSize !== 'All' ? 1 : 0)) > 0 && (
                    <span className="w-5 h-5 rounded-full bg-accent text-accentText text-[11px] font-mono flex items-center justify-center">
                      {(selectedBrand !== 'All' ? 1 : 0) + (selectedGender !== 'All' ? 1 : 0) + (selectedSize !== 'All' ? 1 : 0)}
                    </span>
                  )}
                </button>
              </div>

              {/* Desktop Filters (Gender, Size, Sort) */}
              <div className="hidden sm:flex items-center gap-3 shrink-0">
                <div className="flex items-center gap-1.5">
                  <span className="text-xs font-display font-bold text-appMuted">Gender:</span>
                  <select
                    aria-label="Filter by gender"
                    value={selectedGender}
                    onChange={(e) => applyFilterWithTransition(() => setSelectedGender(e.target.value))}
                    className="px-2.5 py-1.5 rounded-xl bg-appCard border border-appBorder text-xs text-appText font-semibold focus:outline-none focus:border-accent"
                  >
                    {genders.map((g) => <option key={g} value={g}>{g}</option>)}
                  </select>
                </div>

                <div className="flex items-center gap-1.5">
                  <span className="text-xs font-display font-bold text-appMuted">Size:</span>
                  <select
                    aria-label="Filter by shoe size"
                    value={selectedSize}
                    onChange={(e) => applyFilterWithTransition(() => setSelectedSize(e.target.value))}
                    className="px-2.5 py-1.5 rounded-xl bg-appCard border border-appBorder text-xs text-appText font-semibold focus:outline-none focus:border-accent"
                  >
                    {availableSizes.map((s) => <option key={s} value={s}>{s}</option>)}
                  </select>
                </div>

                <div className="flex items-center gap-1.5">
                  <span className="text-xs font-display font-bold text-appMuted">Sort:</span>
                  <select
                    aria-label="Sort sneakers by criteria"
                    value={sortBy}
                    onChange={(e) => applyFilterWithTransition(() => setSortBy(e.target.value))}
                    className="px-2.5 py-1.5 rounded-xl bg-appCard border border-appBorder text-xs text-appText font-semibold focus:outline-none focus:border-accent"
                  >
                    <option value="savings">Biggest Savings (₹ Amount)</option>
                    <option value="discount">Highest Discount (%)</option>
                    <option value="priceLow">Price: Low to High</option>
                    <option value="priceHigh">Price: High to Low</option>
                    <option value="recentDrop">Biggest Recent Drop</option>
                  </select>
                </div>
              </div>
            </div>

            {/* Active Filters Pill Bar */}
            {(selectedBrand !== 'All' || selectedGender !== 'All' || selectedSize !== 'All' || quickBucket !== 'All' || search) && (
              <div className="max-w-7xl mx-auto flex items-center gap-2 pt-2.5 flex-wrap text-xs">
                <span className="font-mono text-appMuted font-bold">{filteredShoes.length} kicks match:</span>
                {selectedBrand !== 'All' && (
                  <button onClick={() => applyFilterWithTransition(() => setSelectedBrand('All'))} className="px-2.5 py-0.5 rounded-xl bg-appSurface border border-appBorder text-appText hover:border-accent flex items-center gap-1 font-semibold">
                    Brand: {selectedBrand} ✕
                  </button>
                )}
                {selectedGender !== 'All' && (
                  <button onClick={() => applyFilterWithTransition(() => setSelectedGender('All'))} className="px-2.5 py-0.5 rounded-xl bg-appSurface border border-appBorder text-appText hover:border-accent flex items-center gap-1 font-semibold">
                    Gender: {selectedGender} {selectedGender !== 'Unisex' ? '(+ Unisex)' : ''} ✕
                  </button>
                )}
                {selectedSize !== 'All' && (
                  <button onClick={() => applyFilterWithTransition(() => setSelectedSize('All'))} className="px-2.5 py-0.5 rounded-xl bg-appSurface border border-appBorder text-appText hover:border-accent flex items-center gap-1 font-semibold">
                    Size: {selectedSize} ✕
                  </button>
                )}
                {quickBucket !== 'All' && (
                  <button onClick={() => applyFilterWithTransition(() => setQuickBucket('All'))} className="px-2.5 py-0.5 rounded-xl bg-appSurface border border-appBorder text-appText hover:border-accent flex items-center gap-1 font-semibold">
                    {bucketLabels[quickBucket]} ✕
                  </button>
                )}
                {search && (
                  <button onClick={() => applyFilterWithTransition(() => setSearch(''))} className="px-2.5 py-0.5 rounded-xl bg-appSurface border border-appBorder text-appText hover:border-accent flex items-center gap-1 font-semibold">
                    Query: "{search}" ✕
                  </button>
                )}
                <button
                  type="button"
                  onClick={() => applyFilterWithTransition(() => {
                    setSelectedBrand('All');
                    setSelectedGender('All');
                    setSelectedSize('All');
                    setQuickBucket('All');
                    setSearch('');
                  })}
                  className="text-xs text-accent font-display font-bold hover:underline ml-2"
                >
                  Clear all
                </button>
              </div>
            )}
          </div>

          {/* Mobile Filter Drawer */}
          {mobileFilterOpen && (
            <div
              onClick={() => setMobileFilterOpen(false)}
              className="fixed inset-0 z-50 flex items-end justify-center bg-black/70 backdrop-blur-sm sm:hidden animate-backdrop"
              role="dialog"
              aria-modal="true"
            >
              <div
                onClick={(e) => e.stopPropagation()}
                className="w-full bg-appCard border-t border-appBorder rounded-t-3xl p-6 text-appText max-h-[85dvh] overflow-y-auto space-y-4 animate-modal"
              >
                <div className="w-12 h-1.5 rounded-full bg-appBorder mx-auto mb-2"></div>
                <div className="flex items-center justify-between">
                  <h3 className="font-display font-bold text-lg">Filter & Sort</h3>
                  <button onClick={() => setMobileFilterOpen(false)} className="p-1.5 rounded-lg bg-appSurface">
                    <IconClose />
                  </button>
                </div>

                <div>
                  <label className="text-xs font-display font-bold text-appMuted block mb-1">Brand</label>
                  <select value={selectedBrand} onChange={(e) => setSelectedBrand(e.target.value)} className="w-full p-2.5 rounded-xl bg-appSurface border border-appBorder text-xs text-appText font-semibold">
                    {availableBrands.map((b) => <option key={b} value={b}>{b}</option>)}
                  </select>
                </div>

                <div>
                  <label className="text-xs font-display font-bold text-appMuted block mb-1">Gender</label>
                  <select value={selectedGender} onChange={(e) => setSelectedGender(e.target.value)} className="w-full p-2.5 rounded-xl bg-appSurface border border-appBorder text-xs text-appText font-semibold">
                    {genders.map((g) => <option key={g} value={g}>{g}</option>)}
                  </select>
                </div>

                <div>
                  <label className="text-xs font-display font-bold text-appMuted block mb-1">UK Size</label>
                  <select value={selectedSize} onChange={(e) => setSelectedSize(e.target.value)} className="w-full p-2.5 rounded-xl bg-appSurface border border-appBorder text-xs text-appText font-semibold">
                    {availableSizes.map((s) => <option key={s} value={s}>{s}</option>)}
                  </select>
                </div>

                <div>
                  <label className="text-xs font-display font-bold text-appMuted block mb-1">Sort By</label>
                  <select value={sortBy} onChange={(e) => setSortBy(e.target.value)} className="w-full p-2.5 rounded-xl bg-appSurface border border-appBorder text-xs text-appText font-semibold">
                    <option value="savings">Biggest Savings (₹ Amount)</option>
                    <option value="discount">Highest Discount (%)</option>
                    <option value="priceLow">Price: Low to High</option>
                    <option value="priceHigh">Price: High to Low</option>
                    <option value="recentDrop">Biggest Recent Drop</option>
                  </select>
                </div>

                <button
                  type="button"
                  onClick={() => setMobileFilterOpen(false)}
                  className="w-full py-3 rounded-xl bg-accent text-accentText font-display font-bold text-xs"
                >
                  Apply Filters ({filteredShoes.length} pairs)
                </button>
              </div>
            </div>
          )}

          {/* Main Sneaker Gallery */}
          <main className="max-w-7xl mx-auto px-4 sm:px-6 py-8 flex-1 w-full pb-32">
            {filteredShoes.length > 0 ? (
              <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
                {filteredShoes.map((shoe, idx) => (
                  <SneakerCard
                    key={shoe.id}
                    shoe={shoe}
                    idx={idx}
                    isFirstMount={firstLoad}
                    isSaved={watchlist.includes(shoe.id)}
                    isComparing={compareList.includes(shoe.id)}
                    onCardClick={(s) => setActiveModal({ type: 'detail', shoe: s })}
                    onToggleWatchlist={toggleWatchlist}
                    onToggleCompare={toggleCompare}
                    onOpenAlert={(s) => setActiveModal({ type: 'alert', shoe: s })}
                  />
                ))}
              </div>
            ) : (
              <div className="text-center py-20 border-2 border-dashed border-appBorder rounded-3xl bg-appCard/50 p-6">
                <div className="w-12 h-12 mx-auto rounded-2xl bg-appSurface text-appMuted flex items-center justify-center mb-3">
                  <IconSearch />
                </div>
                <h3 className="text-base font-display font-bold text-appText">No kicks match your criteria</h3>
                <p className="text-xs text-appMuted mt-1 max-w-sm mx-auto font-semibold">
                  Try clearing your search term, switching gender/size, or browsing our ₹2,500+ savings tier.
                </p>
                <div className="flex flex-wrap justify-center gap-2 mt-4">
                  <button
                    type="button"
                    onClick={() => { setSelectedBrand('All'); setSelectedGender('All'); setSelectedSize('All'); setQuickBucket('savings_2500'); setSearch(''); }}
                    className="px-3.5 py-1.5 rounded-xl text-xs font-display font-bold bg-appSurface text-appText border border-appBorder"
                  >
                    Try ₹2,500+ Savings
                  </button>
                  <button
                    type="button"
                    onClick={() => { setSelectedBrand('All'); setSelectedGender('All'); setSelectedSize('All'); setQuickBucket('All'); setSearch(''); }}
                    className="px-3.5 py-1.5 rounded-xl text-xs font-display font-bold bg-accent text-accentText hover:bg-accentHover transition"
                  >
                    Reset All Filters
                  </button>
                </div>
              </div>
            )}
          </main>

          {/* Floating Compare Tray */}
          {compareList.length > 0 && (
            <div className="fixed bottom-6 left-1/2 -translate-x-1/2 z-40 flex items-center gap-3 px-5 py-3 rounded-2xl bg-appCard/95 backdrop-blur-xl border border-accent/40 shadow-2xl animate-tray text-xs font-display">
              <span className="font-bold text-appText">Comparing ({compareList.length}/3) kicks</span>
              <button
                type="button"
                onClick={() => {
                  const items = PROCESSED_SHOES.filter((s) => compareList.includes(s.id));
                  setActiveModal({ type: 'compare', compareItems: items });
                }}
                className="px-3.5 py-1.5 rounded-xl font-bold bg-accent text-accentText hover:bg-accentHover transition flex items-center gap-1 shadow-sm"
              >
                <IconCompare /> View Side-by-Side
              </button>
              <button
                type="button"
                onClick={() => setCompareList([])}
                className="text-appMuted hover:text-appText p-1"
                title="Clear Compare Tray"
              >
                <IconClose />
              </button>
            </div>
          )}

          {/* Unified Accessible Modal Host */}
          <ModalManager
            activeModal={activeModal}
            onCloseModal={() => setActiveModal(null)}
            onSwitchModal={(next) => setActiveModal(next)}
            watchlist={watchlist}
            onToggleWatchlist={toggleWatchlist}
            onTriggerToast={(msg) => setToastMsg(msg)}
          />

          {/* Accessible Toast Notification */}
          {toastMsg && (
            <div
              role="status"
              aria-live="polite"
              className="fixed bottom-6 right-6 z-50 flex items-center gap-2.5 px-4 py-3 rounded-2xl bg-appCard border border-accent/40 text-appText shadow-2xl text-xs font-display font-bold animate-modal"
            >
              <IconCheck />
              <span>{toastMsg}</span>
              <button type="button" onClick={() => setToastMsg(null)} className="ml-2 text-appMuted hover:text-appText">
                <IconClose />
              </button>
            </div>
          )}

          {/* Footer */}
          <footer className="border-t border-appBorder bg-appCard text-appMuted text-xs py-10 px-4 sm:px-6">
            <div className="max-w-7xl mx-auto space-y-4">
              <div className="flex flex-col sm:flex-row items-center justify-between gap-4">
                <div className="flex items-center gap-2">
                  <span className="font-display font-bold text-base text-appText">ShoeDealFinder</span>
                  <span className="font-semibold">— Sample Indian sneaker deal tracker</span>
                </div>
                <div className="flex gap-4 text-xs font-mono font-bold">
                  <span>Tracked: {PROCESSED_SHOES.length} pairs</span>
                  <span>Currency: INR (₹)</span>
                </div>
              </div>
              <p className="text-xs text-appMuted/80 text-center sm:text-left leading-relaxed font-semibold">
                Affiliate & Trademark Notice: ShoeDealFinder is an independent demonstration price search engine. Retailer names, trademarks, and silhouettes belong to their respective brand owners. Store links direct to partner search results.
              </p>
            </div>
          </footer>
        </div>
      );
    }

    ReactDOM.createRoot(document.getElementById('root')).render(<App />);
  </script>
</body>
</html>
