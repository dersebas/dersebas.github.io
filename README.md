<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KE-Rechner</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap" rel="stylesheet"> <!-- Fetterer Font -->
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #121212; 
            margin: 0;
            padding: 0;
            color: #f4f4f4;
            display: flex;
            justify-content: center;
            align-items: center; 
            height: 100vh; 
        }

        /* Container */
        .container {
            max-width: 400px;
            width: 100%; 
            height: 80%;
            background: #1e1e1e; 
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            color: #e0e0e0;
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            position: relative;
            margin-top: 20px;
        }

        h2 {
            text-align: center;
            color: #778899;
            margin: 20px 0 30px;
            font-weight: 700;
            font-size: 36px;
        }

        /* Stil für Eingabefelder, Buttons und Auswahlfelder */
        input, select, button, #totalKEBox {
            width: 100%;
            height: 50px; 
            margin: 15px 0;
            padding: 10px;
            font-size: 22px;
            border: none;
            border-radius: 8px;
            background-color: #2a2a2a; 
            color: #e0e0e0;
            box-sizing: border-box;
            font-weight: 700;
        }

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
            box-shadow: 0 0 5px #778899;
        }

        /* Button-Styling */
        button {
            background-color: #778899;
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease;
            font-weight: 700; 
            font-size: 22px;

        button:hover {
            background-color: #4d6469;
        }

        #foodSuggestions {
            max-height: 200px; 
            background-color: #333;
            position: relative;
            width: 100%;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            z-index: 1000;
            display: none;
            color: #f4f4f4;
            font-weight: 700;
            overflow: hidden;
        }

        #foodSuggestions div {
            padding: 10px;
            cursor: pointer;
            transition: background-color 0.2s ease;
            font-size: 20px;
            color: #e0e0e0;
        }

        #foodSuggestions div:hover {
            background-color: #555;
        }

        ul {
            list-style-type: none;
            padding: 0;
            margin-top: 20px;
            width: 100%;
            background: #1e1e1e;
        }

        ul li {
            background: #2a2a2a;
            padding: 10px;
            margin: 5px 0;
            border-radius: 8px;
            color: #cccccc;
            display: flex;
            justify-content: space-between; 
            align-items: center;
            box-sizing: border-box;
            font-size: 20px;
            font-weight: 700; 
            height: 50px;
        }

        .delete-btn {
            background-color: transparent;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 20px;
            font-weight: 700;
            transition: background-color 0.3s ease;
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center; 
            justify-content: center;
        }

        .delete-btn:hover {
            background-color: #ff4d4d;
        }

        #totalKEBox {
            text-align: center;
            background-color: transparent;
            color: #778899; 
            height: 60px; 
            line-height: 60px;
            font-size: 24px; 
            font-weight: 700;
            width: 100%;
            margin-top: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        #portionLabel {
            text-align: center;
            font-size: 18px;
            color: #e0e0e0;
            margin-top: 10px;
            display: none;
            font-weight: 700;
        }

        @media (max-width: 500px) {
            body {
                padding: 10px;
            }

            .container {
                padding: 25px;
                margin-top: 20px; 
                height: 80%;
            }

            h2 {
                font-size: 32px;
            }

            input, button, #totalKEBox {
                height: 50px;
                font-size: 20px; 
            }

            ul li {
                font-size: 18px; 
            }

            #portionLabel {
                font-size: 16px;
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
        suggestions.innerHTML = "";

        if (input.length > 0) {
            const filteredFoods = Object.keys(carbData).filter(food => food.toLowerCase().includes(input.toLowerCase()));
            filteredFoods.forEach(food => {
                const div = document.createElement("div");
                div.textContent = food;
                div.onclick = () => {
                    document.getElementById("foodSearch").value = food; 
                    suggestions.style.display = "none"; 
                };
                suggestions.appendChild(div);
            });
            if (filteredFoods.length > 0) {
                suggestions.style.display = "block"; 
            } else {
                suggestions.style.display = "none";
            }
        } else {
            suggestions.style.display = "none"; 
        }
    }

    // KE berechnen
    function calculateCarbs() {
        const food = document.getElementById("foodSearch").value;
        const amount = parseFloat(document.getElementById("foodAmount").value);

        if (food in carbData && !isNaN(amount) && amount > 0) {
            const carbsPer100g = carbData[food].carbs; 
            const carbsPerGram = carbsPer100g / 100; 
            const totalCarbs = carbsPerGram * amount; 
            const totalKE = totalCarbs / 10; 

            const foodList = document.getElementById("foodList");
            const li = document.createElement("li");
            li.textContent = `${food}: ${amount}g (KE: ${totalKE.toFixed(2)})`; 


            const deleteButton = document.createElement("button");
            deleteButton.textContent = "✖"; 
            deleteButton.classList.add("delete-btn");
            deleteButton.onclick = () => deleteItem(foodList.children.length);
            li.appendChild(deleteButton);
            foodList.appendChild(li);

            updateTotalKE();

            document.getElementById("foodSearch").value = "";
            document.getElementById("foodAmount").value = "";
            document.getElementById("portionLabel").style.display = "none"; 
        } else {
            alert("Bitte ein gültiges Lebensmittel und eine Menge angeben.");
        }
    }

  
    function updateTotalKE() {
        const foodList = document.getElementById("foodList");
        const totalKEBox = document.getElementById("totalKEBox");
        let totalKE = 0;
        const items = foodList.getElementsByTagName("li");
        for (let i = 0; i < items.length; i++) {
            const keValue = parseFloat(items[i].textContent.split("KE: ")[1]); 
            totalKE += isNaN(keValue) ? 0 : keValue;
        }
        totalKEBox.textContent = `Gesamt KE: ${totalKE.toFixed(2)}`;
    }


    function deleteItem(index) {
        const foodList = document.getElementById("foodList");
        foodList.removeChild(foodList.children[index - 1]); 
        updateTotalKE();
    }
</script>

</body>
</html>
