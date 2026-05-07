<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cosmic Tarot - AI Readings</title>
    <link href="[fonts.googleapis.com](https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700&family=Raleway:wght@300;400;500&display=swap)" rel="stylesheet">
    <style>
        :root {
            --primary: #6b5ce7;
            --secondary: #f0c674;
            --bg-dark: #0a0a14;
            --bg-card: #12121f;
            --text-light: #e8e8f0;
            --text-muted: #8888a0;
            --glow: rgba(107, 92, 231, 0.4);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Raleway', sans-serif;
            background: var(--bg-dark);
            color: var(--text-light);
            min-height: 100vh;
            overflow-x: hidden;
        }

        /* Starfield Background */
        .starfield {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
        }

        .star {
            position: absolute;
            background: white;
            border-radius: 50%;
            animation: twinkle 3s infinite ease-in-out;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        /* Header */
        header {
            position: relative;
            z-index: 10;
            padding: 2rem;
            text-align: center;
            border-bottom: 1px solid rgba(107, 92, 231, 0.2);
        }

        .logo {
            font-family: 'Cinzel', serif;
            font-size: 2.5rem;
            font-weight: 700;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-shadow: 0 0 40px var(--glow);
        }

        .tagline {
            color: var(--text-muted);
            margin-top: 0.5rem;
            font-weight: 300;
            letter-spacing: 2px;
            text-transform: uppercase;
            font-size: 0.85rem;
        }

        /* Main Container */
        main {
            position: relative;
            z-index: 10;
            max-width: 1200px;
            margin: 0 auto;
            padding: 3rem 1.5rem;
        }

        /* Section Titles */
        .section-title {
            font-family: 'Cinzel', serif;
            font-size: 1.8rem;
            text-align: center;
            margin-bottom: 2rem;
            color: var(--secondary);
        }

        /* Theme Selection */
        .theme-section {
            margin-bottom: 3rem;
        }

        .theme-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 1.5rem;
        }

        .theme-card {
            background: var(--bg-card);
            border: 1px solid rgba(107, 92, 231, 0.3);
            border-radius: 16px;
            padding: 2rem;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
        }

        .theme-card:hover {
            transform: translateY(-5px);
            border-color: var(--primary);
            box-shadow: 0 10px 40px var(--glow);
        }

        .theme-card.selected {
            border-color: var(--secondary);
            background: linear-gradient(135deg, rgba(107, 92, 231, 0.2), rgba(240, 198, 116, 0.1));
        }

        .theme-icon {
            font-size: 3rem;
            margin-bottom: 1rem;
        }

        .theme-name {
            font-family: 'Cinzel', serif;
            font-size: 1.3rem;
            margin-bottom: 0.5rem;
        }

        .theme-desc {
            color: var(--text-muted);
            font-size: 0.9rem;
        }

        /* Spread Selection */
        .spread-section {
            margin-bottom: 3rem;
        }

        .spread-options {
            display: flex;
            justify-content: center;
            gap: 1.5rem;
            flex-wrap: wrap;
        }

        .spread-btn {
            background: transparent;
            border: 2px solid var(--primary);
            color: var(--text-light);
            padding: 1rem 2.5rem;
            border-radius: 50px;
            font-family: 'Raleway', sans-serif;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .spread-btn:hover {
            background: var(--primary);
            box-shadow: 0 0 30px var(--glow);
        }

        .spread-btn.selected {
            background: linear-gradient(135deg, var(--primary), #8b7cf7);
            border-color: transparent;
        }

        /* Reading Button */
        .reading-section {
            text-align: center;
            margin-bottom: 4rem;
        }

        .reading-btn {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            border: none;
            color: var(--bg-dark);
            padding: 1.2rem 4rem;
            border-radius: 50px;
            font-family: 'Cinzel', serif;
            font-size: 1.3rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 30px var(--glow);
        }

        .reading-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 10px 50px var(--glow);
        }

        .reading-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        /* Cards Display */
        .cards-section {
            display: none;
            margin-bottom: 3rem;
        }

        .cards-section.visible {
            display: block;
            animation: fadeIn 0.8s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .cards-container {
            display: flex;
            justify-content: center;
            gap: 2rem;
            flex-wrap: wrap;
            perspective: 1000px;
        }

        .tarot-card {
            width: 180px;
            height: 300px;
            position: relative;
            cursor: pointer;
            transform-style: preserve-3d;
            transition: transform 0.8s ease;
        }

        .tarot-card.flipped {
            transform: rotateY(180deg);
        }

        .card-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
        }

        .card-back {
            background: linear-gradient(135deg, #1a1a2e, #2d2d4a);
            border: 2px solid var(--primary);
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
        }

        .card-back-pattern {
            width: 80%;
            height: 80%;
            border: 1px solid var(--primary);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: repeating-linear-gradient(
                45deg,
                transparent,
                transparent 10px,
                rgba(107, 92, 231, 0.1) 10px,
                rgba(107, 92, 231, 0.1) 20px
            );
        }

        .card-back-symbol {
            font-size: 3rem;
            color: var(--secondary);
        }

        .card-front {
            background: linear-gradient(180deg, #1e1e3a, #12121f);
            border: 2px solid var(--secondary);
            transform: rotateY(180deg);
            padding: 1rem;
            text-align: center;
        }

        .card-image {
            width: 100%;
            height: 180px;
            background: linear-gradient(135deg, rgba(107, 92, 231, 0.3), rgba(240, 198, 116, 0.2));
            border-radius: 8px;
            margin-bottom: 0.8rem;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 4rem;
        }

        .card-title {
            font-family: 'Cinzel', serif;
            font-size: 0.95rem;
            color: var(--secondary);
            margin-bottom: 0.3rem;
        }

        .card-position {
            font-size: 0.75rem;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        /* Interpretation */
        .interpretation-section {
            display: none;
            background: var(--bg-card);
            border: 1px solid rgba(107, 92, 231, 0.3);
            border-radius: 20px;
            padding: 2.5rem;
            margin-top: 2rem;
        }

        .interpretation-section.visible {
            display: block;
            animation: fadeIn 1s ease;
        }

        .interpretation-header {
            text-align: center;
            margin-bottom: 2rem;
        }

        .interpretation-title {
            font-family: 'Cinzel', serif;
            font-size: 1.5rem;
            color: var(--secondary);
            margin-bottom: 0.5rem;
        }

        .interpretation-subtitle {
            color: var(--text-muted);
            font-size: 0.9rem;
        }

        .interpretation-content {
            line-height: 1.8;
        }

        .card-interpretation {
            margin-bottom: 2rem;
            padding-bottom: 2rem;
            border-bottom: 1px solid rgba(107, 92, 231, 0.2);
        }

        .card-interpretation:last-child {
            border-bottom: none;
            margin-bottom: 0;
            padding-bottom: 0;
        }

        .card-interpretation h4 {
            font-family: 'Cinzel', serif;
            color: var(--primary);
            margin-bottom: 0.8rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .card-interpretation p {
            color: var(--text-light);
            opacity: 0.9;
        }

        .overall-message {
            background: linear-gradient(135deg, rgba(107, 92, 231, 0.15), rgba(240, 198, 116, 0.1));
            padding: 1.5rem;
            border-radius: 12px;
            margin-top: 2rem;
        }

        .overall-message h4 {
            font-family: 'Cinzel', serif;
            color: var(--secondary);
            margin-bottom: 0.8rem;
        }

        /* New Reading Button */
        .new-reading-btn {
            display: none;
            margin: 2rem auto;
            background: transparent;
            border: 2px solid var(--primary);
            color: var(--text-light);
            padding: 1rem 3rem;
            border-radius: 50px;
            font-family: 'Raleway', sans-serif;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .new-reading-btn.visible {
            display: block;
        }

        .new-reading-btn:hover {
            background: var(--primary);
            box-shadow: 0 0 30px var(--glow);
        }

        /* Loading Animation */
        .loading {
            display: none;
            text-align: center;
            padding: 3rem;
        }

        .loading.visible {
            display: block;
        }

        .loading-orb {
            width: 80px;
            height: 80px;
            margin: 0 auto 1.5rem;
            border-radius: 50%;
            background: radial-gradient(circle, var(--secondary), var(--primary));
            animation: pulse 1.5s infinite ease-in-out;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); opacity: 0.8; }
            50% { transform: scale(1.2); opacity: 1; }
        }

        .loading-text {
            color: var(--text-muted);
            font-style: italic;
        }

        /* Footer */
        footer {
            position: relative;
            z-index: 10;
            text-align: center;
            padding: 2rem;
            border-top: 1px solid rgba(107, 92, 231, 0.2);
            color: var(--text-muted);
            font-size: 0.85rem;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .logo { font-size: 1.8rem; }
            .theme-grid { grid-template-columns: 1fr; }
            .tarot-card { width: 140px; height: 240px; }
            .card-image { height: 140px; font-size: 3rem; }
            .reading-btn { padding: 1rem 2.5rem; font-size: 1.1rem; }
        }
    </style>
</head>
<body>
    <div class="starfield" id="starfield"></div>

    <header>
        <h1 class="logo">✧ Cosmic Tarot ✧</h1>
        <p class="tagline">AI-Powered Mystical Guidance</p>
    </header>

    <main>
        <!-- Theme Selection -->
        <section class="theme-section">
            <h2 class="section-title">Choose Your Focus</h2>
            <div class="theme-grid">
                <div class="theme-card" data-theme="love">
                    <div class="theme-icon">💕</div>
                    <h3 class="theme-name">Love & Relationships</h3>
                    <p class="theme-desc">Explore matters of the heart and emotional connections</p>
                </div>
                <div class="theme-card" data-theme="career">
                    <div class="theme-icon">✨</div>
                    <h3 class="theme-name">Career & Success</h3>
                    <p class="theme-desc">Guidance for professional growth and opportunities</p>
                </div>
                <div class="theme-card" data-theme="growth">
                    <div class="theme-icon">🌙</div>
                    <h3 class="theme-name">Personal Growth</h3>
                    <p class="theme-desc">Self-discovery, spirituality, and inner wisdom</p>
                </div>
            </div>
        </section>

        <!-- Spread Selection -->
        <section class="spread-section">
            <h2 class="section-title">Select Your Spread</h2>
            <div class="spread-options">
                <button class="spread-btn" data-spread="single">Single Card</button>
                <button class="spread-btn" data-spread="three">Three Cards</button>
            </div>
        </section>

        <!-- Reading Button -->
        <section class="reading-section">
            <button class="reading-btn" id="startReading" disabled>Begin Your Reading</button>
        </section>

        <!-- Loading -->
        <div class="loading" id="loading">
            <div class="loading-orb"></div>
            <p class="loading-text">The cosmos is revealing your path...</p>
        </div>

        <!-- Cards Display -->
        <section class="cards-section" id="cardsSection">
            <h2 class="section-title">Your Cards</h2>
            <p style="text-align: center; color: var(--text-muted); margin-bottom: 2rem;">Click each card to reveal its message</p>
            <div class="cards-container" id="cardsContainer"></div>
        </section>

        <!-- Interpretation -->
        <section class="interpretation-section" id="interpretationSection">
            <div class="interpretation-header">
                <h3 class="interpretation-title">Your Reading</h3>
                <p class="interpretation-subtitle" id="interpretationTheme"></p>
            </div>
            <div class="interpretation-content" id="interpretationContent"></div>
        </section>

        <!-- New Reading Button -->
        <button class="new-reading-btn" id="newReading">Start New Reading</button>
    </main>

    <footer>
        <p>✧ For entertainment and self-reflection purposes ✧</p>
        <p style="margin-top: 0.5rem;">Cosmic Tarot © 2024</p>
    </footer>

    <script>
        // Tarot Card Data
        const tarotCards = [
            # Moebius风格 78张塔罗牌 AI生成网页（HTML）

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Moebius Tarot 78</title>
<style>
body {
    margin: 0;
    background: #0a0f1e;
    color: #f5f5f5;
    font-family: Helvetica, Arial, sans-serif;
}

header {
    padding: 40px;
    text-align: center;
    border-bottom: 1px solid rgba(255,255,255,0.1);
}

h1 {
    font-size: 42px;
    letter-spacing: 2px;
}

p {
    color: #bbbbbb;
}

.grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 24px;
    padding: 40px;
}

.card {
    background: rgba(255,255,255,0.04);
    border: 1px solid rgba(255,255,255,0.08);
    border-radius: 18px;
    padding: 20px;
    backdrop-filter: blur(12px);
}

.card h2 {
    font-size: 20px;
    margin-bottom: 12px;
}

.prompt {
    white-space: pre-wrap;
    line-height: 1.6;
    color: #d7d7d7;
    font-size: 14px;
}
</style>
</head>
<body>

<header>
<h1>MOEBIUS TAROT 78</h1>
<p>超现实宇宙感 · 梦境科幻 · AI Spiritual Tarot</p>
</header>

<div class="grid">

<script>
const baseStyle = `
Moebius style, futuristic tarot illustration, surreal cosmic landscape,
clean line art, spiritual sci-fi atmosphere, dreamlike desert planet,
mystical universe, cinematic composition, elegant color palette,
French sci-fi comic aesthetic, transcendental symbolism,
ethereal light, philosophical visual language,
high detail, ultra clean illustration,
3:4 aspect ratio
`;

const cards = [

// Major Arcana
{
name: "0 The Fool",
prompt: `young cosmic traveler walking toward a floating cliff in space, celestial birds, endless dream desert, glowing stars, innocence and transcendence, ${baseStyle}`
},
{
name: "I The Magician",
prompt: `mystic engineer manipulating floating holographic symbols, cosmic tools orbiting around hands, surreal futuristic temple, ${baseStyle}`
},
{
name: "II The High Priestess",
prompt: `silent oracle seated between cosmic pillars, moon portal behind her, dream architecture and spiritual AI atmosphere, ${baseStyle}`
},
{
name: "III The Empress",
prompt: `cosmic mother in floating bio-organic garden, surreal vegetation on alien planet, nurturing universe energy, ${baseStyle}`
},
{
name: "IV The Emperor",
prompt: `ancient galactic ruler seated on geometric throne, monumental cosmic structures, stoic philosophical atmosphere, ${baseStyle}`
},
{
name: "V The Hierophant",
prompt: `spiritual teacher transmitting holographic wisdom, sacred futuristic cathedral, cosmic ritual atmosphere, ${baseStyle}`
},
{
name: "VI The Lovers",
prompt: `two souls connected through luminous energy bridge in surreal cosmic valley, transcendent romance, ${baseStyle}`
},
{
name: "VII The Chariot",
prompt: `celestial traveler piloting biomechanical chariot through stars and desert planets, movement and determination, ${baseStyle}`
},
{
name: "VIII Strength",
prompt: `mystic figure gently taming cosmic lion made of starlight, spiritual courage and calm energy, ${baseStyle}`
},
{
name: "IX The Hermit",
prompt: `solitary wanderer carrying glowing lantern across infinite alien desert under giant galaxies, spiritual pilgrimage, ${baseStyle}`
},
{
name: "X Wheel of Fortune",
prompt: `massive cosmic wheel floating in deep universe, abstract destiny mechanism, dreamlike celestial motion, ${baseStyle}`
},
{
name: "XI Justice",
prompt: `ethereal judge balancing planets and light, geometric symmetry, futuristic sacred courtroom, ${baseStyle}`
},
{
name: "XII The Hanged Man",
prompt: `floating enlightened figure suspended upside down in cosmic void, transcendental awakening, surreal dream state, ${baseStyle}`
},
{
name: "XIII Death",
prompt: `silent cosmic transformation figure crossing alien wasteland, rebirth through starlight and ruins, ${baseStyle}`
},
{
name: "XIV Temperance",
prompt: `angelic being pouring luminous liquid between futuristic vessels under galaxy sky, harmony and balance, ${baseStyle}`
},
{
name: "XV The Devil",
prompt: `surreal technological temptation, chained souls connected to glowing machine, abstract psychological symbolism, ${baseStyle}`
},
{
name: "XVI The Tower",
prompt: `futuristic tower collapsing in cosmic lightning storm, surreal destruction and awakening, ${baseStyle}`
},
{
name: "XVII The Star",
prompt: `ethereal cosmic figure pouring starlight into dream ocean beneath massive constellations, hope and transcendence, ${baseStyle}`
},
{
name: "XVIII The Moon",
prompt: `mysterious lunar desert with strange creatures and psychic pathways, surreal subconscious dreamscape, ${baseStyle}`
},
{
name: "XIX The Sun",
prompt: `radiant cosmic child beneath giant surreal sun, spiritual illumination and warmth, ${baseStyle}`
},
{
name: "XX Judgement",
prompt: `souls awakening from crystal pods beneath celestial signal, transcendence and rebirth, ${baseStyle}`
},
{
name: "XXI The World",
prompt: `cosmic dancer floating within sacred geometric portal, completion and universal harmony, ${baseStyle}`
},

// Wands
{
name: "Ace of Wands",
prompt: `glowing energy staff emerging from cosmic clouds, creative force awakening, ${baseStyle}`
},
{
name: "Two of Wands",
prompt: `traveler observing multiple planets from futuristic balcony, visionary ambition, ${baseStyle}`
},
{
name: "Three of Wands",
prompt: `cosmic explorer watching ships crossing stars, expansion and discovery, ${baseStyle}`
},
{
name: "Four of Wands",
prompt: `celebration beneath floating geometric arches in alien oasis, harmony and joy, ${baseStyle}`
},
{
name: "Five of Wands",
prompt: `abstract cosmic conflict between energy staffs and dream warriors, ${baseStyle}`
},
{
name: "Six of Wands",
prompt: `victorious traveler riding luminous creature across futuristic city, triumph and recognition, ${baseStyle}`
},
{
name: "Seven of Wands",
prompt: `guardian defending elevated cosmic platform with glowing staff, resilience and courage, ${baseStyle}`
},
{
name: "Eight of Wands",
prompt: `energy staffs flying rapidly through cosmic sky, movement and acceleration, ${baseStyle}`
},
{
name: "Nine of Wands",
prompt: `weary spiritual warrior protecting sacred portal in alien desert, persistence, ${baseStyle}`
},
{
name: "Ten of Wands",
prompt: `traveler carrying heavy luminous staffs through dreamlike wasteland, burden and endurance, ${baseStyle}`
},

// Cups
{
name: "Ace of Cups",
prompt: `floating cosmic chalice overflowing with starlight and spiritual water, emotional awakening, ${baseStyle}`
},
{
name: "Two of Cups",
prompt: `two cosmic beings exchanging luminous energy cups, sacred union, ${baseStyle}`
},
{
name: "Three of Cups",
prompt: `three celestial figures celebrating beneath stars in surreal oasis, friendship and joy, ${baseStyle}`
},
{
name: "Four of Cups",
prompt: `meditative traveler ignoring floating cosmic chalices, introspection and detachment, ${baseStyle}`
},
{
name: "Five of Cups",
prompt: `lonely figure mourning spilled starlight cups beneath moonlit desert, melancholy atmosphere, ${baseStyle}`
},
{
name: "Six of Cups",
prompt: `dreamlike childhood memory in futuristic village with glowing cups, nostalgia and innocence, ${baseStyle}`
},
{
name: "Seven of Cups",
prompt: `floating surreal visions emerging from cosmic chalices, illusion and imagination, ${baseStyle}`
},
{
name: "Eight of Cups",
prompt: `solitary traveler leaving glowing cups behind, walking toward distant cosmic mountains, spiritual search, ${baseStyle}`
},
{
name: "Nine of Cups",
prompt: `peaceful cosmic sage surrounded by luminous cups and celestial abundance, fulfillment, ${baseStyle}`
},
{
name: "Ten of Cups",
prompt: `harmonious cosmic family beneath rainbow nebula, emotional completion, ${baseStyle}`
},

// Swords
{
name: "Ace of Swords",
prompt: `luminous cosmic sword emerging from clouds, clarity and truth, ${baseStyle}`
},
{
name: "Two of Swords",
prompt: `blindfolded futuristic oracle balancing crossed energy blades under moonlight, ${baseStyle}`
},
{
name: "Three of Swords",
prompt: `surreal crystal heart pierced by cosmic blades beneath storm sky, emotional pain, ${baseStyle}`
},
{
name: "Four of Swords",
prompt: `resting traveler inside sacred futuristic chamber, meditation and healing, ${baseStyle}`
},
{
name: "Five of Swords",
prompt: `abstract battlefield with abandoned energy swords and psychological tension, ${baseStyle}`
},
{
name: "Six of Swords",
prompt: `journey across silent cosmic sea in futuristic vessel, transition and healing, ${baseStyle}`
},
{
name: "Seven of Swords",
prompt: `mysterious rogue carrying glowing blades through surreal dream city, strategy and secrecy, ${baseStyle}`
},
{
name: "Eight of Swords",
prompt: `trapped figure surrounded by floating energy swords in psychic desert, limitation and illusion, ${baseStyle}`
},
{
name: "Nine of Swords",
prompt: `nightmare chamber filled with cosmic anxiety and glowing blades, dream terror, ${baseStyle}`
},
{
name: "Ten of Swords",
prompt: `fallen traveler beneath cosmic sunrise, ending and transformation, ${baseStyle}`
},

// Pentacles
{
name: "Ace of Pentacles",
prompt: `floating cosmic coin surrounded by sacred geometry and alien vegetation, material manifestation, ${baseStyle}`
},
{
name: "Two of Pentacles",
prompt: `acrobat balancing luminous planetary coins in zero gravity, adaptability and rhythm, ${baseStyle}`
},
{
name: "Three of Pentacles",
prompt: `futuristic architects building sacred cosmic structure together, collaboration, ${baseStyle}`
},
{
name: "Four of Pentacles",
prompt: `solitary ruler protecting glowing cosmic coins in geometric fortress, control and stability, ${baseStyle}`
},
{
name: "Five of Pentacles",
prompt: `wanderers crossing frozen alien city in hardship beneath distant light, ${baseStyle}`
},
{
name: "Six of Pentacles",
prompt: `cosmic benefactor sharing luminous coins with travelers, generosity and balance, ${baseStyle}`
},
{
name: "Seven of Pentacles",
prompt: `traveler contemplating glowing energy plants in surreal garden, patience and growth, ${baseStyle}`
},
{
name: "Eight of Pentacles",
prompt: `futuristic artisan crafting sacred geometric coins with precision, mastery and dedication, ${baseStyle}`
},
{
name: "Nine of Pentacles",
prompt: `elegant cosmic noble walking through luxurious alien garden, refinement and independence, ${baseStyle}`
},
{
name: "Ten of Pentacles",
prompt: `vast cosmic civilization with sacred architecture and ancestral wisdom, legacy and abundance, ${baseStyle}`
}
];

// 自动补足宫廷牌
const suits = ["Wands", "Cups", "Swords", "Pentacles"];
const courts = ["Page", "Knight", "Queen", "King"];

suits.forEach(suit => {
    courts.forEach(rank => {
        cards.push({
            name: `${rank} of ${suit}`,
            prompt: `${rank.toLowerCase()} archetype of ${suit.toLowerCase()}, surreal cosmic symbolism, spiritual sci-fi character design, mystical futuristic landscape, ${baseStyle}`
        });
    });
});

const container = document.querySelector('.grid');

cards.forEach(card => {
    const div = document.createElement('div');
    div.className = 'card';
    div.innerHTML = `
        <h2>${card.name}</h2>
        <div class="prompt">${card.prompt}</div>
    `;
    container.appendChild(div);
});
</script>
</body>
</html>
        ];

        const themeInterpretations = {
            love: {
                title: "Love & Relationships Reading",
                context: "romantic connections, emotional bonds, and heart matters"
            },
            career: {
                title: "Career & Success Reading",
                context: "professional growth, opportunities, and achievement"
            },
            growth: {
                title: "Personal Growth Reading",
                context: "self-discovery, spiritual development, and inner wisdom"
            }
        };

        const positions = {
            single: ["Your Message"],
            three: ["Past", "Present", "Future"]
        };

        // State
        let selectedTheme = null;
        let selectedSpread = null;
        let drawnCards = [];
        let revealedCards = 0;

        // Create starfield
        function createStarfield() {
            const starfield = document.getElementById('starfield');
            for (let i = 0; i < 100; i++) {
                const star = document.createElement('div');
                star.className = 'star';
                star.style.left = Math.random() * 100 + '%';
                star.style.top = Math.random() * 100 + '%';
                star.style.width = star.style.height = Math.random() * 3 + 1 + 'px';
                star.style.animationDelay = Math.random() * 3 + 's';
                starfield.appendChild(star);
            }
        }

        // Theme selection
        document.querySelectorAll('.theme-card').forEach(card => {
            card.addEventListener('click', () => {
                document.querySelectorAll('.theme-card').forEach(c => c.classList.remove('selected'));
                card.classList.add('selected');
                selectedTheme = card.dataset.theme;
                updateReadingButton();
            });
        });

        // Spread selection
        document.querySelectorAll('.spread-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.spread-btn').forEach(b => b.classList.remove('selected'));
                btn.classList.add('selected');
                selectedSpread = btn.dataset.spread;
                updateReadingButton();
            });
        });

        function updateReadingButton() {
            document.getElementById('startReading').disabled = !(selectedTheme && selectedSpread);
        }

        // Start reading
        document.getElementById('startReading').addEventListener('click', startReading);

        function startReading() {
            document.getElementById('loading').classList.add('visible');
            document.getElementById('cardsSection').classList.remove('visible');
            document.getElementById('interpretationSection').classList.remove('visible');
            document.getElementById('newReading').classList.remove('visible');
            
            // Simulate AI processing
            setTimeout(() => {
                document.getElementById('loading').classList.remove('visible');
                drawCards();
            }, 2000);
        }

        function drawCards() {
            const numCards = selectedSpread === 'single' ? 1 : 3;
            drawnCards = [];
            revealedCards = 0;
            
            // Shuffle and draw
            const shuffled = [...tarotCards].sort(() => Math.random() - 0.5);
            drawnCards = shuffled.slice(0, numCards);
            
            // Render cards
            const container = document.getElementById('cardsContainer');
            container.innerHTML = '';
            
            drawnCards.forEach((card, index) => {
                const cardEl = document.createElement('div');
                cardEl.className = 'tarot-card';
                cardEl.innerHTML = `
                    <div class="card-face card-back">
                        <div class="card-back-pattern">
                            <span class="card-back-symbol">✧</span>
                        </div>
                    </div>
                    <div class="card-face card-front">
                        <div class="card-image">${card.symbol}</div>
                        <h4 class="card-title">${card.name}</h4>
                        <p class="card-position">${positions[selectedSpread][index]}</p>
                    </div>
                `;
                
                cardEl.addEventListener('click', () => flipCard(cardEl));
                container.appendChild(cardEl);
            });
            
            document.getElementById('cardsSection').classList.add('visible');
        }

        function flipCard(cardEl) {
            if (!cardEl.classList.contains('flipped')) {
                cardEl.classList.add('flipped');
                revealedCards++;
                
                if (revealedCards === drawnCards.length) {
                    setTimeout(showInterpretation, 800);
                }
            }
        }

        function showInterpretation() {
            const theme = themeInterpretations[selectedTheme];
            document.getElementById('interpretationTheme').textContent = 
                `Exploring ${theme.context}`;
            
            let content = '';
            
            drawnCards.forEach((card, index) => {
                const position = positions[selectedSpread][index];
                content += `
                    <div class="card-interpretation">
                        <h4>${card.symbol} ${card.name} — ${position}</h4>
                        <p>${generateInterpretation(card, position, selectedTheme)}</p>
                    </div>
                `;
            });
            
            content += `
                <div class="overall-message">
                    <h4>✧ Overall Message</h4>
                    <p>${generateOverallMessage(drawnCards, selectedTheme)}</p>
                </div>
            `;
            
            document.getElementById('interpretationContent').innerHTML = content;
            document.getElementById('interpretationSection').classList.add('visible');
            document.getElementById('newReading').classList.add('visible');
        }

        function generateInterpretation(card, position, theme) {
            const interpretations = {
                love: {
                    past: `In matters of the heart, ${card.name} reveals the energy of ${card.meaning.toLowerCase()}. This influence has shaped your romantic journey and emotional patterns, laying the groundwork for where you stand today.`,
                    present: `${card.name} illuminates your current love life with the energy of ${card.meaning.toLowerCase()}. This card invites you to embrace these qualities in your relationships and emotional connections right now.`,
                    future: `Looking ahead in love, ${card.name} suggests ${card.meaning.toLowerCase()} will play a significant role. Open your heart to these possibilities and trust that the universe is guiding your romantic path.`,
                    message: `${card.name} brings a powerful message about love: ${card.meaning.toLowerCase()}. Reflect on how this energy resonates with your heart's desires and relationship dynamics.`
                },
                career: {
                    past: `In your professional journey, ${card.name} represents ${card.meaning.toLowerCase()}. These experiences have built the foundation for your current career standing and work ethic.`,
                    present: `${card.name} reflects your current career energy: ${card.meaning.toLowerCase()}. Consider how these qualities can be leveraged for professional growth and success right now.`,
                    future: `For your career path ahead, ${card.name} indicates ${card.meaning.toLowerCase()}. Prepare to embrace these opportunities and challenges as they unfold in your professional life.`,
                    message: `${card.name} offers career guidance through ${card.meaning.toLowerCase()}. Apply this wisdom to your professional decisions and aspirations.`
                },
                growth: {
                    past: `On your spiritual journey, ${card.name} shows that ${card.meaning.toLowerCase()} has been instrumental in shaping your inner landscape and personal evolution.`,
                    present: `${card.name} illuminates your current growth with ${card.meaning.toLowerCase()}. This is the energy to work with as you continue your path of self-discovery.`,
                    future: `For your personal development, ${card.name} foretells ${card.meaning.toLowerCase()}. Embrace this coming energy as a gift for your continued evolution.`,
                    message: `${card.name} speaks to your soul with the wisdom of ${card.meaning.toLowerCase()}. Let this guide your inner journey and personal transformation.`
                }
            };
            
            const positionKey = position.toLowerCase().replace('your ', '');
            return interpretations[theme][positionKey] || interpretations[theme].message;
        }

        function generateOverallMessage(cards, theme) {
            const cardNames = cards.map(c => c.name).join(', ');
            const messages = {
                love: `The combination of ${cardNames} creates a powerful tapestry for your love life. Trust your heart's wisdom, remain open to connection, and remember that love often arrives when we embrace our authentic selves. The universe supports your journey toward meaningful relationships.`,
                career: `With ${cardNames} guiding your professional path, you're being called to step into your power. Success comes through alignment with your true purpose. Trust your abilities, seize opportunities with confidence, and remember that every challenge is a stepping stone to greater achievement.`,
                growth: `The appearance of ${cardNames} marks a significant moment in your spiritual journey. You're being invited to dive deeper into self-awareness and embrace transformation. Trust the process, honor your intuition, and know that every step forward brings you closer to your highest self.`
            };
            
            return messages[theme];
        }

        // New reading
        document.getElementById('newReading').addEventListener('click', () => {
            document.querySelectorAll('.theme-card').forEach(c => c.classList.remove('selected'));
            document.querySelectorAll('.spread-btn').forEach(b => b.classList.remove('selected'));
            selectedTheme = null;
            selectedSpread = null;
            updateReadingButton();
            
            document.getElementById('cardsSection').classList.remove('visible');
            document.getElementById('interpretationSection').classList.remove('visible');
            document.getElementById('newReading').classList.remove('visible');
            
            window.scrollTo({ top: 0, behavior: 'smooth' });
        });

        // Initialize
        createStarfield();
    </script>
</body>
</html>
