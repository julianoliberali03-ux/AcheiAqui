<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AcheiAqui - Protótipo Verde</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">
  <style>
    :root {
      --green:#10b981;       /* verde claro do topo */
      --green-dark:#059669;  /* verde escuro (hover/botões) */
      --bg:#ecfdf5;          /* fundo suave */
      --card:#fff;
      --muted:#6b7280;
      --shadow:0 4px 12px rgba(0,0,0,0.08);
      --ink:#0f172a;         /* texto escuro */
    }
    * { box-sizing:border-box; margin:0; padding:0; }
    body { font-family:Inter, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; background:var(--bg); color:var(--ink); }
    header { background:var(--green); color:#fff; padding:12px 20px; display:flex; align-items:center; justify-content:space-between; position:sticky; top:0; z-index:50; }
    .brand { display:flex; align-items:center; gap:12px; }
    .brand svg { display:block; height:36px; width:auto; }
    nav { display:flex; gap:8px; }
    .btn { background:#e5e7eb; border:none; border-radius:9999px; padding:8px 14px; font-weight:700; cursor:pointer; }
    .btn:hover{ background:#d1d5db; }
    .btn.active { background:var(--green-dark); color:#fff; }
    main { display:grid; grid-template-columns:260px 1fr; gap:16px; max-width:1200px; margin:20px auto; padding:0 12px; }
    aside { background:var(--card); border-radius:16px; box-shadow:var(--shadow); padding:12px; display:flex; flex-direction:column; gap:8px; height:fit-content; }
    aside h3 { font-size:14px; font-weight:800; margin-bottom:4px; color:var(--ink); }
    aside button{ background:#f3f4f6; border:none; padding:8px 12px; text-align:left; border-radius:8px; cursor:pointer; font-size:14px; }
    aside button:hover{ background:#e5e7eb; }
    aside button.active { background:var(--green); color:#fff; }
    section { display:flex; flex-direction:column; gap:16px; }
    .banner { background:#d1fae5; border-radius:16px; height:120px; display:grid; place-items:center; font-weight:800; color:#065f46; cursor:pointer; }
    .banner:hover { background:#bbf7d0; }
    .ads-grid{ display:grid; grid-template-columns:repeat(auto-fit, minmax(260px,1fr)); gap:16px; }
    .ad-card{ background:var(--card); border-radius:16px; overflow:hidden; box-shadow:var(--shadow); transition:transform .2s; }
    .ad-card:hover{ transform:translateY(-4px); }
    .ad-img{ width:100%; height:160px; object-fit:cover; background:#eee; }
    .ad-body{ padding:12px; }
    .badge{ display:inline-block; padding:2px 10px; border-radius:9999px; font-size:12px; font-weight:800; }
    .badge.basic{ background:#e5e7eb; color:#111827; }
    .badge.plus{ background:#3b82f6; color:#fff; }
    .badge.turbo{ background:#ef4444; color:#fff; }
    .ad-actions{ display:flex; gap:8px; margin-top:8px; }
    .btn-small{ padding:6px 10px; border-radius:10px; font-size:12px; background:var(--green-dark); color:#fff; border:none; cursor:pointer; font-weight:700; }
    .btn-small:hover { background:#047857; }
    .btn-small.favorited { background:#ef4444; }
    
    /* Modal Styles */
    .modal { display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.5); z-index:1000; }
    .modal.show { display:flex; align-items:center; justify-content:center; }
    .modal-content { background:var(--card); border-radius:16px; max-width:500px; width:90%; max-height:80%; overflow-y:auto; }
    .modal-header { padding:20px; border-bottom:1px solid #e5e7eb; display:flex; justify-content:space-between; align-items:center; }
    .modal-body { padding:20px; }
    .close { background:none; border:none; font-size:24px; cursor:pointer; }
    
    /* Form Styles */
    .form-group { margin-bottom:16px; }
    .form-group label { display:block; margin-bottom:4px; font-weight:600; }
    .form-group input, .form-group select, .form-group textarea { width:100%; padding:8px 12px; border:1px solid #d1d5db; border-radius:8px; font-size:14px; }
    .form-group textarea { min-height:80px; resize:vertical; }
    .btn-primary { background:var(--green); color:#fff; border:none; padding:12px 24px; border-radius:8px; font-weight:700; cursor:pointer; }
    .btn-primary:hover { background:var(--green-dark); }
    
    /* Admin Panel Styles */
    .admin-panel { display:none; }
    .admin-panel.active { display:block; }
    .stats-grid { display:grid; grid-template-columns:repeat(auto-fit, minmax(200px, 1fr)); gap:16px; margin-bottom:20px; }
    .stat-card { background:var(--card); padding:20px; border-radius:16px; box-shadow:var(--shadow); text-align:center; }
    .stat-number { font-size:32px; font-weight:800; color:var(--green); }
    .stat-label { color:var(--muted); margin-top:4px; }
    
    /* Reports Styles */
    .reports-panel { display:none; }
    .reports-panel.active { display:block; }
    .chart-container { background:var(--card); padding:20px; border-radius:16px; box-shadow:var(--shadow); margin-bottom:20px; }
    
    /* Profile Styles */
    .profile-panel { display:none; }
    .profile-panel.active { display:block; }
    
    /* Publish Form Styles */
    .publish-panel { display:none; }
    .publish-panel.active { display:block; }
    .publish-form { background:var(--card); padding:20px; border-radius:16px; box-shadow:var(--shadow); }
    .publish-tab { display:none; }
    .publish-tab.active { display:block; }
    
    /* Search Styles */
    .search-container { margin-bottom:16px; }
    .search-input { width:100%; padding:12px; border:1px solid #d1d5db; border-radius:8px; font-size:16px; }
    
    /* Filters */
    .filters { display:flex; gap:8px; margin-bottom:16px; flex-wrap:wrap; }
    .filter-btn { background:#f3f4f6; border:none; padding:6px 12px; border-radius:20px; cursor:pointer; font-size:12px; }
    .filter-btn.active { background:var(--green); color:#fff; }
    
    /* Hidden content areas */
    .content-area { display:none; }
    .content-area.active { display:block; }
  </style>
</head>
<body>
  <header>
    <div class="brand" aria-label="AcheiAqui">
      <svg width="220" height="52" viewBox="0 0 220 52" fill="none" xmlns="http://www.w3.org/2000/svg" role="img">
        <g transform="translate(8,6)">
          <path d="M18 0C8.611 0 1 7.611 1 17C1 27.5 10.2 34.9 16.3 39.8C17.2 40.5 18.8 40.5 19.7 39.8C25.8 34.9 35 27.5 35 17C35 7.611 27.389 0 18 0Z" fill="#0f172a"/>
          <circle cx="18" cy="17" r="7" fill="#14b8a6"/>
        </g>
        <text x="58" y="35" style="fill:#0f172a; font-family: Inter, Segoe UI, Arial, sans-serif; font-weight:800;" font-size="26">AcheiAqui</text>
      </svg>
    </div>
    <nav>
      <button class="btn active" onclick="showContent('home')">Home</button>
      <button class="btn" onclick="showContent('publish')">Publicar</button>
      <button class="btn" onclick="showContent('profile')">Perfil</button>
      <button class="btn" onclick="showContent('admin')">Admin</button>
      <button class="btn" onclick="showContent('reports')">Relatórios</button>
    </nav>
  </header>
  
  <main>
    <aside>
      <h3>Categorias</h3>
      <button onclick="filterByCategory('all')" class="category-btn active">Todas</button>
      <button onclick="filterByCategory('veiculos')" class="category-btn">Veículos</button>
      <button onclick="filterByCategory('imoveis')" class="category-btn">Imóveis</button>
      <button onclick="filterByCategory('servicos')" class="category-btn">Serviços</button>
      <button onclick="filterByCategory('eletronicos')" class="category-btn">Eletrônicos</button>
      <button onclick="filterByCategory('moda')" class="category-btn">Moda</button>
      <button onclick="filterByCategory('esportes')" class="category-btn">Esportes</button>
      <button onclick="filterByCategory('outros')" class="category-btn">Outros</button>
      <div class="banner" onclick="alert('Banner lateral clicado!')">Banner Lateral</div>
      <div class="banner" onclick="alert('Contrate nossos espaços publicitários!')">Seu anúncio aqui</div>
    </aside>
    
    <!-- HOME CONTENT -->
    <section id="home-content" class="content-area active">
      <div class="banner" onclick="alert('Banner rotativo clicado!')">Banner Topo Rotativo</div>
      
      <div class="search-container">
        <input type="text" class="search-input" placeholder="Buscar anúncios..." oninput="searchAds(this.value)">
      </div>
      
      <div class="filters">
        <button class="filter-btn active" onclick="filterByPrice('all')">Todos os preços</button>
        <button class="filter-btn" onclick="filterByPrice('0-1000')">Até R$ 1.000</button>
        <button class="filter-btn" onclick="filterByPrice('1000-5000')">R$ 1.000 - R$ 5.000</button>
        <button class="filter-btn" onclick="filterByPrice('5000+')">Acima de R$ 5.000</button>
      </div>
      
      <h2 style="font-weight:800; margin-top:4px;">Anúncios</h2>
      <div class="ads-grid" id="ads-container">
        <!-- Os anúncios serão carregados dinamicamente -->
      </div>
      <div class="banner" onclick="alert('Banner rodapé clicado!')">Banner Rodapé</div>
    </section>
    
    <!-- PUBLISH CONTENT -->
    <section id="publish-content" class="content-area">
      <div style="display:flex; gap:8px; margin-bottom:20px;">
        <button class="btn active" onclick="showPublishTab('ads')">Publicar Anúncio</button>
        <button class="btn" onclick="showPublishTab('banner')">Publicar Banner</button>
      </div>
      
      <!-- PUBLISH ADS TAB -->
      <div id="publish-ads-tab" class="publish-tab active">
        <h2 style="font-weight:800;">Publicar Anúncio</h2>
        <div class="publish-form">
          <form onsubmit="publishAd(event)">
            <div class="form-group">
              <label>Seu Email (para confirmações e contato)</label>
              <input type="email" name="email" required placeholder="seu@email.com">
            </div>
            <div class="form-group">
              <label>Título do Anúncio</label>
              <input type="text" name="title" required>
            </div>
            <div class="form-group">
              <label>Categoria</label>
              <select name="category" required>
                <option value="">Selecione uma categoria</option>
                <option value="veiculos">Veículos</option>
                <option value="imoveis">Imóveis</option>
                <option value="servicos">Serviços</option>
                <option value="eletronicos">Eletrônicos</option>
                <option value="moda">Moda</option>
                <option value="esportes">Esportes</option>
                <option value="outros">Outros</option>
              </select>
            </div>
            <div class="form-group">
              <label>Preço (R$)</label>
              <input type="number" name="price" step="0.01" required>
            </div>
            <div class="form-group">
              <label>Localização</label>
              <input type="text" name="location" required>
            </div>
            <div class="form-group">
              <label>Tipo de Anúncio</label>
              <select name="type" required>
                <option value="basic">Básico (Gratuito)</option>
                <option value="plus">Destaque (+R$ 10)</option>
                <option value="turbo">Turbo (+R$ 25)</option>
              </select>
            </div>
            <div class="form-group">
              <label>Descrição</label>
              <textarea name="description" required></textarea>
            </div>
            <div class="form-group">
              <label>URL da Imagem</label>
              <input type="url" name="image">
            </div>
            <button type="submit" class="btn-primary">Publicar Anúncio</button>
          </form>
        </div>
      </div>
      
      <!-- PUBLISH BANNER TAB -->
      <div id="publish-banner-tab" class="publish-tab">
        <h2 style="font-weight:800;">Publicar Banner</h2>
        <div class="publish-form">
          <form onsubmit="publishBanner(event)">
            <div class="form-group">
              <label>Seu Email (para confirmações e contato)</label>
              <input type="email" name="email" required placeholder="seu@email.com">
            </div>
            <div class="form-group">
              <label>Título do Banner</label>
              <input type="text" name="title" required>
            </div>
            <div class="form-group">
              <label>Posição do Banner</label>
              <select name="position" required>
                <option value="">Selecione uma posição</option>
                <option value="topo">Banner Topo Rotativo</option>
                <option value="lateral">Banner Lateral</option>
                <option value="rodape">Banner Rodapé</option>
              </select>
            </div>
            <div class="form-group">
              <label>Valor do Banner (R$)</label>
              <input type="number" name="price" step="0.01" required>
            </div>
            <div class="form-group">
              <label>Empresa/Cliente</label>
              <input type="text" name="company" required>
            </div>
            <div class="form-group">
              <label>Período de Exibição</label>
              <select name="duration" required>
                <option value="">Selecione o período</option>
                <option value="7">7 dias (+R$ 50)</option>
                <option value="15">15 dias (+R$ 90)</option>
                <option value="30">30 dias (+R$ 150)</option>
              </select>
            </div>
            <div class="form-group">
              <label>Texto do Banner</label>
              <textarea name="text" placeholder="Texto que aparecerá no banner" required></textarea>
            </div>
            <div class="form-group">
              <label>URL da Imagem do Banner</label>
              <input type="url" name="image">
            </div>
            <div class="form-group">
              <label>Link de Destino</label>
              <input type="url" name="link" placeholder="Para onde o banner deve levar ao ser clicado">
            </div>
            <button type="submit" class="btn-primary">Publicar Banner</button>
          </form>
        </div>
      </div>
    </section>
    
    <!-- PROFILE CONTENT -->
    <section id="profile-content" class="content-area">
      <h2 style="font-weight:800;">Meu Perfil</h2>
      <div class="publish-form">
        <div class="form-group">
          <label>Nome</label>
          <input type="text" value="Usuário Demo" readonly>
        </div>
        <div class="form-group">
          <label>Email</label>
          <input type="email" value="usuario@demo.com" readonly>
        </div>
        <div class="form-group">
          <label>Anúncios Publicados</label>
          <input type="text" value="12" readonly>
        </div>
        <div class="form-group">
          <label>Favoritos</label>
          <input type="text" value="5" readonly>
        </div>
        <button class="btn-primary">Editar Perfil</button>
      </div>
    </section>
    
    <!-- ADMIN CONTENT -->
    <section id="admin-content" class="content-area">
      <h2 style="font-weight:800;">Painel Administrativo</h2>
      <div class="stats-grid">
        <div class="stat-card">
          <div class="stat-number" id="total-ads">0</div>
          <div class="stat-label">Total de Anúncios</div>
        </div>
        <div class="stat-card">
          <div class="stat-number">1.247</div>
          <div class="stat-label">Usuários Ativos</div>
        </div>
        <div class="stat-card">
          <div class="stat-number">R$ 3.420</div>
          <div class="stat-label">Receita Mensal</div>
        </div>
        <div class="stat-card">
          <div class="stat-number">95%</div>
          <div class="stat-label">Satisfação</div>
        </div>
      </div>
      
      <div class="chart-container">
        <h3>Últimas Atividades</h3>
        <div id="admin-activities">
          <p>• Novo anúncio publicado: "Honda Civic 2020"</p>
          <p>• Usuário cadastrado: joao@email.com</p>
          <p>• Anúncio promovido para Turbo: "Apartamento Centro"</p>
          <p>• Relatório mensal gerado</p>
          <p>• Sistema atualizado com sucesso</p>
        </div>
      </div>
    </section>
    
    <!-- REPORTS CONTENT -->
    <section id="reports-content" class="content-area">
      <h2 style="font-weight:800;">Relatórios</h2>
      <div class="chart-container">
        <h3>Anúncios por Categoria</h3>
        <div id="category-chart">
          <p>Veículos: 45% (234 anúncios)</p>
          <p>Imóveis: 25% (130 anúncios)</p>
          <p>Eletrônicos: 15% (78 anúncios)</p>
          <p>Serviços: 10% (52 anúncios)</p>
          <p>Outros: 5% (26 anúncios)</p>
        </div>
      </div>
      
      <div class="chart-container">
        <h3>Receita por Tipo de Anúncio</h3>
        <div id="revenue-chart">
          <p>Básico: R$ 0 (gratuito)</p>
          <p>Destaque: R$ 1.240 (124 anúncios × R$ 10)</p>
          <p>Turbo: R$ 2.180 (87 anúncios × R$ 25)</p>
          <p><strong>Total: R$ 3.420</strong></p>
        </div>
      </div>
    </section>
  </main>
  
  <!-- Modal for Ad Details -->
  <div id="ad-modal" class="modal">
    <div class="modal-content">
      <div class="modal-header">
        <h3 id="modal-title"></h3>
        <button class="close" onclick="closeModal()">&times;</button>
      </div>
      <div class="modal-body">
        <img id="modal-image" style="width:100%; height:200px; object-fit:cover; border-radius:8px; margin-bottom:16px;">
        <p id="modal-price" style="font-size:18px; font-weight:700; color:var(--green);"></p>
        <p id="modal-location" style="color:var(--muted); margin-bottom:16px;"></p>
        <div id="modal-description"></div>
        <div style="margin-top:16px;">
          <button class="btn-primary">Entrar em Contato</button>
          <button class="btn-primary" style="margin-left:8px;" onclick="reportAd()">Denunciar</button>
        </div>
      </div>
    </div>
  </div>

  <script>
    // Sample ads data
    let ads = [
      {
        id: 1,
        title: "Fiat Uno 2015",
        price: 15000,
        location: "São Paulo/SP",
        category: "veiculos",
        type: "turbo",
        image: "https://picsum.photos/640/360?car",
        description: "Fiat Uno em excelente estado, revisado, documentação em dia.",
        email: "usuario1@email.com",
        status: "published", // published, pending_payment
        favorited: false
      },
      {
        id: 2,
        title: "Apartamento 2 quartos",
        price: 350000,
        location: "RJ",
        category: "imoveis",
        type: "plus",
        image: "https://picsum.photos/640/360?home",
        description: "Apartamento bem localizado, 2 quartos, 1 banheiro, cozinha americana.",
        email: "usuario2@email.com",
        status: "published",
        favorited: false
      },
      {
        id: 3,
        title: "Notebook Acer",
        price: 2500,
        location: "Curitiba/PR",
        category: "eletronicos",
        type: "basic",
        image: "https://picsum.photos/640/360?laptop",
        description: "Notebook Acer i5, 8GB RAM, SSD 256GB, placa de vídeo dedicada.",
        email: "usuario3@email.com",
        status: "published",
        favorited: false
      },
      {
        id: 4,
        title: "iPhone 12",
        price: 2800,
        location: "Campo Bom/RS",
        category: "eletronicos",
        type: "plus",
        image: "https://picsum.photos/640/360?phone",
        description: "iPhone 12 128GB, sem riscos, bateria em perfeito estado.",
        email: "usuario4@email.com",
        status: "published",
        favorited: false
      },
      {
        id: 5,
        title: "Serviços de Pintura",
        price: 50,
        location: "Porto Alegre/RS",
        category: "servicos",
        type: "basic",
        image: "https://picsum.photos/640/360?paint",
        description: "Serviços profissionais de pintura residencial e comercial.",
        email: "usuario5@email.com",
        status: "published",
        favorited: false
      }
    ];

    // Sample banners data
    let banners = [
      {
        id: 1,
        title: "Banner Coca-Cola",
        position: "topo",
        price: 300,
        company: "Coca-Cola Brasil",
        duration: 30,
        text: "Abra a felicidade - Coca-Cola",
        image: "https://picsum.photos/800/120?coca",
        link: "https://www.cocacola.com.br",
        email: "coca@email.com",
        status: "published",
        active: true
      },
      {
        id: 2,
        title: "Banner Loja de Carros",
        position: "lateral",
        price: 150,
        company: "AutoMax Veículos",
        duration: 15,
        text: "Os melhores carros você encontra aqui!",
        image: "https://picsum.photos/240/120?cars",
        link: "https://www.automax.com.br",
        email: "automax@email.com",
        status: "published",
        active: true
      }
    ];

    let currentFilter = 'all';
    let currentPriceFilter = 'all';
    let currentSearch = '';

    // Initialize the page
    document.addEventListener('DOMContentLoaded', function() {
      displayAds();
      updateAdminStats();
    });

    // Navigation functions
    function showContent(section) {
      // Hide all content areas
      document.querySelectorAll('.content-area').forEach(area => {
        area.classList.remove('active');
      });
      
      // Remove active class from all nav buttons
      document.querySelectorAll('nav .btn').forEach(btn => {
        btn.classList.remove('active');
      });
      
      // Show selected content
      document.getElementById(section + '-content').classList.add('active');
      
      // Add active class to clicked button
      event.target.classList.add('active');
      
      // If showing publish, ensure ads tab is shown by default
      if (section === 'publish') {
        showPublishTab('ads');
      }
    }

    // Publish tab navigation
    function showPublishTab(tab) {
      // Hide all publish tabs
      document.querySelectorAll('.publish-tab').forEach(tabElement => {
        tabElement.classList.remove('active');
      });
      
      // Remove active class from tab buttons
      document.querySelectorAll('#publish-content .btn').forEach(btn => {
        btn.classList.remove('active');
      });
      
      // Show selected tab
      document.getElementById(`publish-${tab}-tab`).classList.add('active');
      
      // Add active class to clicked button
      event.target.classList.add('active');
    }

    // Display ads function
    function displayAds() {
      const container = document.getElementById('ads-container');
      
      // Filter ads to show only published ones
      let publishedAds = ads.filter(ad => ad.status === 'published');
      
      let filteredAds = publishedAds.filter(ad => {
        let matchesCategory = currentFilter === 'all' || ad.category === currentFilter;
        let matchesPrice = true;
        let matchesSearch = currentSearch === '' || ad.title.toLowerCase().includes(currentSearch.toLowerCase());
        
        if (currentPriceFilter !== 'all') {
          if (currentPriceFilter === '0-1000') {
            matchesPrice = ad.price <= 1000;
          } else if (currentPriceFilter === '1000-5000') {
            matchesPrice = ad.price > 1000 && ad.price <= 5000;
          } else if (currentPriceFilter === '5000+') {
            matchesPrice = ad.price > 5000;
          }
        }
        
        return matchesCategory && matchesPrice && matchesSearch;
      });
      
      container.innerHTML = filteredAds.map(ad => {
        const typeLabels = { basic: 'Básico', plus: 'Destaque', turbo: 'Turbo' };
        const favoriteIcon = ad.favorited ? '❤️' : '⭐';
        const favoriteClass = ad.favorited ? 'favorited' : '';
        
        return `
          <div class="ad-card">
            <img src="${ad.image}" class="ad-img" alt="${ad.title}">
            <div class="ad-body">
              <span class="badge ${ad.type}">${typeLabels[ad.type]}</span>
              <h4 style="margin-top:6px;">${ad.title}</h4>
              <p style="color:#374151;">R$ ${ad.price.toLocaleString()} — ${ad.location}</p>
              <div class="ad-actions">
                <button class="btn-small" onclick="viewAd(${ad.id})">Ver</button>
                <button class="btn-small ${favoriteClass}" onclick="toggleFavorite(${ad.id})">${favoriteIcon}</button>
              </div>
            </div>
          </div>
        `;
      }).join('');
    }

    // Filter functions
    function filterByCategory(category) {
      currentFilter = category;
      
      // Update active button
      document.querySelectorAll('.category-btn').forEach(btn => {
        btn.classList.remove('active');
      });
      event.target.classList.add('active');
      
      displayAds();
    }

    function filterByPrice(priceRange) {
      currentPriceFilter = priceRange;
      
      // Update active button
      document.querySelectorAll('.filter-btn').forEach(btn => {
        btn.classList.remove('active');
      });
      event.target.classList.add('active');
      
      displayAds();
    }

    function searchAds(query) {
      currentSearch = query;
      displayAds();
    }

    // View ad details
    function viewAd(id) {
      const ad = ads.find(a => a.id === id);
      if (!ad) return;
      
      document.getElementById('modal-title').textContent = ad.title;
      document.getElementById('modal-image').src = ad.image;
      document.getElementById('modal-price').textContent = `R$ ${ad.price.toLocaleString()}`;
      document.getElementById('modal-location').textContent = ad.location;
      document.getElementById('modal-description').textContent = ad.description;
      
      document.getElementById('ad-modal').classList.add('show');
    }

    function closeModal() {
      document.getElementById('ad-modal').classList.remove('show');
    }

    // Toggle favorite
    function toggleFavorite(id) {
      const ad = ads.find(a => a.id === id);
      if (ad) {
        ad.favorited = !ad.favorited;
        displayAds();
      }
    }

    // Publish new ad with payment status logic
    function publishAd(event) {
      event.preventDefault();
      const formData = new FormData(event.target);
      
      const adType = formData.get('type');
      const prices = { basic: 0, plus: 10, turbo: 25 };
      const adPrice = prices[adType];
      
      const newAd = {
        id: ads.length + 1,
        title: formData.get('title'),
        price: parseFloat(formData.get('price')),
        location: formData.get('location'),
        category: formData.get('category'),
        type: adType,
        image: formData.get('image') || 'https://picsum.photos/640/360?random=' + Date.now(),
        description: formData.get('description'),
        email: formData.get('email'),
        status: adPrice > 0 ? 'pending_payment' : 'published', // Logic for payment status
        favorited: false
      };
      
      if (adPrice === 0) {
        // Free ad - publish immediately
        ads.unshift(newAd);
        displayAds();
        updateAdminStats();
        
        alert(`✅ Anúncio BÁSICO publicado com sucesso!

📧 Email: ${newAd.email}
🆓 Tipo: Gratuito
📍 Status: Publicado imediatamente

Seu anúncio já está visível no site!`);
        
      } else {
        // Paid ad - needs payment approval
        ads.unshift(newAd);
        updateAdminStats();
        
        alert(`⏳ Anúncio ${adType.toUpperCase()} criado - Aguardando pagamento!

📧 Email: ${newAd.email}
💰 Valor: R$ ${adPrice.toFixed(2)}
📍 Status: Pendente de pagamento

Seu anúncio será publicado após confirmação do pagamento PIX.
Você receberá as instruções de pagamento em breve.`);
      }
      
      event.target.reset();
      showContent('home');
    }

    // Publish new banner with payment status logic
    function publishBanner(event) {
      event.preventDefault();
      const formData = new FormData(event.target);
      
      const duration = parseInt(formData.get('duration'));
      const durationPrices = { 7: 50, 15: 90, 30: 150 };
      const basePrices = parseFloat(formData.get('price'));
      const totalPrice = basePrices + durationPrices[duration];
      
      const newBanner = {
        id: banners.length + 1,
        title: formData.get('title'),
        position: formData.get('position'),
        price: basePrices,
        company: formData.get('company'),
        duration: duration,
        text: formData.get('text'),
        image: formData.get('image') || 'https://picsum.photos/800/120?banner=' + Date.now(),
        link: formData.get('link'),
        email: formData.get('email'),
        status: 'pending_payment', // All banners need payment
        active: false
      };
      
      banners.push(newBanner);
      updateAdminStats();
      
      alert(`⏳ Banner criado - Aguardando pagamento!

📧 Email: ${newBanner.email}
📍 Posição: ${newBanner.position}
📅 Período: ${duration} dias
💰 Valor total: R$ ${totalPrice.toFixed(2)}
📍 Status: Pendente de pagamento

Seu banner será ativado após confirmação do pagamento PIX.
Você receberá as instruções de pagamento em breve.`);
      
      event.target.reset();
      showContent('home');
    }

    // Update admin statistics
    function updateAdminStats() {
      // Count only published ads for display
      const publishedAds = ads.filter(ad => ad.status === 'published');
      document.getElementById('total-ads').textContent = publishedAds.length;
      
      // Count pending payment ads
      const pendingAds = ads.filter(ad => ad.status === 'pending_payment');
      const pendingBanners = banners.filter(banner => banner.status === 'pending_payment');
      const totalPending = pendingAds.length + pendingBanners.length;
      
      // Update banner statistics (only active banners)
      const activeBanners = banners.filter(banner => banner.status === 'published' && banner.active);
      const bannerRevenue = activeBanners.reduce((total, banner) => {
        const durationPrices = { 7: 50, 15: 90, 30: 150 };
        return total + (banner.price + durationPrices[banner.duration]);
      }, 0);
      
      // Update admin activities with payment status info
      const activitiesEl = document.getElementById('admin-activities');
      if (activitiesEl) {
        activitiesEl.innerHTML = `
          <p>• Anúncios publicados: ${publishedAds.length}</p>
          <p>• Anúncios pendentes: ${pendingAds.length}</p>
          <p>• Banners ativos: ${activeBanners.length}</p>
          <p>• Banners pendentes: ${pendingBanners.length}</p>
          <p>• Total aguardando pagamento: ${totalPending}</p>
          <p>• Receita confirmada: R$ ${bannerRevenue.toLocaleString()}</p>
          <p>• Sistema de status: Ativo ✅</p>
        `;
      }
    }

    // Report ad function
    function reportAd() {
      alert('Denúncia enviada! Obrigado por nos ajudar a manter a comunidade segura.');
      closeModal();
    }

    // Close modal when clicking outside
    window.onclick = function(event) {
      const modal = document.getElementById('ad-modal');
      if (event.target === modal) {
        closeModal();
      }
    }
  </script>
</body>
</html># AcheiAqui
