<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>🌿 EcoShop - Telegram Mini App</title>
    
    <!-- Telegram WebApp SDK -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css" />
    
    <style>
        /* ============================================================
           ALL STYLES - Complete CSS
           ============================================================ */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        :root {
            --primary: #1a1a2e;
            --secondary: #e94560;
            --accent: #22c55e;
            --gold: #fbbf24;
            --bg: #f8f9fa;
            --white: #ffffff;
            --gray: #6b7280;
            --light-gray: #e5e7eb;
            --radius: 16px;
            --shadow: 0 4px 24px rgba(0, 0, 0, 0.08);
            --safe-bottom: env(safe-area-inset-bottom, 0px);
            --transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background: var(--bg);
            color: var(--primary);
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* --- APP CONTAINER --- */
        .app {
            max-width: 430px;
            margin: 0 auto;
            background: var(--white);
            min-height: 100vh;
            padding-bottom: calc(90px + var(--safe-bottom));
            position: relative;
        }

        /* --- LOADING SCREEN --- */
        .loading-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background: var(--white);
            gap: 20px;
        }
        .loading-screen .spinner {
            width: 48px;
            height: 48px;
            border: 4px solid var(--light-gray);
            border-top-color: var(--secondary);
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        .loading-screen .brand {
            font-size: 24px;
            font-weight: 800;
            letter-spacing: -0.5px;
        }
        .loading-screen .brand span { color: var(--secondary); }
        .loading-screen .sub {
            color: var(--gray);
            font-size: 14px;
        }

        /* --- STICKY NAV --- */
        .sticky-nav {
            position: sticky;
            top: 0;
            z-index: 100;
            background: rgba(255, 255, 255, 0.92);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            padding: 10px 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(0, 0, 0, 0.04);
            transition: all var(--transition);
            min-height: 56px;
        }
        .sticky-nav.shrunk {
            padding: 4px 16px;
            min-height: 44px;
        }
        .sticky-nav .logo {
            display: flex;
            align-items: center;
            gap: 8px;
            font-weight: 800;
            font-size: 18px;
            letter-spacing: -0.5px;
        }
        .sticky-nav .logo svg { flex-shrink: 0; }
        .sticky-nav .nav-center {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .sticky-nav .impact-badge {
            font-size: 12px;
            font-weight: 600;
            color: var(--accent);
            background: rgba(34, 197, 94, 0.12);
            padding: 4px 12px;
            border-radius: 20px;
            white-space: nowrap;
        }
        .sticky-nav .nav-right {
            display: flex;
            gap: 6px;
            align-items: center;
        }
        .sticky-nav .nav-btn {
            background: none;
            border: none;
            font-size: 20px;
            cursor: pointer;
            padding: 6px;
            border-radius: 50%;
            transition: background 0.2s;
            position: relative;
            color: var(--primary);
        }
        .sticky-nav .nav-btn:active { background: rgba(0, 0, 0, 0.05); }
        .sticky-nav .cart-badge {
            position: absolute;
            top: 0;
            right: 0;
            background: var(--secondary);
            color: white;
            font-size: 10px;
            font-weight: 700;
            min-width: 18px;
            height: 18px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transform: translate(25%, -25%);
            padding: 0 4px;
        }

        /* --- HERO SECTION --- */
        .hero {
            position: relative;
            height: 80vh;
            min-height: 500px;
            overflow: hidden;
            background: var(--primary);
        }
        .hero-bg-placeholder {
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 60px;
        }
        .hero-overlay {
            position: absolute;
            inset: 0;
            background: linear-gradient(to top, rgba(0, 0, 0, 0.8) 0%, transparent 50%);
        }
        .hero-content {
            position: absolute;
            bottom: 15%;
            left: 0;
            right: 0;
            padding: 0 24px;
            color: white;
        }
        .hero-badge {
            display: inline-block;
            background: rgba(255, 255, 255, 0.12);
            backdrop-filter: blur(10px);
            padding: 4px 14px;
            border-radius: 50px;
            font-size: 11px;
            font-weight: 600;
            letter-spacing: 1px;
            text-transform: uppercase;
            margin-bottom: 12px;
        }
        .hero-content h1 {
            font-size: 38px;
            font-weight: 800;
            line-height: 1.1;
            margin-bottom: 8px;
            letter-spacing: -1px;
        }
        .hero-content .subtitle {
            font-size: 15px;
            opacity: 0.85;
            margin-bottom: 20px;
            max-width: 280px;
            line-height: 1.5;
        }
        .cta-primary {
            background: var(--secondary);
            color: white;
            border: none;
            padding: 14px 32px;
            border-radius: 50px;
            font-size: 16px;
            font-weight: 700;
            cursor: pointer;
            transition: transform 0.15s ease, box-shadow 0.2s;
            box-shadow: 0 8px 32px rgba(233, 69, 96, 0.35);
        }
        .cta-primary:active { transform: scale(0.96); }

        /* --- TRUST BAR --- */
        .trust-bar {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 4px;
            padding: 12px 8px;
            background: var(--bg);
            border-bottom: 1px solid rgba(0, 0, 0, 0.04);
        }
        .trust-item {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 6px;
            font-size: 11px;
            font-weight: 500;
            padding: 4px 0;
            color: var(--gray);
        }
        .trust-item .icon { font-size: 16px; }

        /* --- PRODUCT GRID --- */
        .product-section {
            padding: 20px 16px 12px;
        }
        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 16px;
        }
        .section-header h2 {
            font-size: 20px;
            font-weight: 700;
        }
        .section-header .view-all {
            background: none;
            border: none;
            color: var(--secondary);
            font-weight: 600;
            font-size: 13px;
            cursor: pointer;
        }

        .product-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
        }
        .product-card {
            background: var(--white);
            border-radius: var(--radius);
            overflow: hidden;
            box-shadow: var(--shadow);
            transition: transform 0.15s ease;
            border: 1px solid rgba(0, 0, 0, 0.04);
        }
        .product-card:active { transform: scale(0.97); }
        .product-card .image-wrap {
            position: relative;
            padding-top: 100%;
            background: #f3f4f6;
            overflow: hidden;
        }
        .product-card .image-wrap img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .product-card .badge {
            position: absolute;
            top: 8px;
            left: 8px;
            background: var(--secondary);
            color: white;
            font-size: 10px;
            font-weight: 700;
            padding: 2px 10px;
            border-radius: 20px;
        }
        .product-card .info {
            padding: 10px 12px 12px;
        }
        .product-card .info .name {
            font-size: 13px;
            font-weight: 600;
            margin-bottom: 2px;
            line-height: 1.3;
        }
        .product-card .info .price {
            font-size: 16px;
            font-weight: 700;
            color: var(--secondary);
            margin-bottom: 6px;
        }
        .product-card .info .price .original {
            font-size: 12px;
            color: var(--gray);
            text-decoration: line-through;
            font-weight: 400;
            margin-left: 6px;
        }
        .product-card .swatches {
            display: flex;
            gap: 4px;
            margin-bottom: 8px;
            flex-wrap: wrap;
        }
        .product-card .swatches .dot {
            width: 18px;
            height: 18px;
            border-radius: 50%;
            border: 2px solid transparent;
            cursor: pointer;
            transition: border-color 0.2s;
        }
        .product-card .swatches .dot.active {
            border-color: var(--primary);
        }
        .quick-add-btn {
            width: 100%;
            background: var(--primary);
            color: white;
            border: none;
            padding: 9px;
            border-radius: 8px;
            font-weight: 600;
            font-size: 12px;
            cursor: pointer;
            transition: background 0.2s;
        }
        .quick-add-btn:active { background: #2d2d44; }
        .quick-add-btn.added { background: var(--accent); }

        /* --- SOCIAL PROOF --- */
        .social-proof {
            padding: 24px 16px 16px;
            background: var(--bg);
            text-align: center;
            margin-top: 8px;
        }
        .social-proof .stars {
            font-size: 28px;
            letter-spacing: 3px;
            margin-bottom: 6px;
        }
        .social-proof .review-text {
            font-size: 18px;
            font-weight: 500;
            line-height: 1.5;
            max-width: 400px;
            margin: 0 auto 8px;
        }
        .social-proof .review-author {
            font-weight: 600;
            color: var(--primary);
        }
        .social-proof .review-count {
            font-size: 13px;
            color: var(--gray);
        }
        .social-proof .ugc-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 6px;
            margin-top: 12px;
        }
        .social-proof .ugc-grid img {
            width: 100%;
            aspect-ratio: 1;
            object-fit: cover;
            border-radius: 8px;
        }

        /* --- BOTTOM CART BAR (Thumb Zone) --- */
        .bottom-bar {
            position: fixed;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 100%;
            max-width: 430px;
            background: rgba(255, 255, 255, 0.97);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            padding: 10px 16px;
            padding-bottom: calc(10px + var(--safe-bottom));
            box-shadow: 0 -4px 30px rgba(0, 0, 0, 0.08);
            display: none;
            justify-content: space-between;
            align-items: center;
            border-top: 1px solid rgba(0, 0, 0, 0.04);
            z-index: 200;
            animation: slideUp 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .bottom-bar.visible { display: flex; }
        @keyframes slideUp {
            from { transform: translateX(-50%) translateY(100%); }
            to { transform: translateX(-50%) translateY(0); }
        }
        .bottom-bar .summary {
            display: flex;
            flex-direction: column;
            gap: 1px;
        }
        .bottom-bar .summary .count {
            font-size: 12px;
            color: var(--gray);
        }
        .bottom-bar .summary .total {
            font-size: 20px;
            font-weight: 700;
        }
        .bottom-bar .checkout-btn {
            background: var(--secondary);
            color: white;
            border: none;
            padding: 12px 28px;
            border-radius: 50px;
            font-weight: 700;
            font-size: 15px;
            cursor: pointer;
            min-width: 120px;
            transition: transform 0.15s ease;
            box-shadow: 0 4px 20px rgba(233, 69, 96, 0.3);
        }
        .bottom-bar .checkout-btn:active { transform: scale(0.95); }

        /* --- TOAST NOTIFICATION --- */
        .toast {
            position: fixed;
            top: 70px;
            left: 50%;
            transform: translateX(-50%) translateY(-20px);
            background: var(--primary);
            color: white;
            padding: 12px 20px;
            border-radius: 12px;
            font-weight: 500;
            font-size: 14px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
            z-index: 300;
            opacity: 0;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            pointer-events: none;
            max-width: 90%;
        }
        .toast.show {
            opacity: 1;
            transform: translateX(-50%) translateY(0);
        }
        .toast .toast-icon { margin-right: 8px; }

        /* --- CART DRAWER --- */
        .drawer-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.4);
            z-index: 150;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
        }
        .drawer-overlay.open {
            opacity: 1;
            visibility: visible;
        }
        .drawer {
            position: fixed;
            right: 0;
            top: 0;
            bottom: 0;
            width: 85%;
            max-width: 360px;
            background: var(--white);
            z-index: 160;
            padding: 20px;
            transform: translateX(100%);
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            overflow-y: auto;
            box-shadow: -8px 0 40px rgba(0, 0, 0, 0.1);
        }
        .drawer.open { transform: translateX(0); }
        .drawer .drawer-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        .drawer .drawer-header h3 {
            font-size: 20px;
            font-weight: 700;
        }
        .drawer .drawer-close {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            padding: 4px;
            color: var(--gray);
        }
        .drawer .drawer-item {
            display: flex;
            gap: 12px;
            padding: 12px 0;
            border-bottom: 1px solid var(--light-gray);
            align-items: center;
        }
        .drawer .drawer-item img {
            width: 56px;
            height: 56px;
            object-fit: cover;
            border-radius: 8px;
            flex-shrink: 0;
        }
        .drawer .drawer-item .item-info { flex: 1; }
        .drawer .drawer-item .item-info .item-name {
            font-weight: 600;
            font-size: 14px;
        }
        .drawer .drawer-item .item-info .item-price {
            color: var(--secondary);
            font-weight: 700;
        }
        .drawer .drawer-item .item-qty {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .drawer .drawer-item .item-qty button {
            background: var(--light-gray);
            border: none;
            width: 28px;
            height: 28px;
            border-radius: 50%;
            font-size: 16px;
            font-weight: 700;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .drawer .drawer-item .item-qty button:active { background: #d1d5db; }
        .drawer .drawer-total {
            padding: 16px 0;
            font-size: 18px;
            font-weight: 700;
            display: flex;
            justify-content: space-between;
            border-top: 2px solid var(--primary);
            margin-top: 8px;
        }
        .drawer .drawer-checkout {
            width: 100%;
            padding: 14px;
            background: var(--secondary);
            color: white;
            border: none;
            border-radius: 12px;
            font-weight: 700;
            font-size: 16px;
            cursor: pointer;
            margin-top: 8px;
        }
        .drawer .drawer-empty {
            text-align: center;
            padding: 40px 0;
            color: var(--gray);
        }
        .drawer .drawer-empty .icon {
            font-size: 48px;
            margin-bottom: 12px;
        }

        /* --- RESPONSIVE --- */
        @media (max-width: 380px) {
            .hero-content h1 { font-size: 30px; }
            .product-card .info .name { font-size: 12px; }
            .product-card .info .price { font-size: 14px; }
            .sticky-nav .impact-badge { font-size: 10px; padding: 2px 8px; }
        }
        @media (min-width: 431px) {
            .app {
                border-left: 1px solid rgba(0, 0, 0, 0.04);
                border-right: 1px solid rgba(0, 0, 0, 0.04);
            }
            .bottom-bar {
                border-left: 1px solid rgba(0, 0, 0, 0.04);
                border-right: 1px solid rgba(0, 0, 0, 0.04);
            }
        }

        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--light-gray); border-radius: 10px; }
    </style>
</head>
<body>

    <!-- ============================================================
    HTML STRUCTURE
    ============================================================ -->

    <!-- Toast Notification -->
    <div class="toast" id="toast"></div>

    <!-- Loading Screen -->
    <div class="loading-screen" id="loadingScreen">
        <div class="spinner"></div>
        <div class="brand">Eco<span>Shop</span></div>
        <div class="sub">Loading your experience...</div>
    </div>

    <!-- Main App -->
    <div class="app" id="app" style="display:none;">

        <!-- Sticky Nav -->
        <nav class="sticky-nav" id="stickyNav">
            <div class="logo">
                <svg width="28" height="28" viewBox="0 0 32 32" fill="none">
                    <rect width="32" height="32" rx="8" fill="#1a1a2e"/>
                    <path d="M8 12L16 20L24 12" stroke="white" stroke-width="3" stroke-linecap="round"/>
                </svg>
                EcoShop
            </div>
            <div class="nav-center">
                <span class="impact-badge">🌱 Impact</span>
            </div>
            <div class="nav-right">
                <button class="nav-btn" id="searchBtn" aria-label="Search">
                    <i class="fas fa-search"></i>
                </button>
                <button class="nav-btn" id="cartBtn" aria-label="Cart">
                    <i class="fas fa-shopping-bag"></i>
                    <span class="cart-badge" id="navCartBadge" style="display:none;">0</span>
                </button>
            </div>
        </nav>

        <!-- Hero -->
        <section class="hero">
            <div class="hero-bg-placeholder">🌿</div>
            <div class="hero-overlay"></div>
            <div class="hero-content">
                <div class="hero-badge">✦ New Collection</div>
                <h1>THE NEW<br />STANDARD</h1>
                <p class="subtitle">Premium, sustainable essentials for everyday life.</p>
                <button class="cta-primary" id="shopNowBtn">Shop Collection →</button>
            </div>
        </section>

        <!-- Trust Bar -->
        <div class="trust-bar">
            <div class="trust-item"><span class="icon">⚡</span> Fast Shipping</div>
            <div class="trust-item"><span class="icon">🌱</span> Eco-Friendly</div>
            <div class="trust-item"><span class="icon">🔄</span> 30-Day Returns</div>
        </div>

        <!-- Products -->
        <section class="product-section" id="productsSection">
            <div class="section-header">
                <h2>⭐ Best Sellers</h2>
                <button class="view-all">View All →</button>
            </div>
            <div class="product-grid" id="productGrid"></div>
        </section>

        <!-- Social Proof -->
        <section class="social-proof">
            <div class="stars">⭐️⭐️⭐️⭐️⭐️</div>
            <p class="review-text">"The material is incredibly soft and sustainable!"</p>
            <p class="review-author">— Sarah M.</p>
            <p class="review-count">Based on 12,000+ verified reviews</p>
            <div class="ugc-grid">
                <img src="https://picsum.photos/seed/user1/200/200" alt="User" loading="lazy" />
                <img src="https://picsum.photos/seed/user2/200/200" alt="User" loading="lazy" />
                <img src="https://picsum.photos/seed/user3/200/200" alt="User" loading="lazy" />
                <img src="https://picsum.photos/seed/user4/200/200" alt="User" loading="lazy" />
            </div>
        </section>

        <!-- Bottom Cart Bar -->
        <div class="bottom-bar" id="bottomBar">
            <div class="summary">
                <span class="count" id="cartItemCount">0 items</span>
                <span class="total" id="cartTotal">$0.00</span>
            </div>
            <button class="checkout-btn" id="checkoutBtn">Checkout →</button>
        </div>

        <!-- Cart Drawer -->
        <div class="drawer-overlay" id="drawerOverlay"></div>
        <div class="drawer" id="cartDrawer">
            <div class="drawer-header">
                <h3>🛒 Your Cart</h3>
                <button class="drawer-close" id="drawerClose">✕</button>
            </div>
            <div id="drawerContent"></div>
        </div>

    </div>

    <!-- ============================================================
    JAVASCRIPT - Complete Logic
    ============================================================ -->
    <script>
        (function() {
            'use strict';

            // ============================================================
            // 1. PRODUCT DATA - Edit this to change your products!
            // ============================================================
            const PRODUCTS = [{
                id: 1,
                name: 'Eco Tote Bag',
                price: 45,
                originalPrice: 60,
                image: 'https://picsum.photos/seed/tote/400/400',
                colors: ['#e94560', '#4a90d9', '#22c55e'],
                badge: '#1'
            }, {
                id: 2,
                name: 'Bamboo Essentials',
                price: 60,
                image: 'https://picsum.photos/seed/bamboo/400/400',
                colors: ['#1a1a2e', '#f5f5f5', '#8b6914'],
                badge: null
            }, {
                id: 3,
                name: 'Recycled Backpack',
                price: 30,
                image: 'https://picsum.photos/seed/backpack/400/400',
                colors: null,
                badge: null
            }, {
                id: 4,
                name: 'Organic Cotton Tee',
                price: 35,
                originalPrice: 45,
                image: 'https://picsum.photos/seed/tee/400/400',
                colors: ['#e8d5b7', '#2c3e50', '#e74c3c'],
                badge: 'Sale'
            }];

            // ============================================================
            // 2. STATE
            // ============================================================
            let cart = [];
            let isDrawerOpen = false;
            let tg = null;

            // ============================================================
            // 3. DOM REFS
            // ============================================================
            const $ = (s) => document.querySelector(s);
            const $$ = (s) => document.querySelectorAll(s);

            const app = $('#app');
            const loadingScreen = $('#loadingScreen');
            const productGrid = $('#productGrid');
            const bottomBar = $('#bottomBar');
            const cartItemCount = $('#cartItemCount');
            const cartTotal = $('#cartTotal');
            const navCartBadge = $('#navCartBadge');
            const cartBtn = $('#cartBtn');
            const cartDrawer = $('#cartDrawer');
            const drawerOverlay = $('#drawerOverlay');
            const drawerContent = $('#drawerContent');
            const drawerClose = $('#drawerClose');
            const checkoutBtn = $('#checkoutBtn');
            const shopNowBtn = $('#shopNowBtn');
            const searchBtn = $('#searchBtn');
            const toast = $('#toast');
            const stickyNav = $('#stickyNav');

            // ============================================================
            // 4. TELEGRAM INIT
            // ============================================================
            function initTelegram() {
                try {
                    if (window.Telegram && window.Telegram.WebApp) {
                        tg = window.Telegram.WebApp;
                        tg.expand();
                        tg.ready();
                        tg.setHeaderColor('#ffffff');
                        tg.setBackgroundColor('#f8f9fa');
                        console.log('✅ Telegram Mini App ready');
                    }
                } catch (e) {
                    console.log('ℹ️ Running outside Telegram');
                }
            }

            // ============================================================
            // 5. TOAST
            // ============================================================
            let toastTimeout;

            function showToast(msg, icon = '✓', duration = 2500) {
                toast.innerHTML = `<span class="toast-icon">${icon}</span> ${msg}`;
                toast.classList.add('show');
                clearTimeout(toastTimeout);
                toastTimeout = setTimeout(() => toast.classList.remove('show'), duration);
            }

            // ============================================================
            // 6. HAPTIC
            // ============================================================
            function haptic(style = 'light') {
                if (tg && tg.HapticFeedback) {
                    try { tg.HapticFeedback.impactOccurred(style); } catch (e) {}
                }
            }

            // ============================================================
            // 7. CART
            // ============================================================
            function saveCart() {
                try { localStorage.setItem('telegram_shop_cart', JSON.stringify(cart)); } catch (e) {}
            }

            function loadCart() {
                try {
                    const saved = localStorage.getItem('telegram_shop_cart');
                    if (saved) cart = JSON.parse(saved);
                } catch (e) {}
            }

            function addToCart(productId) {
                const product = PRODUCTS.find(p => p.id === productId);
                if (!product) return;

                const existing = cart.find(item => item.id === productId);
                if (existing) {
                    existing.quantity += 1;
                } else {
                    cart.push({ ...product, quantity: 1 });
                }

                saveCart();
                updateUI();
                haptic('medium');
                showToast(`${product.name} added to cart!`, '🛒');
                if (cart.length > 0) bottomBar.classList.add('visible');
            }

            function removeFromCart(productId) {
                cart = cart.filter(item => item.id !== productId);
                saveCart();
                updateUI();
                haptic('light');
                if (cart.length === 0) bottomBar.classList.remove('visible');
            }

            function updateQuantity(productId, delta) {
                const item = cart.find(i => i.id === productId);
                if (!item) return;
                const newQty = item.quantity + delta;
                if (newQty <= 0) { removeFromCart(productId); return; }
                item.quantity = newQty;
                saveCart();
                updateUI();
                haptic('light');
            }

            function getTotal() {
                return cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            }

            function getItemCount() {
                return cart.reduce((sum, item) => sum + item.quantity, 0);
            }

            // ============================================================
            // 8. UI UPDATE
            // ============================================================
            function updateUI() {
                const count = getItemCount();
                const total = getTotal();

                cartItemCount.textContent = `${count} item${count !== 1 ? 's' : ''}`;
                cartTotal.textContent = `$${total.toFixed(2)}`;

                if (count > 0) {
                    navCartBadge.textContent = count;
                    navCartBadge.style.display = 'flex';
                    bottomBar.classList.add('visible');
                } else {
                    navCartBadge.style.display = 'none';
                    bottomBar.classList.remove('visible');
                }

                if (isDrawerOpen) renderDrawer();

                $$('.quick-add-btn').forEach(btn => {
                    const id = parseInt(btn.dataset.id);
                    const inCart = cart.some(item => item.id === id);
                    if (inCart) {
                        btn.textContent = '✓ Added';
                        btn.classList.add('added');
                    } else {
                        btn.textContent = 'Quick Add';
                        btn.classList.remove('added');
                    }
                });
            }

            // ============================================================
            // 9. RENDER PRODUCTS
            // ============================================================
            function renderProducts() {
                let html = '';
                PRODUCTS.forEach(p => {
                    const swatches = p.colors ?
                        p.colors.map(c => `<span class="dot" style="background:${c};"></span>`).join('') :
                        '';
                    const badge = p.badge ? `<span class="badge">${p.badge}</span>` : '';
                    const original = p.originalPrice ?
                        `<span class="original">$${p.originalPrice}</span>` :
                        '';

                    html += `
                        <div class="product-card">
                            <div class="image-wrap">
                                <img src="${p.image}" alt="${p.name}" loading="lazy" />
                                ${badge}
                            </div>
                            <div class="info">
                                <div class="name">${p.name}</div>
                                <div class="price">$${p.price} ${original}</div>
                                ${p.colors ? `<div class="swatches">${swatches}</div>` : ''}
                                <button class="quick-add-btn" data-id="${p.id}">Quick Add</button>
                            </div>
                        </div>
                    `;
                });
                productGrid.innerHTML = html;

                $$('.quick-add-btn').forEach(btn => {
                    btn.addEventListener('click', function(e) {
                        e.stopPropagation();
                        addToCart(parseInt(this.dataset.id));
                    });
                });

                $$('.swatches .dot').forEach(dot => {
                    dot.addEventListener('click', function(e) {
                        e.stopPropagation();
                        this.closest('.swatches').querySelectorAll('.dot')
                            .forEach(d => d.classList.remove('active'));
                        this.classList.add('active');
                    });
                });

                updateUI();
            }

            // ============================================================
            // 10. RENDER DRAWER
            // ============================================================
            function renderDrawer() {
                if (cart.length === 0) {
                    drawerContent.innerHTML = `
                        <div class="drawer-empty">
                            <div class="icon">🛍️</div>
                            <p>Your cart is empty</p>
                            <p style="font-size:13px;margin-top:4px;">Start shopping to add items!</p>
                        </div>
                    `;
                    return;
                }

                let itemsHtml = '';
                cart.forEach(item => {
                    itemsHtml += `
                        <div class="drawer-item">
                            <img src="${item.image}" alt="${item.name}" />
                            <div class="item-info">
                                <div class="item-name">${item.name}</div>
                                <div class="item-price">$${(item.price * item.quantity).toFixed(2)}</div>
                            </div>
                            <div class="item-qty">
                                <button data-id="${item.id}" data-delta="-1">−</button>
                                <span>${item.quantity}</span>
                                <button data-id="${item.id}" data-delta="1">+</button>
                            </div>
                        </div>
                    `;
                });

                const total = getTotal();
                drawerContent.innerHTML = `
                    ${itemsHtml}
                    <div class="drawer-total">
                        <span>Total</span>
                        <span>$${total.toFixed(2)}</span>
                    </div>
                    <button class="drawer-checkout" id="drawerCheckoutBtn">Proceed to Checkout →</button>
                `;

                drawerContent.querySelectorAll('.drawer-item .item-qty button').forEach(btn => {
                    btn.addEventListener('click', function(e) {
                        e.stopPropagation();
                        updateQuantity(parseInt(this.dataset.id), parseInt(this.dataset.delta));
                    });
                });

                const drawerCheckout = drawerContent.querySelector('#drawerCheckoutBtn');
                if (drawerCheckout) drawerCheckout.addEventListener('click', handleCheckout);
            }

            // ============================================================
            // 11. DRAWER CONTROLS
            // ============================================================
            function openDrawer() {
                isDrawerOpen = true;
                cartDrawer.classList.add('open');
                drawerOverlay.classList.add('open');
                renderDrawer();
                haptic('light');
            }

            function closeDrawer() {
                isDrawerOpen = false;
                cartDrawer.classList.remove('open');
                drawerOverlay.classList.remove('open');
            }

            // ============================================================
            // 12. CHECKOUT
            // ============================================================
            function handleCheckout() {
                if (cart.length === 0) {
                    showToast('Your cart is empty!', '⚠️');
                    return;
                }

                haptic('heavy');

                const total = getTotal();
                const count = getItemCount();
                const items = cart.map(item => `${item.name} (${item.quantity}x)`).join('\n');

                if (tg) {
                    tg.showPopup({
                        title: '📦 Order Summary',
                        message: `${items}\n\nTotal: $${total.toFixed(2)}\nItems: ${count}\n\nThank you for your purchase! 🎉`,
                        buttons: [
                            { type: 'default', text: '✅ Confirm', id: 'confirm' },
                            { type: 'destructive', text: 'Cancel', id: 'cancel' }
                        ]
                    }, function(buttonId) {
                        if (buttonId === 'confirm') {
                            cart = [];
                            saveCart();
                            updateUI();
                            closeDrawer();
                            showToast('🎉 Order placed successfully!', '✅');
                            haptic('heavy');
                            console.log('Order placed:', { items: cart, total, count });
                        }
                    });
                } else {
                    if (confirm(`Order Summary:\n${items}\n\nTotal: $${total.toFixed(2)}\n\nConfirm purchase?`)) {
                        cart = [];
                        saveCart();
                        updateUI();
                        closeDrawer();
                        alert('🎉 Order placed successfully!');
                    }
                }
            }

            // ============================================================
            // 13. SCROLL
            // ============================================================
            function handleScroll() {
                if (window.scrollY > 80) {
                    stickyNav.classList.add('shrunk');
                } else {
                    stickyNav.classList.remove('shrunk');
                }
            }

            // ============================================================
            // 14. SEARCH
            // ============================================================
            function handleSearch() {
                haptic('light');
                if (tg) {
                    tg.showPopup({
                        title: '🔍 Search',
                        message: 'Search for products...',
                        buttons: [{ type: 'default', text: 'OK' }]
                    });
                } else {
                    const query = prompt('Search for products:');
                    if (query) showToast(`Searching for "${query}"...`, '🔍');
                }
            }

            // ============================================================
            // 15. INIT
            // ============================================================
            function init() {
                loadCart();
                initTelegram();
                renderProducts();
                updateUI();

                if (cart.length > 0) bottomBar.classList.add('visible');

                loadingScreen.style.display = 'none';
                app.style.display = 'block';

                // Events
                cartBtn.addEventListener('click', openDrawer);
                drawerClose.addEventListener('click', closeDrawer);
                drawerOverlay.addEventListener('click', closeDrawer);

                checkoutBtn.addEventListener('click', function() {
                    openDrawer();
                    setTimeout(() => {
                        const btn = drawerContent.querySelector('#drawerCheckoutBtn');
                        if (btn) btn.scrollIntoView({ behavior: 'smooth', block: 'center' });
                    }, 350);
                });

                shopNowBtn.addEventListener('click', function() {
                    $('#productsSection').scrollIntoView({ behavior: 'smooth' });
                    haptic('medium');
                });

                searchBtn.addEventListener('click', handleSearch);
                window.addEventListener('scroll', handleScroll, { passive: true });

                document.addEventListener('keydown', function(e) {
                    if (e.key === 'Escape' && isDrawerOpen) closeDrawer();
                });

                if (tg) {
                    tg.onEvent('mainButtonClicked', openDrawer);
                }

                console.log('🚀 Shop ready!');
                console.log(`📦 ${cart.length} items in cart`);
            }

            // ============================================================
            // 16. START
            // ============================================================
            if (document.readyState === 'loading') {
                document.addEventListener('DOMContentLoaded', init);
            } else {
                init();
            }

        })();
    </script>

</body>
</html>
