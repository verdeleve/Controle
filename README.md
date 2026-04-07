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
    border-radius: 16px;
    margin-bottom: 20px;
}

input, select, button {
    padding: 10px;
    margin: 5px;
    border-radius: 10px;
    border: none;
}

button {
    background: #22c55e;
    color: white;
    cursor: pointer;
}

.delete {
    background: red;
}

.item {
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid #334155;
    padding: 8px 0;
}
</style>
</head>

<body>

<h1>💰 App Financeiro</h1>

<div class="card">
    <h2>Total mês: R$ <span id="total">0</span></h2>
</div>

<div class="card">
    <input id="desc" placeholder="Descrição">
    
    <select id="cartao">
        <option>Nubank</option>
        <option>Ourocard</option>
    </select>

    <input id="val" type="number" placeholder="Valor">
    <button onclick="add()">Adicionar</button>
</div>

<div class="card">
    <canvas id="graficoCategoria"></canvas>
</div>

<div class="card">
    <canvas id="graficoMes"></canvas>
</div>

<div class="card">
    <h3>📋 Gastos</h3>
    <div id="lista"></div>
</div>

<script>

let dados = JSON.parse(localStorage.getItem("dados")) || [];

function detectarCategoria(desc) {
    desc = desc.toLowerCase();

    if (desc.includes("uber")) return "Transporte";
    if (desc.includes("mercado") || desc.includes("padaria")) return "Alimentação";
    if (desc.includes("spotify")) return "Assinatura";
    if (desc.includes("posto")) return "Transporte";

    return "Outros";
}

function add() {
    let desc = document.getElementById("desc").value;
    let cartao = document.getElementById("cartao").value;
    let val = parseFloat(document.getElementById("val").value);

    if (!desc || !val) return;

    let data = new Date();
    let mes = data.getMonth() + 1;

    let categoria = detectarCategoria(desc);

    dados.push({desc, cartao, val, mes, categoria});

    document.getElementById("desc").value = "";
    document.getElementById("val").value = "";

    atualizar();
}

function remover(i) {
    dados.splice(i,1);
    atualizar();
}

function atualizar() {

    let mesAtual = new Date().getMonth() + 1;

    let dadosMes = dados.filter(d => d.mes === mesAtual);

    let total = dadosMes.reduce((a,b)=>a+b.val,0);
    document.getElementById("total").innerText = total.toFixed(2);

    let lista = document.getElementById("lista");
    lista.innerHTML = dadosMes.map((d,i)=>`
        <div class="item">
            <span>${d.desc} (${d.cartao}) - R$ ${d.val.toFixed(2)}</span>
            <button class="delete" onclick="remover(${i})">X</button>
        </div>
    `).join("");

    // gráfico categoria
    let cat = {};
    dadosMes.forEach(d=>{
        cat[d.categoria] = (cat[d.categoria]||0) + d.val;
    });

    if(window.g1) window.g1.destroy();
    window.g1 = new Chart(document.getElementById("graficoCategoria"), {
        type: 'pie',
        data: {
            labels: Object.keys(cat),
            datasets: [{ data: Object.values(cat) }]
        }
    });

    // gráfico mês (histórico)
    let meses = {};
    dados.forEach(d=>{
        meses[d.mes] = (meses[d.mes]||0) + d.val;
    });

    if(window.g2) window.g2.destroy();
    window.g2 = new Chart(document.getElementById("graficoMes"), {
        type: 'bar',
        data: {
            labels: Object.keys(meses),
            datasets: [{ data: Object.values(meses) }]
        }
    });

    localStorage.setItem("dados", JSON.stringify(dados));
}

atualizar();

</script>

</body>
</html>
