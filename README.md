<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Egg Clicker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f1f1f1;
            color: #333;
        }
        h1 {
            font-size: 2.5em;
            margin-top: 20px;
        }
        #egg {
            width: 150px;
            height: 150px;
            cursor: pointer;
            margin: 20px;
        }
        #stats {
            font-size: 1.2em;
            margin-top: 20px;
        }
        .store-item {
            margin: 10px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 8px;
            background-color: #fafafa;
            display: inline-block;
            width: 200px;
        }
        button {
            padding: 8px 12px;
            cursor: pointer;
            background-color: #008cba;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 1em;
        }
        button:disabled {
            background-color: #888;
            cursor: not-allowed;
        }
    </style>
</head>
<body>
    <h1>Egg Clicker</h1>
    <img id="egg" src="https://upload.wikimedia.org/wikipedia/commons/4/43/White_egg.jpg" alt="Ei" onclick="clickEgg()">
    <div id="stats">
        <p>Eier: <span id="eggCount">0</span></p>
        <p>Eier pro Klick (EPC): <span id="epc">1</span></p>
        <p>Eier pro Sekunde (EPS): <span id="eps">0</span></p>
    </div>

    <h2>Hilfsmittel kaufen</h2>
    <div id="store"></div>

    <script>
        let eggCount = 0;
        let epc = 1; // Eier pro Klick
        let eps = 0; // Eier pro Sekunde

        // Hilfsmittel-Einstellungen (Name, Grundkosten, EPS-Wert)
        const items = [
            { name: "Huhn", baseCost: 10, eps: 1 },
            { name: "Hühnerstall", baseCost: 50, eps: 5 },
            { name: "Geflügelfarm", baseCost: 200, eps: 20 },
            { name: "Kleinbauer", baseCost: 500, eps: 50 },
            { name: "Großbauer", baseCost: 1000, eps: 100 },
            { name: "Eiermanufaktur", baseCost: 5000, eps: 200 },
            { name: "Bauernhof", baseCost: 10000, eps: 500 },
            { name: "Eierproduzent", baseCost: 50000, eps: 1000 },
            { name: "Geflügelproduzent", baseCost: 100000, eps: 2000 },
            { name: "Hühnerimperium", baseCost: 500000, eps: 5000 },
            { name: "Fabrik", baseCost: 1000000, eps: 10000 },
            { name: "Verpackungszentrum", baseCost: 5000000, eps: 20000 },
            { name: "Transportzentrum", baseCost: 10000000, eps: 50000 },
            { name: "Marketing-Team", baseCost: 50000000, eps: 100000 },
            { name: "Export-Hub", baseCost: 100000000, eps: 200000 },
            { name: "Egg-Investor", baseCost: 500000000, eps: 500000 },
            { name: "Egg-Riese", baseCost: 1000000000, eps: 1000000 },
            { name: "Egg-Gigant", baseCost: 5000000000, eps: 2000000 },
            { name: "Egg-Monopolist", baseCost: 10000000000, eps: 5000000 },
            { name: "Egg-Legende", baseCost: 50000000000, eps: 10000000 }
        ];

        // Hilfsmittel-Daten (dynamisch gefüllt)
        const store = items.map(item => ({
            name: item.name,
            cost: item.baseCost,
            eps: item.eps,
            quantity: 0
        }));

        // Rendern der Hilfsmittel im Store
        function renderStore() {
            const storeDiv = document.getElementById("store");
            storeDiv.innerHTML = ""; // Store zurücksetzen

            store.forEach((item, index) => {
                const itemDiv = document.createElement("div");
                itemDiv.className = "store-item";

                itemDiv.innerHTML = `
                    <p>${item.name} (+${item.eps} EPS) - <strong>Kosten: ${item.cost} Eier</strong></p>
                    <button onclick="buyItem(${index})" id="itemButton${index}">
                        Kaufen (${item.quantity})
                    </button>
                    <button onclick="upgradeItem(${index})" id="upgradeButton${index}">
                        Aufrüsten für ${Math.floor(item.cost * 0.8)} Eier (Effekt +10%)
                    </button>
                `;

                storeDiv.appendChild(itemDiv);
            });
        }

        // Funktion, wenn das Ei geklickt wird
        function clickEgg() {
            eggCount += epc;
            updateStats();
        }

        // Hilfsmittel kaufen
        function buyItem(index) {
            const item = store[index];
            if (eggCount >= item.cost) {
                eggCount -= item.cost;
                item.quantity++;
                eps += item.eps;
                item.cost = Math.floor(item.cost * 1.15); // Preis steigt um 15%
                updateStats();
                renderStore();
            }
        }

        // Hilfsmittel aufrüsten (Erhöhung des EPS-Effekts um 10%)
        function upgradeItem(index) {
            const item = store[index];
            const upgradeCost = Math.floor(item.cost * 0.8);
            if (eggCount >= upgradeCost) {
                eggCount -= upgradeCost;
                item.eps = Math.floor(item.eps * 1.1); // EPS um 10% erhöhen
                eps += Math.floor(item.eps * 0.1); // EPS-Anstieg auf den Gesamt-EPS
                updateStats();
                renderStore();
            }
        }

        // Spiel-Status aktualisieren
        function updateStats() {
            document.getElementById("eggCount").textContent = eggCount;
            document.getElementById("epc").textContent = epc;
            document.getElementById("eps").textContent = eps;
        }

        // Eier pro Sekunde (EPS) erhöhen
        setInterval(() => {
            eggCount += eps;
            updateStats();
        }, 1000); // Update alle 1000ms (1 Sekunde)

        // Initiales Rendern des Stores
        renderStore();
    </script>
</body>
</html>
