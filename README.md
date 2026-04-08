<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no">
<title>App Financeiro PRO - Mobile</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html, body {
    width: 100%;
    overflow-x: hidden;
}

body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    background: #0f172a;
    color: #f1f5f9;
    padding: 16px;
    min-height: 100vh;
}

/* Container principal - OCUPA 100% DA LARGURA */
.container {
    width: 100%;
    max-width: 600px;
    margin: 0 auto;
}

/* Títulos */
h1 {
    text-align: center;
    font-size: 1.8rem;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
}

h2, h3 {
    font-size: 1.2rem;
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    gap: 8px;
}

/* Cards */
.card {
    background: #1e293b;
    border-radius: 20px;
    padding: 20px;
    margin-bottom: 16px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.3);
    width: 100%;
}

/* Card de total - destaque */
.card-total {
    background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%);
    border: 1px solid #334155;
    text-align: center;
}

.total-valor {
    font-size: 2.5rem;
    font-weight: bold;
    color: #22c55e;
    word-break: break-word;
}

/* Formulário */
.form-group {
    margin-bottom: 12px;
    width: 100%;
}

label {
    display: block;
    font-size: 0.85rem;
    color: #94a3b8;
    margin-bottom: 4px;
    font-weight: 500;
}

input, select, button {
    width: 100%;
    padding: 14px 12px;
    font-size: 16px;
    border-radius: 12px;
    border: none;
    background: #334155;
    color: #f1f5f9;
    transition: all 0.2s ease;
}

input:focus, select:focus {
    outline: none;
    background: #475569;
    box-shadow: 0 0 0 2px #22c55e;
}

input::placeholder {
    color: #94a3b8;
}

button {
    background: #22c55e;
    color: white;
    font-weight: bold;
    cursor: pointer;
    border: none;
    margin-top: 8px;
}

button:active {
    transform: scale(0.98);
}

button.btn-secondary {
    background: #475569;
}

button.btn-danger {
    background: #ef4444;
}

button.btn-warning {
    background: #f59e0b;
}

button.btn-info {
    background: #3b82f6;
}

/* Grid de botões */
.button-group {
    display: flex;
    gap: 10px;
    margin-top: 8px;
    flex-wrap: wrap;
}

.button-group button {
    flex: 1;
    margin-top: 0;
}

/* Grid de cards de pagamento */
.grid-cards {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-top: 8px;
}

.badge-card {
    background: #334155;
    padding: 10px 14px;
    border-radius: 30px;
    font-size: 0.85rem;
    font-weight: 500;
    flex: 1 0 auto;
    text-align: center;
    word-break: keep-all;
}

/* Lista de gastos */
.lista-gastos {
    max-height: 400px;
    overflow-y: auto;
}

.item-gasto {
    background: #0f172a;
    border-radius: 14px;
    padding: 14px;
    margin-bottom: 10px;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
    gap: 10px;
}

.item-info {
    flex: 2;
    min-width: 150px;
}

.item-desc {
    font-weight: bold;
    font-size: 1rem;
    word-break: break-word;
}

.item-meta {
    font-size: 0.75rem;
    color: #94a3b8;
    margin-top: 4px;
}

.item-valor {
    font-weight: bold;
    font-size: 1.1rem;
    color: #22c55e;
}

.item-actions {
    display: flex;
    gap: 8px;
}

.item-actions button {
    width: auto;
    padding: 8px 14px;
    margin: 0;
    font-size: 0.85rem;
}

/* Gráfico responsivo */
canvas {
    max-height: 280px;
    width: 100% !important;
    margin-top: 10px;
}

/* Filtro de mês */
.filtro-row {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    align-items: flex-end;
}

.filtro-row .form-group {
    flex: 2;
    margin-bottom: 0;
}

.filtro-row button {
    flex: 1;
    margin-top: 0;
}

/* Mensagem sem dados */
.sem-dados {
    text-align: center;
    padding: 40px 20px;
    color: #64748b;
}

/* Scroll suave */
::-webkit-scrollbar {
    width: 6px;
    height: 6px;
}

::-webkit-scrollbar-track {
    background: #1e293b;
    border-radius: 10px;
}

::-webkit-scrollbar-thumb {
    background: #475569;
    border-radius: 10px;
}

/* ========== RESPONSIVIDADE GARANTIDA PARA GITHUB PAGES ========== */
@media (max-width: 600px) {
    body {
        padding: 12px;
    }
    
    .card {
        padding: 16px;
    }
    
    .total-valor {
        font-size: 2rem;
    }
    
    .item-gasto {
        flex-direction: column;
        align-items: stretch;
    }
    
    .item-actions {
        justify-content: flex-end;
    }
    
    .badge-card {
        white-space: normal;
        word-break: break-word;
    }
    
    .filtro-row {
        flex-direction: column;
    }
    
    .filtro-row .form-group {
        width: 100%;
    }
    
    .button-group {
        flex-direction: column;
    }
    
    .button-group button {
        width: 100%;
    }
}

@media (max-width: 400px) {
    body {
        padding: 8px;
    }
    
    .card {
        padding: 12px;
    }
    
    .total-valor {
        font-size: 1.6rem;
    }
    
    h1 {
        font-size: 1.4rem;
    }
    
    h2, h3 {
        font-size: 1rem;
    }
    
    input, select, button {
        padding: 12px 10px;
        font-size: 14px;
    }
}

@media (min-width: 601px) and (max-width: 768px) {
    .container {
        max-width: 550px;
    }
}

@media (min-width: 769px) {
    .container {
        max-width: 600px;
    }
}

@media (orientation: landscape) and (max-height: 500px) {
    .lista-gastos {
        max-height: 180px;
    }
    
    .total-valor {
        font-size: 1.5rem;
    }
}
</style>
</head>

<body>
<div class="container">
    <h1>
        <span>💰</span> App Financeiro
    </h1>

    <!-- Card Total -->
    <div class="card card-total">
        <h2>📊 Total do mês</h2>
        <div class="total-valor" id="total">R$ 0,00</div>
    </div>

    <!-- Filtro -->
    <div class="card">
        <h3>📅 Período</h3>
        <div class="filtro-row">
            <div class="form-group">
                <label>Mês/Ano</label>
                <input type="month" id="mesSeletor">
            </div>
            <button id="btnFiltrar" class="btn-info">🔍 Filtrar</button>
        </div>
    </div>

    <!-- Cards por pagamento -->
    <div class="card">
        <h3>💳 Por forma de pagamento</h3>
        <div id="cards" class="grid-cards"></div>
    </div>

    <!-- Formulário de adicionar -->
    <div class="card">
        <h3>➕ Adicionar gasto</h3>
        
        <div class="form-group">
            <label>Descrição</label>
            <input id="desc" placeholder="Ex: Ifood, Uber, Posto..." autocomplete="off">
        </div>

        <div class="form-group">
            <label>Categoria</label>
            <select id="categoria">
                <option value="">🤖 Automático</option>
                <option>🍔 Alimentação</option>
                <option>⛽ Combustível</option>
                <option>🚗 Transporte</option>
                <option>🛍️ Compras</option>
                <option>📺 Assinatura</option>
                <option>💊 Saúde</option>
                <option>🏠 Casa</option>
                <option>📌 Outros</option>
            </select>
        </div>

        <div class="form-group">
            <label>Forma de pagamento</label>
            <select id="cartao">
                <option>💳 Nubank</option>
                <option>💳 Ourocard</option>
                <option>💳 Débito</option>
                <option>🍽️ VR</option>
                <option>💵 Dinheiro</option>
            </select>
        </div>

        <div class="form-group">
            <label>Valor (R$)</label>
            <input id="val" type="number" step="0.01" placeholder="0,00" min="0.01">
        </div>

        <div class="button-group">
            <button id="btnAdd">➕ Adicionar</button>
            <button id="btnCancelar" class="btn-secondary" style="display:none;">✖️ Cancelar</button>
        </div>
    </div>

    <!-- Importar PDF -->
    <div class="card">
        <h3>📂 Importar fatura (PDF)</h3>
        <input type="file" id="pdfInput" accept=".pdf">
        <small style="color:#94a3b8; display:block; margin-top:8px;">📌 Suporte para faturas Nubank e Ourocard</small>
    </div>

    <!-- Gráfico -->
    <div class="card">
        <h3>📊 Gastos por categoria</h3>
        <canvas id="grafico"></canvas>
    </div>

    <!-- Lista de gastos -->
    <div class="card">
        <h3>📋 Lista de gastos</h3>
        <div id="lista" class="lista-gastos"></div>
        <div id="semDados" class="sem-dados">✨ Nenhum gasto no período</div>
    </div>

    <!-- Botões extras -->
    <div class="button-group" style="margin-bottom: 20px;">
        <button id="btnExportar" class="btn-info">📎 Exportar backup</button>
        <button id="btnLimparTudo" class="btn-danger">⚠️ Limpar tudo</button>
    </div>
</div>

<script>
// ---------- DECLARAÇÃO DE VARIÁVEIS ----------
let dados = JSON.parse(localStorage.getItem("dados")) || [];
let editId = null;
let mesAtualFiltro = new Date().toISOString().slice(0, 7);

// Elementos DOM
const descEl = document.getElementById("desc");
const catEl = document.getElementById("categoria");
const cartaoEl = document.getElementById("cartao");
const valEl = document.getElementById("val");
const totalEl = document.getElementById("total");
const cardsDiv = document.getElementById("cards");
const pdfInput = document.getElementById("pdfInput");
const listaDiv = document.getElementById("lista");
const btnAdd = document.getElementById("btnAdd");
const btnCancelar = document.getElementById("btnCancelar");
const mesSeletor = document.getElementById("mesSeletor");
const btnFiltrar = document.getElementById("btnFiltrar");
const btnExportar = document.getElementById("btnExportar");
const btnLimparTudo = document.getElementById("btnLimparTudo");
const semDadosDiv = document.getElementById("semDados");
const graficoCanvas = document.getElementById("grafico");

let chartInstance = null;

// ---------- FUNÇÕES AUXILIARES ----------
function gerarId() {
    return Date.now() + '-' + Math.random().toString(36).substr(2, 8);
}

function detectarCategoria(desc) {
    desc = desc.toLowerCase();
    if (desc.includes("mercado") || desc.includes("padaria") || desc.includes("restaurante") || desc.includes("ifood")) return "🍔 Alimentação";
    if (desc.includes("posto") || desc.includes("shell") || desc.includes("gasolina")) return "⛽ Combustível";
    if (desc.includes("uber") || desc.includes("99")) return "🚗 Transporte";
    if (desc.includes("netflix") || desc.includes("spotify")) return "📺 Assinatura";
    if (desc.includes("farmacia") || desc.includes("medico")) return "💊 Saúde";
    if (desc.includes("luz") || desc.includes("agua") || desc.includes("aluguel")) return "🏠 Casa";
    if (desc.includes("roupa") || desc.includes("shopping")) return "🛍️ Compras";
    return "📌 Outros";
}

function detectarCartaoPdf(linha) {
    if (linha.includes("••••")) return "💳 Nubank";
    if (linha.toLowerCase().includes("ourocard")) return "💳 Ourocard";
    return "💳 Nubank";
}

function salvarEAtualizar() {
    localStorage.setItem("dados", JSON.stringify(dados));
    atualizarTela();
}

function cancelarEdicao() {
    editId = null;
    btnAdd.textContent = "➕ Adicionar";
    btnCancelar.style.display = "none";
    descEl.value = "";
    catEl.value = "";
    cartaoEl.value = "💳 Nubank";
    valEl.value = "";
}

function adicionarOuEditar() {
    let desc = descEl.value.trim();
    let categoria = catEl.value;
    let cartao = cartaoEl.value;
    let val = parseFloat(valEl.value);

    if (!desc) {
        alert("Digite uma descrição!");
        return;
    }
    if (isNaN(val) || val <= 0) {
        alert("Digite um valor válido (maior que zero)!");
        return;
    }

    if (!categoria || categoria === "") {
        categoria = detectarCategoria(desc);
    }

    const hoje = new Date();
    const dia = hoje.getDate();
    const mes = hoje.getMonth() + 1;
    const ano = hoje.getFullYear();

    if (editId !== null) {
        const index = dados.findIndex(item => item.id === editId);
        if (index !== -1) {
            dados[index] = { ...dados[index], desc, categoria, cartao, val, dia, mes, ano };
        }
        cancelarEdicao();
    } else {
        dados.push({
            id: gerarId(),
            desc,
            categoria,
            cartao,
            val,
            dia,
            mes,
            ano
        });
    }

    descEl.value = "";
    catEl.value = "";
    valEl.value = "";
    salvarEAtualizar();
}

function editarGasto(id) {
    const item = dados.find(d => d.id === id);
    if (!item) return;

    descEl.value = item.desc;
    catEl.value = item.categoria;
    cartaoEl.value = item.cartao;
    valEl.value = item.val;

    editId = id;
    btnAdd.textContent = "💾 Salvar edição";
    btnCancelar.style.display = "block";
    document.querySelector(".card:nth-of-type(3)").scrollIntoView({ behavior: "smooth" });
}

function removerGasto(id) {
    if (confirm("Tem certeza que deseja excluir este gasto?")) {
        dados = dados.filter(d => d.id !== id);
        if (editId === id) cancelarEdicao();
        salvarEAtualizar();
    }
}

function processarPdf(texto) {
    let linhas = texto.split(/(?=\d{2}\/\d{2})|(?=\d{1,2} [A-Z]{3})/);
    let novosGastos = 0;

    linhas.forEach(linha => {
        let data = linha.match(/(\d{2})\/(\d{2})/);
        let nubank = linha.match(/(\d{1,2}) [A-Z]{3}/);
        let valorMatch = linha.match(/R\$ ?([\d.,]+)/);
        
        if (!valorMatch) return;

        let valorStr = valorMatch[1].replace(/\./g, '').replace(',', '.');
        let valor = parseFloat(valorStr);
        if (isNaN(valor)) return;

        let desc = linha.replace(/R\$.*$/, "").replace(/- Parcela.*$/, "").replace(/.*\d{4}/, "").trim();
        if (desc.length < 2) desc = "Gasto identificado";

        let dia = 1, mes = new Date().getMonth() + 1, ano = new Date().getFullYear();
        if (data) { dia = parseInt(data[1]); mes = parseInt(data[2]); }
        if (nubank) dia = parseInt(nubank[1]);

        const jaExiste = dados.some(d => d.desc === desc && Math.abs(d.val - valor) < 0.01 && d.dia === dia && d.mes === mes);
        if (!jaExiste) {
            dados.push({
                id: gerarId(),
                desc: desc,
                categoria: detectarCategoria(desc),
                cartao: detectarCartaoPdf(linha),
                val: valor,
                dia: dia,
                mes: mes,
                ano: ano
            });
            novosGastos++;
        }
    });

    if (novosGastos > 0) {
        alert(`✅ ${novosGastos} novos gastos importados!`);
        salvarEAtualizar();
    } else {
        alert("⚠️ Nenhum gasto novo encontrado no PDF.");
    }
}

async function importarPdf(file) {
    const reader = new FileReader();
    reader.onload = async function(event) {
        try {
            const pdf = await pdfjsLib.getDocument({ data: new Uint8Array(event.target.result) }).promise;
            let textoCompleto = "";
            for (let i = 1; i <= pdf.numPages; i++) {
                const page = await pdf.getPage(i);
                const content = await page.getTextContent();
                textoCompleto += content.items.map(item => item.str).join(" ") + "\n";
            }
            processarPdf(textoCompleto);
        } catch (error) {
            alert("Erro ao processar o PDF.");
        }
    };
    reader.readAsArrayBuffer(file);
}

function atualizarTela() {
    const [anoSelecionado, mesSelecionado] = mesAtualFiltro.split('-').map(Number);
    const dadosFiltrados = dados.filter(d => d.ano === anoSelecionado && d.mes === mesSelecionado);

    if (dadosFiltrados.length === 0) {
        semDadosDiv.style.display = "block";
        listaDiv.innerHTML = "";
    } else {
        semDadosDiv.style.display = "none";
        listaDiv.innerHTML = dadosFiltrados.map(item => `
            <div class="item-gasto">
                <div class="item-info">
                    <div class="item-desc">${item.desc}</div>
                    <div class="item-meta">${item.dia}/${item.mes} • ${item.cartao} • ${item.categoria}</div>
                </div>
                <div class="item-valor">R$ ${item.val.toFixed(2)}</div>
                <div class="item-actions">
                    <button class="btn-warning" onclick="editarGasto('${item.id}')">✏️</button>
                    <button class="btn-danger" onclick="removerGasto('${item.id}')">🗑️</button>
                </div>
            </div>
        `).join("");
    }

    const total = dadosFiltrados.reduce((s, i) => s + i.val, 0);
    totalEl.innerHTML = `R$ ${total.toFixed(2)}`;

    const cartoesMap = {};
    dadosFiltrados.forEach(i => cartoesMap[i.cartao] = (cartoesMap[i.cartao] || 0) + i.val);
    cardsDiv.innerHTML = Object.entries(cartoesMap).length ? 
        Object.entries(cartoesMap).map(([k, v]) => `<div class="badge-card">${k}: R$ ${v.toFixed(2)}</div>`).join("") :
        '<div class="badge-card">Nenhum gasto no período</div>';

    const categoriaMap = {};
    dadosFiltrados.forEach(i => categoriaMap[i.categoria] = (categoriaMap[i.categoria] || 0) + i.val);

    if (chartInstance) chartInstance.destroy();
    chartInstance = new Chart(graficoCanvas, {
        type: 'pie',
        data: {
            labels: Object.keys(categoriaMap).length ? Object.keys(categoriaMap) : ['Sem dados'],
            datasets: [{
                data: Object.keys(categoriaMap).length ? Object.values(categoriaMap) : [1],
                backgroundColor: ['#22c55e', '#3b82f6', '#f59e0b', '#ef4444', '#8b5cf6', '#ec489a', '#14b8a6', '#f97316', '#64748b'],
                borderWidth: 0
            }]
        },
        options: { responsive: true, maintainAspectRatio: true, plugins: { legend: { position: 'bottom', labels: { color: '#f1f5f9' } } } }
    });
}

function exportarDados() {
    const dataStr = JSON.stringify(dados, null, 2);
    const blob = new Blob([dataStr], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `financeiro_backup_${new Date().toISOString().slice(0,19)}.json`;
    a.click();
    URL.revokeObjectURL(url);
    alert("✅ Backup exportado!");
}

function limparTodosDados() {
    if (confirm("⚠️ ATENÇÃO: Isso vai apagar TODOS os seus gastos. Tem certeza?")) {
        dados = [];
        cancelarEdicao();
        salvarEAtualizar();
        alert("Todos os dados foram removidos.");
    }
}

function init() {
    const hoje = new Date();
    mesAtualFiltro = `${hoje.getFullYear()}-${String(hoje.getMonth() + 1).padStart(2, '0')}`;
    mesSeletor.value = mesAtualFiltro;

    btnAdd.onclick = adicionarOuEditar;
    btnCancelar.onclick = cancelarEdicao;
    btnFiltrar.onclick = () => { mesAtualFiltro = mesSeletor.value; atualizarTela(); };
    btnExportar.onclick = exportarDados;
    btnLimparTudo.onclick = limparTodosDados;
    pdfInput.onchange = (e) => { if (e.target.files.length) importarPdf(e.target.files[0]); pdfInput.value = ""; };

    atualizarTela();
}

init();
</script>
</body>
</html>
