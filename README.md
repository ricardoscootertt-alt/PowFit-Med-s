<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PowFit Med's - Avaliação Integrada</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            /* Tema Dark Profissional */
            --bg-body: #020617;
            --bg-card: #0f172a;
            --bg-input: #1e293b;
            --primary: #3b82f6;
            --primary-hover: #2563eb;
            --accent: #0ea5e9;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border: #334155;
            --success: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
            --radius: 14px;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', 'Segoe UI', Roboto, sans-serif; scroll-behavior: smooth; -webkit-tap-highlight-color: transparent;}
        body { background-color: var(--bg-body); color: var(--text-main); -webkit-font-smoothing: antialiased; padding-bottom: 30px; overflow-x: hidden; }

        /* SCROLLBAR */
        ::-webkit-scrollbar { width: 4px; height: 4px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }

        /* AVISOS TOAST (Substitui os alerts do navegador) */
        #toast-container { position: fixed; top: 20px; right: 20px; z-index: 9999; display: flex; flex-direction: column; gap: 10px; }
        .toast { background: var(--bg-card); border-left: 4px solid var(--primary); color: white; padding: 15px 20px; border-radius: 8px; box-shadow: 0 10px 25px rgba(0,0,0,0.5); font-size: 14px; font-weight: 600; animation: slideIn 0.3s ease-out forwards; display: flex; align-items: center; gap: 10px; }
        @keyframes slideIn { from { transform: translateX(100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }

        /* MODAIS E TELAS DE CARREGAMENTO */
        .overlay-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(2, 6, 23, 0.98); backdrop-filter: blur(8px);
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            z-index: 1000; padding: 20px;
        }
        .modal-box {
            background-color: var(--bg-card); padding: 35px 25px; border-radius: var(--radius);
            border: 1px solid var(--border); box-shadow: 0 15px 40px rgba(0,0,0,0.8);
            text-align: center; max-width: 420px; width: 100%; max-height: 90vh; overflow-y: auto;
        }
        .modal-box h1 { color: white; margin-bottom: 10px; font-size: 26px; letter-spacing: 0.5px; }
        .modal-box h1 span { color: var(--accent); }
        .modal-box p { color: var(--text-muted); margin-bottom: 25px; font-size: 14px; line-height: 1.5; }
        
        .btn-google {
            background-color: white; color: #0f172a; border: none; padding: 16px 20px;
            border-radius: 10px; font-weight: 800; font-size: 15px; cursor: pointer;
            display: flex; align-items: center; justify-content: center; gap: 12px; width: 100%;
            transition: transform 0.1s; box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        .btn-google:active { transform: scale(0.97); }
        .btn-google img { width: 22px; }

        /* LISTA DE PROFISSIONAIS */
        .prof-list-item {
            background: var(--bg-input); border: 2px solid var(--border); border-radius: 10px;
            padding: 16px; margin-bottom: 12px; cursor: pointer; transition: all 0.2s;
            display: flex; flex-direction: column; align-items: flex-start; text-align: left;
        }
        .prof-list-item:active { border-color: var(--primary); background: rgba(59,130,246,0.15); transform: scale(0.98); }
        .prof-list-item strong { color: var(--text-main); font-size: 16px; margin-bottom: 4px; font-weight: 700;}
        .prof-list-item span { color: var(--accent); font-size: 13px; font-weight: 600;}

        /* NAVBAR MOBILE FIRST */
        #app-content { display: none; }
        .navbar {
            background: var(--bg-card); padding: 15px 20px;
            box-shadow: 0 4px 25px rgba(0,0,0,0.8); position: sticky; top: 0; z-index: 100;
            border-bottom: 1px solid var(--border);
        }
        .nav-header { display: flex; justify-content: space-between; align-items: center; width: 100%; margin-bottom: 15px;}
        .logo { font-size: 22px; font-weight: 900; color: white; text-transform: uppercase; }
        .logo span { color: var(--primary); }
        
        .user-info { display: flex; align-items: center; gap: 12px; }
        .user-info img { width: 38px; height: 38px; border-radius: 50%; border: 2px solid var(--primary); object-fit: cover; }
        .user-details { display: flex; flex-direction: column; align-items: flex-end;}
        .user-details span { font-size: 13px; font-weight: 700; color: var(--text-main); }
        .btn-logout { background: transparent; border: none; color: var(--danger); font-size: 12px; text-decoration: underline; cursor: pointer; padding: 2px 0;}
        
        .nav-links {
            display: flex; gap: 10px; overflow-x: auto; padding-bottom: 5px;
            -webkit-overflow-scrolling: touch; scrollbar-width: none;
        }
        .nav-links::-webkit-scrollbar { display: none; }
        .nav-links button {
            background: var(--bg-input); border: 1px solid var(--border);
            color: var(--text-muted); padding: 12px 20px; border-radius: 25px; cursor: pointer;
            transition: all 0.2s; font-weight: 700; font-size: 13px; white-space: nowrap; flex-shrink: 0;
        }
        .nav-links button.active { background: var(--primary); border-color: var(--primary); color: white; }

        /* CONTAINERS E GRIDS */
        .container { max-width: 900px; margin: 20px auto; padding: 0 15px; }
        .view-section { display: none; animation: fadeIn 0.3s ease-in-out; }
        .view-section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

        .card {
            background-color: var(--bg-card); border-radius: var(--radius); padding: 20px;
            margin-bottom: 20px; border: 1px solid var(--border); box-shadow: 0 8px 20px rgba(0,0,0,0.4);
        }
        .section-title {
            color: var(--primary); font-size: 16px; font-weight: 800; margin-bottom: 20px;
            padding-bottom: 12px; border-bottom: 1px solid var(--border); display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; text-transform: uppercase; letter-spacing: 0.5px;
        }

        .grid { display: grid; grid-template-columns: 1fr; gap: 15px; margin-bottom: 15px; }
        @media (min-width: 768px) {
            .grid-2 { grid-template-columns: repeat(2, 1fr); }
            .grid-3 { grid-template-columns: repeat(3, 1fr); }
            .grid-4 { grid-template-columns: repeat(4, 1fr); }
            .navbar { padding: 15px 30px; flex-direction: row; display: flex; justify-content: space-between; align-items: center; }
            .nav-header { margin-bottom: 0; width: auto; }
            .nav-links { padding-bottom: 0; margin-left: 20px; flex: 1; }
        }

        /* INPUTS MODERNOS */
        .input-group { display: flex; flex-direction: column; gap: 6px; text-align: left; }
        .input-group label { font-size: 11px; color: var(--text-muted); font-weight: 800; text-transform: uppercase; letter-spacing: 0.5px;}
        input, select, textarea {
            background-color: var(--bg-body); border: 2px solid var(--border); color: var(--text-main);
            padding: 15px; border-radius: 10px; font-size: 16px; transition: all 0.2s; outline: none; width: 100%;
            -webkit-appearance: none;
        }
        input:focus, select:focus { border-color: var(--primary); background-color: var(--bg-input); }
        select { background-image: url('data:image/svg+xml;utf8,<svg fill="%233b82f6" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/><path d="M0 0h24v24H0z" fill="none"/></svg>'); background-repeat: no-repeat; background-position-x: 96%; background-position-y: center; }
        .highlight-input { border-color: var(--accent); background-color: rgba(14, 165, 233, 0.05); }

        /* FOTOS */
        .photo-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(100px, 1fr)); gap: 12px; }
        .photo-upload {
            border: 2px dashed var(--border); border-radius: var(--radius); padding: 20px 10px; text-align: center;
            cursor: pointer; position: relative; overflow: hidden; min-height: 140px;
            display: flex; flex-direction: column; align-items: center; justify-content: center; color: var(--text-muted); background: var(--bg-body);
        }
        .photo-upload img { width: 100%; height: 100%; object-fit: cover; position: absolute; top: 0; left: 0; }
        .photo-upload input { position: absolute; top: 0; left: 0; width: 100%; height: 100%; opacity: 0; cursor: pointer; z-index: 10; }
        .photo-upload .icon { font-size: 28px; margin-bottom: 8px; color: var(--primary); }

        /* BOTÕES DE AÇÃO */
        .btn { padding: 18px 20px; border: none; border-radius: 10px; font-size: 15px; font-weight: 800; cursor: pointer; transition: all 0.2s; text-transform: uppercase; width: 100%; letter-spacing: 1px; display: flex; align-items: center; justify-content: center; gap: 8px;}
        .btn:active { transform: scale(0.97); }
        .btn-primary { background-color: var(--primary); color: white; box-shadow: 0 4px 15px rgba(59, 130, 246, 0.4); }
        .btn-success { background-color: var(--success); color: white; box-shadow: 0 4px 15px rgba(16, 185, 129, 0.4);}
        .btn-outline { background-color: transparent; border: 2px solid var(--border); color: var(--text-main); }
        .btn-sm { padding: 10px 16px; font-size: 12px; border-radius: 8px; width: auto; border-color: var(--border);}

        /* DASHBOARD RESULTADOS E RECOMENDAÇÕES */
        .dashboard-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 12px; margin-bottom: 20px; }
        .dash-card { background-color: var(--bg-body); padding: 18px 15px; border-radius: var(--radius); text-align: center; border: 2px solid var(--border); }
        .dash-card h4 { color: var(--text-muted); font-size: 11px; margin-bottom: 10px; text-transform: uppercase; letter-spacing: 0.5px; font-weight: 800;}
        .dash-card .value { font-size: 28px; font-weight: 900; color: var(--text-main); margin-bottom: 10px; }
        .badge { display: inline-block; padding: 6px 12px; border-radius: 20px; font-size: 11px; font-weight: 800; text-transform: uppercase; }
        
        .badge.success { background: rgba(16, 185, 129, 0.15); color: var(--success); }
        .badge.warning { background: rgba(245, 158, 11, 0.15); color: var(--warning); }
        .badge.danger { background: rgba(239, 68, 68, 0.15); color: var(--danger); }
        .badge.info { background: rgba(59, 130, 246, 0.15); color: var(--primary); }

        .ai-suggestions { background: var(--bg-body); border-left: 4px solid var(--accent); padding: 20px; border-radius: var(--radius); margin-top: 20px; border: 1px solid var(--border); }
        .ai-suggestions h4 { color: var(--accent); margin-bottom: 15px; font-size: 16px; font-weight: 800;}
        .rec-item { margin-bottom: 15px; padding-bottom: 15px; border-bottom: 1px dashed var(--border); text-align: left;}
        .rec-item:last-child { border-bottom: none; margin-bottom: 0; padding-bottom: 0; }
        .rec-item strong { color: var(--primary); display: block; margin-bottom: 6px; font-size: 14px;}
        .rec-item p { color: var(--text-muted); font-size: 13px; line-height: 1.6; margin: 0; }

        .legal-text { font-size: 11px; color: var(--text-muted); margin-top: 15px; padding-top: 15px; border-top: 1px dashed var(--border); line-height: 1.5; text-align: justify;}
        .rt-footer { background: var(--bg-body); border: 1px solid var(--border); padding: 15px; text-align: center; font-size: 11px; color: var(--text-muted); border-radius: 8px; margin-top: 20px; line-height: 1.5;}
        .rt-footer strong { color: var(--accent); font-size: 13px; display: block; margin-bottom: 5px;}

        /* HISTÓRICO E FILTROS */
        .report-filters { display: flex; gap: 8px; margin-bottom: 20px; overflow-x: auto; padding-bottom: 5px; scrollbar-width: none; }
        .report-filters::-webkit-scrollbar { display: none; }
        .report-filters button {
            background: var(--bg-body); border: 2px solid var(--border); color: var(--text-muted);
            padding: 12px 18px; border-radius: 25px; cursor: pointer; font-size: 13px; font-weight: 800; white-space: nowrap; flex: 1; text-align: center;
        }
        .report-filters button.active { background: var(--primary); border-color: var(--primary); color: white; }

        .history-list { display: flex; flex-direction: column; gap: 15px; }
        .history-item { background: var(--bg-body); padding: 20px; border-radius: var(--radius); border: 2px solid var(--border); display: flex; flex-direction: column; gap: 15px; }
        .hist-info strong { color: var(--text-main); font-size: 18px; margin-bottom: 8px; display: block; font-weight: 800;}
        .hist-info small { color: var(--text-muted); font-size: 12px; display: flex; flex-wrap: wrap; gap: 8px;}
        .hist-info small span { background: var(--bg-input); padding: 6px 10px; border-radius: 6px; font-weight: 600;}
        .hist-actions { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; }
        
        .btn-action-sm { padding: 12px 5px; font-size: 11px; border-radius: 8px; font-weight: 800; border: none; text-align: center; text-transform: uppercase;}
        .btn-edit { background: rgba(59,130,246,0.15); color: var(--primary); }
        .btn-print { background: rgba(16,185,129,0.15); color: var(--success); }
        .btn-delete { background: rgba(239,68,68,0.15); color: var(--danger); }

        .edit-mode-banner { background-color: rgba(245, 158, 11, 0.15); color: var(--warning); padding: 15px; text-align: center; font-size: 14px; font-weight: 800; border-radius: var(--radius); margin-bottom: 20px; border: 2px solid var(--warning); }

        /* IMPRESSÃO (PDF) OTIMIZADA PARA A4 */
        @media print {
            body { background: #fff !important; color: #000 !important; font-size: 11px; padding: 0;}
            .navbar, #login-screen, #modal-profissionais, .btn, .hist-actions, #view-history, .photo-upload input, .edit-mode-banner, #toast-container { display: none !important; }
            .container { margin: 0; padding: 0; width: 100%; max-width: 100%; }
            .card { box-shadow: none; border: none; padding: 0; margin-bottom: 15px; border-bottom: 1px solid #ccc; border-radius: 0; background: transparent;}
            .section-title { color: #000 !important; border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px; flex-direction: row;}
            .section-title button, .section-title select { display: none !important; }
            .grid, .grid-2, .grid-3, .grid-4 { display: flex !important; flex-wrap: wrap !important; flex-direction: row !important; gap: 10px; margin-bottom: 10px;}
            .input-group { flex: 1 1 22%; min-width: 120px; }
            input, select, textarea { background: transparent !important; border: none !important; border-bottom: 1px solid #000 !important; color: #000 !important; padding: 2px 0 !important; border-radius: 0 !important;}
            .dash-card { border: 1px solid #000 !important; background: transparent !important; padding: 10px;}
            .dash-card h4, .dash-card .value { color: #000 !important; }
            .badge { border: 1px solid #000 !important; color: #000 !important; }
            .ai-suggestions { background: #fff !important; border: 1px solid #000 !important; padding: 10px;}
            .ai-suggestions h4, .rec-item strong, .rec-item p { color: #000 !important; }
            .rt-footer, .legal-text { border: 1px dashed #000 !important; color: #000 !important; }
        }
    </style>
</head>
<body>

    <!-- Container para Notificações Elegantes (Toasts) -->
    <div id="toast-container"></div>

    <!-- ================= 1. TELA DE LOGIN ================= -->
    <div id="login-screen" class="overlay-screen">
        <div class="modal-box">
            <h1>PowFit <span>Med's</span></h1>
            <p>Plataforma Profissional de Avaliação</p>
            <button class="btn-google" onclick="window.Auth.login()">
                <img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg" alt="Google Logo">
                Entrar com Conta Google
            </button>
        </div>
    </div>

    <!-- ================= 2. MODAL DE PROFISSIONAIS E ERRO FIREBASE ================= -->
    <div id="modal-profissionais" class="overlay-screen" style="display: none;">
        <div class="modal-box">
            <h1 id="prof-modal-title">Selecione o <span>Perfil</span></h1>
            <p id="prof-modal-desc">Escolha qual profissional vai assinar as avaliações.</p>
            
            <div id="prof-list-container" style="max-height: 250px; overflow-y: auto; margin-bottom: 20px;"></div>

            <button class="btn btn-outline" onclick="window.ProfUI.showForm()" id="btn-show-add-prof">+ Novo Profissional</button>

            <!-- Formulário de Novo Profissional -->
            <form id="form-add-prof" style="display: none; margin-top: 20px;" onsubmit="event.preventDefault(); window.ProfUI.saveProfissional();">
                <div class="grid">
                    <div class="input-group">
                        <label>Nome Completo *</label>
                        <input type="text" id="p_nome" required placeholder="Ex: João Silva">
                    </div>
                    <div class="input-group">
                        <label>Atuação *</label>
                        <select id="p_tipo" required onchange="window.ProfUI.toggleCref()">
                            <option value="Profissional de Educação Física">Educação Física</option>
                            <option value="Treinador Esportivo">Treinador Esportivo</option>
                        </select>
                    </div>
                    <div class="input-group" id="group_cref">
                        <label>Registro (CREF) *</label>
                        <input type="text" id="p_cref" placeholder="Ex: 000000-G">
                    </div>
                    <div class="input-group">
                        <label>Estado (UF) *</label>
                        <select id="p_estado" required>
                            <option value="AC">AC</option><option value="AL">AL</option><option value="AP">AP</option><option value="AM">AM</option><option value="BA">BA</option><option value="CE">CE</option><option value="DF">DF</option><option value="ES">ES</option><option value="GO">GO</option><option value="MA">MA</option><option value="MT">MT</option><option value="MS">MS</option><option value="MG">MG</option><option value="PA">PA</option><option value="PB">PB</option><option value="PR">PR</option><option value="PE">PE</option><option value="PI">PI</option><option value="RJ">RJ</option><option value="RN" selected>RN</option><option value="RS">RS</option><option value="RO">RO</option><option value="RR">RR</option><option value="SC">SC</option><option value="SP">SP</option><option value="SE">SE</option><option value="TO">TO</option>
                        </select>
                    </div>
                </div>
                <div class="grid grid-2">
                    <button type="button" class="btn btn-outline" onclick="window.ProfUI.hideForm()">Voltar</button>
                    <button type="submit" class="btn btn-primary" id="btn-save-prof">Salvar Perfil</button>
                </div>
            </form>
        </div>
    </div>

    <!-- ================= 3. CONTEÚDO PRINCIPAL DO APP ================= -->
    <div id="app-content">
        <!-- Navbar Mobile Otimizada -->
        <nav class="navbar">
            <div class="nav-header">
                <div class="logo">PowFit <span>Med's</span></div>
                <div class="user-info">
                    <div class="user-details">
                        <span id="active-prof-name">--</span>
                        <button class="btn-logout" onclick="window.Auth.logout()">Sair (Logout)</button>
                    </div>
                    <img id="user-photo" src="" alt="Foto" style="display:none;">
                </div>
            </div>
            <div class="nav-links">
                <button id="tab-form" class="active" onclick="window.UI.switchTab('form')">📄 Ficha de Avaliação</button>
                <button id="tab-hist" onclick="window.UI.switchTab('history')">📂 Meus Pacientes</button>
                <button onclick="window.UI.toggleTotem()">⛶ Tela Cheia</button>
            </div>
        </nav>

        <div class="container">
            
            <!-- ====== TELA: FORMULÁRIO DE AVALIAÇÃO ====== -->
            <div id="view-form" class="view-section active">
                
                <div id="edit-banner" class="edit-mode-banner">
                    ⚠️ MODO DE EDIÇÃO: Você está alterando uma Ficha Salva.
                </div>

                <form id="formAvaliacao" onsubmit="event.preventDefault(); window.App.processAvaliacao();">
                    
                    <!-- Avaliador -->
                    <div class="card">
                        <div class="section-title">
                            👨‍⚕️ Responsável Técnico
                            <button type="button" class="btn-outline btn-sm" onclick="window.ProfUI.showSelectionModal()">Trocar</button>
                        </div>
                        <div class="grid grid-3">
                            <div class="input-group"><label>Nome</label><input type="text" id="av_nome" readonly style="color:var(--primary); font-weight: bold;"></div>
                            <div class="input-group"><label>Atuação</label><input type="text" id="av_tipo" readonly></div>
                            <div class="input-group"><label>Reg./UF</label><input type="text" id="av_reg" readonly></div>
                        </div>
                        <div id="av_legal_text" class="legal-text"></div>
                    </div>

                    <!-- Cliente -->
                    <div class="card">
                        <div class="section-title">👤 Paciente e Anamnese</div>
                        <div class="grid grid-2">
                            <div class="input-group"><label>Nome do Paciente *</label><input type="text" id="in_nome" required></div>
                            <div class="input-group"><label>Data da Avaliação *</label><input type="date" id="in_data" required></div>
                        </div>
                        <div class="grid grid-4">
                            <div class="input-group"><label>Idade *</label><input type="number" id="in_idade" required></div>
                            <div class="input-group"><label>Peso (kg) *</label><input type="number" step="0.1" id="in_peso" required></div>
                            <div class="input-group"><label>Altura (cm) *</label><input type="number" id="in_altura" required></div>
                            <div class="input-group"><label>Sexo *</label><select id="in_sexo"><option value="Masculino">Homem</option><option value="Feminino">Mulher</option></select></div>
                        </div>
                        <div class="grid grid-3">
                            <div class="input-group"><label>Objetivo</label><select id="in_objetivo"><option value="Emagrecimento">Emagrecimento</option><option value="Hipertrofia">Hipertrofia</option><option value="Saúde">Saúde/Condicionamento</option><option value="Performance">Performance Esportiva</option></select></div>
                            <div class="input-group"><label>Atividade Física</label><select id="in_atividade"><option value="Sedentário">Sedentário</option><option value="Leve">Leve</option><option value="Moderado">Moderado</option><option value="Intenso">Intenso</option></select></div>
                            <div class="input-group"><label>Qualidade do Sono</label><select id="in_sono"><option value="Boa">Boa (7-8h)</option><option value="Regular">Regular</option><option value="Ruim">Ruim (Insônia)</option></select></div>
                        </div>
                        <div class="grid grid-2">
                            <div class="input-group"><label>Lesões/Dores Físicas</label><input type="text" id="in_lesoes" placeholder="Opcional. Ex: Dor lombar"></div>
                            <div class="input-group"><label>Medicamentos/Suplementos</label><input type="text" id="in_medicamentos" placeholder="Opcional."></div>
                        </div>
                    </div>

                    <!-- Perímetros -->
                    <div class="card">
                        <div class="section-title">📏 Perímetros (cm)</div>
                        <div class="grid grid-4">
                            <div class="input-group"><label>Pescoço</label><input type="number" step="0.1" id="in_p_pescoco"></div>
                            <div class="input-group"><label>Ombros</label><input type="number" step="0.1" id="in_p_ombros"></div>
                            <div class="input-group"><label>Tórax</label><input type="number" step="0.1" id="in_p_torax"></div>
                            <div class="input-group"><label>Abdominal</label><input type="number" step="0.1" id="in_p_abdominal"></div>
                            <div class="input-group"><label style="color:var(--accent);">Cintura *</label><input type="number" step="0.1" id="in_p_cintura" required class="highlight-input"></div>
                            <div class="input-group"><label style="color:var(--accent);">Quadril *</label><input type="number" step="0.1" id="in_p_quadril" required class="highlight-input"></div>
                            <div class="input-group"><label>Braço Esq.</label><input type="number" step="0.1" id="in_p_bracoE"></div>
                            <div class="input-group"><label>Braço Dir.</label><input type="number" step="0.1" id="in_p_bracoD"></div>
                            <div class="input-group"><label>Antebraço Esq.</label><input type="number" step="0.1" id="in_p_antebraE"></div>
                            <div class="input-group"><label>Antebraço Dir.</label><input type="number" step="0.1" id="in_p_antebraD"></div>
                            <div class="input-group"><label>Coxa Esq.</label><input type="number" step="0.1" id="in_p_coxaE"></div>
                            <div class="input-group"><label>Coxa Dir.</label><input type="number" step="0.1" id="in_p_coxaD"></div>
                            <div class="input-group"><label>Panturrilha Esq.</label><input type="number" step="0.1" id="in_p_pantE"></div>
                            <div class="input-group"><label>Panturrilha Dir.</label><input type="number" step="0.1" id="in_p_pantD"></div>
                        </div>
                    </div>

                    <!-- Dobras -->
                    <div class="card">
                        <div class="section-title">
                            🤏 Dobras (mm)
                            <select id="in_protocolo" style="width:auto; padding: 6px; font-size: 13px; font-weight: bold; border-color: var(--primary);">
                                <option value="pollock3">Pollock 3</option>
                                <option value="pollock7">Pollock 7</option>
                                <option value="imc">Só IMC/RCQ</option>
                            </select>
                        </div>
                        <div class="grid grid-4">
                            <div class="input-group"><label>Peitoral</label><input type="number" step="0.1" id="in_d_peito"></div>
                            <div class="input-group"><label>Tríceps</label><input type="number" step="0.1" id="in_d_triceps"></div>
                            <div class="input-group"><label>Subescapular</label><input type="number" step="0.1" id="in_d_sub"></div>
                            <div class="input-group"><label>Axilar Média</label><input type="number" step="0.1" id="in_d_axilar"></div>
                            <div class="input-group"><label>Suprailíaca</label><input type="number" step="0.1" id="in_d_supra"></div>
                            <div class="input-group"><label>Abdominal</label><input type="number" step="0.1" id="in_d_abd"></div>
                            <div class="input-group"><label>Coxa</label><input type="number" step="0.1" id="in_d_coxa"></div>
                            <div class="input-group"><label>Panturrilha</label><input type="number" step="0.1" id="in_d_pant"></div>
                        </div>
                    </div>

                    <!-- Fotos -->
                    <div class="card">
                        <div class="section-title">📷 Fotos (Opcional)</div>
                        <div class="photo-grid">
                            <div class="photo-upload">
                                <div class="icon">🧍‍♂️</div><span id="txt-foto-frente">Frente</span>
                                <img id="preview-frente" style="display:none;"><input type="file" accept="image/*" onchange="window.UI.handlePhoto(this, 'preview-frente', 'txt-foto-frente', 'foto_frente')">
                            </div>
                            <div class="photo-upload">
                                <div class="icon">🚶‍♂️</div><span id="txt-foto-lado">Perfil</span>
                                <img id="preview-lado" style="display:none;"><input type="file" accept="image/*" onchange="window.UI.handlePhoto(this, 'preview-lado', 'txt-foto-lado', 'foto_lado')">
                            </div>
                            <div class="photo-upload">
                                <div class="icon">🧍‍♂️</div><span id="txt-foto-costas">Costas</span>
                                <img id="preview-costas" style="display:none;"><input type="file" accept="image/*" onchange="window.UI.handlePhoto(this, 'preview-costas', 'txt-foto-costas', 'foto_costas')">
                            </div>
                        </div>
                    </div>

                    <div class="grid grid-2">
                        <button type="button" class="btn btn-outline" onclick="window.App.cancelarEdicaoOuLimpar()">Limpar Tudo</button>
                        <button type="submit" class="btn btn-primary" id="btn-gerar">Processar Avaliação</button>
                    </div>
                </form>

                <!-- ================= RESULTADOS ================= -->
                <div id="painel-resultados" class="card" style="display: none; margin-top: 20px;">
                    <div class="section-title">📊 Diagnóstico Clínico</div>
                    
                    <div class="grid grid-3 dashboard-grid">
                        <div class="dash-card"><h4>IMC</h4><div class="value" id="res-imc">--</div><div class="badge" id="badge-imc">--</div></div>
                        <div class="dash-card"><h4>RCQ (Cint/Quad)</h4><div class="value" id="res-rcq">--</div><div class="badge" id="badge-rcq">--</div></div>
                        <div class="dash-card"><h4>Gordura %</h4><div class="value" id="res-gordura">--%</div><div class="badge" id="badge-gordura">--</div></div>
                    </div>

                    <div class="grid grid-2 dashboard-grid">
                        <div class="dash-card"><h4>Massa Gorda</h4><div class="value" style="color: var(--danger)" id="res-mg">-- kg</div></div>
                        <div class="dash-card"><h4>Massa Magra</h4><div class="value" style="color: var(--success)" id="res-mm">-- kg</div></div>
                    </div>

                    <div class="ai-suggestions" id="container-recomendacoes">
                        <h4>🩺 Recomendações</h4>
                        <div id="rec-imc" class="rec-item"></div>
                        <div id="rec-rcq" class="rec-item"></div>
                        <div id="rec-bf" class="rec-item"></div>
                    </div>

                    <div class="rt-footer">
                        <strong>Responsabilidade Técnica Clínica (Motor de Decisão)</strong>
                        Luiz André — CREF: 008094-G/RN<br>
                        <em>Guia de suporte clínico. A prescrição final é do profissional titular.</em>
                    </div>

                    <div style="margin-top: 20px;">
                        <button class="btn btn-success" id="btn-salvar-imprimir" style="padding: 20px;" onclick="window.App.salvarEImprimir()">
                            💾 Salvar Paciente & 🖨️ PDF
                        </button>
                    </div>
                </div>
            </div>

            <!-- ====== TELA: HISTÓRICO ====== -->
            <div id="view-history" class="view-section">
                
                <div class="report-filters">
                    <button class="active" id="btn-filtro-todos" onclick="window.UI.setReportFilter('todos')">Todos os Meses</button>
                    <button id="btn-filtro-mes" onclick="window.UI.setReportFilter('mes')">Este Mês</button>
                    <button id="btn-filtro-ano" onclick="window.UI.setReportFilter('ano')">Este Ano</button>
                </div>

                <div class="card">
                    <div class="section-title"><div id="chart-title">📈 Evolução de Peso x Gordura</div></div>
                    <div style="height: 250px; width: 100%; position: relative;">
                        <canvas id="evolutionChart"></canvas>
                    </div>
                </div>

                <div class="card">
                    <div class="section-title">📂 Pacientes Salvos</div>
                    <input type="text" id="search-history" placeholder="Buscar paciente por nome..." style="margin-bottom: 15px; width: 100%; padding: 16px; border-radius:10px; border: 2px solid var(--border); background: var(--bg-body); color: white; outline: none; font-size: 15px;" onkeyup="window.UI.renderHistoryList()">
                    <div class="history-list" id="lista-historico">
                        <p style="color: var(--text-muted); text-align: center; padding: 20px;">Carregando histórico do servidor...</p>
                    </div>
                </div>
            </div>

        </div>
    </div>

<!-- FIREBASE SDK E LÓGICA -->
<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getAuth, GoogleAuthProvider, signInWithPopup, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
    import { getFirestore, collection, addDoc, getDocs, doc, updateDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

    const firebaseConfig = {
      apiKey: "AIzaSyA9icSOYHPq-5p-GJaFmKZ02DMMFpohk7g",
      authDomain: "powfit-med-s.firebaseapp.com",
      projectId: "powfit-med-s",
      storageBucket: "powfit-med-s.firebasestorage.app",
      messagingSenderId: "878219813171",
      appId: "1:878219813171:web:8fb80b8e1192aff1d55e46"
    };

    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);
    const db = getFirestore(app);
    const provider = new GoogleAuthProvider();

    let currentUser = null;
    let profissionais = [];
    let activeProfId = null;
    let avaliacoes = []; 
    let currentReportFilter = 'todos'; 
    let currentEditingId = null;
    let currentCalculatedData = null;
    let currentPhotos = { foto_frente: null, foto_lado: null, foto_costas: null };

    // SISTEMA DE NOTIFICAÇÕES (Substitui Alertas)
    window.showToast = (message, type = 'info') => {
        const container = document.getElementById('toast-container');
        const toast = document.createElement('div');
        toast.className = 'toast';
        let icon = 'ℹ️';
        if (type === 'error') { icon = '❌'; toast.style.borderLeftColor = 'var(--danger)'; }
        if (type === 'success') { icon = '✅'; toast.style.borderLeftColor = 'var(--success)'; }
        toast.innerHTML = `<span>${icon}</span> <div>${message}</div>`;
        container.appendChild(toast);
        setTimeout(() => { toast.style.opacity = '0'; setTimeout(() => toast.remove(), 300); }, 4000);
    };

    window.Auth = {
        login: async () => {
            const btn = document.querySelector('.btn-google');
            btn.innerHTML = 'Conectando ao Google...';
            try { await signInWithPopup(auth, provider); } 
            catch (e) {
                window.showToast("Falha ao logar. Tente novamente.", 'error');
                btn.innerHTML = `<img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg" alt="Google">Entrar com Conta Google`;
            }
        },
        logout: () => signOut(auth).then(() => window.location.reload())
    };

    onAuthStateChanged(auth, async (user) => {
        if (user) {
            currentUser = user;
            document.getElementById('login-screen').style.display = 'none';
            if(user.photoURL) {
                document.getElementById('user-photo').src = user.photoURL;
                document.getElementById('user-photo').style.display = 'block';
            }
            window.showToast("Login realizado com sucesso!", 'success');
            await window.ProfUI.loadProfissionais();
        } else {
            currentUser = null;
            document.getElementById('login-screen').style.display = 'flex';
            document.getElementById('app-content').style.display = 'none';
            document.getElementById('modal-profissionais').style.display = 'none';
        }
    });

    window.ProfUI = {
        getProfCollection() { 
            // Usando estrutura padrão e segura para qualquer Firebase
            return collection(db, "powfit_users", currentUser.uid, "profissionais"); 
        },
        
        async loadProfissionais() {
            try {
                const snapshot = await getDocs(this.getProfCollection());
                profissionais = [];
                snapshot.forEach(doc => profissionais.push({ id: doc.id, ...doc.data() }));
                
                if (profissionais.length === 0) {
                    this.showSelectionModal(); this.showForm(true); 
                } else {
                    this.showSelectionModal(); this.renderList();
                }
            } catch(e) { 
                console.error(e);
                // Tratamento elegante do erro do Firebase sem alert()
                const container = document.getElementById('prof-list-container');
                document.getElementById('prof-modal-title').innerHTML = "Banco <span>Bloqueado</span>";
                document.getElementById('prof-modal-desc').innerHTML = "Acesse o Console do Firebase e libere as <b>Regras (Rules)</b> do Firestore Database para continuar.";
                container.innerHTML = `
                    <div style="background: rgba(239, 68, 68, 0.1); border: 1px solid var(--danger); padding: 15px; border-radius: 8px; text-align: left; font-size: 13px; color: #fca5a5;">
                        <b>Cole isto nas Regras do seu Firebase:</b><br><br>
                        <code style="background: rgba(0,0,0,0.3); padding: 8px; display: block; border-radius: 4px;">rules_version = '2';<br>service cloud.firestore {<br> match /databases/{database}/documents {<br>  match /{document=**} {<br>   allow read, write: if request.auth != null;<br>  }<br> }<br>}</code>
                    </div>`;
                document.getElementById('btn-show-add-prof').style.display = 'none';
                document.getElementById('modal-profissionais').style.display = 'flex';
            }
        },
        showSelectionModal() {
            document.getElementById('modal-profissionais').style.display = 'flex';
            document.getElementById('app-content').style.display = 'none';
            this.hideForm(); this.renderList();
        },
        renderList() {
            const container = document.getElementById('prof-list-container');
            container.innerHTML = '';
            if(profissionais.length > 0) {
                profissionais.forEach(p => {
                    const info = p.tipo === 'Treinador Esportivo' ? 'Treinador Esportivo' : `CREF: ${p.cref} - ${p.estado}`;
                    container.innerHTML += `
                        <div class="prof-list-item" onclick="window.ProfUI.selectProfissional('${p.id}')">
                            <strong>${p.nome}</strong><span>${info}</span>
                        </div>`;
                });
                document.getElementById('btn-show-add-prof').style.display = 'block';
            }
        },
        showForm(force = false) {
            document.getElementById('form-add-prof').style.display = 'block';
            document.getElementById('btn-show-add-prof').style.display = 'none';
            document.getElementById('prof-list-container').style.display = 'none';
            if(force) document.querySelector('#form-add-prof .btn-outline').style.display = 'none';
            else document.querySelector('#form-add-prof .btn-outline').style.display = 'flex';
        },
        hideForm() {
            document.getElementById('form-add-prof').style.display = 'none';
            document.getElementById('form-add-prof').reset();
            document.getElementById('btn-show-add-prof').style.display = 'block';
            document.getElementById('prof-list-container').style.display = 'block';
            this.toggleCref();
        },
        toggleCref() {
            const tipo = document.getElementById('p_tipo').value;
            const group = document.getElementById('group_cref');
            const input = document.getElementById('p_cref');
            if(tipo === 'Treinador Esportivo') { group.style.display = 'none'; input.required = false; } 
            else { group.style.display = 'flex'; input.required = true; }
        },
        async saveProfissional() {
            const btn = document.getElementById('btn-save-prof');
            btn.innerText = 'Aguarde...'; btn.disabled = true;
            try {
                const payload = {
                    nome: document.getElementById('p_nome').value, tipo: document.getElementById('p_tipo').value,
                    estado: document.getElementById('p_estado').value, cref: document.getElementById('p_tipo').value === 'Treinador Esportivo' ? '' : document.getElementById('p_cref').value
                };
                const docRef = await addDoc(this.getProfCollection(), payload);
                profissionais.push({id: docRef.id, ...payload});
                window.showToast("Profissional cadastrado!", 'success');
                this.selectProfissional(docRef.id);
            } catch(e) { 
                window.showToast("Erro ao salvar. Verifique as Regras do Firebase.", 'error');
                btn.innerText = 'Salvar Perfil'; btn.disabled = false; 
            }
        },
        async selectProfissional(id) {
            activeProfId = id;
            const prof = profissionais.find(p => p.id === id);
            
            document.getElementById('modal-profissionais').style.display = 'none';
            document.getElementById('app-content').style.display = 'block';
            
            const nA = prof.nome.split(' ')[0];
            document.getElementById('active-prof-name').innerText = nA.length > 12 ? nA.substring(0,12)+'...' : nA;
            document.getElementById('av_nome').value = prof.nome;
            document.getElementById('av_tipo').value = prof.tipo;
            document.getElementById('av_reg').value = prof.tipo === 'Treinador Esportivo' ? prof.estado : `CREF: ${prof.cref} / ${prof.estado}`;
            
            const legalDiv = document.getElementById('av_legal_text');
            if(prof.tipo === 'Treinador Esportivo') legalDiv.innerHTML = "⚖️ <strong>Base Legal Esportiva:</strong> Amparado pela Lei Federal nº 9.615/1998 (Lei Pelé), focado no treinamento técnico e tático desportivo.";
            else legalDiv.innerHTML = "⚖️ <strong>Base Legal Clínica:</strong> Atuação regulamentada pela Lei Federal nº 9.696/1998 para avaliação e prescrição voltada à saúde.";

            document.getElementById('in_data').valueAsDate = new Date();
            await window.DB.loadAvaliacoes();
        }
    };

    window.DB = {
        getAvaliacaoCollection() { return collection(db, "powfit_users", currentUser.uid, "avaliacoes"); },

        async loadAvaliacoes() {
            if (!currentUser || !activeProfId) return;
            const list = document.getElementById('lista-historico');
            try {
                const snapshot = await getDocs(this.getAvaliacaoCollection());
                let todas = [];
                snapshot.forEach(doc => todas.push({ id: doc.id, ...doc.data() }));
                avaliacoes = todas.filter(a => a.profId === activeProfId);
                avaliacoes.sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
                window.UI.processReports(); 
            } catch (e) { 
                console.error(e);
                list.innerHTML = `<div style="padding:20px; text-align:center; color:var(--danger); border:1px solid var(--danger); border-radius:8px; background:rgba(239, 68, 68, 0.1);">Erro de permissão.<br>Libere as Regras no Firebase.</div>`; 
            }
        },
        async saveOrUpdate(dataObj) {
            if (!currentUser || !activeProfId) return;
            const payload = { ...dataObj, profId: activeProfId, timestamp: new Date().toISOString() };
            if (currentEditingId) {
                const docRef = doc(db, "powfit_users", currentUser.uid, "avaliacoes", currentEditingId);
                await updateDoc(docRef, payload);
            } else {
                await addDoc(this.getAvaliacaoCollection(), payload);
            }
            await this.loadAvaliacoes();
        },
        async deleteAvaliacao(id) {
            if(!confirm("⚠️ Remover esta ficha permanentemente?")) return;
            try {
                await deleteDoc(doc(db, "powfit_users", currentUser.uid, "avaliacoes", id));
                window.showToast("Ficha removida.", 'success');
                await this.loadAvaliacoes();
            } catch (e) { window.showToast("Erro ao excluir.", 'error'); }
        }
    };

    window.Calc = {
        imc: (p, a) => {
            const val = p / Math.pow(a/100, 2);
            let classif = "Obesidade", type = "danger";
            if(val < 18.5) { classif = "Baixo Peso"; type = "warning"; }
            else if(val < 24.9) { classif = "Normal"; type = "success"; }
            else if(val < 29.9) { classif = "Sobrepeso"; type = "warning"; }
            return { value: val.toFixed(1), classif, type };
        },
        rcq: (c, q, sx) => {
            const val = c / q;
            let classif = "Alto Risco", type = "danger";
            if(sx === 'Masculino') {
                if(val < 0.90) { classif = "Baixo"; type = "success"; }
                else if(val <= 0.95) { classif = "Moderado"; type = "warning"; }
            } else {
                if(val < 0.80) { classif = "Baixo"; type = "success"; }
                else if(val <= 0.85) { classif = "Moderado"; type = "warning"; }
            }
            return { value: val.toFixed(2), classif, type };
        },
        gorduraPollock3: (sx, id, d) => {
            const soma = (sx === 'Masculino') ? (d.peito + d.abd + d.coxa) : (d.triceps + d.supra + d.coxa);
            if(!soma || soma <= 0) return null;
            let den = sx === 'Masculino' ? 1.10938 - (0.0008267*soma) + (0.0000016*soma*soma) - (0.0002574*id) : 1.0994921 - (0.0009929*soma) + (0.0000023*soma*soma) - (0.0001392*id);
            return Math.max(3, ((4.95/den) - 4.5)*100).toFixed(1);
        },
        gorduraPollock7: (sx, id, d) => {
            const soma = d.peito + d.triceps + d.sub + d.axilar + d.supra + d.abd + d.coxa;
            if(!soma || soma <= 0) return null;
            let den = sx === 'Masculino' ? 1.112 - (0.00043499*soma) + (0.00000055*soma*soma) - (0.00028826*id) : 1.097 - (0.00046971*soma) + (0.00000056*soma*soma) - (0.00012828*id);
            return Math.max(3, ((4.95/den) - 4.5)*100).toFixed(1);
        },
        classificarGordura: (bf, sx) => {
            if(!bf) return { c: 'N/A', t: 'info' };
            const b = parseFloat(bf);
            if(sx === 'Masculino') {
                if(b<10) return {c:"Atleta", t:"info"};
                if(b<=18) return {c:"Bom", t:"success"};
                if(b<=24) return {c:"Acima", t:"warning"};
                return {c:"Risco", t:"danger"};
            } else {
                if(b<17) return {c:"Atleta", t:"info"};
                if(b<=25) return {c:"Bom", t:"success"};
                if(b<=31) return {c:"Acima", t:"warning"};
                return {c:"Risco", t:"danger"};
            }
        },
        getRecomendacoes(imcObj, rcqObj, bfObj) {
            let r_imc = "", r_rcq = "", r_bf = "";
            if(imcObj.type === 'danger') r_imc = "Quadro indica sobrecarga articular sistêmica. Priorize baixo impacto e déficit calórico.";
            else if(imcObj.type === 'warning') r_imc = "Proporção Estatura/Peso aponta leve descompasso. Avaliar se o excesso provém de massa muscular.";
            else if(imcObj.type === 'success') r_imc = "Peso estável nos parâmetros de segurança clínica. Foco em manutenção de força.";
            else r_imc = "Baixo peso estrutural. Prescrição focada em hipertrofia e superávit nutricional.";

            if(rcqObj.type === 'danger') r_rcq = "ALERTA: Acúmulo de gordura visceral identificado. Alta propensão à síndrome metabólica. Treino aeróbio mandatório.";
            else if(rcqObj.type === 'warning') r_rcq = "Distribuição adiposa central requer atenção. Elevar frequência do estímulo cardiovascular.";
            else r_rcq = "Índices seguros de distribuição de gordura. Baixa tendência a doenças metabólicas primárias.";

            if(!bfObj.c || bfObj.t === 'info') r_bf = "Tecido adiposo muito baixo. Monitorar integridade hormonal, articular e imunológica.";
            else if(bfObj.t === 'success') r_bf = "Reservas lipídicas saudáveis. Quadro perfeito para extrair o máximo de performance e evolução.";
            else if(bfObj.t === 'warning') r_bf = "Gordura corporal discretamente acima da meta de saúde plena. Protocolo leve de oxidação recomendado.";
            else r_bf = "Excesso de tecido adiposo constatado. A prescrição deve focar prioritariamente na regulação metabólica (Aeróbico + Resistido).";

            return { r_imc: `<p>${r_imc}</p>`, r_rcq: `<p>${r_rcq}</p>`, r_bf: `<p>${r_bf}</p>` };
        }
    };

    window.UI = {
        switchTab: (id) => {
            document.querySelectorAll('.view-section').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.nav-links button').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${id}`).classList.add('active');
            document.getElementById(`tab-${id.substring(0,4)}`).classList.add('active');
        },
        toggleTotem: () => {
            if (!document.fullscreenElement) document.documentElement.requestFullscreen().catch(()=>{});
            else document.exitFullscreen();
        },
        handlePhoto: (input, imgId, txtId, key) => {
            if (input.files && input.files[0]) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    const img = new Image();
                    img.onload = () => {
                        const canvas = document.createElement('canvas'); const ctx = canvas.getContext('2d');
                        const maxW = 300; const scale = maxW / img.width;
                        canvas.width = maxW; canvas.height = img.height * scale;
                        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                        const b64 = canvas.toDataURL('image/jpeg', 0.6);
                        document.getElementById(imgId).src = b64; document.getElementById(imgId).style.display = 'block';
                        document.getElementById(txtId).style.display = 'none'; currentPhotos[key] = b64;
                    };
                    img.src = e.target.result;
                };
                reader.readAsDataURL(input.files[0]);
            }
        },
        setReportFilter: (type) => {
            currentReportFilter = type;
            document.querySelectorAll('.report-filters button').forEach(b => b.classList.remove('active'));
            document.getElementById(`btn-filtro-${type}`).classList.add('active');
            const titles = { 'todos': '📈 Evolução Geral', 'mes': '📅 Pacientes deste Mês', 'ano': '📊 Pacientes do Ano' };
            document.getElementById('chart-title').innerText = titles[type];
            this.processReports();
        },
        processReports: () => {
            const now = new Date();
            let arr = avaliacoes.filter(h => {
                if(!h.timestamp) return false;
                const d = new Date(h.timestamp);
                if(currentReportFilter === 'mes') return d.getMonth() === now.getMonth() && d.getFullYear() === now.getFullYear();
                if(currentReportFilter === 'ano') return d.getFullYear() === now.getFullYear();
                return true;
            });
            this.renderHistoryList(arr);
            this.renderChart(arr);
        },
        renderHistoryList: (filteredData = null) => {
            const dataToRender = filteredData || avaliacoes; 
            const termo = (document.getElementById('search-history')?.value || '').toLowerCase();
            const list = document.getElementById('lista-historico');
            list.innerHTML = '';
            
            const finalArr = dataToRender.filter(h => h.inputs.nome.toLowerCase().includes(termo));
            if(finalArr.length === 0) { list.innerHTML = '<p style="color:var(--text-muted); text-align:center; padding: 20px;">Nenhum paciente cadastrado neste filtro.</p>'; return; }

            finalArr.forEach(item => {
                const d = new Date(item.timestamp).toLocaleDateString();
                list.innerHTML += `
                    <div class="history-item">
                        <div class="hist-info">
                            <strong>${item.inputs.nome}</strong>
                            <small>
                                <span>📅 ${d}</span> <span>⚖️ ${item.inputs.peso}kg</span> <span>🔥 BF: ${item.resultados.bf ? item.resultados.bf+'%' : '--'}</span>
                            </small>
                        </div>
                        <div class="hist-actions">
                            <button class="btn-action-sm btn-edit" onclick="window.App.editar('${item.id}')">✏️ Editar</button>
                            <button class="btn-action-sm btn-print" onclick="window.App.imprimirAntigo('${item.id}')">🖨️ Imprimir</button>
                            <button class="btn-action-sm btn-delete" onclick="window.DB.deleteAvaliacao('${item.id}')">🗑️ Apagar</button>
                        </div>
                    </div>`;
            });
        },
        renderChart: (dataArray) => {
            const recent = dataArray.slice(0, 15).reverse(); 
            if(window.myChart) window.myChart.destroy();
            const ctx = document.getElementById('evolutionChart').getContext('2d');
            Chart.defaults.color = '#94a3b8';
            window.myChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: recent.map(h => h.inputs.nome.split(' ')[0] + ' ('+ new Date(h.timestamp).getDate() +'/'+ (new Date(h.timestamp).getMonth()+1) +')'),
                    datasets: [
                        { label: 'Peso (kg)', data: recent.map(h => h.inputs.peso), borderColor: '#3b82f6', backgroundColor: 'rgba(59,130,246,0.1)', fill:true, tension: 0.3},
                        { label: 'Gordura (%)', data: recent.map(h => h.resultados.bf || 0), borderColor: '#0ea5e9', borderDash:[5,5], tension: 0.3}
                    ]
                },
                options: { responsive: true, maintainAspectRatio: false, interaction: { mode: 'index', intersect: false } }
            });
        }
    };

    window.App = {
        getForm: () => {
            return {
                nome: document.getElementById('in_nome').value, data: document.getElementById('in_data').value,
                idade: parseFloat(document.getElementById('in_idade').value), peso: parseFloat(document.getElementById('in_peso').value),
                altura: parseFloat(document.getElementById('in_altura').value), sexo: document.getElementById('in_sexo').value,
                objetivo: document.getElementById('in_objetivo').value, atividade: document.getElementById('in_atividade').value,
                sono: document.getElementById('in_sono').value, lesoes: document.getElementById('in_lesoes').value, med: document.getElementById('in_medicamentos').value,
                p_cintura: parseFloat(document.getElementById('in_p_cintura').value), p_quadril: parseFloat(document.getElementById('in_p_quadril').value),
                p_torax: document.getElementById('in_p_torax').value, p_abd: document.getElementById('in_p_abdominal').value,
                p_bracoE: document.getElementById('in_p_bracoE').value, p_bracoD: document.getElementById('in_p_bracoD').value,
                p_coxaE: document.getElementById('in_p_coxaE').value, p_coxaD: document.getElementById('in_p_coxaD').value,
                p_pantE: document.getElementById('in_p_pantE').value, p_pantD: document.getElementById('in_p_pantD').value,
                protocolo: document.getElementById('in_protocolo').value,
                d: {
                    peito: parseFloat(document.getElementById('in_d_peito').value||0), triceps: parseFloat(document.getElementById('in_d_triceps').value||0),
                    sub: parseFloat(document.getElementById('in_d_sub').value||0), axilar: parseFloat(document.getElementById('in_d_axilar').value||0),
                    supra: parseFloat(document.getElementById('in_d_supra').value||0), abd: parseFloat(document.getElementById('in_d_abd').value||0),
                    coxa: parseFloat(document.getElementById('in_d_coxa').value||0), pant: parseFloat(document.getElementById('in_d_pant').value||0)
                }, fotos: currentPhotos
            };
        },

        setForm: (data) => {
            document.getElementById('in_nome').value = data.nome; document.getElementById('in_data').value = data.data;
            document.getElementById('in_idade').value = data.idade; document.getElementById('in_peso').value = data.peso;
            document.getElementById('in_altura').value = data.altura; document.getElementById('in_sexo').value = data.sexo;
            document.getElementById('in_objetivo').value = data.objetivo || 'Saúde'; document.getElementById('in_atividade').value = data.atividade || 'Sedentário';
            document.getElementById('in_sono').value = data.sono || 'Boa'; document.getElementById('in_lesoes').value = data.lesoes || ''; document.getElementById('in_medicamentos').value = data.med || '';
            document.getElementById('in_p_cintura').value = data.p_cintura; document.getElementById('in_p_quadril').value = data.p_quadril;
            
            document.getElementById('in_p_torax').value = data.p_torax||''; document.getElementById('in_p_abdominal').value = data.p_abd||'';
            document.getElementById('in_p_bracoE').value = data.p_bracoE||''; document.getElementById('in_p_bracoD').value = data.p_bracoD||'';
            document.getElementById('in_p_coxaE').value = data.p_coxaE||''; document.getElementById('in_p_coxaD').value = data.p_coxaD||'';
            document.getElementById('in_p_pantE').value = data.p_pantE||''; document.getElementById('in_p_pantD').value = data.p_pantD||'';
            
            document.getElementById('in_protocolo').value = data.protocolo || 'pollock3';
            if(data.d) {
                document.getElementById('in_d_peito').value = data.d.peito||''; document.getElementById('in_d_triceps').value = data.d.triceps||'';
                document.getElementById('in_d_sub').value = data.d.sub||''; document.getElementById('in_d_axilar').value = data.d.axilar||'';
                document.getElementById('in_d_supra').value = data.d.supra||''; document.getElementById('in_d_abd').value = data.d.abd||'';
                document.getElementById('in_d_coxa').value = data.d.coxa||''; document.getElementById('in_d_pant').value = data.d.pant||'';
            }

            currentPhotos = data.fotos || { foto_frente: null, foto_lado: null, foto_costas: null };
            ['frente', 'lado', 'costas'].forEach(pos => {
                if(currentPhotos['foto_'+pos]) {
                    document.getElementById('preview-'+pos).src = currentPhotos['foto_'+pos];
                    document.getElementById('preview-'+pos).style.display = 'block';
                    document.getElementById('txt-foto-'+pos).style.display = 'none';
                }
            });
        },

        processAvaliacao: () => {
            const i = window.App.getForm();
            
            const resImc = window.Calc.imc(i.peso, i.altura);
            const resRcq = window.Calc.rcq(i.p_cintura, i.p_quadril, i.sexo);
            
            let resBf = null;
            if(i.protocolo === 'pollock3') resBf = window.Calc.gorduraPollock3(i.sexo, i.idade, i.d);
            else if(i.protocolo === 'pollock7') resBf = window.Calc.gorduraPollock7(i.sexo, i.idade, i.d);
            const classBf = window.Calc.classificarGordura(resBf, i.sexo);

            document.getElementById('painel-resultados').style.display = 'block';
            document.getElementById('res-imc').innerText = resImc.value; document.getElementById('badge-imc').innerText = resImc.classif; document.getElementById('badge-imc').className = `badge ${resImc.type}`;
            document.getElementById('res-rcq').innerText = resRcq.value; document.getElementById('badge-rcq').innerText = resRcq.classif; document.getElementById('badge-rcq').className = `badge ${resRcq.type}`;

            let mg = 0, mm = 0;
            if(resBf) {
                document.getElementById('res-gordura').innerText = resBf + '%'; document.getElementById('badge-gordura').innerText = classBf.c; document.getElementById('badge-gordura').className = `badge ${classBf.t}`;
                mg = i.peso * (resBf/100); mm = i.peso - mg;
                document.getElementById('res-mg').innerText = mg.toFixed(1) + ' kg'; document.getElementById('res-mm').innerText = mm.toFixed(1) + ' kg';
            } else {
                document.getElementById('res-gordura').innerText = '--'; document.getElementById('badge-gordura').innerText = 'N/A';
                document.getElementById('res-mg').innerText = '--'; document.getElementById('res-mm').innerText = '--';
            }

            const recs = window.Calc.getRecomendacoes(resImc, resRcq, classBf);
            document.getElementById('rec-imc').innerHTML = `<strong>• Peso Corporal (IMC):</strong> ${recs.r_imc}`;
            document.getElementById('rec-rcq').innerHTML = `<strong>• Prevenção Cardiovascular:</strong> ${recs.r_rcq}`;
            if(resBf) document.getElementById('rec-bf').innerHTML = `<strong>• Tecido Adiposo (Gordura):</strong> ${recs.r_bf}`;
            else document.getElementById('rec-bf').innerHTML = "";

            currentCalculatedData = { inputs: i, resultados: { imc: resImc.value, rcq: resRcq.value, bf: resBf, mg: mg, mm: mm } };
            document.getElementById('painel-resultados').scrollIntoView();
            window.showToast("Cálculos gerados com sucesso!", 'success');
        },

        async salvarEImprimir() {
            const btn = document.getElementById('btn-salvar-imprimir');
            btn.innerText = 'Sincronizando Nuvem...'; btn.disabled = true;
            try {
                await window.DB.saveOrUpdate(currentCalculatedData);
                btn.innerText = 'Gerando PDF...';
                const n = document.getElementById('in_nome').value; const t = document.title;
                document.title = `PowFit_Meds_${n ? n.replace(/\s+/g,'_') : 'Relatorio'}`;
                
                setTimeout(() => {
                    window.print();
                    document.title = t;
                    window.App.cancelarEdicaoOuLimpar();
                    window.UI.switchTab('history');
                }, 500);
            } catch(e) { window.showToast("Erro ao salvar: " + e.message, 'error'); } 
            finally { btn.innerText = '💾 Salvar Paciente & 🖨️ PDF'; btn.disabled = false; }
        },

        editar: (id) => {
            const item = avaliacoes.find(x => x.id === id);
            if(!item) return;
            window.App.cancelarEdicaoOuLimpar(); 
            currentEditingId = id; 
            window.App.setForm(item.inputs);
            
            document.getElementById('edit-banner').style.display = 'block';
            window.UI.switchTab('form');
            window.App.processAvaliacao();
        },

        imprimirAntigo: (id) => {
            window.App.editar(id);
            window.showToast("Preparando PDF...", 'info');
            setTimeout(() => {
                const n = document.getElementById('in_nome').value; const t = document.title;
                document.title = `PowFit_Meds_${n ? n.replace(/\s+/g,'_') : 'Relatorio'}`;
                window.print();
                document.title = t;
            }, 800);
        },

        cancelarEdicaoOuLimpar: () => {
            currentEditingId = null; currentCalculatedData = null;
            document.getElementById('formAvaliacao').reset();
            document.getElementById('in_data').valueAsDate = new Date();
            
            const prof = profissionais.find(p => p.id === activeProfId);
            if(prof) {
                document.getElementById('av_nome').value = prof.nome;
                document.getElementById('av_tipo').value = prof.tipo;
                document.getElementById('av_reg').value = prof.tipo === 'Treinador Esportivo' ? prof.estado : `CREF: ${prof.cref} / ${prof.estado}`;
            }

            currentPhotos = { foto_frente: null, foto_lado: null, foto_costas: null };
            ['frente', 'lado', 'costas'].forEach(pos => {
                document.getElementById('preview-'+pos).style.display = 'none';
                document.getElementById('txt-foto-'+pos).style.display = 'block';
            });

            document.getElementById('edit-banner').style.display = 'none';
            document.getElementById('painel-resultados').style.display = 'none';
            window.scrollTo(0,0);
            window.showToast("Ficha limpa e pronta para nova avaliação.", 'info');
        }
    };
</script>

</body>
</html>
