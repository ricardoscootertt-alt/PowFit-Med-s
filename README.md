<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PowFit Med's - Avaliação Inteligente na Nuvem</title>
    <!-- Chart.js para gráficos -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            /* Tema Dark & Azul */
            --bg-body: #060b14;
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
            --radius: 12px;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; scroll-behavior: smooth; }
        body { background-color: var(--bg-body); color: var(--text-main); }

        /* SCROLLBAR */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: var(--bg-body); }
        ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--primary); }

        /* MODAIS DE LOGIN E PERFIL */
        .overlay-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: linear-gradient(135deg, rgba(6,11,20,0.95), rgba(15,23,42,0.95));
            backdrop-filter: blur(5px);
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            z-index: 1000;
        }
        .modal-box {
            background-color: var(--bg-card); padding: 40px; border-radius: var(--radius);
            border: 1px solid var(--border); box-shadow: 0 10px 40px rgba(0,0,0,0.8);
            text-align: center; max-width: 500px; width: 90%; max-height: 90vh; overflow-y: auto;
        }
        .modal-box h1 { color: white; margin-bottom: 10px; font-size: 30px; letter-spacing: 1px; }
        .modal-box h1 span { color: var(--accent); }
        .modal-box p { color: var(--text-muted); margin-bottom: 25px; font-size: 15px; }
        
        .btn-google {
            background-color: white; color: #333; border: none; padding: 14px 20px;
            border-radius: 8px; font-weight: bold; font-size: 16px; cursor: pointer;
            display: flex; align-items: center; justify-content: center; gap: 12px; width: 100%;
            transition: all 0.2s; box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        .btn-google:hover { background-color: #f1f5f9; transform: translateY(-2px); box-shadow: 0 6px 12px rgba(0,0,0,0.2); }
        .btn-google img { width: 22px; }

        /* LISTA DE PROFISSIONAIS */
        .prof-list-item {
            background: var(--bg-input); border: 1px solid var(--border); border-radius: 8px;
            padding: 15px; margin-bottom: 10px; cursor: pointer; transition: all 0.2s;
            display: flex; flex-direction: column; align-items: flex-start; text-align: left;
        }
        .prof-list-item:hover { border-color: var(--primary); background: rgba(59,130,246,0.1); }
        .prof-list-item strong { color: var(--accent); font-size: 18px; margin-bottom: 4px;}
        .prof-list-item span { color: var(--text-muted); font-size: 13px; }

        /* NAVBAR */
        #app-content { display: none; }
        .navbar {
            background: linear-gradient(135deg, #0f172a, var(--primary)); padding: 15px 20px;
            display: flex; justify-content: space-between; align-items: center;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5); position: sticky; top: 0; z-index: 100; flex-wrap: wrap; gap: 15px;
        }
        .logo { font-size: 24px; font-weight: 900; letter-spacing: 1px; color: white; text-transform: uppercase; }
        .logo span { color: var(--accent); }
        .nav-links { display: flex; gap: 10px; align-items: center; flex-wrap: wrap; }
        .nav-links button {
            background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2);
            color: white; padding: 8px 16px; border-radius: 8px; cursor: pointer; transition: all 0.3s;
            font-weight: 600;
        }
        .nav-links button.active, .nav-links button:hover { background: var(--primary); border-color: var(--primary); }
        .user-info { display: flex; align-items: center; gap: 15px; font-size: 14px; color: #e2e8f0; font-weight: 600;}
        .user-info img { width: 32px; height: 32px; border-radius: 50%; border: 2px solid var(--accent); }
        .btn-logout { background: transparent; border: 1px solid #ef4444 !important; color: #ef4444 !important; padding: 6px 12px !important;}
        .btn-logout:hover { background: #ef4444 !important; color: white !important; }

        /* CONTAINER E CARDS */
        .container { max-width: 1000px; margin: 30px auto; padding: 0 15px; }
        .view-section { display: none; animation: fadeIn 0.4s ease-in-out; }
        .view-section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .card {
            background-color: var(--bg-card); border-radius: var(--radius); padding: 25px;
            margin-bottom: 25px; border: 1px solid var(--border); box-shadow: 0 8px 25px rgba(0,0,0,0.3);
        }
        .section-title {
            color: var(--accent); font-size: 20px; font-weight: 700; margin-bottom: 20px;
            padding-bottom: 10px; border-bottom: 1px solid var(--border); display: flex; align-items: center; gap: 10px; justify-content: space-between;
        }

        /* GRIDS E FORMS */
        .grid-2 { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; }
        .grid-3 { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 15px; }
        .grid-4 { display: grid; grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); gap: 15px; }
        
        .input-group { display: flex; flex-direction: column; gap: 6px; text-align: left; margin-bottom: 15px;}
        .input-group label { font-size: 12px; color: var(--text-muted); font-weight: 700; text-transform: uppercase; letter-spacing: 0.5px;}
        input, select, textarea {
            background-color: var(--bg-input); border: 1px solid var(--border); color: var(--text-main);
            padding: 12px; border-radius: 8px; font-size: 15px; transition: all 0.3s; outline: none; width: 100%;
        }
        input:focus, select:focus, textarea:focus { border-color: var(--primary); box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.2); }
        .highlight-input { border-bottom: 2px solid var(--accent); }

        /* FOTOS UPLOAD */
        .photo-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; }
        .photo-upload {
            border: 2px dashed var(--border); border-radius: var(--radius); padding: 20px; text-align: center;
            cursor: pointer; position: relative; overflow: hidden; min-height: 180px;
            display: flex; flex-direction: column; align-items: center; justify-content: center; color: var(--text-muted);
            background: var(--bg-input); transition: all 0.2s;
        }
        .photo-upload:hover { border-color: var(--primary); color: var(--primary); }
        .photo-upload img { width: 100%; height: 100%; object-fit: cover; position: absolute; top: 0; left: 0; }
        .photo-upload input { position: absolute; top: 0; left: 0; width: 100%; height: 100%; opacity: 0; cursor: pointer; z-index: 10; }
        .photo-upload .icon { font-size: 30px; margin-bottom: 10px; }

        /* BOTÕES */
        .btn { padding: 15px 25px; border: none; border-radius: 8px; font-size: 16px; font-weight: bold; cursor: pointer; transition: all 0.2s; text-transform: uppercase; width: 100%; letter-spacing: 1px;}
        .btn-primary { background-color: var(--primary); color: white; box-shadow: 0 4px 12px rgba(59, 130, 246, 0.4); }
        .btn-primary:hover { background-color: var(--primary-hover); transform: translateY(-2px); }
        .btn-success { background-color: var(--success); color: white; box-shadow: 0 4px 12px rgba(16, 185, 129, 0.4);}
        .btn-success:hover { background-color: #059669; transform: translateY(-2px); }
        .btn-outline { background-color: transparent; border: 2px solid var(--border); color: var(--text-main); }
        .btn-outline:hover { background-color: var(--bg-input); }
        .btn-sm { padding: 8px 16px; font-size: 13px; border-radius: 6px; width: auto;}

        /* DASHBOARD RESULTADOS E RECOMENDAÇÕES */
        .dashboard-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 15px; margin-bottom: 20px; }
        .dash-card { background-color: var(--bg-input); padding: 20px; border-radius: var(--radius); text-align: center; border-left: 4px solid var(--primary); position: relative; overflow: hidden;}
        .dash-card h4 { color: var(--text-muted); font-size: 13px; margin-bottom: 10px; text-transform: uppercase; letter-spacing: 0.5px;}
        .dash-card .value { font-size: 32px; font-weight: 800; color: var(--text-main); margin-bottom: 10px; }
        .badge { display: inline-block; padding: 6px 12px; border-radius: 20px; font-size: 12px; font-weight: bold; text-transform: uppercase; }
        
        .badge.success { background: rgba(16, 185, 129, 0.15); color: var(--success); border: 1px solid rgba(16, 185, 129, 0.3);}
        .badge.warning { background: rgba(245, 158, 11, 0.15); color: var(--warning); border: 1px solid rgba(245, 158, 11, 0.3);}
        .badge.danger { background: rgba(239, 68, 68, 0.15); color: var(--danger); border: 1px solid rgba(239, 68, 68, 0.3);}
        .badge.info { background: rgba(59, 130, 246, 0.15); color: var(--primary); border: 1px solid rgba(59, 130, 246, 0.3);}

        .ai-suggestions { background: linear-gradient(to bottom, rgba(30, 41, 59, 0.8), rgba(15, 23, 42, 0.8)); border-left: 4px solid var(--accent); padding: 20px; border-radius: var(--radius); margin-top: 20px; border: 1px solid var(--border); }
        .ai-suggestions h4 { color: var(--accent); margin-bottom: 15px; display: flex; align-items: center; gap: 8px; font-size: 18px;}
        .rec-item { margin-bottom: 15px; padding-bottom: 15px; border-bottom: 1px solid rgba(255,255,255,0.05); }
        .rec-item:last-child { border-bottom: none; margin-bottom: 0; padding-bottom: 0; }
        .rec-item strong { color: var(--primary); display: block; margin-bottom: 5px; font-size: 15px;}
        .rec-item p { color: var(--text-main); font-size: 14px; line-height: 1.6; }

        /* LEGAL & RT TEXT */
        .legal-text { font-size: 11px; color: var(--text-muted); margin-top: 15px; padding-top: 15px; border-top: 1px solid var(--border); line-height: 1.5; text-align: justify;}
        .rt-footer { background: #020617; border: 1px solid var(--border); padding: 15px; text-align: center; font-size: 12px; color: var(--text-muted); border-radius: 8px; margin-top: 25px; }
        .rt-footer strong { color: var(--accent); font-size: 14px; display: block; margin-bottom: 5px;}

        /* HISTÓRICO E RELATÓRIOS */
        .report-filters { display: flex; gap: 10px; margin-bottom: 15px; flex-wrap: wrap; }
        .report-filters button {
            background: transparent; border: 1px solid var(--border); color: var(--text-muted);
            padding: 6px 12px; border-radius: 20px; cursor: pointer; transition: all 0.2s; font-size: 13px; font-weight: bold;
        }
        .report-filters button.active, .report-filters button:hover { background: var(--primary); border-color: var(--primary); color: white; }

        .history-list { margin-top: 10px; display: grid; gap: 15px; }
        .history-item {
            display: flex; justify-content: space-between; align-items: center; background: var(--bg-input);
            padding: 20px; border-radius: var(--radius); border: 1px solid var(--border); flex-wrap: wrap; gap: 15px;
            transition: all 0.2s;
        }
        .history-item:hover { border-color: var(--primary); box-shadow: 0 4px 15px rgba(0,0,0,0.2); }
        .hist-info strong { color: var(--accent); font-size: 18px; display: block; margin-bottom: 5px;}
        .hist-info small { color: var(--text-muted); font-size: 14px; display: flex; gap: 15px; flex-wrap: wrap;}
        .hist-actions { display: flex; gap: 10px; flex-wrap: wrap; }
        
        .btn-action-sm { padding: 8px 16px; font-size: 13px; border-radius: 6px; cursor: pointer; font-weight: bold; border: 1px solid; transition: all 0.2s;}
        .btn-edit { background: rgba(59,130,246,0.1); border-color: var(--primary); color: var(--primary); }
        .btn-edit:hover { background: var(--primary); color: white; }
        .btn-print { background: rgba(16,185,129,0.1); border-color: var(--success); color: var(--success); }
        .btn-print:hover { background: var(--success); color: white; }
        .btn-delete { background: rgba(239,68,68,0.1); border-color: var(--danger); color: var(--danger); }
        .btn-delete:hover { background: var(--danger); color: white; }

        .edit-mode-banner {
            background-color: rgba(245, 158, 11, 0.1); color: var(--warning); padding: 15px; text-align: center;
            font-weight: bold; border-radius: var(--radius); margin-bottom: 20px; display: none; border: 1px solid var(--warning);
        }

        /* IMPRESSÃO (PDF) */
        @media print {
            body { background: #fff !important; color: #000 !important; font-size: 12px; }
            .navbar, #login-screen, #modal-profissionais, .btn, .hist-actions, #view-history, .photo-upload input, .edit-mode-banner { display: none !important; }
            .container { margin: 0; padding: 0; width: 100%; max-width: 100%; }
            .card { box-shadow: none; border: none; padding: 10px 0; margin-bottom: 10px; background: transparent; page-break-inside: avoid; border-bottom: 1px solid #ccc;}
            .section-title { color: #000 !important; border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px;}
            .section-title button, .section-title span.badge { display: none !important; }
            input, select, textarea { background: transparent !important; border: none !important; border-bottom: 1px solid #ccc !important; color: #000 !important; padding: 4px 0 !important; border-radius: 0 !important;}
            .dash-card { background: #f8fafc !important; border: 1px solid #cbd5e1 !important; border-left: 4px solid #000 !important;}
            .dash-card h4 { color: #64748b !important; }
            .dash-card .value { color: #000 !important; }
            
            .ai-suggestions { background: #fff !important; border: 1px solid #000 !important; border-left: 4px solid #000 !important;}
            .ai-suggestions h4, .rec-item strong, .rec-item p { color: #000 !important; }
            .rec-item { border-bottom: 1px solid #eee !important; }
            
            .legal-text { color: #555 !important; border-top: 1px dashed #000 !important; margin-top: 10px; padding-top: 5px;}
            .rt-footer { background: #fff !important; border: 1px solid #000 !important; color: #000 !important; padding: 10px !important;}
            .rt-footer strong { color: #000 !important; }
            
            .badge { border: 1px solid #000 !important; background: transparent !important; color: #000 !important; }
            .photo-grid { gap: 10px; }
            .photo-upload { border: 1px solid #ccc !important; min-height: 120px; background: transparent !important;}
        }
    </style>
</head>
<body>

    <!-- ================= 1. TELA DE LOGIN ================= -->
    <div id="login-screen" class="overlay-screen">
        <div class="modal-box">
            <h1>PowFit <span>Med's</span></h1>
            <p>Plataforma Profissional de Avaliação</p>
            <button class="btn-google" onclick="window.Auth.login()">
                <img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg" alt="Google Logo">
                Entrar com Conta Google
            </button>
            <div id="login-error" style="color: var(--danger); margin-top: 15px; font-size: 14px; display: none;"></div>
        </div>
    </div>

    <!-- ================= 2. MODAL DE PROFISSIONAIS (Seleção/Cadastro) ================= -->
    <div id="modal-profissionais" class="overlay-screen" style="display: none;">
        <div class="modal-box">
            <h1>Selecione o <span>Perfil</span></h1>
            <p>Escolha com qual perfil profissional deseja atuar agora.</p>
            
            <div id="prof-list-container" style="max-height: 250px; overflow-y: auto; margin-bottom: 20px;">
                <!-- Preenchido via JS -->
            </div>

            <button class="btn btn-outline btn-sm" onclick="window.ProfUI.showForm()" id="btn-show-add-prof">+ Adicionar Novo Profissional</button>

            <!-- Formulário de Cadastro de Profissional -->
            <form id="form-add-prof" style="display: none; margin-top: 20px;" onsubmit="event.preventDefault(); window.ProfUI.saveProfissional();">
                <div class="input-group">
                    <label>Nome do Profissional *</label>
                    <input type="text" id="p_nome" required placeholder="Ex: João Silva">
                </div>
                <div class="input-group">
                    <label>Tipo de Atuação *</label>
                    <select id="p_tipo" required onchange="window.ProfUI.toggleCref()">
                        <option value="Profissional de Educação Física">Profissional de Educação Física</option>
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
                        <option value="AC">Acre</option><option value="AL">Alagoas</option><option value="AP">Amapá</option><option value="AM">Amazonas</option><option value="BA">Bahia</option><option value="CE">Ceará</option><option value="DF">Distrito Federal</option><option value="ES">Espírito Santo</option><option value="GO">Goiás</option><option value="MA">Maranhão</option><option value="MT">Mato Grosso</option><option value="MS">Mato Grosso do Sul</option><option value="MG">Minas Gerais</option><option value="PA">Pará</option><option value="PB">Paraíba</option><option value="PR">Paraná</option><option value="PE">Pernambuco</option><option value="PI">Piauí</option><option value="RJ">Rio de Janeiro</option><option value="RN">Rio Grande do Norte</option><option value="RS">Rio Grande do Sul</option><option value="RO">Rondônia</option><option value="RR">Roraima</option><option value="SC">Santa Catarina</option><option value="SP">São Paulo</option><option value="SE">Sergipe</option><option value="TO">Tocantins</option>
                    </select>
                </div>
                <div style="display: flex; gap: 10px; margin-top: 20px;">
                    <button type="button" class="btn btn-outline btn-sm" onclick="window.ProfUI.hideForm()">Cancelar</button>
                    <button type="submit" class="btn btn-primary btn-sm" id="btn-save-prof">Salvar Perfil</button>
                </div>
            </form>
        </div>
    </div>

    <!-- ================= 3. CONTEÚDO PRINCIPAL DO APP ================= -->
    <div id="app-content">
        <!-- Navbar -->
        <nav class="navbar">
            <div class="logo">PowFit <span>Med's</span></div>
            <div class="nav-links">
                <button id="tab-form" class="active" onclick="window.UI.switchTab('form')">Nova Avaliação</button>
                <button id="tab-hist" onclick="window.UI.switchTab('history')">Histórico & Relatórios</button>
                <button onclick="window.UI.toggleTotem()">⛶ Modo Totem</button>
            </div>
            <div class="user-info">
                <span id="active-prof-name" style="color: var(--accent);">--</span>
                <img id="user-photo" src="" alt="Avatar" style="display:none;">
                <button class="btn-logout" onclick="window.Auth.logout()">Sair</button>
            </div>
        </nav>

        <div class="container">
            
            <!-- ====== TELA: FORMULÁRIO DE AVALIAÇÃO ====== -->
            <div id="view-form" class="view-section active">
                
                <div id="edit-banner" class="edit-mode-banner">
                    ⚠️ MODO DE EDIÇÃO ATIVADO: Você está alterando um registro salvo.
                </div>

                <form id="formAvaliacao" onsubmit="event.preventDefault(); window.App.processAvaliacao();">
                    
                    <!-- Dados do Avaliador (Automático via Perfil) -->
                    <div class="card">
                        <div class="section-title">
                            <div>👨‍⚕️ Profissional e Embasamento Legal</div>
                            <button type="button" class="btn-outline btn-sm" style="width: auto; border-radius: 20px;" onclick="window.ProfUI.showSelectionModal()">Trocar Perfil</button>
                        </div>
                        <div class="grid-3">
                            <div class="input-group">
                                <label>Nome</label>
                                <input type="text" id="av_nome" readonly style="font-weight: bold; color: var(--accent); border-bottom-style: dashed;">
                            </div>
                            <div class="input-group">
                                <label>Atuação</label>
                                <input type="text" id="av_tipo" readonly style="border-bottom-style: dashed;">
                            </div>
                            <div class="input-group">
                                <label>Registro / Estado</label>
                                <input type="text" id="av_reg" readonly style="border-bottom-style: dashed;">
                            </div>
                        </div>
                        <div id="av_legal_text" class="legal-text">
                            <!-- Preenchido via JavaScript dependendo do perfil -->
                        </div>
                    </div>

                    <!-- Dados Clínicos & Anamnese -->
                    <div class="card">
                        <div class="section-title">👤 Cliente e Anamnese</div>
                        <div class="grid-2">
                            <div class="input-group"><label>Nome do Cliente *</label><input type="text" id="in_nome" required></div>
                            <div class="input-group"><label>Data da Avaliação *</label><input type="date" id="in_data" required></div>
                        </div>
                        <div class="grid-4">
                            <div class="input-group"><label>Idade *</label><input type="number" id="in_idade" required></div>
                            <div class="input-group"><label>Peso (kg) *</label><input type="number" step="0.1" id="in_peso" required></div>
                            <div class="input-group"><label>Estatura (cm) *</label><input type="number" id="in_altura" required></div>
                            <div class="input-group"><label>Sexo *</label>
                                <select id="in_sexo"><option value="Masculino">Masculino</option><option value="Feminino">Feminino</option></select>
                            </div>
                        </div>
                        <div class="grid-3">
                            <div class="input-group"><label>Objetivo Principal</label>
                                <select id="in_objetivo"><option value="Emagrecimento">Emagrecimento</option><option value="Hipertrofia">Hipertrofia</option><option value="Saúde">Saúde/Condicionamento</option><option value="Performance">Performance Esportiva</option></select>
                            </div>
                            <div class="input-group"><label>Nível de Atividade</label>
                                <select id="in_atividade"><option value="Sedentário">Sedentário (Nenhuma)</option><option value="Leve">Leve (1-2x semana)</option><option value="Moderado">Moderado (3-4x semana)</option><option value="Intenso">Intenso (5+ semana)</option></select>
                            </div>
                            <div class="input-group"><label>Qualidade do Sono</label>
                                <select id="in_sono"><option value="Boa">Boa (7-8h reparadoras)</option><option value="Regular">Regular (Acorda às vezes)</option><option value="Ruim">Ruim (Insônia/Poucas horas)</option></select>
                            </div>
                        </div>
                        <div class="grid-2">
                            <div class="input-group"><label>Lesões ou Dores Relatadas</label><input type="text" id="in_lesoes" placeholder="Ex: Condromalácia patelar dir."></div>
                            <div class="input-group"><label>Uso de Medicamentos</label><input type="text" id="in_medicamentos" placeholder="Ex: Losartana 50mg"></div>
                        </div>
                    </div>

                    <!-- Perímetros -->
                    <div class="card">
                        <div class="section-title">📏 Perímetros Corporais (cm)</div>
                        <div class="grid-4">
                            <div class="input-group"><label>Pescoço</label><input type="number" step="0.1" id="in_p_pescoco"></div>
                            <div class="input-group"><label>Ombros</label><input type="number" step="0.1" id="in_p_ombros"></div>
                            <div class="input-group"><label>Tórax</label><input type="number" step="0.1" id="in_p_torax"></div>
                            <div class="input-group"><label>Abdominal</label><input type="number" step="0.1" id="in_p_abdominal"></div>
                            <div class="input-group"><label style="color:var(--accent);">Cintura *</label><input type="number" step="0.1" id="in_p_cintura" required class="highlight-input"></div>
                            <div class="input-group"><label style="color:var(--accent);">Quadril *</label><input type="number" step="0.1" id="in_p_quadril" required class="highlight-input"></div>
                            <div class="input-group"><label>Braço Esq. (Rel/Cont)</label><input type="text" id="in_p_bracoE" placeholder="Ex: 30 / 32"></div>
                            <div class="input-group"><label>Braço Dir. (Rel/Cont)</label><input type="text" id="in_p_bracoD" placeholder="Ex: 30 / 32"></div>
                            <div class="input-group"><label>Antebraço Esq.</label><input type="number" step="0.1" id="in_p_antebraE"></div>
                            <div class="input-group"><label>Antebraço Dir.</label><input type="number" step="0.1" id="in_p_antebraD"></div>
                            <div class="input-group"><label>Coxa Esq. (Med/Prox)</label><input type="text" id="in_p_coxaE" placeholder="Ex: 55 / 60"></div>
                            <div class="input-group"><label>Coxa Dir. (Med/Prox)</label><input type="text" id="in_p_coxaD" placeholder="Ex: 55 / 60"></div>
                            <div class="input-group"><label>Panturrilha Esq.</label><input type="number" step="0.1" id="in_p_pantE"></div>
                            <div class="input-group"><label>Panturrilha Dir.</label><input type="number" step="0.1" id="in_p_pantD"></div>
                        </div>
                    </div>

                    <!-- Dobras Cutâneas -->
                    <div class="card">
                        <div class="section-title">
                            <div>🤏 Dobras Cutâneas (mm)</div>
                            <select id="in_protocolo" style="width: auto; padding: 6px; font-size: 13px; border-radius: 6px;">
                                <option value="pollock3">Pollock 3 Dobras</option>
                                <option value="pollock7">Pollock 7 Dobras</option>
                                <option value="imc">Apenas IMC/RCQ</option>
                            </select>
                        </div>
                        <div class="grid-4">
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

                    <!-- Upload de Fotos -->
                    <div class="card">
                        <div class="section-title">📷 Evolução Fotográfica</div>
                        <div class="photo-grid">
                            <div class="photo-upload">
                                <div class="icon">🧍‍♂️</div><span id="txt-foto-frente">Frente</span>
                                <img id="preview-frente" style="display:none;">
                                <input type="file" accept="image/*" onchange="window.UI.handlePhoto(this, 'preview-frente', 'txt-foto-frente', 'foto_frente')">
                            </div>
                            <div class="photo-upload">
                                <div class="icon">🚶‍♂️</div><span id="txt-foto-lado">Perfil</span>
                                <img id="preview-lado" style="display:none;">
                                <input type="file" accept="image/*" onchange="window.UI.handlePhoto(this, 'preview-lado', 'txt-foto-lado', 'foto_lado')">
                            </div>
                            <div class="photo-upload">
                                <div class="icon">🧍‍♂️</div><span id="txt-foto-costas">Costas</span>
                                <img id="preview-costas" style="display:none;">
                                <input type="file" accept="image/*" onchange="window.UI.handlePhoto(this, 'preview-costas', 'txt-foto-costas', 'foto_costas')">
                            </div>
                        </div>
                    </div>

                    <div class="grid-2" style="margin-bottom: 30px;">
                        <button type="button" class="btn btn-outline" onclick="window.App.cancelarEdicaoOuLimpar()">Limpar</button>
                        <button type="submit" class="btn btn-primary" id="btn-gerar">Calcular Resultados</button>
                    </div>
                </form>

                <!-- ================= RESULTADOS ================= -->
                <div id="painel-resultados" class="card" style="display: none;">
                    <div class="section-title">📊 Diagnóstico e Prescrição</div>
                    
                    <div class="dashboard-grid">
                        <div class="dash-card"><h4>Índice de Massa (IMC)</h4><div class="value" id="res-imc">--</div><div class="badge" id="badge-imc">--</div></div>
                        <div class="dash-card"><h4>Relação Cint/Quadril</h4><div class="value" id="res-rcq">--</div><div class="badge" id="badge-rcq">--</div></div>
                        <div class="dash-card"><h4>Gordura Corporal</h4><div class="value" id="res-gordura">--%</div><div class="badge" id="badge-gordura">--</div></div>
                    </div>

                    <div class="dashboard-grid" style="grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));">
                        <div class="dash-card"><h4>Massa Gorda (Lípidos)</h4><div class="value" style="color: var(--danger)" id="res-mg">-- kg</div></div>
                        <div class="dash-card"><h4>Massa Magra (Livre de Gordura)</h4><div class="value" style="color: var(--success)" id="res-mm">-- kg</div></div>
                    </div>

                    <div class="ai-suggestions" id="container-recomendacoes">
                        <h4>🩺 Recomendações Clínicas (Automáticas)</h4>
                        <div id="rec-imc" class="rec-item"></div>
                        <div id="rec-rcq" class="rec-item"></div>
                        <div id="rec-bf" class="rec-item"></div>
                    </div>

                    <div class="rt-footer">
                        <strong>Responsabilidade Técnica Clínica (Motor de Decisão)</strong>
                        Luiz André — CREF: 008094-G/RN<br>
                        <em>As recomendações geradas constituem um guia de suporte ao profissional, fundamentadas em algoritmos de saúde. A prescrição final do treinamento é de responsabilidade do profissional titular da avaliação física.</em>
                    </div>

                    <!-- BOTÃO CONSOLIDADO: SALVAR E IMPRIMIR -->
                    <div class="grid-1" style="margin-top: 25px;">
                        <button class="btn btn-success" id="btn-salvar-imprimir" style="font-size: 18px; padding: 18px;" onclick="window.App.salvarEImprimir()">
                            💾 Salvar na Nuvem & 🖨️ Imprimir Relatório
                        </button>
                    </div>
                </div>
            </div>

            <!-- ====== TELA: HISTÓRICO E RELATÓRIOS ====== -->
            <div id="view-history" class="view-section">
                
                <div class="report-filters">
                    <span style="display:flex; align-items:center; font-weight:bold; color:var(--text-muted); margin-right: 10px;">Relatórios:</span>
                    <button class="active" id="btn-filtro-todos" onclick="window.UI.setReportFilter('todos')">Todos</button>
                    <button id="btn-filtro-mes" onclick="window.UI.setReportFilter('mes')">Este Mês</button>
                    <button id="btn-filtro-ano" onclick="window.UI.setReportFilter('ano')">Este Ano</button>
                </div>

                <div class="card">
                    <div class="section-title"><div id="chart-title">📈 Evolução da Base (Todos)</div></div>
                    <div style="height: 350px; width: 100%; position: relative;">
                        <canvas id="evolutionChart"></canvas>
                    </div>
                </div>

                <div class="card">
                    <div class="section-title">📂 Fichas Salvas (Perfil Atual)</div>
                    <input type="text" id="search-history" placeholder="Buscar paciente por nome..." style="margin-bottom: 15px; width: 100%; padding: 10px; border-radius:8px; border: 1px solid var(--border); background: var(--bg-input); color: white;" onkeyup="window.UI.renderHistoryList()">
                    <div class="history-list" id="lista-historico">
                        <p style="color: var(--text-muted)">Carregando dados seguros da nuvem...</p>
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
    const APP_ID = typeof __app_id !== 'undefined' ? __app_id : 'powfit';

    let currentUser = null;
    let profissionais = [];
    let activeProfId = null;
    let avaliacoes = []; 
    let currentReportFilter = 'todos'; 
    let currentEditingId = null;
    let currentCalculatedData = null;
    let currentPhotos = { foto_frente: null, foto_lado: null, foto_costas: null };

    // 1. LOGIN
    window.Auth = {
        login: async () => {
            document.querySelector('.btn-google').innerHTML = 'Conectando...';
            try { await signInWithPopup(auth, provider); } 
            catch (e) {
                document.getElementById('login-error').innerText = "Erro: " + e.message;
                document.getElementById('login-error').style.display = 'block';
                document.querySelector('.btn-google').innerHTML = `Tentar Novamente`;
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
            await window.ProfUI.loadProfissionais();
        } else {
            currentUser = null;
            document.getElementById('login-screen').style.display = 'flex';
            document.getElementById('app-content').style.display = 'none';
            document.getElementById('modal-profissionais').style.display = 'none';
        }
    });

    // 2. PROFISSIONAIS (CADASTRAR E ALTERNAR)
    window.ProfUI = {
        async loadProfissionais() {
            try {
                const colRef = collection(db, "artifacts", APP_ID, "users", currentUser.uid, "profissionais");
                const snapshot = await getDocs(colRef);
                profissionais = [];
                snapshot.forEach(doc => profissionais.push({ id: doc.id, ...doc.data() }));
                
                if (profissionais.length === 0) {
                    this.showSelectionModal();
                    this.showForm(true); 
                } else {
                    this.showSelectionModal();
                    this.renderList();
                }
            } catch(e) { console.error(e); alert("Erro ao carregar banco de profissionais."); }
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
                document.getElementById('btn-show-add-prof').style.display = 'inline-block';
            }
        },
        showForm(force = false) {
            document.getElementById('form-add-prof').style.display = 'block';
            document.getElementById('btn-show-add-prof').style.display = 'none';
            document.getElementById('prof-list-container').style.display = 'none';
            if(force) document.querySelector('#form-add-prof .btn-outline').style.display = 'none';
            else document.querySelector('#form-add-prof .btn-outline').style.display = 'inline-block';
        },
        hideForm() {
            document.getElementById('form-add-prof').style.display = 'none';
            document.getElementById('form-add-prof').reset();
            document.getElementById('btn-show-add-prof').style.display = 'inline-block';
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
            btn.innerText = 'Salvando...'; btn.disabled = true;
            try {
                const payload = {
                    nome: document.getElementById('p_nome').value, tipo: document.getElementById('p_tipo').value,
                    estado: document.getElementById('p_estado').value, cref: document.getElementById('p_tipo').value === 'Treinador Esportivo' ? '' : document.getElementById('p_cref').value
                };
                const colRef = collection(db, "artifacts", APP_ID, "users", currentUser.uid, "profissionais");
                const docRef = await addDoc(colRef, payload);
                profissionais.push({id: docRef.id, ...payload});
                this.selectProfissional(docRef.id);
            } catch(e) { alert("Erro: " + e.message); btn.innerText = 'Salvar Perfil'; btn.disabled = false; }
        },
        async selectProfissional(id) {
            activeProfId = id;
            const prof = profissionais.find(p => p.id === id);
            
            document.getElementById('modal-profissionais').style.display = 'none';
            document.getElementById('app-content').style.display = 'block';
            
            document.getElementById('active-prof-name').innerText = prof.nome;
            document.getElementById('av_nome').value = prof.nome;
            document.getElementById('av_tipo').value = prof.tipo;
            document.getElementById('av_reg').value = prof.tipo === 'Treinador Esportivo' ? prof.estado : `CREF: ${prof.cref} / ${prof.estado}`;
            
            // Texto Legal Dinâmico
            const legalDiv = document.getElementById('av_legal_text');
            if(prof.tipo === 'Treinador Esportivo') {
                legalDiv.innerHTML = "⚖️ <strong>Base Legal de Atuação Esportiva:</strong> Perfil amparado pela Lei Federal nº 9.615/1998 (Lei Pelé) e Art. 5º, XIII da CF/88, focado exclusivamente no treinamento técnico, tático e físico voltado ao esporte e rendimento.";
            } else {
                legalDiv.innerHTML = "⚖️ <strong>Base Legal Clínica e Saúde:</strong> Atuação regulamentada pela Lei Federal nº 9.696/1998. Prerrogativa legal plena para a prestação de serviços de avaliação física, prescrição de exercícios e intervenção voltada à promoção da saúde.";
            }

            document.getElementById('in_data').valueAsDate = new Date();
            await window.DB.loadAvaliacoes();
        }
    };

    // 3. AVALIAÇÕES (DB CRUD)
    window.DB = {
        async loadAvaliacoes() {
            if (!currentUser || !activeProfId) return;
            try {
                const colRef = collection(db, "artifacts", APP_ID, "users", currentUser.uid, "avaliacoes");
                const snapshot = await getDocs(colRef);
                let todas = [];
                snapshot.forEach(doc => todas.push({ id: doc.id, ...doc.data() }));
                avaliacoes = todas.filter(a => a.profId === activeProfId);
                avaliacoes.sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
                window.UI.processReports(); 
            } catch (e) { document.getElementById('lista-historico').innerHTML = `<p style="color:var(--danger)">Erro DB.</p>`; }
        },
        async saveOrUpdate(dataObj) {
            if (!currentUser || !activeProfId) return;
            const payload = { ...dataObj, profId: activeProfId, timestamp: new Date().toISOString() };
            if (currentEditingId) {
                const docRef = doc(db, "artifacts", APP_ID, "users", currentUser.uid, "avaliacoes", currentEditingId);
                await updateDoc(docRef, payload);
            } else {
                const colRef = collection(db, "artifacts", APP_ID, "users", currentUser.uid, "avaliacoes");
                await addDoc(colRef, payload);
            }
            await this.loadAvaliacoes();
        },
        async deleteAvaliacao(id) {
            if(!confirm("⚠️ Excluir ficha definitivamente?")) return;
            try {
                await deleteDoc(doc(db, "artifacts", APP_ID, "users", currentUser.uid, "avaliacoes", id));
                await this.loadAvaliacoes();
            } catch (e) { alert("Erro: " + e.message); }
        }
    };

    // 4. MOTOR CLÍNICO (CÁLCULOS E RECOMENDAÇÕES)
    window.Calc = {
        imc: (p, a) => {
            const val = p / Math.pow(a/100, 2);
            let classif = "Obesidade", type = "danger";
            if(val < 18.5) { classif = "Baixo Peso"; type = "warning"; }
            else if(val < 24.9) { classif = "Peso Normal"; type = "success"; }
            else if(val < 29.9) { classif = "Sobrepeso"; type = "warning"; }
            return { value: val.toFixed(1), classif, type };
        },
        rcq: (c, q, sx) => {
            const val = c / q;
            let classif = "Alto Risco", type = "danger";
            if(sx === 'Masculino') {
                if(val < 0.90) { classif = "Baixo Risco"; type = "success"; }
                else if(val <= 0.95) { classif = "Risco Moderado"; type = "warning"; }
            } else {
                if(val < 0.80) { classif = "Baixo Risco"; type = "success"; }
                else if(val <= 0.85) { classif = "Risco Moderado"; type = "warning"; }
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
                if(b<=18) return {c:"Aceitável/Bom", t:"success"};
                if(b<=24) return {c:"Acima do Ideal", t:"warning"};
                return {c:"Risco Elevado", t:"danger"};
            } else {
                if(b<17) return {c:"Atleta", t:"info"};
                if(b<=25) return {c:"Aceitável/Bom", t:"success"};
                if(b<=31) return {c:"Acima do Ideal", t:"warning"};
                return {c:"Risco Elevado", t:"danger"};
            }
        },
        getRecomendacoes(imcObj, rcqObj, bfObj) {
            // Gera textos médicos/clínicos padronizados
            let r_imc = "", r_rcq = "", r_bf = "";

            if(imcObj.type === 'danger') r_imc = "<p>O quadro indica sobrecarga articular e sistêmica. Recomendada estruturação de rotina com baixo impacto inicial e foco imediato em reeducação e déficit calórico monitorado.</p>";
            else if(imcObj.type === 'warning') r_imc = "<p>Estatura/Peso indica leve descompasso. Sugere-se avaliar a densidade muscular (avaliar se o peso extra é músculo ou gordura) e ajustar a periodização aeróbica/anaeróbica.</p>";
            else if(imcObj.type === 'success') r_imc = "<p>Peso absoluto dentro dos parâmetros seguros para a estatura. A prioridade técnica deve ser a manutenção da flexibilidade, força e estabilização de métricas.</p>";
            else r_imc = "<p>Perfil de baixo peso corporal. Requer prescrição voltada para ganho de massa magra (superávit calórico nutricional e treino resistido tensional).</p>";

            if(rcqObj.type === 'danger') r_rcq = "<p>ALERTA CLÍNICO: A relação aferida demonstra acúmulo patogênico de gordura na região visceral. Indivíduo com probabilidade muito aumentada para desenvolvimento de síndrome metabólica. Exercícios aeróbios e dieta são urgentes.</p>";
            else if(rcqObj.type === 'warning') r_rcq = "<p>Atenção à distribuição da gordura central. É indicado aumentar o volume semanal do treinamento cardiovascular preventivo.</p>";
            else r_rcq = "<p>Distribuição adiposa segura. Baixa incidência de centralização de gordura visceral, o que denota boa saúde cardiovascular primária.</p>";

            if(!bfObj.c || bfObj.t === 'info') r_bf = "<p>Percentual de gordura extremamente baixo, comum em padrões competitivos. Foco na manutenção da integridade hormonal e óssea do paciente.</p>";
            else if(bfObj.t === 'success') r_bf = "<p>Níveis lipídicos corporais correspondentes ao padrão de saúde ideal. Estrutura favorável à otimização de performance e longevidade.</p>";
            else if(bfObj.t === 'warning') r_bf = "<p>Depósitos de gordura ligeiramente acima do recomendado para saúde ideal. Sugere-se protocolo de emagrecimento leve associado à hipertrofia para melhora da taxa metabólica basal.</p>";
            else r_bf = "<p>Excesso evidente de tecido adiposo corporal. O plano de ação deve priorizar a oxidação de lipídios através de treinamento concorrente (Aeróbico + Força) e acompanhamento multidisciplinar obrigatório.</p>";

            return { r_imc, r_rcq, r_bf };
        }
    };

    // 5. UI CONTROLS
    window.UI = {
        switchTab: (id) => {
            document.querySelectorAll('.view-section').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.nav-links button:not(:last-child)').forEach(el => el.classList.remove('active'));
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
            const titles = { 'todos': '📈 Evolução (Base Completa)', 'mes': '📅 Relatório deste Mês', 'ano': '📊 Relatório Anual' };
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
            if(finalArr.length === 0) { list.innerHTML = '<p>Nenhum registro encontrado para este filtro.</p>'; return; }

            finalArr.forEach(item => {
                const d = new Date(item.timestamp).toLocaleDateString();
                list.innerHTML += `
                    <div class="history-item">
                        <div class="hist-info">
                            <strong>${item.inputs.nome}</strong>
                            <small>
                                <span>📅 ${d}</span> <span>⚖️ ${item.inputs.peso}kg</span> <span>🔥 BF: ${item.resultados.bf ? item.resultados.bf+'%' : '--'}</span> <span>🎯 Obj: ${item.inputs.objetivo}</span>
                            </small>
                        </div>
                        <div class="hist-actions">
                            <button class="btn-action-sm btn-edit" onclick="window.App.editar('${item.id}')">✏️ Editar</button>
                            <button class="btn-action-sm btn-print" onclick="window.App.imprimirAntigo('${item.id}')">🖨️ Relatório</button>
                            <button class="btn-action-sm btn-delete" onclick="window.DB.deleteAvaliacao('${item.id}')">🗑️ Excluir</button>
                        </div>
                    </div>`;
            });
        },
        renderChart: (dataArray) => {
            const recent = dataArray.slice(0, 20).reverse(); 
            if(window.myChart) window.myChart.destroy();
            const ctx = document.getElementById('evolutionChart').getContext('2d');
            Chart.defaults.color = '#94a3b8';
            window.myChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: recent.map(h => h.inputs.nome.split(' ')[0] + ' ('+ new Date(h.timestamp).getDate() +'/'+ (new Date(h.timestamp).getMonth()+1) +')'),
                    datasets: [
                        { label: 'Peso Corporal (kg)', data: recent.map(h => h.inputs.peso), borderColor: '#3b82f6', backgroundColor: 'rgba(59,130,246,0.1)', fill:true, tension: 0.3},
                        { label: 'Gordura (%)', data: recent.map(h => h.resultados.bf || 0), borderColor: '#0ea5e9', borderDash:[5,5], tension: 0.3}
                    ]
                },
                options: { responsive: true, maintainAspectRatio: false, interaction: { mode: 'index', intersect: false } }
            });
        }
    };

    // 6. CONTROLADOR DA AVALIAÇÃO (Core)
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

            // Injeta Recomendações
            const recs = window.Calc.getRecomendacoes(resImc, resRcq, classBf);
            document.getElementById('rec-imc').innerHTML = `<strong>• Referente ao Peso Corporal (IMC):</strong> ${recs.r_imc}`;
            document.getElementById('rec-rcq').innerHTML = `<strong>• Referente à Prevenção Cardiovascular (RCQ):</strong> ${recs.r_rcq}`;
            if(resBf) document.getElementById('rec-bf').innerHTML = `<strong>• Referente ao Tecido Adiposo (Gordura):</strong> ${recs.r_bf}`;
            else document.getElementById('rec-bf').innerHTML = "";

            currentCalculatedData = { inputs: i, resultados: { imc: resImc.value, rcq: resRcq.value, bf: resBf, mg: mg, mm: mm } };
            document.getElementById('painel-resultados').scrollIntoView();
        },

        async salvarEImprimir() {
            const btn = document.getElementById('btn-salvar-imprimir');
            btn.innerText = 'Salvando Dados...'; btn.disabled = true;
            try {
                await window.DB.saveOrUpdate(currentCalculatedData);
                btn.innerText = 'Preparando Relatório...';
                const n = document.getElementById('in_nome').value; const t = document.title;
                document.title = `PowFit_Meds_${n ? n.replace(/\s+/g,'_') : 'Relatorio'}`;
                
                setTimeout(() => {
                    window.print();
                    document.title = t;
                    window.App.cancelarEdicaoOuLimpar();
                    window.UI.switchTab('history');
                }, 500);
            } catch(e) { alert("Falha: " + e.message); } 
            finally { btn.innerText = '💾 Salvar na Nuvem & 🖨️ Imprimir Relatório'; btn.disabled = false; }
        },

        editar: (id) => {
            const item = avaliacoes.find(x => x.id === id);
            if(!item) return;
            window.App.cancelarEdicaoOuLimpar(); 
            currentEditingId = id; 
            window.App.setForm(item.inputs);
            
            document.getElementById('edit-banner').style.display = 'block';
            document.getElementById('btn-gerar').innerText = "Atualizar Cálculos";
            window.UI.switchTab('form');
            window.App.processAvaliacao();
        },

        imprimirAntigo: (id) => {
            window.App.editar(id);
            setTimeout(() => {
                const n = document.getElementById('in_nome').value; const t = document.title;
                document.title = `PowFit_Meds_${n ? n.replace(/\s+/g,'_') : 'Relatorio'}`;
                window.print();
                document.title = t;
            }, 600);
        },

        cancelarEdicaoOuLimpar: () => {
            currentEditingId = null; currentCalculatedData = null;
            document.getElementById('formAvaliacao').reset();
            document.getElementById('in_data').valueAsDate = new Date();
            
            // Restaura Profissional
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
            document.getElementById('btn-gerar').innerText = "Calcular Resultados";
            window.scrollTo(0,0);
        }
    };
</script>

</body>
</html>
