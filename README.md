<!DOCTYPE html>
<html>
<head>
    <title>Bad Money Game</title>
    <style>
        body {
            background: linear-gradient(135deg, #1a1f2c, #2d3436);
            color: #fff;
            font-family: 'Segoe UI', Arial, sans-serif;
            margin: 0;
            padding: 20px;
        }

        .header {
            text-align: center;
            margin-bottom: 20px;
            padding: 20px;
            border: 3px solid #00b894;
            border-radius: 15px;
            background: rgba(26, 31, 44, 0.9);
            box-shadow: 0 0 20px rgba(0, 184, 148, 0.3);
        }

        .header h1 {
            color: #00b894;
            text-shadow: 0 0 10px rgba(0, 184, 148, 0.5);
            margin: 0;
            font-size: 2.5em;
            font-weight: 300;
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
            padding: 20px;
            background: rgba(26, 31, 44, 0.8);
            border: 2px solid #00b894;
            border-radius: 15px;
        }

        .stat-item {
            text-align: center;
            padding: 15px;
            background: rgba(45, 52, 54, 0.8);
            border: 1px solid #00b894;
            border-radius: 10px;
            color: #dfe6e9;
        }

        .stat-label {
            color: #00b894;
            font-size: 0.9em;
            margin-bottom: 5px;
        }

        .stat-value {
            font-size: 1.2em;
            font-weight: bold;
        }

        .bank-change {
            font-size: 0.9em;
            margin-top: 5px;
        }

        .positive {
            color: #00b894;
        }

        .negative {
            color: #ff7675;
        }

        .health-bar {
            width: 100%;
            height: 20px;
            background: rgba(45, 52, 54, 0.8);
            border: 1px solid #00b894;
            border-radius: 10px;
            overflow: hidden;
            margin-top: 5px;
        }

        .health-bar-fill {
            height: 100%;
            background: linear-gradient(90deg, #00b894, #55efc4);
            width: 100%;
            transition: width 0.3s;
        }

        .items-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .item-card {
            background: rgba(45, 52, 54, 0.8);
            border: 2px solid #00b894;
            padding: 20px;
            border-radius: 15px;
            position: relative;
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .item-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(0, 184, 148, 0.2);
        }

        .item-card.limited {
            border-color: #ff7675;
            box-shadow: 0 0 15px rgba(255, 118, 117, 0.2);
        }

        .limited-tag {
            position: absolute;
            top: 15px;
            right: 15px;
            background: #ff7675;
            color: white;
            padding: 5px 15px;
            border-radius: 20px;
            font-weight: 500;
            font-size: 0.9em;
        }

        .item-name {
            color: #00b894;
            font-size: 1.3em;
            margin-bottom: 10px;
            font-weight: 500;
        }

        .item-description {
            color: #dfe6e9;
            margin-bottom: 15px;
            font-size: 0.95em;
            line-height: 1.4;
        }

        .item-price {
            color: #ffeaa7;
            font-weight: 500;
            margin-bottom: 15px;
            font-size: 1.1em;
        }

        button {
            background: #00b894;
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            cursor: pointer;
            font-weight: 500;
            width: 100%;
            transition: background 0.2s, transform 0.1s;
        }

        button:hover:not(:disabled) {
            background: #55efc4;
            transform: scale(1.02);
        }

        button:disabled {
            background: #636e72;
            cursor: not-allowed;
        }

        .category-filter {
            margin-bottom: 25px;
            text-align: center;
        }

        .category-filter button {
            margin: 5px;
            width: auto;
            min-width: 120px;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(26, 31, 44, 0.95);
            transition: opacity 0.3s ease-out;
        }

        .modal-content {
            background: rgba(45, 52, 54, 0.95);
            margin: 15% auto;
            padding: 30px;
            border: 2px solid #00b894;
            width: 90%;
            max-width: 500px;
            text-align: center;
            border-radius: 15px;
            box-shadow: 0 0 30px rgba(0, 184, 148, 0.2);
        }

        .modal-content h2 {
            color: #00b894;
            margin-bottom: 20px;
            font-weight: 500;
        }

        .modal-content p {
            color: #dfe6e9;
            margin-bottom: 25px;
            line-height: 1.5;
        }

        .modal-content button {
            width: auto;
            margin: 10px;
            min-width: 120px;
        }

        #game-over {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(26, 31, 44, 0.98);
        }

        .game-over-content {
            background: rgba(45, 52, 54, 0.95);
            padding: 40px;
            border: 3px solid #ff7675;
            width: 90%;
            max-width: 600px;
            margin: 15% auto;
            text-align: center;
            border-radius: 15px;
            box-shadow: 0 0 40px rgba(255, 118, 117, 0.3);
        }

        .game-over-content h1 {
            color: #ff7675;
            font-size: 2.8em;
            margin-bottom: 20px;
            font-weight: 500;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Bad Money Game</h1>
    </div>

    <div class="stats">
        <div class="stat-item">
            <div class="stat-label">Cash</div>
            <div class="stat-value" id="money">$100,000,000</div>
        </div>
        <div class="stat-item">
            <div class="stat-label">Bank Account</div>
            <div class="stat-value" id="bank">$1,000,000</div>
            <div class="bank-change" id="bank-change">+$0/min</div>
        </div>
        <div class="stat-item">
            <div class="stat-label">Health</div>
            <div class="stat-value" id="health">100%</div>
            <div class="health-bar">
                <div class="health-bar-fill" id="health-bar"></div>
            </div>
        </div>
        <div class="stat-item">
            <div class="stat-label">Wanted Level</div>
            <div class="stat-value" id="wanted-level">⭐</div>
        </div>
        <div class="stat-item">
            <div class="stat-label">Market Timer</div>
            <div class="stat-value" id="timer">5:00</div>
        </div>
    </div>

    <div class="category-filter" id="category-filter"></div>
    <div class="items-grid" id="items-container"></div>

    <div id="fixthens-modal" class="modal">
        <div class="modal-content">
            <h2>Add The Fixthens?</h2>
            <p>This will modify your purchase in unexpected ways...</p>
            <button onclick="purchase(false)">No Thanks</button>
            <button onclick="purchase(true)">Add Fixthens!</button>
        </div>
    </div>

    <div id="game-over" class="modal">
        <div class="game-over-content">
            <h1>GAME OVER!</h1>
            <p>You spent all your money!</p>
            <button onclick="restart()">Play Again</button>
        </div>
    </div>

    <script>
        const items = [
            {
                id: 1,
                name: "Cocaine",
                category: "DRUGS",
                price: 50000,
                description: "Pure Colombian. Probably.",
                health: -20,
                limited: true
            },
            {
                id: 2,
                name: "Whole Liquor Store",
                category: "PARTY",
                price: 500000,
                description: "For 'personal use' only",
                health: -10
            },
            {
                id: 3,
                name: "Pet Tiger",
                category: "LUXURY",
                price: 250000,
                description: "Poorly trained but very stylish",
                health: -15
            },
            {
                id: 4,
                name: "Mystery Pills",
                category: "DRUGS",
                price: 75000,
                description: "The label is in hieroglyphics",
                health: -25
            },
            {
                id: 5,
                name: "Time Machine Casino",
                category: "CRIME",
                price: 5000000,
                description: "Betting on yesterday's races",
                health: 0,
                limited: true
            },
            {
                id: 6,
                name: "Experimental Serum",
                category: "DRUGS",
                price: 150000,
                description: "Side effects include time travel (unconfirmed)",
                health: -30,
                limited: true
            },
            {
                id: 7,
                name: "Diamond-Encrusted Everything",
                category: "LUXURY",
                price: 8000000,
                description: "Even your diamonds have diamonds",
                health: 0
            },
            {
                id: 8,
                name: "Infinity Pool of Coffee",
                category: "LUXURY",
                price: 1000000,
                description: "Never sleep again",
                health: -15
            },
            {
                id: 9,
                name: "Underground Fight Club",
                category: "CRIME",
                price: 2000000,
                description: "First rule: Don't talk about the massive property damage",
                health: -25
            },
            {
                id: 10,
                name: "Rocket-Powered Skateboard",
                category: "LUXURY",
                price: 300000,
                description: "No brakes included",
                health: -20,
                limited: true
            },
            {
                id: 11,
                name: "Party Island",
                category: "PARTY",
                price: 15000000,
                description: "It sinks every weekend but resurfaces by Monday",
                health: -10
            },
            {
                id: 12,
                name: "Memory Erasing Spray",
                category: "DRUGS",
                price: 250000,
                description: "Wait, what were we talking about?",
                health: -15,
                limited: true
            },
            {
                id: 13,
                name: "Counterfeit Factory",
                category: "CRIME",
                price: 7500000,
                description: "They only print million dollar bills",
                health: 0
            },
            {
                id: 14,
                name: "Laser Light Nightclub",
                category: "PARTY",
                price: 3000000,
                description: "Warning: May accidentally slice building in half",
                health: -5
            },
            {
                id: 15,
                name: "Quantum Lottery Tickets",
                category: "CRIME",
                price: 1000000,
                description: "Win in multiple dimensions",
                health: 0,
                limited: true
            },
            {
                id: 16,
                name: "Golden Toilet Collection",
                category: "LUXURY",
                price: 5000000,
                description: "For when regular toilets are too peasant",
                health: 5
            },
            {
                id: 17,
                name: "Unicorn Energy Drinks",
                category: "DRUGS",
                price: 100000,
                description: "Made from real unicorns (allegedly)",
                health: -10
            },
            {
                id: 18,
                name: "Cloud Castle",
                category: "LUXURY",
                price: 25000000,
                description: "Real estate that literally doesn't exist",
                health: 0,
                limited: true
            },
            {
                id: 19,
                name: "Time-Traveling Disco",
                category: "PARTY",
                price: 4000000,
                description: "Party like it's 1977 (literally)",
                health: -5
            },
            {
                id: 20,
                name: "Invisible Yacht",
                category: "LUXURY",
                price: 12000000,
                description: "Good luck finding it again",
                health: 0
            },
            {
                id: 21,
                name: "Perpetual Party Machine",
                category: "PARTY",
                price: 2500000,
                description: "It never stops. Ever.",
                health: -20
            },
            {
                id: 22,
                name: "Dream Simulator 3000",
                category: "DRUGS",
                price: 450000,
                description: "Like reality, but weirder",
                health: -15
            },
            {
                id: 23,
                name: "Volcano Lair",
                category: "CRIME",
                price: 20000000,
                description: "Comes with evil laugh lessons",
                health: -10,
                limited: true
            },
            {
                id: 24,
                name: "Solid Gold Jet Ski",
                category: "LUXURY",
                price: 3500000,
                description: "Sinks immediately",
                health: -5
            },
            {
                id: 25,
                name: "Interdimensional Nightclub",
                category: "PARTY",
                price: 8000000,
                description: "Dance floor extends into the void",
                health: -15,
                limited: true
            },
            {
                id: 26,
                name: "Memory Palace",
                category: "LUXURY",
                price: 15000000,
                description: "You forgot where you put the keys",
                health: 0
            },
            {
                id: 27,
                name: "Gravity Defying Shoes",
                category: "LUXURY",
                price: 750000,
                description: "Up is down, down is expensive",
                health: -10
            },
            {
                id: 28,
                name: "Underground Racing Network",
                category: "CRIME",
                price: 10000000,
                description: "All cars are stolen from the future",
                health: -20
            },
            {
                id: 29,
                name: "Mood-Altering Wallpaper",
                category: "DRUGS",
                price: 200000,
                description: "Don't lick the walls",
                health: -15
            },
            {
                id: 30,
                name: "Portable Black Hole",
                category: "CRIME",
                price: 30000000,
                description: "Perfect for quick getaways and storage",
                health: -30,
                limited: true
            }
        ];

        let state = {
            money: 100000000,
            bank: 1000000,
            bankRate: 0,
            health: 100,
            wanted: 1,
            category: 'ALL'
        };

        let selected = null;
        let timer = 300;
        let bankTimer = null;

        function updateBankAccount() {
            // Base interest rate (0.1% per minute)
            let baseRate = state.bank * 0.001;
            
            // Modify rate based on wanted level (each star reduces rate by 20%)
            let wantedPenalty = 1 - (state.wanted * 0.2);
            
            // Modify rate based on health (below 50% health reduces rate)
            let healthMultiplier = state.health < 50 ? state.health / 100 : 1;
            
            // Calculate final rate
            state.bankRate = Math.floor(baseRate * wantedPenalty * healthMultiplier);
            
            // Update bank balance
            state.bank += state.bankRate;
            
            // Update display
            document.getElementById('bank').textContent = '$' + state.bank.toLocaleString();
            document.getElementById('bank-change').textContent = 
                (state.bankRate >= 0 ? '+' : '') + '$' + state.bankRate.toLocaleString() + '/min';
            document.getElementById('bank-change').className = 
                'bank-change ' + (state.bankRate >= 0 ? 'positive' : 'negative');
        }

        function updateTimer() {
            const minutes = Math.floor(timer / 60);
            const seconds = timer % 60;
            document.getElementById('timer').textContent = 
                `${minutes}:${seconds.toString().padStart(2, '0')}`;
            
            if (timer > 0) {
                timer--;
                setTimeout(updateTimer, 1000);
            } else {
                shufflePrices();
                timer = 300;
                updateTimer();
            }
        }

        function shufflePrices() {
            items.forEach(item => {
                item.limited = Math.random() < 0.3;
                item.price = Math.floor(item.price * (0.8 + Math.random() * 0.4));
            });
            showItems();
        }

        function updateStats() {
            document.getElementById('money').textContent = '$' + state.money.toLocaleString();
            document.getElementById('health').textContent = state.health + '%';
            document.getElementById('health-bar').style.width = state.health + '%';
            document.getElementById('wanted-level').textContent = '⭐'.repeat(state.wanted);
        }

        function showCategories() {
            const categories = ['ALL', 'DRUGS', 'PARTY', 'CRIME', 'LUXURY'];
            const container = document.getElementById('category-filter');
            container.innerHTML = categories.map(cat => 
                `<button onclick="filterCategory('${cat}')">${cat}</button>`
            ).join('');
        }

        function filterCategory(category) {
            state.category = category;
            showItems();
        }

        function showItems() {
            const container = document.getElementById('items-container');
            container.innerHTML = items
                .filter(item => state.category === 'ALL' || item.category === state.category)
                .map(item => `
                    <div class="item-card ${item.limited ? 'limited' : ''}">
                        ${item.limited ? '<div class="limited-tag">LIMITED TIME!</div>' : ''}
                        <div class="item-name">${item.name}</div>
                        <div class="item-description">${item.description}</div>
                        <div class="item-price">$${item.price.toLocaleString()}</div>
                        <button onclick="buy(${item.id})" 
                                ${state.money < item.price ? 'disabled' : ''}>
                            Buy
                        </button>
                    </div>
                `).join('');
        }

        function buy(id) {
            selected = items.find(item => item.id === id);
            if (selected && state.money >= selected.price) {
                document.getElementById('fixthens-modal').style.display = 'block';
            }
        }

        function purchase(withFixthens) {
            if (!selected) return;

            let price = selected.price;
            let health = selected.health;
            let message = '';

            if (withFixthens) {
                const effect = Math.random();
                if (effect < 0.2) {
                    price *= 1.5;
                    health *= 0.8;
                    state.bank *= 0.95; // Lose 5% of bank balance
                    message = "Mysterious bonus package... bank account lighter!";
                } else if (effect < 0.4) {
                    price *= 2;
                    health *= 1.2;
                    state.bankRate *= 1.1; // Increase interest rate by 10%
                    message = "Premium quality... bank interest increased!";
                } else if (effect < 0.6) {
                    price *= 0.5;
                    health *= 0.5;
                    state.wanted += 2; // Extra wanted level increase
                    message = "Police very interested in this one!";
                } else if (effect < 0.8) {
                    price *= 1.2;
                    health *= 1.5;
                    state.money += Math.floor(price * 0.3); // Cashback!
                    message = "Counterfeit cashback received!";
                } else {
                    price *= 0.8;
                    if (Math.random() < 0.5) {
                        health = Math.min(100, state.health + 50); // Big health boost
                        message = "Health boost jackpot!";
                    } else {
                        health = Math.max(0, state.health - 50); // Big health drop
                        message = "Definitely not FDA approved!";
                    }
                }

                // Create and show a temporary floating message
                const messageEl = document.createElement('div');
                messageEl.style.position = 'fixed';
                messageEl.style.top = '20%';
                messageEl.style.left = '50%';
                messageEl.style.transform = 'translate(-50%, -50%)';
                messageEl.style.padding = '15px 30px';
                messageEl.style.background = 'rgba(0, 184, 148, 0.9)';
                messageEl.style.color = 'white';
                messageEl.style.borderRadius = '25px';
                messageEl.style.fontWeight = '500';
                messageEl.style.zIndex = '1000';
                messageEl.style.transition = 'opacity 0.5s';
                messageEl.textContent = message;
                document.body.appendChild(messageEl);

                // Fade out and remove the message after 1.5 seconds
                setTimeout(() => {
                    messageEl.style.opacity = '0';
                    setTimeout(() => messageEl.remove(), 500);
                }, 1500);
            }

            state.money -= price;
            state.health = Math.max(0, Math.min(100, state.health + health));
            state.wanted += withFixthens ? 1 : 0;

            updateStats();
            updateBankAccount();
            
            // Smoothly hide the modal
            const modal = document.getElementById('fixthens-modal');
            modal.style.opacity = '0';
            setTimeout(() => {
                modal.style.display = 'none';
                modal.style.opacity = '1';
            }, 300);
            
            selected = null;
            showItems();

            if (state.money <= 0 || state.health <= 0) {
                document.getElementById('game-over').style.display = 'block';
            }
        }

        function restart() {
            state = {
                money: 100000000,
                bank: 1000000,
                bankRate: 0,
                health: 100,
                wanted: 1,
                category: 'ALL'
            };
            updateStats();
            showItems();
            document.getElementById('game-over').style.display = 'none';
        }

        // Initialize game
        showCategories();
        showItems();
        updateStats();
        updateTimer();
        
        // Start bank account updates
        bankTimer = setInterval(updateBankAccount, 60000); // Update every minute
        updateBankAccount(); // Initial update

        // Close modals when clicking outside
        window.onclick = function(event) {
            if (event.target.className === 'modal') {
                event.target.style.display = 'none';
                selected = null;
            }
        };
    </script>
</body>
</html> 
