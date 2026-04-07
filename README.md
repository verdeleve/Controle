<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<title>App Financeiro</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
body {
    font-family: Arial;
    background: #0f172a;
    color: white;
    padding: 20px;
}

h1 { text-align: center; }

.card {
    background: #1e293b;
    padding: 20px;
    border-radius: 12px;
    margin-bottom: 20px;
}

input, select, button {
    padding: 10px;
    margin: 5px;
    border-radius: 8px;
    border: none;
}

button {
    background: #22c55e;
    color: white;
    cursor: pointer;
}

button:hover { background: #16a34a; }

.item {
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid #334155;
    padding: 5px 0;
}

.delete {
    background: red;
    padding: 5px 10px;
    cursor: pointer;
}
</style>
</head>

<body>

<h1>💰 App Financeiro</h1>

<div class="card">
    <h2>Total: R$ <span id="total">0</span></h2>
</div>

<div class="card">
    <input id="desc" placeholder="Descrição">
    <input id="cat" placeholder="Categoria">
    
    <select id="cartao">
        <option>Nubank</option>
        <option>Ourocard</option>
    </select>

    <input id="val" type="number" placeholder="Valor">
    <button onclick="add()">Adicionar</button>
</div>

<div class="card">
    <canvas id="grafico"></canvas>
</div>

<div class="card">
    <h3>📋 Gastos</h3>
    <div id="lista"></div>
</div>

<script>

let dados = JSON.parse(localStorage.getItem("dados")) || [];

function add() {
    let desc = document.getElementById("desc").value;
    let cat = document.getElementById("cat").value;
    let cartao = document.getElementById("cartao").value;
    let val = parseFloat(document.getElementById("val").value);

    if (!desc || !val) return;

    dados.push({desc, cat, cartao, val});

    document.getElementById("desc").value = "";
    document.getElementById("cat").value = "";
    document.getElementById("val").value = "";

    atualizar();
}

function remover(index) {
    dados.splice(index, 1);
    atualizar();
}

function atualizar() {
    let total = dados.reduce((a,b)=>a+b.val,0);
    document.getElementById("total").innerText = total.toFixed(2);

    let lista = document.getElementById("lista");
    lista.innerHTML = dados.map((d,i)=>`
        <div class="item">
            <span>${d.desc} (${d.cartao}) - R$ ${d.val.toFixed(2)}</span>
            <button class="delete" onclick="remover(${i})">X</button>
        </div>
    `).join("");

    let categorias = {};
    dados.forEach(d=>{
        categorias[d.cat] = (categorias[d.cat]||0) + d.val;
    });

    let labels = Object.keys(categorias);
    let valores = Object.values(categorias);

    if(window.chart) window.chart.destroy();

    let ctx = document.getElementById("grafico");
    window.chart = new Chart(ctx, {
        type: 'pie',
        data: {
            labels: labels,
            datasets: [{ data: valores }]
        }
    });

    localStorage.setItem("dados", JSON.stringify(dados));
}

atualizar();

</script>

</body>
</html>
