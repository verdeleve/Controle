<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<title>App Financeiro PRO</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>

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

.grid {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
}

.badge {
    padding: 8px 12px;
    border-radius: 10px;
    background: #334155;
}

.item {
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid #334155;
    padding: 8px 0;
}

.delete { background: red; }
.edit { background: orange; }
</style>
</head>

<body>

<h1>💰 App Financeiro PRO</h1>

<div class="card">
    <h2>Total mês: R$ <span id="total">0</span></h2>
</div>

<div class="card">
    <h3>💳 Por forma de pagamento</h3>
    <div id="cards" class="grid"></div>
</div>

<div class="card">
    <input id="desc" placeholder="Descrição">

    <select id="categoria">
        <option value="">Categoria</option>
        <option>Alimentação</option>
        <option>Combustível</option>
        <option>Transporte</option>
        <option>Compras</option>
        <option>Assinatura</option>
        <option>Saúde</option>
        <option>Casa</option>
        <option>Outros</option>
    </select>

    <!-- 💳 NOVAS OPÇÕES -->
    <select id="cartao">
        <option>Nubank</option>
        <option>Ourocard</option>
        <option>Débito</option>
        <option>VR</option>
        <option>Pix/option>
    </select>

    <input id="val" type="number" placeholder="Valor">

    <button onclick="add()" id="btnAdd">Adicionar</button>
</div>

<div class="card">
    <h3>📂 Importar fatura (PDF)</h3>
    <input type="file" id="pdfInput">
</div>

<div class="card">
    <canvas id="grafico"></canvas>
</div>

<div class="card">
    <h3>📋 Gastos do mês</h3>
    <div id="lista"></div>
</div>

<script>

let dados = JSON.parse(localStorage.getItem("dados")) || [];
let editIndex = null;

// 🧠 Categoria automática
function detectarCategoria(desc) {
    desc = desc.toLowerCase();

    if (desc.includes("mercado") || desc.includes("padaria") || desc.includes("restaurante") || desc.includes("ifood"))
        return "Alimentação";

    if (desc.includes("posto") || desc.includes("shell"))
        return "Combustível";

    if (desc.includes("uber"))
        return "Transporte";

    return "Outros";
}

// 💳 Detectar cartão automático (PDF)
function detectarCartao(linha) {
    let final = linha.match(/(\d{4})/);

    if (linha.includes("••••") && final)
        return "Nubank " + final[1];

    return "Ourocard";
}

// ➕ Adicionar / editar
function add() {

    let desc = descEl.value;
    let categoria = catEl.value || detectarCategoria(desc);
    let cartao = cartaoEl.value;
    let val = parseFloat(valEl.value);

    if (!desc || !val) return;

    let hoje = new Date();

    if (editIndex !== null) {
        dados[editIndex] = {
            desc, categoria, cartao, val,
            dia: hoje.getDate(),
            mes: hoje.getMonth()+1
        };
        editIndex = null;
        btnAdd.innerText = "Adicionar";
    } else {
        dados.push({
            desc, categoria, cartao, val,
            dia: hoje.getDate(),
            mes: hoje.getMonth()+1
        });
    }

    limpar();
    atualizar();
}

// ✏️ Editar
function editar(i) {
    let d = dados[i];

    descEl.value = d.desc;
    catEl.value = d.categoria;
    cartaoEl.value = d.cartao;
    valEl.value = d.val;

    editIndex = i;
    btnAdd.innerText = "Salvar";
}

// 🗑️ Remover
function remover(i) {
    dados.splice(i,1);
    atualizar();
}

// 🧹 Limpar
function limpar() {
    descEl.value = "";
    catEl.value = "";
    valEl.value = "";
}

// 📂 Importar PDF
pdfInput.addEventListener('change', async function(e) {

    const file = e.target.files[0];
    const reader = new FileReader();

    reader.onload = async function() {

        const pdf = await pdfjsLib.getDocument(new Uint8Array(this.result)).promise;

        for (let i = 1; i <= pdf.numPages; i++) {

            let page = await pdf.getPage(i);
            let content = await page.getTextContent();
            let text = content.items.map(i => i.str).join(" ");

            processar(text);
        }
    };

    reader.readAsArrayBuffer(file);
});

// 🧠 Processar PDF
function processar(texto) {

    let linhas = texto.split(/(?=\d{2}\/\d{2})|(?=\d{1,2} [A-Z]{3})/);

    linhas.forEach(linha => {

        let data = linha.match(/(\d{2})\/(\d{2})/);
        let nubank = linha.match(/(\d{1,2}) [A-Z]{3}/);

        let valorMatch = linha.match(/R\$ ?([\d,]+)/);
        if (!valorMatch) return;

        let valor = parseFloat(valorMatch[1].replace(",", "."));

        let desc = linha
            .replace(/R\$.*$/, "")
            .replace(/- Parcela.*$/, "")
            .replace(/.*\d{4}/, "")
            .trim();

        let dia = 1;
        let mes = new Date().getMonth()+1;

        if (data) {
            dia = parseInt(data[1]);
            mes = parseInt(data[2]);
        }

        if (nubank) {
            dia = parseInt(nubank[1]);
        }

        dados.push({
            desc,
            categoria: detectarCategoria(desc),
            cartao: detectarCartao(linha),
            val: valor,
            dia,
            mes
        });

    });

    atualizar();
}

// 🔄 Atualizar
function atualizar() {

    let mesAtual = new Date().getMonth()+1;
    let dadosMes = dados.filter(d => d.mes === mesAtual);

    let total = dadosMes.reduce((a,b)=>a+b.val,0);
    totalEl.innerText = total.toFixed(2);

    // 💳 agrupado
    let cartoes = {};
    dadosMes.forEach(d=>{
        cartoes[d.cartao] = (cartoes[d.cartao]||0)+d.val;
    });

    cards.innerHTML = Object.entries(cartoes).map(([k,v]) =>
        `<div class="badge">${k}: R$ ${v.toFixed(2)}</div>`
    ).join("");

    // 📊 gráfico
    let cat = {};
    dadosMes.forEach(d=>{
        cat[d.categoria] = (cat[d.categoria]||0)+d.val;
    });

    if(window.g) window.g.destroy();
    window.g = new Chart(grafico, {
        type: 'pie',
        data: {
            labels: Object.keys(cat),
            datasets: [{ data: Object.values(cat) }]
        }
    });

    // 📋 lista
    lista.innerHTML = dadosMes.map((d,i)=>`
        <div class="item">
            <span>${d.dia}/${d.mes} - ${d.desc} (${d.cartao}) - R$ ${d.val.toFixed(2)}</span>
            <div>
                <button class="edit" onclick="editar(${i})">✏️</button>
                <button class="delete" onclick="remover(${i})">X</button>
            </div>
        </div>
    `).join("");

    localStorage.setItem("dados", JSON.stringify(dados));
}

atualizar();

// DOM
const descEl = document.getElementById("desc");
const catEl = document.getElementById("categoria");
const cartaoEl = document.getElementById("cartao");
const valEl = document.getElementById("val");
const totalEl = document.getElementById("total");
const cards = document.getElementById("cards");
const pdfInput = document.getElementById("pdfInput");
const lista = document.getElementById("lista");
const btnAdd = document.getElementById("btnAdd");

</script>

</body>
</html>
