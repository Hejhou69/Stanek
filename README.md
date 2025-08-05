# Stanek
<!DOCTYPE html>
<html lang="cs">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Stánek - Prodej</title>
  <style>
    body { font-family: sans-serif; padding: 20px; max-width: 500px; margin: auto; }
    h1 { text-align: center; }
    .item { display: flex; justify-content: space-between; margin: 10px 0; }
    .buttons { display: flex; gap: 5px; }
    .total, .change { font-size: 1.2em; margin: 15px 0; }
    input[type='number'] { width: 100%; padding: 5px; font-size: 1em; }
    button { padding: 5px 10px; font-size: 1em; }
  </style>
</head>
<body>
  <h1>Stánek - Prodej</h1>

  <div id="items"></div>

  <div class="total">Celkem: <span id="total">0</span> Kč</div>

  <label>Zákazník platí:
    <input type="number" id="paid" min="0" oninput="updateChange()">
  </label>

  <div class="change">Vrátit: <span id="change">0</span> Kč</div>

  <button onclick="resetAll()">Vymazat vše</button>

  <script>
    const products = [
      { name: "Pivo", price: 35 },
      { name: "Kofola", price: 25 },
      { name: "Nealko pivo", price: 30 },
      { name: "Křupky", price: 20 },
      { name: "Fruko", price: 15 }
    ];

    const itemsDiv = document.getElementById('items');

    products.forEach((product, index) => {
      const div = document.createElement('div');
      div.className = 'item';
      div.innerHTML = `
        <span>${product.name} (${product.price} Kč)</span>
        <div class="buttons">
          <button onclick="decrease(${index})">-</button>
          <span id="count-${index}">0</span>
          <button onclick="increase(${index})">+</button>
        </div>
      `;
      itemsDiv.appendChild(div);
    });

    let counts = Array(products.length).fill(0);

    function increase(index) {
      counts[index]++;
      updateDisplay(index);
    }

    function decrease(index) {
      if (counts[index] > 0) counts[index]--;
      updateDisplay(index);
    }

    function updateDisplay(index) {
      document.getElementById(`count-${index}`).innerText = counts[index];
      updateTotal();
    }

    function updateTotal() {
      const total = counts.reduce((sum, count, i) => sum + count * products[i].price, 0);
      document.getElementById('total').innerText = total;
      updateChange();
    }

    function updateChange() {
      const paid = Number(document.getElementById('paid').value);
      const total = Number(document.getElementById('total').innerText);
      const change = paid - total;
      document.getElementById('change').innerText = change >= 0 ? change : 0;
    }

    function resetAll() {
      counts = counts.map(() => 0);
      products.forEach((_, i) => updateDisplay(i));
      document.getElementById('paid').value = '';
      updateChange();
    }
  </script>
</body>
</html>
