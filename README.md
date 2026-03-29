<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Corre Holding Lda — Sistema de Gestão</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=DM+Sans:wght@300;400;500;600&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --gold: #C9A84C; --gold-light: #E8C97A; --gold-dim: #8B6E2A;
    --dark: #0D0D0D; --dark2: #141414; --dark3: #1C1C1C; --dark4: #242424;
    --border: #2A2A2A; --text: #E8E4DC; --text-dim: #8A8070;
    --green: #4CAF74; --red: #E05555; --blue: #5588E0; --purple: #9B5DE5;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body { background:var(--dark); color:var(--text); font-family:'DM Sans',sans-serif; min-height:100vh; overflow-x:hidden; }

  .header {
    background:linear-gradient(135deg,var(--dark2) 0%,#1A1508 100%);
    border-bottom:1px solid var(--gold-dim);
    padding:0 24px; display:flex; align-items:center; justify-content:space-between;
    height:70px; position:sticky; top:0; z-index:100;
    box-shadow:0 4px 40px rgba(201,168,76,0.08);
  }
  .logo-area { display:flex; align-items:center; gap:14px; }
  .logo-icon {
    width:42px; height:42px;
    background:linear-gradient(135deg,var(--gold),var(--gold-dim));
    border-radius:10px; display:flex; align-items:center; justify-content:center;
    font-size:20px; font-weight:900; color:var(--dark);
    font-family:'Playfair Display',serif; letter-spacing:-1px;
    box-shadow:0 0 20px rgba(201,168,76,0.3);
  }
  .logo-text h1 { font-family:'Playfair Display',serif; font-size:18px; color:var(--gold-light); }
  .logo-text span { font-size:11px; color:var(--text-dim); letter-spacing:1px; text-transform:uppercase; }
  .header-right { display:flex; align-items:center; gap:10px; }
  .owner-badge { padding:6px 14px; border:1px solid var(--gold-dim); border-radius:20px; font-size:12px; color:var(--gold); }
  .sync-badge {
    padding:5px 12px; border-radius:20px; font-size:11px; font-weight:600;
    display:flex; align-items:center; gap:5px; transition:all 0.3s;
  }
  .sync-badge.online { background:rgba(76,175,116,0.15); border:1px solid var(--green); color:var(--green); }
  .sync-badge.offline { background:rgba(224,85,85,0.15); border:1px solid var(--red); color:var(--red); }
  .sync-badge.syncing { background:rgba(85,136,224,0.15); border:1px solid var(--blue); color:var(--blue); }
  .sync-dot { width:7px; height:7px; border-radius:50%; background:currentColor; }
  .sync-dot.pulse { animation:pulse 1.2s infinite; }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.3} }

  .nav { background:var(--dark2); border-bottom:1px solid var(--border); display:flex; overflow-x:auto; padding:0 24px; }
  .nav-btn {
    padding:14px 18px; border:none; background:none; cursor:pointer;
    font-family:'DM Sans',sans-serif; font-size:13px; font-weight:500;
    color:var(--text-dim); border-bottom:2px solid transparent; white-space:nowrap;
    transition:all 0.2s; display:flex; align-items:center; gap:6px;
  }
  .nav-btn:hover { color:var(--text); }
  .nav-btn.active { color:var(--gold); border-bottom-color:var(--gold); }

  .main { padding:24px; max-width:1400px; margin:0 auto; }
  .section { display:none; }
  .section.active { display:block; animation:fadeIn 0.3s ease; }
  @keyframes fadeIn { from{opacity:0;transform:translateY(8px)} to{opacity:1;transform:translateY(0)} }

  .kpi-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(190px,1fr)); gap:16px; margin-bottom:24px; }
  .kpi-card {
    background:var(--dark3); border:1px solid var(--border); border-radius:14px;
    padding:20px; position:relative; overflow:hidden; transition:transform 0.2s;
  }
  .kpi-card:hover { transform:translateY(-2px); }
  .kpi-card::before { content:''; position:absolute; top:0; left:0; right:0; height:2px; }
  .kpi-card.gold::before { background:linear-gradient(90deg,var(--gold),var(--gold-light)); }
  .kpi-card.green::before { background:var(--green); }
  .kpi-card.red::before { background:var(--red); }
  .kpi-card.blue::before { background:var(--blue); }
  .kpi-card.purple::before { background:var(--purple); }
  .kpi-label { font-size:11px; color:var(--text-dim); text-transform:uppercase; letter-spacing:1px; margin-bottom:8px; }
  .kpi-value { font-family:'DM Mono',monospace; font-size:22px; font-weight:500; }
  .kpi-card.gold .kpi-value { color:var(--gold); }
  .kpi-card.green .kpi-value { color:var(--green); }
  .kpi-card.red .kpi-value { color:var(--red); }
  .kpi-card.blue .kpi-value { color:var(--blue); }
  .kpi-card.purple .kpi-value { color:var(--purple); }
  .kpi-sub { font-size:12px; color:var(--text-dim); margin-top:4px; }

  .panel { background:var(--dark3); border:1px solid var(--border); border-radius:14px; overflow:hidden; margin-bottom:20px; }
  .panel-header {
    padding:16px 20px; border-bottom:1px solid var(--border);
    display:flex; align-items:center; justify-content:space-between; background:var(--dark4);
    flex-wrap:wrap; gap:10px;
  }
  .panel-title { font-family:'Playfair Display',serif; font-size:16px; color:var(--gold-light); }
  .panel-body { padding:20px; }

  .form-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(200px,1fr)); gap:14px; }
  .form-group { display:flex; flex-direction:column; gap:6px; }
  .form-group label { font-size:11px; color:var(--text-dim); text-transform:uppercase; letter-spacing:0.8px; }
  .form-group input, .form-group select, .form-group textarea {
    background:var(--dark2); border:1px solid var(--border); border-radius:8px;
    padding:10px 12px; color:var(--text); font-family:'DM Sans',sans-serif;
    font-size:14px; outline:none; transition:border-color 0.2s; width:100%;
  }
  .form-group input:focus, .form-group select:focus { border-color:var(--gold-dim); box-shadow:0 0 0 2px rgba(201,168,76,0.1); }
  .form-group select option { background:var(--dark2); }

  .btn {
    padding:11px 22px; border-radius:8px; border:none; cursor:pointer;
    font-family:'DM Sans',sans-serif; font-size:14px; font-weight:600;
    transition:all 0.2s; display:inline-flex; align-items:center; gap:7px;
  }
  .btn-gold { background:linear-gradient(135deg,var(--gold),var(--gold-dim)); color:var(--dark); }
  .btn-gold:hover { filter:brightness(1.1); transform:translateY(-1px); }
  .btn-outline { background:transparent; border:1px solid var(--border); color:var(--text-dim); }
  .btn-outline:hover { border-color:var(--gold-dim); color:var(--gold); }
  .btn-green { background:var(--green); color:white; }
  .btn-red { background:var(--red); color:white; }
  .btn-blue { background:var(--blue); color:white; }
  .btn-sm { padding:6px 12px; font-size:12px; }
  .form-actions { display:flex; gap:10px; justify-content:flex-end; margin-top:16px; padding-top:16px; border-top:1px solid var(--border); }

  .table-wrap { overflow-x:auto; }
  table { width:100%; border-collapse:collapse; font-size:13px; }
  thead tr { background:var(--dark4); border-bottom:1px solid var(--border); }
  thead th { padding:11px 14px; text-align:left; font-size:11px; text-transform:uppercase; letter-spacing:0.8px; color:var(--text-dim); white-space:nowrap; }
  tbody tr { border-bottom:1px solid var(--border); transition:background 0.15s; }
  tbody tr:hover { background:var(--dark4); }
  tbody td { padding:11px 14px; }
  .mono { font-family:'DM Mono',monospace; font-size:12px; }

  .badge { display:inline-block; padding:3px 10px; border-radius:20px; font-size:11px; font-weight:500; }
  .badge-green { background:rgba(76,175,116,0.15); color:var(--green); border:1px solid rgba(76,175,116,0.3); }
  .badge-red { background:rgba(224,85,85,0.15); color:var(--red); border:1px solid rgba(224,85,85,0.3); }
  .badge-blue { background:rgba(85,136,224,0.15); color:var(--blue); border:1px solid rgba(85,136,224,0.3); }
  .badge-gold { background:rgba(201,168,76,0.15); color:var(--gold); border:1px solid rgba(201,168,76,0.3); }
  .badge-purple { background:rgba(155,93,229,0.15); color:var(--purple); border:1px solid rgba(155,93,229,0.3); }
  .profit-pos { color:var(--green); font-weight:600; }
  .profit-neg { color:var(--red); font-weight:600; }

  .totals-bar {
    background:var(--dark4); border:1px solid var(--border); border-radius:10px;
    padding:12px 16px; display:flex; gap:24px; flex-wrap:wrap; margin-bottom:16px; font-size:13px;
  }
  .totals-bar span { color:var(--text-dim); }
  .totals-bar strong { color:var(--text); margin-left:4px; font-family:'DM Mono',monospace; font-size:13px; }

  .two-col { display:grid; grid-template-columns:1fr 1fr; gap:20px; }
  @media(max-width:768px) { .two-col { grid-template-columns:1fr; } }

  .nota-box {
    background:rgba(224,85,85,0.08); border:1px solid rgba(224,85,85,0.3);
    border-radius:12px; padding:16px 20px; margin-bottom:20px;
    display:flex; gap:12px; align-items:flex-start;
  }
  .nota-box .text { font-size:13px; color:var(--text-dim); line-height:1.6; }
  .nota-box .text strong { color:var(--red); display:block; margin-bottom:4px; }

  .credit-card {
    background:var(--dark3); border:1px solid var(--border); border-radius:14px;
    padding:20px; margin-bottom:14px;
  }
  .credit-header { display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:14px; }
  .credit-name { font-family:'Playfair Display',serif; font-size:17px; color:var(--gold-light); }
  .credit-bar-wrap { background:var(--dark4); border-radius:6px; height:8px; overflow:hidden; margin:10px 0; }
  .credit-bar { height:100%; border-radius:6px; transition:width 0.5s ease; }
  .credit-details { display:grid; grid-template-columns:repeat(auto-fit,minmax(120px,1fr)); gap:10px; margin-top:12px; }
  .credit-detail-item .lbl { font-size:10px; color:var(--text-dim); text-transform:uppercase; letter-spacing:0.8px; }
  .credit-detail-item .val { font-size:14px; font-weight:500; margin-top:2px; }

  .modal-overlay {
    position:fixed; inset:0; background:rgba(0,0,0,0.7);
    display:flex; align-items:center; justify-content:center;
    z-index:200; opacity:0; pointer-events:none; transition:opacity 0.2s;
  }
  .modal-overlay.open { opacity:1; pointer-events:all; }
  .modal {
    background:var(--dark3); border:1px solid var(--border); border-radius:16px;
    padding:28px; max-width:380px; width:90%;
    transform:scale(0.95); transition:transform 0.2s;
  }
  .modal-overlay.open .modal { transform:scale(1); }
  .modal h3 { font-family:'Playfair Display',serif; color:var(--gold-light); margin-bottom:10px; }
  .modal p { color:var(--text-dim); font-size:14px; margin-bottom:20px; }
  .modal-actions { display:flex; gap:10px; justify-content:flex-end; }

  #toast {
    position:fixed; bottom:24px; right:24px;
    background:var(--dark4); border:1px solid var(--gold-dim);
    color:var(--gold-light); padding:12px 20px; border-radius:10px;
    font-size:13px; z-index:999; opacity:0; transform:translateY(10px);
    transition:all 0.3s; pointer-events:none;
    box-shadow:0 8px 30px rgba(0,0,0,0.4);
  }
  #toast.show { opacity:1; transform:translateY(0); }

  .empty { text-align:center; padding:50px 20px; color:var(--text-dim); }
  .empty-icon { font-size:40px; margin-bottom:12px; }

  .loading-overlay {
    position:fixed; inset:0; background:rgba(13,13,13,0.9);
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    z-index:999; transition:opacity 0.5s;
  }
  .loading-overlay.hidden { opacity:0; pointer-events:none; }
  .loading-logo { font-family:'Playfair Display',serif; font-size:32px; color:var(--gold); margin-bottom:16px; }
  .loading-bar-wrap { width:200px; height:4px; background:var(--dark4); border-radius:2px; overflow:hidden; }
  .loading-bar { height:100%; background:var(--gold); border-radius:2px; animation:loadbar 1.5s ease-out forwards; }
  @keyframes loadbar { from{width:0} to{width:100%} }
  .loading-text { margin-top:12px; font-size:13px; color:var(--text-dim); }

  ::-webkit-scrollbar { width:6px; height:6px; }
  ::-webkit-scrollbar-track { background:var(--dark2); }
  ::-webkit-scrollbar-thumb { background:var(--border); border-radius:3px; }

  @media(max-width:600px) {
    .header { padding:0 14px; }
    .main { padding:14px; }
    .kpi-grid { grid-template-columns:1fr 1fr; }
    .form-grid { grid-template-columns:1fr; }
    .owner-badge { display:none; }
  }
</style>
</head>
<body>

<!-- LOADING -->
<div class="loading-overlay" id="loadingOverlay">
  <div class="loading-logo">Corre Holding</div>
  <div class="loading-bar-wrap"><div class="loading-bar"></div></div>
  <div class="loading-text">A sincronizar dados...</div>
</div>

<!-- HEADER -->
<header class="header">
  <div class="logo-area">
    <div class="logo-icon">CH</div>
    <div class="logo-text">
      <h1>Corre Holding Lda</h1>
      <span>Sistema de Gestão Empresarial</span>
    </div>
  </div>
  <div class="header-right">
    <div class="owner-badge">👤 Jaime Oliveira</div>
    <div class="sync-badge offline" id="syncBadge">
      <div class="sync-dot" id="syncDot"></div>
      <span id="syncText">A ligar...</span>
    </div>
  </div>
</header>

<!-- NAV -->
<nav class="nav">
  <button class="nav-btn active" onclick="switchTab('dashboard',this)">📊 Dashboard</button>
  <button class="nav-btn" onclick="switchTab('compras',this)">🛒 Compras</button>
  <button class="nav-btn" onclick="switchTab('vendas',this)">💰 Vendas</button>
  <button class="nav-btn" onclick="switchTab('estoque',this)">📦 Estoque</button>
  <button class="nav-btn" onclick="switchTab('credito',this)">🏦 Microcrédito</button>
  <button class="nav-btn" onclick="switchTab('relatorio',this)">📈 Relatório</button>
</nav>

<main class="main">

  <!-- DASHBOARD -->
  <div class="section active" id="sec-dashboard">
    <div class="kpi-grid" id="kpiGrid"></div>
    <div class="two-col">
      <div class="panel">
        <div class="panel-header"><span class="panel-title">⚡ Últimas Vendas</span></div>
        <div class="panel-body">
          <div class="table-wrap"><table>
            <thead><tr><th>Produto</th><th>Venda</th><th>Lucro</th><th>Data</th></tr></thead>
            <tbody id="dashVendasBody"></tbody>
          </table></div>
        </div>
      </div>
      <div class="panel">
        <div class="panel-header"><span class="panel-title">⚡ Últimas Compras</span></div>
        <div class="panel-body">
          <div class="table-wrap"><table>
            <thead><tr><th>Produto</th><th>Qtd</th><th>Custo</th><th>Data</th></tr></thead>
            <tbody id="dashComprasBody"></tbody>
          </table></div>
        </div>
      </div>
    </div>
    <div class="panel" id="dashCreditPanel" style="display:none">
      <div class="panel-header"><span class="panel-title">⚠️ Microcrédito em Aberto</span></div>
      <div class="panel-body" id="dashCreditBody"></div>
    </div>
  </div>

  <!-- COMPRAS -->
  <div class="section" id="sec-compras">
    <div class="panel">
      <div class="panel-header"><span class="panel-title">➕ Registar Compra</span></div>
      <div class="panel-body">
        <div class="form-grid">
          <div class="form-group"><label>Nome do Produto</label><input id="c_produto" placeholder="Ex: HP Laptop, JBL Speaker..." /></div>
          <div class="form-group"><label>Quantidade</label><input id="c_qtd" type="number" min="1" value="1" /></div>
          <div class="form-group"><label>Valor de Compra (MZN)</label><input id="c_valor" type="number" placeholder="0" /></div>
          <div class="form-group"><label>Nº da Nota / Recibo</label><input id="c_nota" placeholder="Nº da corda da compra" /></div>
          <div class="form-group"><label>Data da Compra</label><input id="c_data" type="date" /></div>
          <div class="form-group"><label>Fornecedor</label><input id="c_fornecedor" placeholder="Nome do fornecedor" /></div>
          <div class="form-group"><label>Categoria</label>
            <select id="c_categoria">
              <option>Electrónica</option><option>Áudio</option><option>Computadores</option>
              <option>Televisores</option><option>Acessórios</option><option>Outro</option>
            </select>
          </div>
          <div class="form-group"><label>Observações</label><input id="c_obs" placeholder="Notas adicionais..." /></div>
        </div>
        <div class="form-actions">
          <button class="btn btn-outline" onclick="clearForm('c_')">Limpar</button>
          <button class="btn btn-gold" onclick="addCompra()">💾 Guardar Compra</button>
        </div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">📋 Registo de Compras</span>
        <input id="searchCompras" placeholder="🔍 Pesquisar..." oninput="renderCompras()" style="background:var(--dark2);border:1px solid var(--border);border-radius:6px;padding:7px 10px;color:var(--text);outline:none;font-size:13px;width:200px;" />
      </div>
      <div class="panel-body">
        <div class="totals-bar" id="comprasTotals"></div>
        <div class="table-wrap"><table>
          <thead><tr><th>#</th><th>Produto</th><th>Qtd</th><th>Valor Unit.</th><th>Total</th><th>Nº Nota</th><th>Fornecedor</th><th>Categoria</th><th>Data</th><th>Acções</th></tr></thead>
          <tbody id="comprasBody"></tbody>
        </table></div>
      </div>
    </div>
  </div>

  <!-- VENDAS -->
  <div class="section" id="sec-vendas">
    <div class="panel">
      <div class="panel-header"><span class="panel-title">➕ Registar Venda</span></div>
      <div class="panel-body">
        <div class="form-grid">
          <div class="form-group">
            <label>Produto Vendido</label>
            <input id="v_produto" placeholder="Nome do produto vendido" list="produtosList" />
            <datalist id="produtosList"></datalist>
          </div>
          <div class="form-group"><label>Quantidade Vendida</label><input id="v_qtd" type="number" min="1" value="1" oninput="calcLucroPreview()" /></div>
          <div class="form-group"><label>Valor de Venda (MZN)</label><input id="v_valor" type="number" placeholder="0" oninput="calcLucroPreview()" /></div>
          <div class="form-group"><label>Custo Unitário (MZN)</label><input id="v_custo" type="number" placeholder="0" oninput="calcLucroPreview()" /></div>
          <div class="form-group"><label>Lucro Estimado</label><input id="v_lucro_prev" readonly placeholder="Calculado auto..." /></div>
          <div class="form-group"><label>Cliente</label><input id="v_cliente" placeholder="Nome do cliente (opcional)" /></div>
          <div class="form-group"><label>Data da Venda</label><input id="v_data" type="date" /></div>
          <div class="form-group"><label>Forma de Pagamento</label>
            <select id="v_pagamento">
              <option>Dinheiro</option><option>Transferência M-Pesa</option>
              <option>Transferência Emola</option><option>Transferência Banco</option><option>Crédito</option>
            </select>
          </div>
          <div class="form-group"><label>Observações</label><input id="v_obs" placeholder="Notas adicionais..." /></div>
        </div>
        <div class="form-actions">
          <button class="btn btn-outline" onclick="clearForm('v_')">Limpar</button>
          <button class="btn btn-green" onclick="addVenda()">💰 Registar Venda</button>
        </div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">📋 Registo de Vendas</span>
        <input id="searchVendas" placeholder="🔍 Pesquisar..." oninput="renderVendas()" style="background:var(--dark2);border:1px solid var(--border);border-radius:6px;padding:7px 10px;color:var(--text);outline:none;font-size:13px;width:200px;" />
      </div>
      <div class="panel-body">
        <div class="totals-bar" id="vendasTotals"></div>
        <div class="table-wrap"><table>
          <thead><tr><th>#</th><th>Produto</th><th>Qtd</th><th>Venda Unit.</th><th>Total Venda</th><th>Custo</th><th>Lucro</th><th>Cliente</th><th>Pagamento</th><th>Data</th><th>Acções</th></tr></thead>
          <tbody id="vendasBody"></tbody>
        </table></div>
      </div>
    </div>
  </div>

  <!-- ESTOQUE -->
  <div class="section" id="sec-estoque">
    <div class="kpi-grid" id="estoqueKpis"></div>
    <div class="panel">
      <div class="panel-header"><span class="panel-title">📦 Stock Actual</span></div>
      <div class="panel-body">
        <div class="table-wrap"><table>
          <thead><tr><th>Produto</th><th>Categoria</th><th>Qtd Comprada</th><th>Qtd Vendida</th><th>Stock</th><th>Custo Méd.</th><th>Valor Total</th><th>Estado</th></tr></thead>
          <tbody id="estoqueBody"></tbody>
        </table></div>
      </div>
    </div>
  </div>

  <!-- MICROCRÉDITO -->
  <div class="section" id="sec-credito">
    <div class="nota-box">
      <div style="font-size:20px">⚠️</div>
      <div class="text">
        <strong>NOTA IMPORTANTE — FIM DA CONFIANÇA</strong>
        Nunca mais entregar parte algum produto ou mesmo enviar um valor por época nenhuma a qualquer pessoa sem registo completo neste sistema.
      </div>
    </div>
    <div class="panel">
      <div class="panel-header"><span class="panel-title">➕ Novo Microcrédito</span></div>
      <div class="panel-body">
        <div class="form-grid">
          <div class="form-group"><label>Nome do Cliente</label><input id="mc_nome" placeholder="Nome completo" /></div>
          <div class="form-group"><label>Contacto</label><input id="mc_contacto" placeholder="Telemóvel / WhatsApp" /></div>
          <div class="form-group"><label>Valor do Crédito (MZN)</label><input id="mc_valor" type="number" placeholder="0" oninput="calcCredito()" /></div>
          <div class="form-group"><label>Juro (%)</label><input id="mc_juro" type="number" value="0" oninput="calcCredito()" /></div>
          <div class="form-group"><label>Total a Pagar (MZN)</label><input id="mc_total_prev" readonly style="color:var(--gold)" /></div>
          <div class="form-group"><label>Tipo</label>
            <select id="mc_tipo"><option>Dinheiro</option><option>Produto</option><option>Serviço</option></select>
          </div>
          <div class="form-group"><label>Data do Empréstimo</label><input id="mc_data_emp" type="date" /></div>
          <div class="form-group"><label>Data de Vencimento</label><input id="mc_data_venc" type="date" /></div>
          <div class="form-group"><label>Produto / Descrição</label><input id="mc_descricao" placeholder="O que foi emprestado..." /></div>
          <div class="form-group"><label>Garantia</label><input id="mc_garantia" placeholder="Garantia apresentada..." /></div>
        </div>
        <div class="form-actions">
          <button class="btn btn-outline" onclick="clearForm('mc_')">Limpar</button>
          <button class="btn btn-gold" onclick="addCredito()">🏦 Registar Crédito</button>
        </div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-header"><span class="panel-title">📋 Créditos Registados</span></div>
      <div class="panel-body" id="creditoList"></div>
    </div>
  </div>

  <!-- RELATÓRIO -->
  <div class="section" id="sec-relatorio">
    <div class="panel">
      <div class="panel-header"><span class="panel-title">📅 Filtrar Período</span></div>
      <div class="panel-body">
        <div class="form-grid" style="grid-template-columns:repeat(auto-fit,minmax(150px,1fr))">
          <div class="form-group"><label>Data Início</label><input id="r_inicio" type="date" /></div>
          <div class="form-group"><label>Data Fim</label><input id="r_fim" type="date" /></div>
          <div class="form-group" style="justify-content:flex-end;padding-top:22px">
            <button class="btn btn-gold" onclick="gerarRelatorio()">📊 Gerar</button>
          </div>
        </div>
      </div>
    </div>
    <div id="relatorioContent"></div>
  </div>

</main>

<!-- MODAL -->
<div class="modal-overlay" id="modalOverlay">
  <div class="modal">
    <h3>⚠️ Confirmar Eliminação</h3>
    <p>Tem a certeza? Esta acção remove o registo da nuvem permanentemente.</p>
    <div class="modal-actions">
      <button class="btn btn-outline" onclick="closeModal()">Cancelar</button>
      <button class="btn btn-red" onclick="confirmDelete()">Eliminar</button>
    </div>
  </div>
</div>

<div id="toast"></div>

<!-- FIREBASE -->
<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
  import { getDatabase, ref, push, onValue, remove, update }
    from "https://www.gstatic.com/firebasejs/10.12.0/firebase-database.js";

  const firebaseConfig = {
    apiKey: "AIzaSyDLr1-CnkKBIdrR5gduvN-K_El4OCxrec0",
    authDomain: "corre-holding.firebaseapp.com",
    databaseURL: "https://corre-holding-default-rtdb.firebaseio.com",
    projectId: "corre-holding",
    storageBucket: "corre-holding.appspot.com",
    messagingSenderId: "000000000000",
    appId: "1:000000000000:web:000000000000000000"
  };

  const app = initializeApp(firebaseConfig);
  const db  = getDatabase(app);

  // ── STATE ──
  let compras  = {};
  let vendas   = {};
  let creditos = {};

  // ── UTILS ──
  const fmt = n => Number(n||0).toLocaleString('pt-MZ',{minimumFractionDigits:2,maximumFractionDigits:2});
  const today = () => new Date().toISOString().split('T')[0];

  function toast(msg) {
    const t = document.getElementById('toast');
    t.textContent = msg; t.classList.add('show');
    setTimeout(()=>t.classList.remove('show'),2800);
  }

  function setSyncStatus(status) {
    const badge = document.getElementById('syncBadge');
    const dot   = document.getElementById('syncDot');
    const text  = document.getElementById('syncText');
    badge.className = 'sync-badge ' + status;
    dot.className   = 'sync-dot' + (status==='syncing' ? ' pulse' : '');
    text.textContent = status==='online' ? '🔄 Sincronizado' : status==='syncing' ? 'A sincronizar...' : '⚠️ Sem ligação';
  }

  // ── FIREBASE LISTENERS ──
  function initListeners() {
    onValue(ref(db,'compras'), snap => {
      compras = snap.val() || {};
      renderCompras(); renderDashboard(); renderEstoque(); updateProdutosList();
      setSyncStatus('online');
      hideLoading();
    }, () => { setSyncStatus('offline'); hideLoading(); });

    onValue(ref(db,'vendas'), snap => {
      vendas = snap.val() || {};
      renderVendas(); renderDashboard(); renderEstoque();
    });

    onValue(ref(db,'creditos'), snap => {
      creditos = snap.val() || {};
      renderCreditos(); renderDashboard();
    });
  }

  function hideLoading() {
    setTimeout(()=>{
      const el = document.getElementById('loadingOverlay');
      el.classList.add('hidden');
      setTimeout(()=>el.style.display='none', 500);
    }, 800);
  }

  // ── COMPRAS ──
  window.addCompra = async () => {
    const produto    = document.getElementById('c_produto').value.trim();
    const qtd        = parseInt(document.getElementById('c_qtd').value)||1;
    const valor      = parseFloat(document.getElementById('c_valor').value)||0;
    const nota       = document.getElementById('c_nota').value.trim();
    const data       = document.getElementById('c_data').value || today();
    const fornecedor = document.getElementById('c_fornecedor').value.trim();
    const categoria  = document.getElementById('c_categoria').value;
    const obs        = document.getElementById('c_obs').value.trim();
    if(!produto) { toast('⚠️ Insira o nome do produto'); return; }
    if(!valor)   { toast('⚠️ Insira o valor de compra'); return; }
    setSyncStatus('syncing');
    await push(ref(db,'compras'), {produto,qtd,valor,nota,data,fornecedor,categoria,obs});
    clearForm('c_');
    toast('✅ Compra guardada na nuvem!');
  };

  window.deleteCompra = id => openModal(async()=>{
    setSyncStatus('syncing');
    await remove(ref(db,`compras/${id}`));
    toast('🗑 Compra eliminada.');
  });

  function renderCompras() {
    const q = (document.getElementById('searchCompras')?.value||'').toLowerCase();
    const list = Object.entries(compras)
      .map(([id,c])=>({id,...c}))
      .filter(c=>c.produto?.toLowerCase().includes(q)||(c.fornecedor||'').toLowerCase().includes(q))
      .sort((a,b)=>b.data?.localeCompare(a.data));

    const total = list.reduce((s,c)=>s+(c.valor||0)*(c.qtd||1),0);
    document.getElementById('comprasTotals').innerHTML =
      `<span>Registos: <strong>${list.length}</strong></span>
       <span>Total Investido: <strong style="color:var(--red)">MZN ${fmt(total)}</strong></span>`;

    const tbody = document.getElementById('comprasBody');
    if(!list.length) { tbody.innerHTML=`<tr><td colspan="10"><div class="empty"><div class="empty-icon">📦</div><p>Nenhuma compra.</p></div></td></tr>`; return; }
    tbody.innerHTML = list.map((c,i)=>`
      <tr>
        <td class="mono" style="color:var(--text-dim)">${i+1}</td>
        <td><strong>${c.produto}</strong></td>
        <td class="mono">${c.qtd||1}</td>
        <td class="mono">MZN ${fmt(c.valor)}</td>
        <td class="mono" style="color:var(--red)">MZN ${fmt((c.valor||0)*(c.qtd||1))}</td>
        <td class="mono" style="color:var(--text-dim)">${c.nota||'—'}</td>
        <td>${c.fornecedor||'—'}</td>
        <td><span class="badge badge-blue">${c.categoria||'—'}</span></td>
        <td class="mono">${c.data||'—'}</td>
        <td><button class="btn btn-sm btn-red" onclick="deleteCompra('${c.id}')">🗑</button></td>
      </tr>`).join('');
  }

  // ── VENDAS ──
  window.calcLucroPreview = () => {
    const v = parseFloat(document.getElementById('v_valor').value)||0;
    const c = parseFloat(document.getElementById('v_custo').value)||0;
    const q = parseInt(document.getElementById('v_qtd').value)||1;
    const lucro = (v-c)*q;
    const el = document.getElementById('v_lucro_prev');
    el.value = `MZN ${fmt(lucro)}`;
    el.style.color = lucro>=0?'var(--green)':'var(--red)';
  };

  window.addVenda = async () => {
    const produto   = document.getElementById('v_produto').value.trim();
    const qtd       = parseInt(document.getElementById('v_qtd').value)||1;
    const valor     = parseFloat(document.getElementById('v_valor').value)||0;
    const custo     = parseFloat(document.getElementById('v_custo').value)||0;
    const cliente   = document.getElementById('v_cliente').value.trim();
    const data      = document.getElementById('v_data').value || today();
    const pagamento = document.getElementById('v_pagamento').value;
    const obs       = document.getElementById('v_obs').value.trim();
    const lucro     = (valor-custo)*qtd;
    if(!produto) { toast('⚠️ Insira o nome do produto'); return; }
    if(!valor)   { toast('⚠️ Insira o valor de venda'); return; }
    setSyncStatus('syncing');
    await push(ref(db,'vendas'), {produto,qtd,valor,custo,lucro,cliente,data,pagamento,obs});
    clearForm('v_');
    toast('✅ Venda guardada na nuvem!');
  };

  window.deleteVenda = id => openModal(async()=>{
    setSyncStatus('syncing');
    await remove(ref(db,`vendas/${id}`));
    toast('🗑 Venda eliminada.');
  });

  function renderVendas() {
    const q = (document.getElementById('searchVendas')?.value||'').toLowerCase();
    const list = Object.entries(vendas)
      .map(([id,v])=>({id,...v}))
      .filter(v=>v.produto?.toLowerCase().includes(q)||(v.cliente||'').toLowerCase().includes(q))
      .sort((a,b)=>b.data?.localeCompare(a.data));

    const tVenda = list.reduce((s,v)=>s+(v.valor||0)*(v.qtd||1),0);
    const tLucro = list.reduce((s,v)=>s+(v.lucro||0),0);
    document.getElementById('vendasTotals').innerHTML =
      `<span>Registos: <strong>${list.length}</strong></span>
       <span>Receita: <strong style="color:var(--blue)">MZN ${fmt(tVenda)}</strong></span>
       <span>Lucro: <strong style="color:${tLucro>=0?'var(--green)':'var(--red)'}">MZN ${fmt(tLucro)}</strong></span>`;

    const tbody = document.getElementById('vendasBody');
    if(!list.length) { tbody.innerHTML=`<tr><td colspan="11"><div class="empty"><div class="empty-icon">💰</div><p>Nenhuma venda.</p></div></td></tr>`; return; }
    tbody.innerHTML = list.map((v,i)=>`
      <tr>
        <td class="mono" style="color:var(--text-dim)">${i+1}</td>
        <td><strong>${v.produto}</strong></td>
        <td class="mono">${v.qtd||1}</td>
        <td class="mono" style="color:var(--blue)">MZN ${fmt(v.valor)}</td>
        <td class="mono" style="color:var(--blue)">MZN ${fmt((v.valor||0)*(v.qtd||1))}</td>
        <td class="mono" style="color:var(--text-dim)">MZN ${fmt(v.custo)}</td>
        <td class="mono ${(v.lucro||0)>=0?'profit-pos':'profit-neg'}">MZN ${fmt(v.lucro)}</td>
        <td>${v.cliente||'—'}</td>
        <td><span class="badge badge-gold">${v.pagamento||'—'}</span></td>
        <td class="mono">${v.data||'—'}</td>
        <td><button class="btn btn-sm btn-red" onclick="deleteVenda('${v.id}')">🗑</button></td>
      </tr>`).join('');
  }

  function updateProdutosList() {
    const dl = document.getElementById('produtosList');
    if(!dl) return;
    const nomes = [...new Set(Object.values(compras).map(c=>c.produto))];
    dl.innerHTML = nomes.map(n=>`<option value="${n}">`).join('');
  }

  // ── ESTOQUE ──
  function buildEstoque() {
    const map = {};
    Object.values(compras).forEach(c=>{
      const k = (c.produto||'').toLowerCase().trim();
      if(!map[k]) map[k]={produto:c.produto,categoria:c.categoria,qtdC:0,qtdV:0,totalCusto:0};
      map[k].qtdC += (c.qtd||1);
      map[k].totalCusto += (c.valor||0)*(c.qtd||1);
    });
    Object.values(vendas).forEach(v=>{
      const k=(v.produto||'').toLowerCase().trim();
      if(!map[k]) map[k]={produto:v.produto,categoria:'—',qtdC:0,qtdV:0,totalCusto:0};
      map[k].qtdV += (v.qtd||1);
    });
    return Object.values(map).map(m=>({
      ...m,
      custoMed: m.qtdC>0 ? m.totalCusto/m.qtdC : 0,
      stock: m.qtdC - m.qtdV
    }));
  }

  function renderEstoque() {
    const est = buildEstoque();
    const tValor = est.reduce((s,e)=>s+Math.max(0,e.stock)*e.custoMed,0);
    const tItens = est.reduce((s,e)=>s+Math.max(0,e.stock),0);
    document.getElementById('estoqueKpis').innerHTML=`
      <div class="kpi-card blue"><div class="kpi-label">Produtos Únicos</div><div class="kpi-value">${est.length}</div></div>
      <div class="kpi-card gold"><div class="kpi-label">Unidades em Stock</div><div class="kpi-value">${tItens}</div></div>
      <div class="kpi-card green"><div class="kpi-label">Valor Total Stock</div><div class="kpi-value" style="font-size:18px">MZN ${fmt(tValor)}</div></div>`;
    const tbody=document.getElementById('estoqueBody');
    if(!est.length){tbody.innerHTML=`<tr><td colspan="8"><div class="empty"><div class="empty-icon">📦</div><p>Sem dados.</p></div></td></tr>`;return;}
    tbody.innerHTML=est.map(e=>{
      const estado=e.stock<=0?`<span class="badge badge-red">Esgotado</span>`:
                   e.stock<=2?`<span class="badge badge-gold">Baixo</span>`:
                   `<span class="badge badge-green">OK</span>`;
      return`<tr>
        <td><strong>${e.produto}</strong></td>
        <td><span class="badge badge-blue">${e.categoria||'—'}</span></td>
        <td class="mono">${e.qtdC}</td><td class="mono">${e.qtdV}</td>
        <td class="mono" style="color:var(--gold);font-weight:600">${e.stock}</td>
        <td class="mono">MZN ${fmt(e.custoMed)}</td>
        <td class="mono">MZN ${fmt(Math.max(0,e.stock)*e.custoMed)}</td>
        <td>${estado}</td></tr>`;
    }).join('');
  }

  // ── MICROCRÉDITO ──
  window.calcCredito = () => {
    const v=parseFloat(document.getElementById('mc_valor').value)||0;
    const j=parseFloat(document.getElementById('mc_juro').value)||0;
    document.getElementById('mc_total_prev').value=`MZN ${fmt(v*(1+j/100))}`;
  };

  window.addCredito = async () => {
    const nome=document.getElementById('mc_nome').value.trim();
    const contacto=document.getElementById('mc_contacto').value.trim();
    const valor=parseFloat(document.getElementById('mc_valor').value)||0;
    const juro=parseFloat(document.getElementById('mc_juro').value)||0;
    const tipo=document.getElementById('mc_tipo').value;
    const dataEmp=document.getElementById('mc_data_emp').value||today();
    const dataVenc=document.getElementById('mc_data_venc').value;
    const descricao=document.getElementById('mc_descricao').value.trim();
    const garantia=document.getElementById('mc_garantia').value.trim();
    const total=valor*(1+juro/100);
    if(!nome){toast('⚠️ Insira o nome do cliente');return;}
    if(!valor){toast('⚠️ Insira o valor do crédito');return;}
    setSyncStatus('syncing');
    await push(ref(db,'creditos'),{nome,contacto,valor,juro,total,tipo,dataEmp,dataVenc,descricao,garantia,pago:0,status:'ativo'});
    clearForm('mc_');
    toast('✅ Crédito guardado na nuvem!');
  };

  window.pagarCredito = async (id, parcial) => {
    const c=creditos[id]; if(!c) return;
    const restante=c.total-c.pago;
    let pagamento=restante;
    if(parcial){
      const input=prompt(`Pagamento parcial — ${c.nome}\nRestante: MZN ${fmt(restante)}\nValor a pagar:`);
      if(!input) return;
      pagamento=Math.min(parseFloat(input)||0, restante);
    }
    const novoPago=Math.min(c.total,(c.pago||0)+pagamento);
    const novoStatus=novoPago>=c.total?'pago':'ativo';
    setSyncStatus('syncing');
    await update(ref(db,`creditos/${id}`),{pago:novoPago,status:novoStatus});
    toast(`✅ Pagamento de MZN ${fmt(pagamento)} registado!`);
  };

  window.deleteCredito = id => openModal(async()=>{
    await remove(ref(db,`creditos/${id}`));
    toast('🗑 Crédito eliminado.');
  });

  function renderCreditos() {
    const container=document.getElementById('creditoList');
    const list=Object.entries(creditos).map(([id,c])=>({id,...c}));
    if(!list.length){container.innerHTML=`<div class="empty"><div class="empty-icon">🏦</div><p>Nenhum crédito.</p></div>`;return;}
    container.innerHTML=list.sort((a,b)=>a.status?.localeCompare(b.status)).map(c=>{
      const pct=Math.min(100,((c.pago||0)/c.total)*100);
      const restante=c.total-(c.pago||0);
      const vencido=c.dataVenc&&c.dataVenc<today()&&c.status!=='pago';
      const barColor=c.status==='pago'?'var(--green)':vencido?'var(--red)':'var(--gold)';
      const badge=c.status==='pago'?`<span class="badge badge-green">✅ Pago</span>`:
                  vencido?`<span class="badge badge-red">⚠️ Vencido</span>`:
                  `<span class="badge badge-gold">⏳ Activo</span>`;
      return`<div class="credit-card">
        <div class="credit-header">
          <div>
            <div class="credit-name">${c.nome}</div>
            <div style="font-size:12px;color:var(--text-dim);margin-top:3px">${c.contacto||''} • ${c.tipo}</div>
          </div>
          <div style="text-align:right">${badge}<div style="font-family:'DM Mono',monospace;font-size:22px;margin-top:6px">MZN ${fmt(c.total)}</div></div>
        </div>
        <div class="credit-bar-wrap"><div class="credit-bar" style="width:${pct}%;background:${barColor}"></div></div>
        <div style="display:flex;justify-content:space-between;font-size:11px;color:var(--text-dim);margin-bottom:10px">
          <span>Pago: MZN ${fmt(c.pago||0)}</span><span>${pct.toFixed(0)}%</span>
          <span>Restante: <strong style="color:${vencido?'var(--red)':'var(--text)'}">MZN ${fmt(restante)}</strong></span>
        </div>
        <div class="credit-details">
          <div class="credit-detail-item"><div class="lbl">Capital</div><div class="val">MZN ${fmt(c.valor)}</div></div>
          <div class="credit-detail-item"><div class="lbl">Juro</div><div class="val" style="color:var(--gold)">${c.juro||0}%</div></div>
          <div class="credit-detail-item"><div class="lbl">Empréstimo</div><div class="val" style="font-size:13px">${c.dataEmp||'—'}</div></div>
          <div class="credit-detail-item"><div class="lbl">Vencimento</div><div class="val" style="font-size:13px;color:${vencido?'var(--red)':'var(--text)'}">${c.dataVenc||'—'}</div></div>
          <div class="credit-detail-item"><div class="lbl">Descrição</div><div class="val" style="font-size:13px">${c.descricao||'—'}</div></div>
          <div class="credit-detail-item"><div class="lbl">Garantia</div><div class="val" style="font-size:13px">${c.garantia||'—'}</div></div>
        </div>
        ${c.status!=='pago'?`<div style="display:flex;gap:8px;margin-top:14px">
          <button class="btn btn-green btn-sm" onclick="pagarCredito('${c.id}',false)">✅ Pago Total</button>
          <button class="btn btn-blue btn-sm" onclick="pagarCredito('${c.id}',true)">💳 Parcial</button>
          <button class="btn btn-red btn-sm" onclick="deleteCredito('${c.id}')">🗑</button>
        </div>`:`<div style="margin-top:10px"><button class="btn btn-outline btn-sm" onclick="deleteCredito('${c.id}')">🗑 Remover</button></div>`}
      </div>`;
    }).join('');
  }

  // ── DASHBOARD ──
  function renderDashboard() {
    const tC=Object.values(compras).reduce((s,c)=>s+(c.valor||0)*(c.qtd||1),0);
    const tV=Object.values(vendas).reduce((s,v)=>s+(v.valor||0)*(v.qtd||1),0);
    const tL=Object.values(vendas).reduce((s,v)=>s+(v.lucro||0),0);
    const tCred=Object.values(creditos).filter(c=>c.status!=='pago').reduce((s,c)=>s+c.total-(c.pago||0),0);
    const est=buildEstoque();
    const tEstq=est.reduce((s,e)=>s+Math.max(0,e.stock)*e.custoMed,0);
    document.getElementById('kpiGrid').innerHTML=`
      <div class="kpi-card gold"><div class="kpi-label">Capital Investido</div><div class="kpi-value" style="font-size:18px">MZN ${fmt(tC)}</div><div class="kpi-sub">${Object.keys(compras).length} compras</div></div>
      <div class="kpi-card blue"><div class="kpi-label">Receita Total</div><div class="kpi-value" style="font-size:18px">MZN ${fmt(tV)}</div><div class="kpi-sub">${Object.keys(vendas).length} vendas</div></div>
      <div class="kpi-card green"><div class="kpi-label">Lucro Total</div><div class="kpi-value" style="font-size:18px">MZN ${fmt(tL)}</div></div>
      <div class="kpi-card red"><div class="kpi-label">Crédito em Aberto</div><div class="kpi-value" style="font-size:18px">MZN ${fmt(tCred)}</div><div class="kpi-sub">${Object.values(creditos).filter(c=>c.status!=='pago').length} devedores</div></div>
      <div class="kpi-card purple"><div class="kpi-label">Valor em Estoque</div><div class="kpi-value" style="font-size:18px">MZN ${fmt(tEstq)}</div></div>`;

    const rv=[...Object.values(vendas)].sort((a,b)=>b.data?.localeCompare(a.data)).slice(0,5);
    document.getElementById('dashVendasBody').innerHTML=rv.length?rv.map(v=>`
      <tr><td><strong>${v.produto}</strong></td>
      <td class="mono" style="color:var(--blue)">MZN ${fmt(v.valor)}</td>
      <td class="mono ${(v.lucro||0)>=0?'profit-pos':'profit-neg'}">MZN ${fmt(v.lucro)}</td>
      <td class="mono" style="color:var(--text-dim)">${v.data||'—'}</td></tr>`).join(''):
      `<tr><td colspan="4" style="color:var(--text-dim);text-align:center;padding:20px">Sem vendas</td></tr>`;

    const rc=[...Object.values(compras)].sort((a,b)=>b.data?.localeCompare(a.data)).slice(0,5);
    document.getElementById('dashComprasBody').innerHTML=rc.length?rc.map(c=>`
      <tr><td><strong>${c.produto}</strong></td>
      <td class="mono">${c.qtd||1}</td>
      <td class="mono" style="color:var(--red)">MZN ${fmt((c.valor||0)*(c.qtd||1))}</td>
      <td class="mono" style="color:var(--text-dim)">${c.data||'—'}</td></tr>`).join(''):
      `<tr><td colspan="4" style="color:var(--text-dim);text-align:center;padding:20px">Sem compras</td></tr>`;

    const ativos=Object.entries(creditos).filter(([,c])=>c.status!=='pago');
    document.getElementById('dashCreditPanel').style.display=ativos.length?'block':'none';
    document.getElementById('dashCreditBody').innerHTML=ativos.map(([id,c])=>{
      const vencido=c.dataVenc&&c.dataVenc<today();
      return`<div style="display:flex;justify-content:space-between;align-items:center;padding:10px 0;border-bottom:1px solid var(--border)">
        <div><strong>${c.nome}</strong><span style="color:var(--text-dim);font-size:12px;margin-left:8px">${c.contacto||''}</span>
        ${vencido?`<span class="badge badge-red" style="margin-left:8px">VENCIDO</span>`:''}</div>
        <div style="text-align:right"><div class="mono" style="color:var(--red)">MZN ${fmt(c.total-(c.pago||0))}</div>
        <div style="font-size:11px;color:var(--text-dim)">venc: ${c.dataVenc||'—'}</div></div></div>`;
    }).join('');
  }

  // ── RELATÓRIO ──
  window.gerarRelatorio = () => {
    const ini=document.getElementById('r_inicio').value;
    const fim=document.getElementById('r_fim').value;
    let vc=Object.values(compras), vv=Object.values(vendas);
    if(ini){vc=vc.filter(c=>c.data>=ini);vv=vv.filter(v=>v.data>=ini);}
    if(fim){vc=vc.filter(c=>c.data<=fim);vv=vv.filter(v=>v.data<=fim);}
    const tC=vc.reduce((s,c)=>s+(c.valor||0)*(c.qtd||1),0);
    const tV=vv.reduce((s,v)=>s+(v.valor||0)*(v.qtd||1),0);
    const tL=vv.reduce((s,v)=>s+(v.lucro||0),0);
    const periodo=ini||fim?`${ini||'início'} → ${fim||'hoje'}`:'Todo o período';
    document.getElementById('relatorioContent').innerHTML=`
      <div class="kpi-grid">
        <div class="kpi-card gold"><div class="kpi-label">Período</div><div style="font-size:13px;color:var(--gold);margin-top:6px">${periodo}</div></div>
        <div class="kpi-card red"><div class="kpi-label">Total Compras</div><div class="kpi-value" style="font-size:18px">MZN ${fmt(tC)}</div><div class="kpi-sub">${vc.length} registos</div></div>
        <div class="kpi-card blue"><div class="kpi-label">Total Vendas</div><div class="kpi-value" style="font-size:18px">MZN ${fmt(tV)}</div><div class="kpi-sub">${vv.length} registos</div></div>
        <div class="kpi-card green"><div class="kpi-label">Lucro Líquido</div><div class="kpi-value" style="font-size:18px">MZN ${fmt(tL)}</div></div>
      </div>
      <div class="two-col">
        <div class="panel"><div class="panel-header"><span class="panel-title">🛒 Compras</span></div><div class="panel-body"><div class="table-wrap"><table>
          <thead><tr><th>Produto</th><th>Qtd</th><th>Total</th><th>Data</th></tr></thead>
          <tbody>${vc.sort((a,b)=>b.data?.localeCompare(a.data)).map(c=>`<tr><td>${c.produto}</td><td class="mono">${c.qtd||1}</td><td class="mono" style="color:var(--red)">MZN ${fmt((c.valor||0)*(c.qtd||1))}</td><td class="mono">${c.data||'—'}</td></tr>`).join('')||`<tr><td colspan="4" style="color:var(--text-dim);text-align:center;padding:20px">Sem dados</td></tr>`}</tbody>
        </table></div></div></div>
        <div class="panel"><div class="panel-header"><span class="panel-title">💰 Vendas</span></div><div class="panel-body"><div class="table-wrap"><table>
          <thead><tr><th>Produto</th><th>Venda</th><th>Lucro</th><th>Data</th></tr></thead>
          <tbody>${vv.sort((a,b)=>b.data?.localeCompare(a.data)).map(v=>`<tr><td>${v.produto}</td><td class="mono" style="color:var(--blue)">MZN ${fmt(v.valor)}</td><td class="mono ${(v.lucro||0)>=0?'profit-pos':'profit-neg'}">MZN ${fmt(v.lucro)}</td><td class="mono">${v.data||'—'}</td></tr>`).join('')||`<tr><td colspan="4" style="color:var(--text-dim);text-align:center;padding:20px">Sem dados</td></tr>`}</tbody>
        </table></div></div></div>
      </div>`;
  };

  // ── NAV ──
  window.switchTab = (tab, btn) => {
    document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
    document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active'));
    document.getElementById('sec-'+tab).classList.add('active');
    btn.classList.add('active');
    if(tab==='estoque') renderEstoque();
    if(tab==='relatorio') gerarRelatorio();
  };

  window.clearForm = prefix => {
    document.querySelectorAll(`[id^="${prefix}"]`).forEach(el=>{
      if(el.tagName==='SELECT') el.selectedIndex=0;
      else el.value='';
    });
    if(prefix==='c_') document.getElementById('c_data').value=today();
    if(prefix==='v_') document.getElementById('v_data').value=today();
    if(prefix==='mc_') document.getElementById('mc_data_emp').value=today();
  };

  // ── MODAL ──
  let pendingDelete=null;
  window.openModal = fn => { pendingDelete=fn; document.getElementById('modalOverlay').classList.add('open'); };
  window.closeModal = () => { pendingDelete=null; document.getElementById('modalOverlay').classList.remove('open'); };
  window.confirmDelete = () => { if(pendingDelete) pendingDelete(); closeModal(); };

  // ── INIT ──
  document.getElementById('c_data').value=today();
  document.getElementById('v_data').value=today();
  document.getElementById('mc_data_emp').value=today();
  initListeners();
</script>
</body>
</html>
