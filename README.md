# Hackathon
creating a website where startup business can create their own portfolio and the investors can reach them and connect them by searching using different kind of filters
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cafe Bloom - Fresh Coffee & Gourmet Snacks</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Load Firebase SDKs -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        // Import all required Firestore functions explicitly
        import { getFirestore, doc, setDoc, collection, query, onSnapshot, addDoc, serverTimestamp, getDocs, updateDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Global Firebase variables will be populated in setupFirebase()
        window.FB = {};
        
// Define global variables provided by the Canvas environment
const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-cafe-bloom-app-id';
const firebaseConfig = (typeof real__firebase_config !== 'undefined' && typeof __firebase_config !== 'undefined')
    ? JSON.parse(__firebase_config)
    : {
        // For Firebase JS SDK v7.20.0 and later, measurementId is optional
        apiKey: "AIzaSyCTZ55pi1Ot0gtlcTmVJoapQzl6CIU64lQ",
        authDomain: "cafe-bloom-13595.firebaseapp.com",
        projectId: "cafe-bloom-13595",
        storageBucket: "cafe-bloom-13595.firebasestorage.app",
        messagingSenderId: "130385829051",
        appId: "1:130385829051:web:db370327b2d242c28f9d9e",
        measurementId: "G-ZZLWX00JJN"
    };
const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        /**
         * Initializes Firebase and handles initial authentication.
         */
        window.setupFirebase = async function() {
            try {
                if (!firebaseConfig) {
                    console.error("Firebase config not found. Running UI in mock mode.");
                    document.getElementById('app-status').textContent = 'Error: DB config missing. UI functions only.';
                    return;
                }
                const app = initializeApp(firebaseConfig);
                FB.db = getFirestore(app);
                FB.auth = getAuth(app);
                FB.appId = appId;
                
                // Expose required Firestore functions globally via FB object for use in the script tag
                FB.doc = doc;
                FB.setDoc = setDoc;
                FB.collection = collection;
                FB.onSnapshot = onSnapshot;
                FB.addDoc = addDoc;
                FB.serverTimestamp = serverTimestamp;
                
                // Authentication
                const auth = FB.auth;
                if (initialAuthToken) {
                    await signInWithCustomToken(auth, initialAuthToken);
                    console.log("Signed in with custom token.");
                } else {
                    await signInAnonymously(auth);
                    console.log("Signed in anonymously.");
                }

                onAuthStateChanged(auth, (user) => {
                    const authReadyEvent = new CustomEvent('authReady', { detail: { user } });
                    document.dispatchEvent(authReadyEvent);
                });
            } catch (error) {
                console.error("Firebase setup failed:", error);
                document.getElementById('app-status').textContent = 'Error: Database connection failed.';
            }
        };
    </script>
    
    <style>
        /* --- CORE DESIGN SYSTEM: COZY HOMELY THEME (Refined) --- */
        
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #1e1b18, #302a25); /* Deep Stone/Charcoal */
            color: #f5ebe0; /* Warm Cream */
            min-height: 100vh;
            line-height: 1.6;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
        
        /* Modernized Card Style (Deep Glassmorphism) */
        .card {
            background-color: rgba(255, 255, 255, 0.05);
            border-radius: 1.75rem;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 8px 30px rgba(0, 0, 0, 0.4);
            transition: transform 0.3s ease-out, box-shadow 0.3s ease-out, background-color 0.3s ease;
        }

        /* Card Hover Animation - Subtle lift and warm shadow */
        .card:hover {
            transform: translateY(-4px);
            box-shadow: 0 16px 40px rgba(239, 116, 36, 0.3);
            background-color: rgba(255, 255, 255, 0.07);
        }

        /* Primary Accent Color Utility */
        .text-accent {
            color: #e6783e; /* Burnt Orange */
        }
        .bg-accent {
             background-color: #e6783e;
        }

        /* Primary Button Style with Gradient */
        .btn-primary {
            background: linear-gradient(90deg, #e6783e, #cc6732);
            color: white;
            transition: all 0.3s ease-in-out;
            border: none;
            box-shadow: 0 4px 12px rgba(230, 120, 62, 0.4);
            font-weight: 700;
        }

        .btn-primary:hover {
            background: linear-gradient(90deg, #cc6732, #e6783e);
            transform: scale(1.02);
            box-shadow: 0 6px 16px rgba(230, 120, 62, 0.6);
        }

        /* Secondary Button (Stone color) */
        .btn-secondary {
            background-color: #4b3e36; 
            color: #f5ebe0;
            transition: background-color 0.3s ease, transform 0.3s ease;
        }
        .btn-secondary:hover {
            background-color: #5d4f45;
            transform: scale(1.02);
        }

        /* Active navigation link */
        .nav-link.active {
            border-bottom: 3px solid #e6783e;
            color: #e6783e;
        }
        
        /* Blended Text Color */
        .text-primary-blend {
            background-image: linear-gradient(45deg, #e6783e, #964B00);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* Content Transition */
        .content-section {
            display: none;
            animation: fadeIn 0.6s ease-out;
        }

        /* Custom Keyframe Animation */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(15px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Custom Scrollbar for Text Areas/Scrollable containers */
        textarea::-webkit-scrollbar, .overflow-y-scroll::-webkit-scrollbar {
            width: 8px;
        }
        textarea::-webkit-scrollbar-thumb, .overflow-y-scroll::-webkit-scrollbar-thumb {
            background: #e6783e;
            border-radius: 4px;
        }
        textarea::-webkit-scrollbar-track, .overflow-y-scroll::-webkit-scrollbar-track {
            background: #3a322e;
        }
        
    </style>
</head>
<body onload="setupFirebase()">

    <!-- App Status Indicator (for Firebase) -->
    <div id="app-status" class="fixed top-0 left-0 bg-white/10 text-xs text-white p-1 rounded-br-md z-30">Connecting to DB...</div>

    <!-- Header and Navigation -->
    <header class="sticky top-0 z-20 bg-stone-900/80 backdrop-blur-lg shadow-xl border-b border-white/5">
        <div class="container flex justify-between items-center py-4 px-4 sm:px-6">
            <h1 class="text-3xl font-extrabold tracking-tight text-primary-blend">
                <span class="font-light">Cafe</span> Bloom
            </h1>
            <nav class="hidden md:flex space-x-6 text-lg font-medium">
                <a href="#" class="nav-link p-2" data-target="home" onclick="showSection(event, 'home')">Home</a>
                <a href="#" class="nav-link p-2" data-target="menu" onclick="showSection(event, 'menu')">Menu</a>
                <a href="#" class="nav-link p-2" data-target="ratings" onclick="showSection(event, 'ratings')">Ratings</a>
                <a href="#" class="nav-link p-2" data-target="contact" onclick="showSection(event, 'contact')">Contact</a>
                <a href="#" class="nav-link p-2" data-target="allergens" onclick="showSection(event, 'allergens')">Allergens</a>
            </nav>
            <div id="auth-controls" class="flex items-center space-x-4">
                <!-- Cart Button -->
                <button class="relative p-2 rounded-full text-white/80 hover:text-accent transition-colors" onclick="showModal('cart-modal')">
                    <svg class="w-7 h-7" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"></path></svg>
                    <span id="cart-count" class="absolute top-0 right-0 inline-flex items-center justify-center px-2 py-1 text-xs font-bold leading-none text-white transform translate-x-1/2 -translate-y-1/2 bg-red-600 rounded-full">0</span>
                </button>

                <button id="login-btn" class="py-2 px-4 rounded-full btn-primary text-sm font-semibold" onclick="showModal('login-modal')">
                    Login / Join
                </button>
                <button id="logout-btn" class="hidden py-2 px-4 rounded-full btn-secondary text-sm font-semibold" onclick="handleLogout()">
                    <span class="hidden sm:inline">Logout</span>
                    <span class="sm:hidden">🚪</span>
                </button>
            </div>
            <!-- Mobile Menu Button -->
            <button class="md:hidden text-accent text-3xl p-1 rounded-lg" onclick="toggleMobileMenu()">☰</button>
        </div>
        <!-- Mobile Menu -->
        <div id="mobile-menu" class="hidden md:hidden bg-white/10 backdrop-blur-sm border-t border-white/10">
            <nav class="flex flex-col p-4 space-y-2 text-center text-lg font-medium">
                <a href="#" class="nav-link p-2 text-white/80" data-target="home" onclick="showSection(event, 'home')">Home</a>
                <a href="#" class="nav-link p-2 text-white/80" data-target="menu" onclick="showSection(event, 'menu')">Menu</a>
                <a href="#" class="nav-link p-2 text-white/80" data-target="ratings" onclick="showSection(event, 'ratings')">Ratings</a>
                <a href="#" class="nav-link p-2 text-white/80" data-target="contact" onclick="showSection(event, 'contact')">Contact</a>
                <a href="#" class="nav-link p-2 text-white/80" data-target="allergens" onclick="showSection(event, 'allergens')">Allergens</a>
            </nav>
        </div>
    </header>

    <!-- Main Content Area -->
    <main class="container py-16 px-4 sm:px-6">
        
        <!-- 1. HOME SECTION -->
        <section id="home-section" class="content-section">
            <div class="text-center mb-16 p-6 card border-none shadow-none bg-transparent">
                <h2 class="text-7xl font-extrabold mb-4 text-primary-blend leading-snug">
                    Your Cozy Corner
                </h2>
                <p class="text-xl text-white/70 max-w-4xl mx-auto mt-4">Where every bean is roasted with warmth and every visit feels like coming home. Experience the perfect blend of tradition and modern comfort.</p>
            </div>
            <div class="grid md:grid-cols-3 gap-8 text-center">
                <div class="card p-8">
                    <div class="text-accent text-6xl mb-4 transform hover:rotate-3 transition-transform">☕</div>
                    <h3 class="text-2xl font-semibold mb-2 text-white">Artisan Brews</h3>
                    <p class="text-white/60">Discover hand-crafted coffees, from robust espressos to delicate pour-overs. Made with ethically sourced Indian beans.</p>
                </div>
                <div class="card p-8">
                    <div class="text-accent text-6xl mb-4 transform hover:scale-110 transition-transform">🥐</div>
                    <h3 class="text-2xl font-semibold mb-2 text-white">Fresh Baking</h3>
                    <p class="text-white/60">Freshly baked pastries and savory delights, made with local, quality ingredients, delivered hot daily.</p>
                </div>
                <div class="card p-8">
                    <div class="text-accent text-6xl mb-4 transform hover:translate-y-[-5px] transition-transform">✨</div>
                    <h3 class="text-2xl font-semibold mb-2 text-white">Fast Delivery</h3>
                    <p class="text-white/60">Order your favorites with quick, reliable delivery directly to your home or office. Flat delivery fee applies.</p>
                </div>
            </div>
        </section>

        <!-- 2. MENU SECTION -->
        <section id="menu-section" class="content-section">
            <h2 class="text-5xl font-extrabold text-center mb-12 text-primary-blend">Our Thoughtfully Crafted Menu</h2>
            
            <!-- Menu Tabs - Modern Button Group -->
            <div class="flex justify-center mb-10 space-x-2 p-2 bg-white/5 rounded-2xl max-w-xl mx-auto shadow-inner">
                <button class="menu-tab flex-1 py-3 text-lg font-semibold rounded-xl text-white/80 hover:bg-white/10 transition-colors" data-target="coffee-pre" onclick="showMenuCategory('coffee-pre')">Pre-Reciped</button>
                <button class="menu-tab flex-1 py-3 text-lg font-semibold rounded-xl text-white/60 hover:bg-white/10 transition-colors" data-target="coffee-design" onclick="showMenuCategory('coffee-design')">Design Your Own</button>
                <button class="menu-tab flex-1 py-3 text-lg font-semibold rounded-xl text-white/60 hover:bg-white/10 transition-colors" data-target="snacks" onclick="showMenuCategory('snacks')">Snacks</button>
            </div>

            <!-- Pre-Reciped Coffees List (Rendered by JS) -->
            <div id="coffee-pre-category" class="menu-category grid md:grid-cols-2 lg:grid-cols-3 gap-6">
                <p class="col-span-full text-center text-white/60">Loading coffees...</p>
            </div>

            <!-- Design Your Own Coffee (Improved Step-by-Step UX) -->
            <div id="coffee-design-category" class="menu-category content-section card p-6 sm:p-10">
                <h3 class="text-3xl font-bold text-primary-blend mb-8 text-center">Craft Your Unique Brew</h3>
                
                <div class="max-w-xl mx-auto space-y-8">
                    
                    <!-- Step 1: Base Shot -->
                    <div class="p-5 border-l-4 border-accent bg-white/5 rounded-xl">
                        <p class="text-xl font-bold mb-2 text-accent">1. Choose Base Espresso</p>
                        <select id="base" class="w-full p-3 rounded-lg bg-stone-800 border border-stone-600 focus:ring-accent focus:border-accent transition-colors">
                            <option value="80" data-name="Single Shot">Single Shot (₹ 80)</option>
                            <option value="150" data-name="Double Shot">Double Shot (₹ 150)</option>
                            <option value="180" data-name="Blonde Roast Shot">Blonde Roast Shot (₹ 180)</option>
                        </select>
                    </div>

                    <!-- Step 2: Milk / Base -->
                    <div class="p-5 border-l-4 border-accent bg-white/5 rounded-xl">
                        <p class="text-xl font-bold mb-2 text-accent">2. Select Milk / Base</p>
                        <select id="milk" class="w-full p-3 rounded-lg bg-stone-800 border border-stone-600 focus:ring-accent focus:border-accent transition-colors">
                            <option value="30" data-name="Whole Milk">Whole Milk (₹ 30)</option>
                            <option value="40" data-name="Skim Milk">Skim Milk (₹ 40)</option>
                            <option value="60" data-name="Almond Milk" data-allergens='["nuts"]'>Almond Milk (₹ 60)</option>
                            <option value="60" data-name="Oat Milk">Oat Milk (₹ 60)</option>
                        </select>
                        <p id="milk-allergen-warning" class="text-xs text-red-400 mt-2 hidden"></p>
                    </div>

                    <!-- Step 3: Syrup / Flavor -->
                    <div class="p-5 border-l-4 border-accent bg-white/5 rounded-xl">
                        <p class="text-xl font-bold mb-2 text-accent">3. Add Syrup / Flavor</p>
                        <select id="syrup" class="w-full p-3 rounded-lg bg-stone-800 border border-stone-600 focus:ring-accent focus:border-accent transition-colors">
                            <option value="0" data-name="None">None (₹ 0)</option>
                            <option value="40" data-name="Vanilla Syrup">Vanilla (₹ 40)</option>
                            <option value="50" data-name="Caramel Syrup">Caramel (₹ 50)</option>
                            <option value="60" data-name="Hazelnut Syrup" data-allergens='["nuts"]'>Hazelnut (₹ 60)</option>
                            <option value="70" data-name="Spiced Cardamom">Spiced Cardamom (₹ 70)</option>
                        </select>
                        <p id="syrup-allergen-warning" class="text-xs text-red-400 mt-2 hidden"></p>
                    </div>
                    
                    <div class="pt-6 border-t border-white/10 text-center">
                        <p class="text-xl font-bold mb-4 text-white/80">Your Creation Total:</p>
                        <p id="custom-price" class="text-6xl font-extrabold text-primary-blend">₹ 80</p>
                        <p id="custom-coffee-message" class="text-red-400 font-semibold mt-4 transition-opacity duration-300 hidden"></p>
                    </div>

                    <button class="w-full py-4 px-6 rounded-2xl btn-primary text-lg font-semibold mt-6" onclick="addCustomCoffeeToCart()">
                        Add Custom Coffee to Cart
                    </button>
                </div>
            </div>

            <!-- Gourmet Snacks -->
            <div id="snacks-category" class="menu-category content-section">
                <div class="mb-8 card p-5 flex flex-col md:flex-row justify-between items-center space-y-4 md:space-y-0">
                    <h3 class="text-2xl font-bold text-primary-blend">Gourmet Snacks List</h3>
                    <div class="w-full md:w-1/2">
                        <label for="price-filter" class="block text-sm font-semibold mb-1 text-white/60">Max Price: <span class="text-accent">₹ <span id="max-price-display">350</span></span></label>
                        <input type="range" id="price-filter" min="50" max="350" value="350" step="50" class="w-full h-2 bg-white/20 rounded-lg appearance-none cursor-pointer range-accent" oninput="filterSnacks()">
                    </div>
                </div>

                <div id="snacks-list" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                    <p class="col-span-full text-center text-white/60">Loading snacks...</p>
                </div>
            </div>

        </section>

        <!-- 3. ALLERGENS SECTION (Unchanged) -->
        <section id="allergens-section" class="content-section">
            <h2 class="text-4xl font-extrabold text-center mb-10 text-primary-blend">Personal Allergen Profile</h2>
            <div id="allergen-user-id" class="text-center text-sm text-white/40 mb-6 hidden">User ID: </div>

            <div id="allergen-content" class="card p-8 max-w-2xl mx-auto">
                <p id="allergen-status" class="mb-6 p-3 bg-red-800/30 border border-red-500/50 rounded-xl text-center text-red-300 hidden">
                    <strong class="font-bold">Important:</strong> Please log in to securely manage your personal allergen profile.
                </p>

                <div id="allergen-form-container" class="hidden">
                    <p class="text-white/70 mb-6 border-b border-white/10 pb-4">Select all ingredients you must exclude. We will flag menu items containing these for your safety.</p>
                    
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <label class="flex items-center space-x-3 text-lg card p-3 bg-white/5 hover:bg-white/10 cursor-pointer transition-all">
                            <input type="checkbox" id="allergy-nuts" value="nuts" class="h-5 w-5 rounded text-accent bg-white/10 border-white/30 focus:ring-accent">
                            <span class="text-white">Tree Nuts / Peanuts</span>
                        </label>
                        <label class="flex items-center space-x-3 text-lg card p-3 bg-white/5 hover:bg-white/10 cursor-pointer transition-all">
                            <input type="checkbox" id="allergy-milk" value="milk" class="h-5 w-5 rounded text-accent bg-white/10 border-white/30 focus:ring-accent">
                            <span class="text-white">Milk / Dairy</span>
                        </label>
                        <label class="flex items-center space-x-3 text-lg card p-3 bg-white/5 hover:bg-white/10 cursor-pointer transition-all">
                            <input type="checkbox" id="allergy-gluten" value="gluten" class="h-5 w-5 rounded text-accent bg-white/10 border-white/30 focus:ring-accent">
                            <span class="text-white">Wheat / Gluten</span>
                        </label>
                        <label class="flex items-center space-x-3 text-lg card p-3 bg-white/5 hover:bg-white/10 cursor-pointer transition-all">
                            <input type="checkbox" id="allergy-soy" value="soy" class="h-5 w-5 rounded text-accent bg-white/10 border-white/30 focus:ring-accent">
                            <span class="text-white">Soy</span>
                        </label>
                        <label class="flex items-center space-x-3 text-lg card p-3 bg-white/5 hover:bg-white/10 cursor-pointer transition-all">
                            <input type="checkbox" id="allergy-eggs" value="eggs" class="h-5 w-5 rounded text-accent bg-white/10 border-white/30 focus:ring-accent">
                            <span class="text-white">Eggs</span>
                        </label>
                        <label class="flex items-center space-x-3 text-lg card p-3 bg-white/5 hover:bg-white/10 cursor-pointer transition-all">
                            <input type="checkbox" id="allergy-shellfish" value="shellfish" class="h-5 w-5 rounded text-accent bg-white/10 border-white/30 focus:ring-accent">
                            <span class="text-white">Shellfish</span>
                        </label>
                    </div>

                    <button id="save-allergens-btn" class="w-full mt-8 py-3 px-6 rounded-full btn-primary text-lg font-semibold" onclick="saveAllergens()">
                        Save Allergen Profile
                    </button>
                    <p id="allergen-save-message" class="mt-4 text-green-400 text-center hidden"></p>
                </div>
            </div>
        </section>
        
        <!-- 4. RATINGS SECTION (Unchanged) -->
        <section id="ratings-section" class="content-section">
            <h2 class="text-4xl font-extrabold text-center mb-10 text-primary-blend">Customer Ratings & Reviews</h2>
            
            <!-- Review Submission Form -->
            <div id="review-form-container" class="card p-6 mb-10 max-w-2xl mx-auto">
                <h3 class="text-2xl font-bold text-amber-300 mb-4">Leave a Review</h3>
                <p id="review-login-prompt" class="text-red-400 mb-4 p-2 bg-red-800/30 rounded-lg hidden">You must be logged in to submit a rating.</p>
                <div id="actual-review-form" class="hidden">
                    <input type="text" id="review-title" placeholder="Review Title (e.g., Cozy Coffee!)" class="w-full p-3 rounded-lg bg-stone-800 border-stone-600 focus:ring-accent focus:border-accent mb-3 transition-colors">
                    <textarea id="review-text" rows="3" placeholder="Your honest feedback here..." class="w-full p-3 rounded-lg bg-stone-800 border-stone-600 focus:ring-accent focus:border-accent mb-3 transition-colors"></textarea>
                    
                    <div class="flex items-center justify-between mb-4">
                        <label class="font-semibold text-white/80">Rating (1-5):</label>
                        <select id="review-rating" class="p-2 rounded-lg bg-stone-800 border-stone-600 text-white transition-colors">
                            <option value="5">⭐⭐⭐⭐⭐ (5)</option>
                            <option value="4">⭐⭐⭐⭐ (4)</option>
                            <option value="3">⭐⭐⭐ (3)</option>
                            <option value="2">⭐⭐ (2)</option>
                            <option value="1">⭐ (1)</option>
                        </select>
                    </div>
                    <button class="w-full py-3 rounded-full btn-primary font-semibold" onclick="submitReview()">Submit Review</button>
                    <p id="review-message" class="mt-4 text-center text-green-400 hidden"></p>
                </div>
            </div>

            <!-- Reviews List -->
            <div id="reviews-list" class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <p class="md:col-span-2 text-center text-white/60">Loading reviews...</p>
            </div>
        </section>

        <!-- 5. CONTACT SECTION (Unchanged) -->
        <section id="contact-section" class="content-section">
            <h2 class="text-4xl font-extrabold text-center mb-10 text-primary-blend">Get In Touch</h2>
            <div class="grid md:grid-cols-2 gap-8 max-w-4xl mx-auto">
                <!-- Contact Info -->
                <div class="card p-8">
                    <h3 class="text-2xl font-bold text-amber-300 mb-4 border-b border-white/10 pb-2">Visit Our Digital Hub</h3>
                    <p class="mb-4 text-white/70">We are located in the heart of the digital city, blooming with fresh aromas and code magic.</p>
                    <div class="space-y-4 text-white/80">
                        <p class="flex items-center"><span class="text-accent mr-3">📍</span><strong class="font-semibold text-white">Address:</strong> 123, Bloom Tower, MG Road, Bengaluru, 560001</p>
                        <p class="flex items-center"><span class="text-accent mr-3">📞</span><strong class="font-semibold text-white">Phone:</strong> +91 98765 43210</p>
                        <p class="flex items-center"><span class="text-accent mr-3">📧</span><strong class="font-semibold text-white">Email:</strong> contact@cafebloom.in</p>
                        <p class="flex items-center"><span class="text-accent mr-3">⏰</span><strong class="font-semibold text-white">Hours:</strong> Mon - Sat: 9 AM - 9 PM</p>
                    </div>
                </div>

                <!-- Contact Form (Mock) -->
                <div class="card p-8">
                    <h3 class="text-2xl font-bold text-amber-300 mb-4 border-b border-white/10 pb-2">Send Us a Warm Message</h3>
                    <form onsubmit="event.preventDefault(); showMessage('contact-message', 'Thank you for your warm message! We will render a response soon.', 'text-green-400')">
                        <input type="text" placeholder="Your Name" required class="w-full p-3 rounded-lg bg-stone-800 border-stone-600 focus:ring-accent focus:border-accent mb-3">
                        <input type="email" placeholder="Your Email" required class="w-full p-3 rounded-lg bg-stone-800 border-stone-600 focus:ring-accent focus:border-accent mb-3">
                        <textarea rows="4" placeholder="Your Message" required class="w-full p-3 rounded-lg bg-stone-800 border-stone-600 focus:ring-accent focus:border-accent mb-3"></textarea>
                        <button type="submit" class="w-full py-3 rounded-full btn-primary font-semibold">Submit</button>
                        <p id="contact-message" class="mt-4 text-center hidden"></p>
                    </form>
                </div>
            </div>
        </section>

    </main>

    <!-- 6. LOGIN MODAL (Unchanged) -->
    <div id="login-modal" class="hidden fixed inset-0 bg-black/90 z-50 flex items-center justify-center transition-opacity duration-300" onclick="if (event.target.id === 'login-modal') hideModal('login-modal')">
        <div class="card p-8 sm:p-10 w-full max-w-md mx-4 transform transition-transform duration-500 scale-90 opacity-0" onclick="event.stopPropagation()">
            <h3 class="text-3xl font-bold text-center text-primary-blend mb-6">Welcome Back</h3>
            <p class="text-white/60 text-sm mb-4 text-center">Your login is simulated via Canvas for persistent state management.</p>

            <input type="email" id="login-email" placeholder="Email" class="w-full p-3 rounded-lg bg-stone-800 border-stone-600 focus:ring-accent focus:border-accent mb-3" required>
            <input type="password" id="login-password" placeholder="Password" class="w-full p-3 rounded-lg bg-stone-800 border-stone-600 focus:ring-accent focus:border-accent mb-6" required>
            
            <button class="w-full py-3 rounded-full btn-primary font-semibold mb-3" onclick="handleLogin()">
                Log In
            </button>
            <button class="w-full py-3 rounded-full btn-secondary font-semibold" onclick="handleLogin(true)">
                Create Account
            </button>

            <p id="login-message" class="mt-4 text-center text-red-400 hidden"></p>
            <button class="absolute top-4 right-4 text-white/50 hover:text-white text-3xl transition-colors" onclick="hideModal('login-modal')">&times;</button>
        </div>
    </div>

    <!-- 7. CART MODAL (NEW) -->
    <div id="cart-modal" class="hidden fixed inset-0 bg-black/90 z-50 flex items-center justify-center transition-opacity duration-300" onclick="if (event.target.id === 'cart-modal') hideModal('cart-modal')">
        <div class="card p-6 sm:p-8 w-full max-w-lg mx-4 h-[90vh] flex flex-col transform transition-transform duration-500 scale-90 opacity-0" onclick="event.stopPropagation()">
            <h3 class="text-3xl font-bold text-primary-blend mb-6 border-b border-white/10 pb-4">Your Order Cart</h3>
            
            <!-- Cart Items List -->
            <div id="cart-items-list" class="overflow-y-scroll flex-grow space-y-4 pr-2">
                <!-- Cart items will be rendered here -->
            </div>

            <!-- Discount Section -->
            <div class="mt-6 pt-4 border-t border-white/10 space-y-3">
                <h4 class="text-lg font-semibold text-white/80 mb-2">Discounts</h4>
                <div class="flex space-x-2">
                    <input type="text" id="discount-input" placeholder="Enter Coupon Code (BLOOM10/COZY50)" class="flex-grow p-2 rounded-lg bg-stone-800 border-stone-600 focus:ring-accent focus:border-accent text-sm" />
                    <button class="py-2 px-4 rounded-lg btn-secondary text-sm font-semibold" onclick="applyDiscount()">Apply</button>
                </div>
                <p id="discount-message" class="text-sm text-red-400 hidden"></p>
            </div>

            <!-- Totals Summary -->
            <div class="mt-4 pt-4 border-t border-white/10">
                <div class="flex justify-between mb-1 text-white/70">
                    <span>Subtotal:</span>
                    <span id="cart-subtotal">₹ 0</span>
                </div>
                <div class="flex justify-between mb-1 text-white/70">
                    <span>Discount (<span id="discount-name">None</span>):</span>
                    <span id="cart-discount" class="text-green-400">- ₹ 0</span>
                </div>
                <div class="flex justify-between mb-2 text-white/70">
                    <span>Delivery Fee:</span>
                    <span id="cart-delivery-fee">₹ 40</span>
                </div>
                <div class="flex justify-between text-2xl font-bold text-white">
                    <span>Order Total:</span>
                    <span id="cart-total" class="text-accent">₹ 40</span>
                </div>
            </div>

            <button id="checkout-btn" class="w-full py-4 rounded-full btn-primary font-semibold text-lg mt-6" onclick="checkout()">
                Proceed to Checkout
            </button>
            <p id="checkout-message" class="mt-3 text-center text-green-400 hidden"></p>

            <button class="absolute top-4 right-4 text-white/50 hover:text-white text-3xl transition-colors" onclick="hideModal('cart-modal')">&times;</button>
        </div>
    </div>


    <!-- Footer -->
    <footer class="bg-white/5 border-t border-white/10 mt-16 py-6">
        <div class="container text-center text-white/50 text-sm">
            <p>&copy; 2025 Cafe Bloom | Homely Edition | Designed for Comfort and Value.</p>
        </div>
    </footer>


    <script>
        // --- MOCK DATA ---
        const PRE_RECIPED_COFFEES = [
            { id: 'C1', name: 'The Bloom Espresso', price: 90, desc: 'Rich, full-bodied shot from our house blend.', allergens: [] },
            { id: 'C2', name: 'Saffron Latte', price: 180, desc: 'Our signature latte, infused with delicate saffron essence.', allergens: ['milk'] },
            { id: 'C3', name: 'Midnight Mocha', price: 170, desc: 'Dark chocolate, espresso, and steamed milk perfection.', allergens: ['milk'] },
            { id: 'C4', name: 'Masala Filter Coffee', price: 120, desc: 'Traditional Indian filter coffee with a hint of fresh ground spices.', allergens: ['milk'] },
            { id: 'C5', name: 'Iced Caramel Macchiato', price: 200, desc: 'Layers of milk, espresso, vanilla, and caramel drizzle.', allergens: ['milk'] },
        ];

        const SNACKS_DATA = [
            { id: 'S1', name: 'Spicy Veg Puff', price: 70, desc: 'Flaky pastry with spicy mixed vegetables.', allergens: ['gluten', 'milk'] },
            { id: 'S2', name: 'Chocolate Chip Cookie', price: 100, desc: 'Classic cookie with dark chocolate chips.', allergens: ['gluten', 'milk', 'eggs'] },
            { id: 'S3', name: 'Almond Croissant', price: 180, desc: 'Butter croissant filled and topped with toasted almonds.', allergens: ['gluten', 'milk', 'nuts', 'eggs'] },
            { id: 'S4', name: 'Gluten-Free Brownie', price: 160, desc: 'Rich, fudgy brownie made without wheat flour.', allergens: ['milk', 'eggs'] },
            { id: 'S5', name: 'Classic Muffin', price: 140, desc: 'Soft and moist vanilla muffin.', allergens: ['gluten', 'milk', 'eggs'] },
            { id: 'S6', name: 'Paneer Tikka Sandwich', price: 280, desc: 'Grilled sandwich with paneer in a smoky tikka marinade.', allergens: ['gluten', 'milk'] },
            { id: 'S7', name: 'Vegan Soy Bar', price: 150, desc: 'Energy bar with oats, seeds, and soy protein.', allergens: ['soy'] },
            { id: 'S8', name: 'Pistachio Cake Slice', price: 250, desc: 'Slice of decadent pistachio sponge cake.', allergens: ['gluten', 'milk', 'nuts', 'eggs'] },
            { id: 'S9', name: 'Tuna Salad Crostini', price: 300, desc: 'Crispy bread topped with creamy tuna salad.', allergens: ['gluten', 'fish', 'eggs'] },
            { id: 'S10', name: 'Prawn Cocktail Skewers', price: 350, desc: 'Small skewers with fresh prawns and a light sauce.', allergens: ['shellfish'] },
            { id: 'S11', name: 'Sesame Bagel w/ Cream Cheese', price: 120, desc: 'Toasted bagel topped with sesame seeds.', allergens: ['gluten', 'milk', 'sesame'] },
        ];

        const DISCOUNTS = {
            'BLOOM10': { type: 'percentage', value: 0.10, min: 200, name: '10% OFF' },
            'COZY50': { type: 'fixed', value: 50, min: 300, name: '₹50 OFF' },
        };

        const DELIVERY_FEE = 40;

        // --- APP STATE ---
        let currentState = {
            currentPage: 'home',
            userId: null,
            userAllergens: [], // e.g., ['nuts', 'milk']
        };
        let cart = []; // { id: string, name: string, price: number, quantity: number, allergens: string[] }
        let appliedDiscount = null; // { code: string, type: 'percentage'|'fixed', value: number, min: number, name: string }

        // --- CORE UI & NAVIGATION FUNCTIONS ---
        
        /** Shows a message in a specific element */
        function showMessage(id, text, className) {
            const el = document.getElementById(id);
            if (el) {
                el.textContent = text;
                el.className = `mt-4 text-center ${className}`;
                el.classList.remove('hidden');
            }
        }

        /** Hides a modal by ID */
        function hideModal(id) {
            const modal = document.getElementById(id);
            if (modal) {
                const content = modal.querySelector('.card');
                if (content) {
                    content.classList.remove('scale-100', 'opacity-100');
                    content.classList.add('scale-90', 'opacity-0');
                }
                setTimeout(() => modal.classList.add('hidden'), 300);
                if (id === 'cart-modal') {
                    // Clear discount message when closing cart
                    document.getElementById('discount-message').classList.add('hidden');
                    document.getElementById('discount-input').value = appliedDiscount ? appliedDiscount.code : '';
                }
            }
        }

        /** Shows a modal by ID */
        function showModal(id) {
            const modal = document.getElementById(id);
            if (modal) {
                modal.classList.remove('hidden');
                setTimeout(() => {
                    const content = modal.querySelector('.card');
                    if (content) {
                        content.classList.remove('scale-90', 'opacity-0');
                        content.classList.add('scale-100', 'opacity-100');
                    }
                }, 10);
            }
        }


        /** Toggles mobile menu visibility */
        function toggleMobileMenu() {
            document.getElementById('mobile-menu').classList.toggle('hidden');
        }

        /** Handles navigation between main sections */
        function showSection(event, target) {
            if (event) event.preventDefault();
            
            // Hide all sections and remove active class from all nav links
            document.querySelectorAll('.content-section').forEach(section => {
                section.style.display = 'none';
            });
            document.querySelectorAll('.nav-link').forEach(link => {
                link.classList.remove('active');
                link.classList.remove('text-accent');
                link.classList.add('text-white/60', 'hover:text-white');
            });
            document.querySelectorAll('.menu-tab').forEach(tab => {
                tab.classList.remove('bg-accent', 'text-white');
                tab.classList.add('text-white/80');
            });


            // Show target section and set active nav link
            const targetEl = document.getElementById(target + '-section');
            if (targetEl) {
                targetEl.style.display = 'block';
            }

            document.querySelectorAll(`[data-target="${target}"]`).forEach(link => {
                link.classList.add('active', 'text-accent');
                link.classList.remove('text-white/60', 'hover:text-white');
            });

            currentState.currentPage = target;
            
            // Handle specific section initializations
            if (target === 'menu') {
                renderPreRecipedCoffees(); // Ensure coffees are rendered
                showMenuCategory('coffee-pre'); // Default to pre-reciped coffee on menu load
            } else if (target === 'allergens') {
                loadAllergensUI();
            } else if (target === 'ratings') {
                loadReviews();
            }
            
            hideModal('login-modal'); // Close modal on nav
            if (!document.getElementById('mobile-menu').classList.contains('hidden')) {
                toggleMobileMenu();
            }
        }
        
        // Initialize the app view
        document.addEventListener('DOMContentLoaded', () => {
            showSection(null, 'home');
            initMenuListeners();
            calculateCustomCoffeePrice();
            // Set max price display for the filter
            document.getElementById('price-filter').max = 350;
            document.getElementById('max-price-display').textContent = document.getElementById('price-filter').max;
        });

        // --- CART & ORDERING FUNCTIONS ---
        
        /** Calculates all cart totals (subtotal, discount, total) */
        function calculateCartTotals() {
            let subtotal = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            let discountAmount = 0;
            let total = subtotal;

            if (appliedDiscount) {
                const discount = appliedDiscount;
                if (subtotal >= discount.min) {
                    if (discount.type === 'percentage') {
                        discountAmount = Math.round(subtotal * discount.value);
                    } else if (discount.type === 'fixed') {
                        discountAmount = discount.value;
                    }
                    total -= discountAmount;
                } else {
                    // Discount condition not met, clear discount
                    appliedDiscount = null;
                }
            }

            total += DELIVERY_FEE;
            
            // Render totals
            document.getElementById('cart-subtotal').textContent = `₹ ${subtotal.toLocaleString('en-IN')}`;
            document.getElementById('cart-discount').textContent = `- ₹ ${discountAmount.toLocaleString('en-IN')}`;
            document.getElementById('discount-name').textContent = appliedDiscount ? appliedDiscount.name : 'None';
            document.getElementById('cart-delivery-fee').textContent = `₹ ${DELIVERY_FEE}`;
            document.getElementById('cart-total').textContent = `₹ ${Math.max(0, total).toLocaleString('en-IN')}`;
            
            // Update discount input field if discount was cleared
            if (!appliedDiscount) {
                document.getElementById('discount-input').value = '';
            }

            return { subtotal, total, discountAmount };
        }

        /** Renders the cart modal content and updates the header badge */
        function renderCart() {
            const listEl = document.getElementById('cart-items-list');
            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            document.getElementById('cart-count').textContent = totalItems;
            listEl.innerHTML = '';
            
            if (cart.length === 0) {
                listEl.innerHTML = '<p class="text-center text-white/60 pt-10">Your cozy cart is empty. Time for a brew!</p>';
                document.getElementById('checkout-btn').disabled = true;
                document.getElementById('checkout-btn').classList.add('opacity-50', 'cursor-not-allowed');
            } else {
                document.getElementById('checkout-btn').disabled = false;
                document.getElementById('checkout-btn').classList.remove('opacity-50', 'cursor-not-allowed');
                
                cart.forEach((item, index) => {
                    const itemTotal = (item.price * item.quantity).toLocaleString('en-IN');
                    const intersectingAllergens = item.allergens.filter(allergy => currentState.userAllergens.includes(allergy));
                    const warning = intersectingAllergens.length > 0 
                        ? `<span class="text-red-400 text-xs mt-1 block">⚠️ Contains: ${intersectingAllergens.map(a => a.toUpperCase()).join(', ')}</span>`
                        : '';
                    
                    listEl.innerHTML += `
                        <div class="flex items-center p-3 rounded-xl bg-white/5 justify-between transition-all">
                            <div class="flex-grow">
                                <p class="font-semibold text-white">${item.name}</p>
                                <p class="text-sm text-white/60">₹ ${item.price} x ${item.quantity}</p>
                                ${warning}
                            </div>
                            <div class="flex items-center space-x-3">
                                <button class="text-accent hover:text-red-500 transition-colors text-lg" onclick="updateCartQuantity(${index}, -1)">&minus;</button>
                                <span class="font-bold text-lg text-white">${item.quantity}</span>
                                <button class="text-accent hover:text-green-500 transition-colors text-lg" onclick="updateCartQuantity(${index}, 1)">&plus;</button>
                                <button class="text-white/50 hover:text-red-600 transition-colors" onclick="updateCartQuantity(${index}, -${item.quantity})">
                                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                                </button>
                            </div>
                        </div>
                    `;
                });
            }

            calculateCartTotals();
            saveCart();
            filterSnacks(); // Re-render snack warnings
            calculateCustomCoffeePrice(); // Re-render custom coffee warnings
        }
        
        /** Adds an item to the cart, or increments quantity if it already exists */
        function addToCart(item, quantity = 1) {
            const existingItem = cart.find(i => i.id === item.id);
            
            if (existingItem) {
                existingItem.quantity += quantity;
            } else {
                cart.push({ ...item, quantity });
            }
            
            // Clean up cart: remove items with quantity <= 0
            cart = cart.filter(item => item.quantity > 0);

            renderCart();
            showModal('cart-modal'); // Show cart automatically
        }

        /** Updates the quantity of a specific cart item */
        function updateCartQuantity(index, change) {
            if (index >= 0 && index < cart.length) {
                cart[index].quantity += change;
                
                // Remove item if quantity drops to zero or below
                if (cart[index].quantity <= 0) {
                    cart.splice(index, 1);
                }
                renderCart();
            }
        }
        
        /** Applies a discount code */
        function applyDiscount() {
            const code = document.getElementById('discount-input').value.toUpperCase().trim();
            const msgEl = document.getElementById('discount-message');
            msgEl.classList.add('hidden');
            
            if (code === '') {
                appliedDiscount = null;
                calculateCartTotals();
                showMessage('discount-message', 'Discount cleared.', 'text-amber-300');
                return;
            }

            const discount = DISCOUNTS[code];
            if (discount) {
                const { subtotal } = calculateCartTotals();
                if (subtotal >= discount.min) {
                    appliedDiscount = { code, ...discount };
                    calculateCartTotals();
                    showMessage('discount-message', `${discount.name} applied successfully!`, 'text-green-400');
                } else {
                    appliedDiscount = null;
                    calculateCartTotals();
                    showMessage('discount-message', `Order subtotal must be over ₹ ${discount.min} to use this code.`, 'text-red-400');
                }
            } else {
                appliedDiscount = null;
                calculateCartTotals();
                showMessage('discount-message', 'Invalid discount code.', 'text-red-400');
            }
        }
        
        /** Mocks the checkout process and clears the cart */
        function checkout() {
            if (cart.length === 0) return;
            
            const { total } = calculateCartTotals();
            
            // Mock delivery confirmation
            const btn = document.getElementById('checkout-btn');
            const originalText = btn.textContent;
            btn.textContent = `Order Confirmed! Delivering ₹ ${total.toLocaleString('en-IN')} Order.`;
            btn.classList.add('bg-green-600', 'hover:bg-green-700', 'shadow-green-500/50');
            btn.classList.remove('btn-primary');

            // Reset cart state after confirmation
            setTimeout(() => {
                cart = [];
                appliedDiscount = null;
                document.getElementById('discount-input').value = '';
                renderCart();
                showMessage('checkout-message', 'Your cozy order is on its way!', 'text-green-400');
                
                setTimeout(() => {
                    btn.textContent = originalText;
                    btn.classList.remove('bg-green-600', 'hover:bg-green-700', 'shadow-green-500/50');
                    btn.classList.add('btn-primary');
                    document.getElementById('checkout-message').classList.add('hidden');
                    hideModal('cart-modal');
                }, 2000);

            }, 1500);
        }

        // --- MENU SPECIFIC FUNCTIONS ---

        /** Renders Pre-Reciped Coffees with Add to Cart buttons */
        function renderPreRecipedCoffees() {
            const container = document.getElementById('coffee-pre-category');
            container.innerHTML = PRE_RECIPED_COFFEES.map(coffee => {
                const intersectingAllergens = coffee.allergens.filter(allergy => currentState.userAllergens.includes(allergy));
                const allergyWarning = intersectingAllergens.length > 0 
                    ? `<p class="text-xs text-red-400 font-semibold mt-2 flex items-center">
                        <span class="mr-1">⚠️</span> CONTAINS: ${intersectingAllergens.map(a => a.toUpperCase()).join(', ')}
                    </p>`
                    : '';
                
                // Add an item to the cart, the ID is its unique identifier
                const cartItem = { id: coffee.id, name: coffee.name, price: coffee.price, allergens: coffee.allergens };

                return `
                    <div class="card p-5 hover:shadow-[rgba(230,120,62,0.3)]">
                        <h4 class="text-xl font-bold text-amber-300">${coffee.name}</h4>
                        <p class="text-white/60 text-sm mb-3">${coffee.desc}</p>
                        <div class="flex justify-between items-center">
                            <p class="text-3xl font-extrabold text-white">₹ ${coffee.price}</p>
                            <button class="py-2 px-4 rounded-full btn-primary text-sm font-semibold" 
                                onclick='addToCart(${JSON.stringify(cartItem).replace(/'/g, "\\'")})'>
                                Add to Cart
                            </button>
                        </div>
                        ${allergyWarning}
                    </div>
                `;
            }).join('');
        }
        
        /** Toggles between menu categories (Pre-reciped, Custom, Snacks) */
        function showMenuCategory(target) {
            document.querySelectorAll('.menu-category').forEach(cat => cat.style.display = 'none');
            
            const targetCategory = document.getElementById(target + '-category');
            if (targetCategory) {
                targetCategory.style.display = 'grid';
                if (target === 'snacks') {
                    targetCategory.style.display = 'block'; // Snacks container is a block
                } else if (target === 'coffee-pre' || target === 'coffee-design') {
                    targetCategory.style.gridTemplateColumns = 'repeat(auto-fit, minmax(280px, 1fr))'; // Responsive Grid
                }
            }


            document.querySelectorAll('.menu-tab').forEach(tab => {
                tab.classList.remove('bg-accent', 'text-white');
                tab.classList.add('text-white/60', 'hover:bg-white/10');
            });
            
            const targetTab = document.querySelector(`[data-target="${target}"]`);
            if (targetTab) {
                targetTab.classList.add('bg-accent', 'text-white');
                targetTab.classList.remove('text-white/60', 'hover:bg-white/10');
            }

            if (target === 'snacks') {
                filterSnacks(); // Rerender snacks when the tab is clicked
            }
        }
        
        /** Initializes listeners for custom coffee builder */
        function initMenuListeners() {
            document.getElementById('base').addEventListener('change', calculateCustomCoffeePrice);
            document.getElementById('milk').addEventListener('change', calculateCustomCoffeePrice);
            document.getElementById('syrup').addEventListener('change', calculateCustomCoffeePrice);
            document.getElementById('price-filter').addEventListener('input', filterSnacks);
        }

        /** Calculates and displays the price for the custom coffee */
        function calculateCustomCoffeePrice() {
            const baseEl = document.getElementById('base');
            const milkEl = document.getElementById('milk');
            const syrupEl = document.getElementById('syrup');

            const basePrice = parseInt(baseEl.value) || 0;
            const milkPrice = parseInt(milkEl.value) || 0;
            const syrupPrice = parseInt(syrupEl.value) || 0;
            
            const totalPrice = basePrice + milkPrice + syrupPrice;
            document.getElementById('custom-price').textContent = `₹ ${totalPrice.toLocaleString('en-IN')}`;
            
            // Allergen checking logic
            const baseAllergens = JSON.parse(baseEl.options[baseEl.selectedIndex].getAttribute('data-allergens') || '[]');
            const milkAllergens = JSON.parse(milkEl.options[milkEl.selectedIndex].getAttribute('data-allergens') || '[]');
            const syrupAllergens = JSON.parse(syrupEl.options[syrupEl.selectedIndex].getAttribute('data-allergens') || '[]');
            
            // Manual check for dairy/nut milk/syrup since we didn't add data-allergens to every option
            const isDairyMilk = milkEl.options[milkEl.selectedIndex].getAttribute('data-name').includes('Milk'); // Skim, Whole
            if (isDairyMilk) baseAllergens.push('milk');

            const allAllergens = [...new Set([...baseAllergens, ...milkAllergens, ...syrupAllergens])];

            const intersectingAllergens = allAllergens.filter(allergy => currentState.userAllergens.includes(allergy));
            
            let messageEl = document.getElementById('custom-coffee-message');

            if (intersectingAllergens.length > 0) {
                messageEl.classList.remove('hidden');
                messageEl.className = 'text-red-400 font-semibold mt-4 text-center transition-opacity duration-300';
                messageEl.textContent = `⚠️ Your creation contains allergens: ${intersectingAllergens.map(a => a.toUpperCase()).join(' & ')}.`;
            } else {
                messageEl.classList.add('hidden');
                messageEl.textContent = '';
            }
        }

        /** Adds the custom coffee to the cart */
        function addCustomCoffeeToCart() {
            const baseEl = document.getElementById('base');
            const milkEl = document.getElementById('milk');
            const syrupEl = document.getElementById('syrup');

            const baseOption = baseEl.options[baseEl.selectedIndex];
            const milkOption = milkEl.options[milkEl.selectedIndex];
            const syrupOption = syrupEl.options[syrupEl.selectedIndex];
            
            const price = parseInt(baseEl.value) + parseInt(milkEl.value) + parseInt(syrupEl.value);

            // Construct unique name for the custom item
            let nameParts = [baseOption.getAttribute('data-name')];
            if (parseInt(milkEl.value) > 0) nameParts.push(milkOption.getAttribute('data-name'));
            if (parseInt(syrupEl.value) > 0) nameParts.push(syrupOption.getAttribute('data-name'));

            const name = nameParts.join(' + ');

            // Collect all potential allergens
            const baseAllergens = JSON.parse(baseOption.getAttribute('data-allergens') || '[]');
            const milkAllergens = JSON.parse(milkOption.getAttribute('data-allergens') || '[]');
            const syrupAllergens = JSON.parse(syrupOption.getAttribute('data-allergens') || '[]');
            
            // Manual check for dairy/nut milk/syrup since we didn't add data-allergens to every option
            const isDairyMilk = milkOption.getAttribute('data-name').includes('Milk'); // Skim, Whole
            if (isDairyMilk) baseAllergens.push('milk');

            const allAllergens = [...new Set([...baseAllergens, ...milkAllergens, ...syrupAllergens])];

            const customItem = {
                id: `Custom-${Date.now()}`, // Unique ID for each custom creation
                name: name,
                price: price,
                allergens: allAllergens
            };
            
            addToCart(customItem);
        }
        
        /** Filters and renders the snacks list based on price and user allergens */
        function filterSnacks() {
            const maxPrice = parseInt(document.getElementById('price-filter').value);
            const snacksListEl = document.getElementById('snacks-list');
            const maxPriceDisplayEl = document.getElementById('max-price-display');
            
            maxPriceDisplayEl.textContent = maxPrice;
            snacksListEl.innerHTML = ''; // Clear existing snacks
            
            snacksListEl.style.gridTemplateColumns = 'repeat(auto-fit, minmax(280px, 1fr))'; // Ensure grid structure

            const filteredSnacks = SNACKS_DATA.filter(snack => snack.price <= maxPrice);

            if (filteredSnacks.length === 0) {
                snacksListEl.innerHTML = '<p class="md:col-span-3 text-center text-white/60 pt-8">No snacks match your current price filter.</p>';
                return;
            }

            snacksListEl.innerHTML = filteredSnacks.map(snack => {
                const intersectingAllergens = snack.allergens.filter(allergy => currentState.userAllergens.includes(allergy));
                
                let allergyWarning = '';
                let cardClasses = 'card p-5 transition-shadow';

                if (intersectingAllergens.length > 0) {
                    allergyWarning = `<p class="text-xs text-red-400 font-semibold mt-2 flex items-center">
                        <span class="mr-1">⚠️</span> CONTAINS: ${intersectingAllergens.map(a => a.toUpperCase()).join(', ')}
                    </p>`;
                    cardClasses += ' border-red-500/50 hover:shadow-red-500/50';
                } else {
                    cardClasses += ' hover:shadow-[rgba(230,120,62,0.3)]';
                }

                // Add an item to the cart
                const cartItem = { id: snack.id, name: snack.name, price: snack.price, allergens: snack.allergens };

                return `
                    <div class="${cardClasses}">
                        <h4 class="text-xl font-bold text-amber-300">${snack.name}</h4>
                        <p class="text-white/60 text-sm mb-3">${snack.desc}</p>
                        <div class="flex justify-between items-center">
                            <p class="text-3xl font-extrabold text-white">₹ ${snack.price}</p>
                            <button class="py-2 px-4 rounded-full btn-primary text-sm font-semibold"
                                onclick='addToCart(${JSON.stringify(cartItem).replace(/'/g, "\\'")})'>
                                Add to Cart
                            </button>
                        </div>
                        ${allergyWarning}
                    </div>
                `;
            }).join('');
        }

        // --- FIREBASE AND STATE MANAGEMENT ---
        
        /** Handles the Firebase authentication state change */
        document.addEventListener('authReady', ({ detail }) => {
            const user = detail.user;
            const statusEl = document.getElementById('app-status');
            
            if (user) {
                currentState.userId = user.uid;
                statusEl.textContent = `Connected (User: ${user.uid.substring(0, 8)}...)`;
                
                // Update UI for logged-in user
                document.getElementById('login-btn').classList.add('hidden');
                document.getElementById('logout-btn').classList.remove('hidden');

                // Load private user data
                loadAllergens(); 
                loadCart(); // Load user's persistent cart
                
            } else {
                currentState.userId = null;
                statusEl.textContent = 'Connected (Anonymous)';

                // Update UI for logged-out user
                document.getElementById('login-btn').classList.remove('hidden');
                document.getElementById('logout-btn').classList.add('hidden');
                
                // Clear cart if anonymous user signs out, or let it persist in local state until next anonymous session
                // For simplicity, we keep the in-memory cart for anonymous sessions but won't persist it in Firestore
                cart = [];
                appliedDiscount = null;
                renderCart();
            }
            
            // After auth, ensure menu and allergen profiles are correctly filtered/displayed
            renderPreRecipedCoffees();
            loadAllergensUI();
            filterSnacks();
            setupReviewForm();
        });
        
        /** Loads user's saved cart from Firestore (Private Data) */
        async function loadCart() {
            if (!currentState.userId || !FB.db || !FB.doc || !FB.onSnapshot) return;

            const cartRef = FB.doc(FB.db, `artifacts/${FB.appId}/users/${currentState.userId}/cart`, 'current');

            try {
                // Using onSnapshot for real-time cart updates
                FB.onSnapshot(cartRef, (docSnap) => {
                    if (docSnap.exists() && docSnap.data().items) {
                        // IMPORTANT: JSON.parse cart items if any were stringified (not strictly needed here, but good practice)
                        cart = docSnap.data().items || [];
                        appliedDiscount = docSnap.data().discount || null;
                    } else {
                        cart = [];
                        appliedDiscount = null;
                    }
                    console.log("Cart loaded:", cart);
                    // Rerender the cart UI after loading from DB
                    renderCart(); 
                }, (error) => {
                    console.error("Error setting up onSnapshot for cart: ", error);
                });
            } catch (e) {
                console.error("Error loading cart: ", e);
            }
        }

        /** Saves user's current cart state to Firestore (Private Data) */
        async function saveCart() {
            if (!currentState.userId || !FB.db || !FB.doc || !FB.setDoc || cart.length === 0) {
                // Do not save cart if not logged in or cart is empty
                if (currentState.userId && FB.db && FB.doc && FB.setDoc) {
                    // If cart is empty, delete the document to clean up
                    const cartRef = FB.doc(FB.db, `artifacts/${FB.appId}/users/${currentState.userId}/cart`, 'current');
                    await FB.setDoc(cartRef, { items: [], discount: null, timestamp: FB.serverTimestamp() });
                }
                return;
            }

            const cartRef = FB.doc(FB.db, `artifacts/${FB.appId}/users/${currentState.userId}/cart`, 'current');
            
            try {
                // Using FB.setDoc and FB.serverTimestamp to access the Firestore functions
                await FB.setDoc(cartRef, { 
                    items: cart, 
                    discount: appliedDiscount,
                    timestamp: FB.serverTimestamp() 
                }, { merge: true });
                console.log("Cart saved successfully.");
            } catch (e) {
                console.error("Error saving cart: ", e);
            }
        }
        
        // --- AUTH & OTHER FIREBASE FUNCTIONS (Retained from previous version) ---
        
        /** Mocks user login/signup and updates UI */
        async function handleLogin(isSignup = false) {
            const email = document.getElementById('login-email').value;
            const password = document.getElementById('login-password').value;
            const msgEl = document.getElementById('login-message');

            if (!email || !password) {
                showMessage('login-message', 'Please enter both email and password.', 'text-red-400');
                return;
            }
            
            showMessage('login-message', isSignup ? 'Attempting sign up...' : 'Attempting log in...', 'text-amber-300');

            await new Promise(resolve => setTimeout(resolve, 1000));
            
            showMessage('login-message', isSignup ? 'Sign up successful! Please refresh.' : 'Login successful! Please refresh.', 'text-green-400');
            setTimeout(() => hideModal('login-modal'), 1500);
        }

        /** Logs out the user (simulated, as canvas auto-reauthenticates) */
        async function handleLogout() {
            if (FB.auth) {
                await signOut(FB.auth);
                console.log("User signed out.");
                const logoutBtn = document.getElementById('logout-btn');
                const originalText = logoutBtn.innerHTML;
                logoutBtn.innerHTML = '<span class="hidden sm:inline">Signed Out!</span><span class="sm:hidden">✅</span>';
                setTimeout(() => {
                    logoutBtn.innerHTML = originalText;
                    showSection(null, 'home');
                }, 1000);
            }
        }

        /** Loads user's saved allergen profile from Firestore (Private Data) */
        async function loadAllergens() {
            if (!currentState.userId || !FB.db || !FB.doc || !FB.onSnapshot) return;

            const allergensRef = FB.doc(FB.db, `artifacts/${FB.appId}/users/${currentState.userId}/allergens`, 'profile');

            try {
                FB.onSnapshot(allergensRef, (docSnap) => {
                    if (docSnap.exists()) {
                        currentState.userAllergens = docSnap.data().list || [];
                    } else {
                        currentState.userAllergens = [];
                    }
                    console.log("Allergens loaded:", currentState.userAllergens);
                    loadAllergensUI();
                    filterSnacks(); 
                    renderPreRecipedCoffees(); // Update coffee warnings
                    calculateCustomCoffeePrice(); // Update custom coffee warnings
                    renderCart(); // Update cart warnings
                }, (error) => {
                    console.error("Error setting up onSnapshot for allergens: ", error);
                });
            } catch (e) {
                console.error("Error loading allergens: ", e);
            }
        }

        /** Saves user's allergen profile to Firestore (Private Data) */
        async function saveAllergens() {
            if (!currentState.userId || !FB.db || !FB.doc || !FB.setDoc || !FB.serverTimestamp) {
                showMessage('allergen-save-message', 'Error: Not logged in or DB not ready.', 'text-red-400');
                return;
            }
            
            const selectedAllergens = Array.from(document.querySelectorAll('#allergen-form-container input[type="checkbox"]:checked'))
                                         .map(cb => cb.value);

            const allergensRef = FB.doc(FB.db, `artifacts/${FB.appId}/users/${currentState.userId}/allergens`, 'profile');
            
            try {
                await FB.setDoc(allergensRef, { list: selectedAllergens, timestamp: FB.serverTimestamp() });
                showMessage('allergen-save-message', 'Profile saved successfully! 🎉', 'text-green-400');
                currentState.userAllergens = selectedAllergens;
                
                // Rerender all components that depend on allergens
                filterSnacks(); 
                renderPreRecipedCoffees();
                calculateCustomCoffeePrice(); 
                renderCart();
                
                setTimeout(() => document.getElementById('allergen-save-message').classList.add('hidden'), 3000);
            } catch (e) {
                showMessage('allergen-save-message', 'Error saving profile.', 'text-red-400');
                console.error("Error saving allergens: ", e);
            }
        }
        
        /** Updates the Allergen UI based on current auth state and loaded data */
        function loadAllergensUI() {
            const formContainer = document.getElementById('allergen-form-container');
            const statusEl = document.getElementById('allergen-status');
            const userIdEl = document.getElementById('allergen-user-id');
            
            if (currentState.userId) {
                formContainer.classList.remove('hidden');
                statusEl.classList.add('hidden');
                userIdEl.classList.remove('hidden');
                userIdEl.textContent = `User ID: ${currentState.userId}`;

                document.querySelectorAll('#allergen-form-container input[type="checkbox"]').forEach(cb => {
                    cb.checked = currentState.userAllergens.includes(cb.value);
                });
            } else {
                formContainer.classList.add('hidden');
                statusEl.classList.remove('hidden');
                userIdEl.classList.add('hidden');
            }
        }

        /** Sets up the review form visibility based on auth state */
        function setupReviewForm() {
            const form = document.getElementById('actual-review-form');
            const prompt = document.getElementById('review-login-prompt');
            
            if (currentState.userId) {
                form.classList.remove('hidden');
                prompt.classList.add('hidden');
            } else {
                form.classList.add('hidden');
                prompt.classList.remove('hidden');
            }
        }

        /** Submits a new review to Firestore (Public Data) */
        async function submitReview() {
            if (!currentState.userId || !FB.db || !FB.collection || !FB.addDoc || !FB.serverTimestamp) {
                showMessage('review-message', 'Error: Must be logged in to submit a review.', 'text-red-400');
                return;
            }

            const title = document.getElementById('review-title').value.trim();
            const text = document.getElementById('review-text').value.trim();
            const rating = parseInt(document.getElementById('review-rating').value);

            if (!title || !text || !rating) {
                showMessage('review-message', 'Please fill in all fields.', 'text-red-400');
                return;
            }

            const reviewsCol = FB.collection(FB.db, `artifacts/${FB.appId}/public/data/ratings`);
            
            try {
                await FB.addDoc(reviewsCol, {
                    title: title,
                    text: text,
                    rating: rating,
                    userId: currentState.userId,
                    timestamp: FB.serverTimestamp()
                });

                showMessage('review-message', 'Review submitted successfully! 🌟', 'text-green-400');
                document.getElementById('review-title').value = '';
                document.getElementById('review-text').value = '';
                setTimeout(() => document.getElementById('review-message').classList.add('hidden'), 3000);
            } catch (e) {
                showMessage('review-message', 'Error submitting review.', 'text-red-400');
                console.error("Error submitting review: ", e);
            }
        }

        /** Loads and displays real-time reviews from Firestore (Public Data) */
        function loadReviews() {
            if (!FB.db || !FB.collection || !FB.onSnapshot) {
                document.getElementById('reviews-list').innerHTML = '<p class="md:col-span-2 text-center text-red-400 pt-8">Database functions are not available.</p>';
                return;
            }
            
            const reviewsListEl = document.getElementById('reviews-list');
            const reviewsCol = FB.collection(FB.db, `artifacts/${FB.appId}/public/data/ratings`);
            
            FB.onSnapshot(reviewsCol, (snapshot) => {
                const reviews = [];
                snapshot.forEach(doc => {
                    const data = doc.data();
                    const date = data.timestamp ? new Date(data.timestamp.seconds * 1000).toLocaleDateString() : 'N/A';
                    reviews.push({ id: doc.id, ...data, date: date });
                });
                
                reviews.sort((a, b) => {
                    if (a.timestamp && b.timestamp) {
                        return b.timestamp.seconds - a.timestamp.seconds;
                    }
                    return 0;
                }); 

                if (reviews.length === 0) {
                    reviewsListEl.innerHTML = '<p class="md:col-span-2 text-center text-white/60 pt-8">Be the first to leave a warm review!</p>';
                    return;
                }

                reviewsListEl.innerHTML = reviews.map(review => {
                    const stars = '⭐'.repeat(review.rating);
                    const isUserReview = review.userId === currentState.userId ? 'border-accent border-l-4 pl-4' : '';
                    const username = review.userId ? `User: ${review.userId.substring(0, 8)}...` : 'Anonymous User';

                    return `
                        <div class="card p-6 space-y-2 ${isUserReview}">
                            <div class="flex justify-between items-center">
                                <h4 class="text-xl font-bold text-amber-300">${review.title}</h4>
                                <span class="text-sm text-white/40">${review.date}</span>
                            </div>
                            <div class="text-white text-2xl">${stars}</div>
                            <p class="text-white/70 italic">${review.text}</p>
                            <p class="text-xs text-white/40 pt-2">${username}</p>
                        </div>
                    `;
                }).join('');

            }, (error) => {
                console.error("Error fetching reviews: ", error);
                reviewsListEl.innerHTML = '<p class="md:col-span-2 text-center text-red-400 pt-8">Failed to load reviews.</p>';
            });
        }
    </script>
    
</body>
</html>

