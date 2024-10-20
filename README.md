<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KE-Rechner</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap" rel="stylesheet"> <!-- Fetterer Font -->
    <style>
        /* Globale Schriftart und dunkler Hintergrund */
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #121212; /* Dunkler Hintergrund */
            margin: 0;
            padding: 0; /* Kein zusätzliches Padding */
            color: #f4f4f4;
            display: flex;
            justify-content: center; /* Zentrieren des Containers horizontal */
            align-items: center; /* Container mittig vertikal ausrichten */
            height: 100vh; /* Vollständige Höhe des Viewports */
        }

        /* Container */
        .container {
            max-width: 400px;
            width: 100%; /* Volle Breite bis zur Maximalbreite */
            height: 80%; /* Containerhöhe reduziert */
            background: #1e1e1e; /* Dunkler Hintergrund für den Container */
            padding: 30px; /* Mehr Padding für den Container */
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            color: #e0e0e0;
            display: flex;
            flex-direction: column;
            justify-content: flex-start; /* Container-Inhalt oben ausrichten */
            position: relative; /* Für den Rand */
            margin-top: 20px; /* Container nach oben verschieben */
        }

        h2 {
            text-align: center;
            color: #778899; /* Angepasste Akzentfarbe */
            margin: 20px 0 30px; /* Mehr Platz über der Überschrift */
            font-weight: 700; /* Fetterer Text */
            font-size: 36px; /* Größere Schriftgröße */
        }

        /* Stil für Eingabefelder, Buttons und Auswahlfelder */
        input, select, button, #totalKEBox {
            width: 100%;
            height: 50px; /* Größere Höhe */
            margin: 15px 0; /* Mehr Abstand zwischen den Elementen */
            padding: 10px; /* Mehr Padding für Eingabefelder */
            font-size: 22px; /* Größere Schriftgröße */
            border: none;
            border-radius: 8px;
            background-color: #2a2a2a; /* Dunklerer Hintergrund für Eingabefelder */
            color: #e0e0e0;
            box-sizing: border-box;
            font-weight: 700; /* Fetterer Text */
        }

        /* Entfernen der Pfeile für das Zahleneingabefeld */
        input[type="number"]::-webkit-inner-spin-button,
        input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }

        input[type="number"] {
            -moz-appearance: textfield;
        }

        input:focus, button:focus {
            outline: none;
            box-shadow: 0 0 5px #778899; /* Angepasste Akzentfarbe */
        }

        /* Button-Styling */
        button {
            background-color: #778899; /* Angepasste Akzentfarbe */
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease;
            font-weight: 700; /* Fetterer Text */
            font-size: 22px; /* Größere Schriftgröße */
        }

        button:hover {
            background-color: #4d6469; /* Dunklerer Farbton für Hover-Effekt */
        }

        /* Styling für die Suchvorschläge */
        #foodSuggestions {
            max-height: 200px; /* Höhere Vorschlagsbox */
            background-color: #333; /* Dunkler Hintergrund für Vorschläge */
            position: relative;
            width: 100%;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            z-index: 1000;
            display: none;
            color: #f4f4f4;
            font-weight: 700; /* Fetterer Text */
            overflow: hidden; /* Scrollbalken ausblenden */
        }

        #foodSuggestions div {
            padding: 10px; /* Mehr Padding für Vorschläge */
            cursor: pointer;
            transition: background-color 0.2s ease;
            font-size: 20px; /* Größere Schriftgröße für Vorschläge */
            color: #e0e0e0;
        }

        #foodSuggestions div:hover {
            background-color: #555;
        }

        /* Ergebnisliste Styling */
        ul {
            list-style-type: none;
            padding: 0;
            margin-top: 20px;
            width: 100%;
            background: #1e1e1e; /* Dunkler Hintergrund für die Liste */
        }

        ul li {
            background: #2a2a2a; /* Dunklerer Hintergrund für Listeneinträge */
            padding: 10px; /* Mehr Padding für Listeneinträge */
            margin: 5px 0;
            border-radius: 8px;
            color: #cccccc;
            display: flex;
            justify-content: space-between; /* Flexbox für gleichmäßige Verteilung */
            align-items: center;
            box-sizing: border-box;
            font-size: 20px; /* Größere Schriftgröße für Listeneinträge */
            font-weight: 700; /* Fetterer Text */
            height: 50px; /* Gleiche Höhe für alle Listeneinträge */
        }

        /* Stil für den kleinen, unauffälligen Löschen-Button */
        .delete-btn {
            background-color: transparent;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 20px; /* Größere Schriftgröße für den Button */
            font-weight: 700; /* Fetterer Text */
            transition: background-color 0.3s ease;
            width: 30px; /* Größere Breite für den Button */
            height: 30px; /* Größere Höhe für den Button */
            display: flex;
            align-items: center; /* Zentrieren */
            justify-content: center; /* Zentrieren */
        }

        .delete-btn:hover {
            background-color: #ff4d4d; /* Rot für Löschen */
        }

        /* Gesamt-KE-Box Styling */
        #totalKEBox {
            text-align: center;
            background-color: transparent;
            color: #778899; /* Angepasste Akzentfarbe */
            height: 60px; /* Größere Höhe */
            line-height: 60px; /* Vertikale Zentrierung */
            font-size: 24px; /* Größere Schriftgröße */
            font-weight: 700; /* Fetterer Text */
            width: 100%;
            margin-top: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* Label für Portionsgröße */
        #portionLabel {
            text-align: center;
            font-size: 18px; /* Größere Schriftgröße */
            color: #e0e0e0;
            margin-top: 10px; /* Mehr Platz über dem Label */
            display: none;
            font-weight: 700; /* Fetterer Text */
        }

        /* Responsive Design */
        @media (max-width: 500px) {
            body {
                padding: 10px;
            }

            .container {
                padding: 25px; /* Mehr Padding für den Container */
                margin-top: 20px; /* Container nach oben verschieben */
                height: 80%; /* Containerhöhe reduziert */
            }

            h2 {
                font-size: 32px; /* Größere Schriftgröße für mobile Ansicht */
            }

            input, button, #totalKEBox {
                height: 50px; /* Gleiche Höhe für alle Eingabefelder und Buttons */
                font-size: 20px; /* Größere Schriftgröße für mobile Ansicht */
            }

            ul li {
                font-size: 18px; /* Größere Schriftgröße für Listeneinträge */
            }

            #portionLabel {
                font-size: 16px; /* Größere Schriftgröße für Portionsgröße */
            }
        }
    </style>
</head>
<body>

<div class="container">
    <h2>Kohlenhydrate Rechner</h2>
    <input type="text" id="foodSearch" placeholder="Lebensmittel suchen..." oninput="showSuggestions()">
    <div id="foodSuggestions"></div>
    <input type="number" id="foodAmount" placeholder="Menge (g)" min="1">
    <button onclick="calculateCarbs()">Berechnen</button>
    <ul id="foodList"></ul>
    <div id="totalKEBox">Gesamt KE: 0.00</div>
    <div id="portionLabel">Portionsgröße: 0 g</div>
</div>

<script>
    // Kohlenhydrataufstellung
    const carbData = {
        "Reis": { carbs: 77 },
        "Brot": { carbs: 49 },
        "Kartoffeln": { carbs: 17 },
        "Nudeln": { carbs: 25 },
        "Haferflocken": { carbs: 66 },
        "Zucker": { carbs: 100 },
        "Kekse": { carbs: 65 },
        "Schokolade": { carbs: 60 }
    };

    // Suchvorschläge anzeigen
    function showSuggestions() {
        const input = document.getElementById("foodSearch").value;
        const suggestions = document.getElementById("foodSuggestions");
        suggestions.innerHTML = ""; // Alte Vorschläge löschen

        if (input.length > 0) {
            const filteredFoods = Object.keys(carbData).filter(food => food.toLowerCase().includes(input.toLowerCase()));
            filteredFoods.forEach(food => {
                const div = document.createElement("div");
                div.textContent = food;
                div.onclick = () => {
                    document.getElementById("foodSearch").value = food; // Ausgewähltes Lebensmittel in das Suchfeld einfügen
                    suggestions.style.display = "none"; // Vorschläge verstecken
                };
                suggestions.appendChild(div);
            });
            if (filteredFoods.length > 0) {
                suggestions.style.display = "block"; // Vorschläge anzeigen
            } else {
                suggestions.style.display = "none"; // Vorschläge verstecken
            }
        } else {
            suggestions.style.display = "none"; // Vorschläge verstecken
        }
    }

    // KE berechnen
    function calculateCarbs() {
        const food = document.getElementById("foodSearch").value;
        const amount = parseFloat(document.getElementById("foodAmount").value);

        if (food in carbData && !isNaN(amount) && amount > 0) {
            const carbsPer100g = carbData[food].carbs; // Kohlenhydrate pro 100g
            const carbsPerGram = carbsPer100g / 100; // Kohlenhydrate pro Gramm
            const totalCarbs = carbsPerGram * amount; // Gesamte Kohlenhydrate für die eingegebene Menge
            const totalKE = totalCarbs / 10; // KE-Berechnung (1 KE = 10g Kohlenhydrate)

            // Listen-Element erstellen
            const foodList = document.getElementById("foodList");
            const li = document.createElement("li");
            li.textContent = `${food}: ${amount}g (KE: ${totalKE.toFixed(2)})`; // Eingabemenge und KE anzeigen

            // Löschen-Button hinzufügen
            const deleteButton = document.createElement("button");
            deleteButton.textContent = "✖"; // Löschen-Symbol
            deleteButton.classList.add("delete-btn");
            deleteButton.onclick = () => deleteItem(foodList.children.length); // Löschen-Funktion
            li.appendChild(deleteButton);
            foodList.appendChild(li);

            // Gesamt-KE berechnen
            updateTotalKE();
            // Eingabefelder zurücksetzen
            document.getElementById("foodSearch").value = "";
            document.getElementById("foodAmount").value = "";
            document.getElementById("portionLabel").style.display = "none"; // Portionsgröße verstecken
        } else {
            alert("Bitte ein gültiges Lebensmittel und eine Menge angeben.");
        }
    }

    // Gesamt-KE aktualisieren
    function updateTotalKE() {
        const foodList = document.getElementById("foodList");
        const totalKEBox = document.getElementById("totalKEBox");
        let totalKE = 0;
        const items = foodList.getElementsByTagName("li");
        for (let i = 0; i < items.length; i++) {
            const keValue = parseFloat(items[i].textContent.split("KE: ")[1]); // KE-Wert extrahieren
            totalKE += isNaN(keValue) ? 0 : keValue;
        }
        totalKEBox.textContent = `Gesamt KE: ${totalKE.toFixed(2)}`;
    }

    // Löschen-Funktion
    function deleteItem(index) {
        const foodList = document.getElementById("foodList");
        foodList.removeChild(foodList.children[index - 1]); // Den korrekten Index verwenden
        updateTotalKE();
    }
</script>

</body>
</html>
