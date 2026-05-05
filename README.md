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

        /* TELA DE LOGIN */
        #login-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: linear-gradient(135deg, #060b14, #0f172a);
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            z-index: 1000;
        }
        .login-box {
            background-color: var(--bg-card); padding: 40px; border-radius: var(--radius);
            border: 1px solid var(--border); box-shadow: 0 10px 40px rgba(0,0,0,0.8);
            text-align: center; max-width: 400px; width: 90%;
        }
        .login-box h1 { color: white; margin-bottom: 10px; font-size: 32px; letter-spacing: 1px; }
        .login-box h1 span { color: var(--accent); }
        .login-box p { color: var(--text-muted); margin-bottom: 30px; font-size: 15px; }
        .btn-google {
            background-color: white; color: #333; border: none; padding: 14px 20px;
            border-radius: 8px; font-weight: bold; font-size: 16px; cursor: pointer;
            display: flex; align-items: center; justify-content: center; gap: 12px; width: 100%;
            transition: all 0.2s; box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        .btn-google:hover { background-color: #f1f5f9; transform: translateY(-2px); box-shadow: 0 6px 12px rgba(0,0,0,0.2); }
        .btn-google img { width: 22px; }

        /* APP CONTENT */
        #app-content { display: none; }

        /* NAVBAR */
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
            padding-bottom: 10px; border-bottom: 1px solid var(--border); display: flex; align-items: center; gap: 10px;
        }

        /* GRIDS E FORMS */
        .grid-2 { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; }
        .grid-3 { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 15px; }
        .grid-4 { display: grid; grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); gap: 15px; }
        
        .input-group { display: flex; flex-direction: column; gap: 6px; }
        .input-group label { font-size: 12px; color: var(--text-muted); font-weight: 700; text-transform: uppercase; letter-spacing: 0.5px;}
        input, select, textarea {
            background-color: var(--bg-input); border: 1px solid var(--border); color: var(--text-main);
            padding: 12px; border-radius: 8px; font-size: 15px; transition: all 0.3s; outline: none; width: 100%;
        }
        input:focus, select:focus, textarea:focus { border-color: var(--primary); box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.2); }
        textarea { resize: vertical; min-height: 80px; }
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

        /* DASHBOARD RESULTADOS */
        .dashboard-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 15px; margin-bottom: 20px; }
        .dash-card { background-color: var(--bg-input); padding: 20px; border-radius: var(--radius); text-align: center; border-left: 4px solid var(--primary); position: relative; overflow: hidden;}
        .dash-card h4 { color: var(--text-muted); font-size: 13px; margin-bottom: 10px; text-transform: uppercase; letter-spacing: 0.5px;}
        .dash-card .value { font-size: 32px; font-weight: 800; color: var(--text-main); margin-bottom: 10px; }
        .badge { display: inline-block; padding: 6px 12px; border-radius: 20px; font-size: 12px; font-weight: bold; text-transform: uppercase; }
        
        .badge.success { background: rgba(16, 185, 129, 0.15); color: var(--success); border: 1px solid rgba(16, 185, 129, 0.3);}
        .badge.warning { background: rgba(245, 158, 11, 0.15); color: var(--warning); border: 1px solid rgba(245, 158, 11, 0.3);}
        .badge.danger { background: rgba(239, 68, 68, 0.15); color: var(--danger); border: 1px solid rgba(239, 68, 68, 0.3);}
        .badge.info { background: rgba(59, 130, 246, 0.15); color: var(--primary); border: 1px solid rgba(59, 130, 246, 0.3);}

        .ai-suggestions { background: linear-gradient(to right, rgba(14, 165, 233, 0.1), rgba(59, 130, 246, 0.05)); border-left: 4px solid var(--accent); padding: 20px; border-radius: var(--radius); margin-top: 20px; }
        .ai-suggestions h4 { color: var(--accent); margin-bottom: 10px; display: flex; align-items: center; gap: 8px;}
        .ai-suggestions ul { margin-left: 20px; color: var(--text-main); line-height: 1.8; font-size: 15px;}
        .ai-suggestions li strong { color: var(--primary); }

        /* HISTÓRICO */
        .history-list { margin-top: 20px; display: grid; gap: 15px; }
        .history-item {
            display: flex; justify-content: space-between; align-items: center; background: var(--bg-input);
            padding: 20px; border-radius: var(--radius); border: 1px solid var(--border); flex-wrap: wrap; gap: 15px;
            transition: all 0.2s;
        }
        .history-item:hover { border-color: var(--primary); box-shadow: 0 4px 15px rgba(0,0,0,0.2); }
        .hist-info strong { color: var(--accent); font-size: 18px; display: block; margin-bottom: 5px;}
        .hist-info small { color: var(--text-muted); font-size: 14px; display: flex; gap: 15px; flex-wrap: wrap;}
        .hist-actions { display: flex; gap: 10px; flex-wrap: wrap; }
        .hist-actions button { padding: 8px 16px; font-size: 13px; border-radius: 6px; cursor: pointer; font-weight: bold; border: 1px solid; transition: all 0.2s;}
        
        .btn-edit { background: rgba(59,130,246,0.1); border-color: var(--primary); color: var(--primary); }
        .btn-edit:hover { background: var(--primary); color: white; }
        .btn-print { background: rgba(16,185,129,0.1); border-color: var(--success); color: var(--success); }
        .btn-print:hover { background: var(--success); color: white; }
        .btn-delete { background: rgba(239,68,68,0.1); border-color: var(--danger); color: var(--danger); }
        .btn-delete:hover { background: var(--danger); color: white; }

        /* BANNER EDIÇÃO */
        .edit-mode-banner {
            background-color: rgba(245, 158, 11, 0.1); color: var(--warning); padding: 15px; text-align: center;
            font-weight: bold; border-radius: var(--radius); margin-bottom: 20px; display: none; border: 1px solid var(--warning);
        }

        /* IMPRESSÃO (PDF) */
        @media print {
            body { background: #fff !important; color: #000 !important; font-size: 12px; }
            .navbar, #login-screen, .btn, .hist-actions, #view-history, .photo-upload input, .edit-mode-banner { display: none !important; }
            .container { margin: 0; padding: 0; width: 100%; max-width: 100%; }
            .card { box-shadow: none; border: none; padding: 10px 0; margin-bottom: 10px; background: transparent; page-break-inside: avoid; border-bottom: 1px solid #eee;}
            .section-title { color: #000 !important; border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px;}
            input, select, textarea { background: transparent !important; border: none !important; border-bottom: 1px solid #ccc !important; color: #000 !important; padding: 4px 0 !important; border-radius: 0 !important;}
            .dash-card { background: #f8fafc !important; border: 1px solid #cbd5e1 !important; border-left: 4px solid #000 !important;}
            .dash-card h4 { color: #64748b !important; }
            .dash-card .value { color: #000 !important; }
            .ai-suggestions { background: #fff !important; border: 1px solid #e2e8f0 !important; border-left: 4px solid #000 !important;}
            .ai-suggestions h4, .ai-suggestions li strong { color: #000 !important; }
            .badge { border: 1px solid #000 !important; background: transparent !important; color: #000 !important; }
            .photo-grid { gap: 10px; }
            .photo-upload { border: 1px solid #ccc !important; min-height: 120px; background: transparent !important;}
        }
    </style>
</head>
<body>

    <!-- ================= TELA DE LOGIN ================= -->
    <div id="login-screen">
        <div class="login-box">
            <h1>PowFit <span>Med's</span></h1>
            <p>Plataforma Inteligente de Avaliação Física</p>
            <button class="btn-google" onclick="window.Auth.login()">
                <img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg" alt="Google Logo">
                Entrar com Conta Google
            </button>
            <div id="login-error" style="color: var(--danger); margin-top: 15px; font-size: 14px; display: none;"></div>
        </div>
    </div>

    <!-- ================= CONTEÚDO PRINCIPAL ================= -->
    <div id="app-content">
        <!-- Navbar -->
        <nav class="navbar">
            <div class="logo">PowFit <span>Med's</span></div>
            <div class="nav-links">
                <button id="tab-form" class="active" onclick="window.UI.switchTab('form')">Nova Avaliação</button>
                <button id="tab-hist" onclick="window.UI.switchTab('history')">Meus Pacientes</button>
                <button onclick="window.UI.toggleTotem()">⛶ Modo Totem</button>
            </div>
            <div class="user-info">
                <img id="user-photo" src="" alt="Avatar" style="display:none;">
                <span id="user-name">Carregando...</span>
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
                    
                    <!-- Dados do Avaliador -->
                    <div class="card">
                        <div class="section-title">👨‍⚕️ Dados do Avaliador</div>
                        <div class="grid-3">
                            <div class="input-group">
                                <label>Nome do Avaliador *</label>
                                <input type="text" id="av_nome" placeholder="Seu nome" required onchange="window.App.salvarDadosAvaliador()">
                            </div>
                            <div class="input-group">
                                <label>Registro (CREF/CRM) *</label>
                                <input type="text" id="av_cref" placeholder="Ex: 000000-G" required onchange="window.App.salvarDadosAvaliador()">
                            </div>
                            <div class="input-group">
                                <label>Estado (UF) *</label>
                                <select id="av_estado" required onchange="window.App.salvarDadosAvaliador()">
                                    <option value="">Selecione...</option>
                                    <option value="AC">Acre</option><option value="AL">Alagoas</option><option value="AP">Amapá</option><option value="AM">Amazonas</option><option value="BA">Bahia</option><option value="CE">Ceará</option><option value="DF">Distrito Federal</option><option value="ES">Espírito Santo</option><option value="GO">Goiás</option><option value="MA">Maranhão</option><option value="MT">Mato Grosso</option><option value="MS">Mato Grosso do Sul</option><option value="MG">Minas Gerais</option><option value="PA">Pará</option><option value="PB">Paraíba</option><option value="PR">Paraná</option><option value="PE">Pernambuco</option><option value="PI">Piauí</option><option value="RJ">Rio de Janeiro</option><option value="RN">Rio Grande do Norte</option><option value="RS">Rio Grande do Sul</option><option value="RO">Rondônia</option><option value="RR">Roraima</option><option value="SC">Santa Catarina</option><option value="SP">São Paulo</option><option value="SE">Sergipe</option><option value="TO">Tocantins</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <!-- Dados Clínicos & Anamnese -->
                    <div class="card">
                        <div class="section-title">👤 Cliente e Anamnese</div>
                        <div class="grid-2">
                            <div class="input-group"><label>Nome do Cliente *</label><input type="text" id="in_nome" required></div>
                            <div class="input-group"><label>Data da Avaliação *</label><input type="date" id="in_data" required></div>
                        </div>
                        <div class="grid-4" style="margin-top: 15px;">
                            <div class="input-group"><label>Idade *</label><input type="number" id="in_idade" required></div>
                            <div class="input-group"><label>Peso (kg) *</label><input type="number" step="0.1" id="in_peso" required></div>
                            <div class="input-group"><label>Estatura (cm) *</label><input type="number" id="in_altura" required></div>
                            <div class="input-group"><label>Sexo *</label>
                                <select id="in_sexo"><option value="Masculino">Masculino</option><option value="Feminino">Feminino</option></select>
                            </div>
                        </div>
                        <div class="grid-3" style="margin-top: 15px;">
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
                        <div class="grid-2" style="margin-top: 15px;">
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
                        <div class="section-title" style="justify-content: space-between;">
                            <span>🤏 Dobras Cutâneas (mm)</span>
                            <select id="in_protocolo" style="width: auto; padding: 8px;">
                                <option value="pollock3">Pollock 3 Dobras</option>
                                <option value="pollock7">Pollock 7 Dobras</option>
                                <option value="guedes">Protocolo Guedes</option>
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
                        <div class="section-title">📷 Evolução Fotográfica (Opcional)</div>
                        <p style="color:var(--text-muted); margin-bottom: 15px; font-size:13px;">As fotos são comprimidas automaticamente para economizar espaço no banco de dados.</p>
                        <div class="photo-grid">
                            <div class="photo-upload">
                                <div class="icon">🧍‍♂️</div>
                                <span id="txt-foto-frente">Frente</span>
                                <img id="preview-frente" style="display:none;">
                                <input type="file" accept="image/*" onchange="window.UI.handlePhoto(this, 'preview-frente', 'txt-foto-frente', 'foto_frente')">
                            </div>
                            <div class="photo-upload">
                                <div class="icon">🚶‍♂️</div>
                                <span id="txt-foto-lado">Perfil</span>
                                <img id="preview-lado" style="display:none;">
                                <input type="file" accept="image/*" onchange="window.UI.handlePhoto(this, 'preview-lado', 'txt-foto-lado', 'foto_lado')">
                            </div>
                            <div class="photo-upload">
                                <div class="icon">🧍‍♂️</div>
                                <span id="txt-foto-costas">Costas</span>
                                <img id="preview-costas" style="display:none;">
                                <input type="file" accept="image/*" onchange="window.UI.handlePhoto(this, 'preview-costas', 'txt-foto-costas', 'foto_costas')">
                            </div>
                        </div>
                    </div>

                    <!-- Botões de Ação Formulário -->
                    <div class="grid-2" style="margin-bottom: 30px;">
                        <button type="button" class="btn btn-outline" onclick="window.App.cancelarEdicaoOuLimpar()">Limpar Formulário</button>
                        <button type="submit" class="btn btn-primary" id="btn-gerar">Processar Resultados</button>
                    </div>
                </form>

                <!-- ================= RESULTADOS ================= -->
                <div id="painel-resultados" class="card" style="display: none;">
                    <div class="section-title">📊 Diagnóstico Inteligente e Composição</div>
                    
                    <div class="dashboard-grid">
                        <div class="dash-card">
                            <h4>Índice de Massa (IMC)</h4>
                            <div class="value" id="res-imc">--</div>
                            <div class="badge" id="badge-imc">--</div>
                        </div>
                        <div class="dash-card">
                            <h4>Relação Cint/Quadril</h4>
                            <div class="value" id="res-rcq">--</div>
                            <div class="badge" id="badge-rcq">--</div>
                        </div>
                        <div class="dash-card">
                            <h4>Gordura Corporal</h4>
                            <div class="value" id="res-gordura">--%</div>
                            <div class="badge" id="badge-gordura">--</div>
                        </div>
                    </div>

                    <div class="dashboard-grid" style="grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));">
                        <div class="dash-card">
                            <h4>Massa Gorda (Lípidos)</h4>
                            <div class="value" style="color: var(--danger)" id="res-mg">-- kg</div>
                        </div>
                        <div class="dash-card">
                            <h4>Massa Magra (Livre de Gordura)</h4>
                            <div class="value" style="color: var(--success)" id="res-mm">-- kg</div>
                        </div>
                    </div>

                    <div class="ai-suggestions">
                        <h4>💡 Interpretação Clínica</h4>
                        <ul id="res-dicas"></ul>
                    </div>

                    <div class="grid-2" style="margin-top: 25px;">
                        <button class="btn btn-success" id="btn-salvar" onclick="window.App.salvarNoBanco()">Salvar no Banco de Dados</button>
                        <button class="btn btn-outline" onclick="window.App.imprimirPDF()">Imprimir Relatório (PDF)</button>
                    </div>
                </div>
            </div>

            <!-- ====== TELA: HISTÓRICO ====== -->
            <div id="view-history" class="view-section">
                <div class="card">
                    <div class="section-title">📈 Visão Geral da Base de Clientes</div>
                    <div style="height: 350px; width: 100%; position: relative;">
                        <canvas id="evolutionChart"></canvas>
                    </div>
                </div>

                <div class="card">
                    <div class="section-title">📂 Avaliações Salvas na Nuvem</div>
                    <input type="text" id="search-history" placeholder="Buscar paciente por nome..." style="margin-bottom: 15px;" onkeyup="window.UI.filterHistory()">
                    <div class="history-list" id="lista-historico">
                        <p style="color: var(--text-muted)">Carregando dados seguros da nuvem...</p>
                    </div>
                </div>
            </div>

        </div>
    </div>

<!-- FIREBASE SDK & LÓGICA DO APP (Módulos JS) -->
<script type="module">
    // 1. IMPORTAÇÕES FIREBASE (Versão 11.6.1)
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getAuth, GoogleAuthProvider, signInWithPopup, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
    import { getFirestore, collection, addDoc, getDocs, doc, updateDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

    // 2. CONFIGURAÇÃO (Usando as chaves fornecidas pelo usuário)
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

    // 3. ESTADO GLOBAL
    let currentUser = null;
    let historyArray = [];
    let currentEditingId = null;
    let currentCalculatedData = null;
    let currentPhotos = { foto_frente: null, foto_lado: null, foto_costas: null };

    // 4. AUTENTICAÇÃO E LOGIN
    window.Auth = {
        login: async () => {
            const btn = document.querySelector('.btn-google');
            btn.innerHTML = 'Aguarde... conectando ao Google';
            try {
                await signInWithPopup(auth, provider);
            } catch (error) {
                console.error("Erro Login:", error);
                document.getElementById('login-error').innerText = "Erro: " + error.message;
                document.getElementById('login-error').style.display = 'block';
                btn.innerHTML = `<img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg"> Tentar Novamente`;
            }
        },
        logout: () => signOut(auth).then(() => window.location.reload())
    };

    onAuthStateChanged(auth, (user) => {
        if (user) {
            currentUser = user;
            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('app-content').style.display = 'block';
            document.getElementById('user-name').innerText = user.displayName;
            if(user.photoURL) {
                document.getElementById('user-photo').src = user.photoURL;
                document.getElementById('user-photo').style.display = 'block';
            }
            window.App.carregarDadosAvaliador();
            window.DB.loadHistory();
        } else {
            currentUser = null;
            document.getElementById('login-screen').style.display = 'flex';
            document.getElementById('app-content').style.display = 'none';
        }
    });

    // 5. BANCO DE DADOS (FIRESTORE)
    window.DB = {
        async loadHistory() {
            if (!currentUser) return;
            try {
                // Fetch simple collection (conforme regra de otimização)
                const colRef = collection(db, "powfit_users", currentUser.uid, "avaliacoes");
                const snapshot = await getDocs(colRef);
                historyArray = [];
                snapshot.forEach(doc => historyArray.push({ id: doc.id, ...doc.data() }));
                
                // Sort no Javascript em memória (mais rápido e não exige Index no Firestore)
                historyArray.sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
                
                window.UI.renderHistory();
            } catch (error) {
                console.error("Firestore DB Error:", error);
                document.getElementById('lista-historico').innerHTML = `<p style="color:var(--danger)">Erro de permissão no Firebase. Configure as regras do Firestore.</p>`;
            }
        },

        async saveOrUpdate(dataObj) {
            if (!currentUser) return;
            const btn = document.getElementById('btn-salvar');
            btn.innerText = "Sincronizando com a nuvem...";
            btn.disabled = true;

            try {
                const payload = { ...dataObj, timestamp: new Date().toISOString() };
                
                if (currentEditingId) {
                    const docRef = doc(db, "powfit_users", currentUser.uid, "avaliacoes", currentEditingId);
                    await updateDoc(docRef, payload);
                    alert("✅ Avaliação atualizada com sucesso!");
                } else {
                    const colRef = collection(db, "powfit_users", currentUser.uid, "avaliacoes");
                    await addDoc(colRef, payload);
                    alert("✅ Nova avaliação salva com segurança!");
                }
                
                window.App.cancelarEdicaoOuLimpar();
                await this.loadHistory();
                window.UI.switchTab('history');
                
            } catch (error) {
                alert("Erro ao salvar: " + error.message);
            } finally {
                btn.innerText = "Salvar no Banco de Dados";
                btn.disabled = false;
            }
        },

        async deleteAvaliacao(id) {
            if(!confirm("⚠️ Tem certeza? Esta exclusão não pode ser desfeita.")) return;
            try {
                await deleteDoc(doc(db, "powfit_users", currentUser.uid, "avaliacoes", id));
                await this.loadHistory();
            } catch (e) { alert("Erro ao excluir: " + e.message); }
        }
    };

    // 6. MOTOR DE CÁLCULOS CLÍNICOS
    window.Calc = {
        imc: (p, a) => {
            const val = p / Math.pow(a/100, 2);
            let classif = "Obesidade", type = "danger";
            if(val < 18.5) { classif = "Baixo Peso"; type = "warning"; }
            else if(val < 24.9) { classif = "Eutrofia (Normal)"; type = "success"; }
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
            let den = sx === 'Masculino' 
                ? 1.10938 - (0.0008267*soma) + (0.0000016*soma*soma) - (0.0002574*id)
                : 1.0994921 - (0.0009929*soma) + (0.0000023*soma*soma) - (0.0001392*id);
            return Math.max(3, ((4.95/den) - 4.5)*100).toFixed(1);
        },
        gorduraPollock7: (sx, id, d) => {
            const soma = d.peito + d.triceps + d.sub + d.axilar + d.supra + d.abd + d.coxa;
            if(!soma || soma <= 0) return null;
            let den = sx === 'Masculino'
                ? 1.112 - (0.00043499*soma) + (0.00000055*soma*soma) - (0.00028826*id)
                : 1.097 - (0.00046971*soma) + (0.00000056*soma*soma) - (0.00012828*id);
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
        }
    };

    // 7. CONTROLE DE INTERFACE (UI)
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
                        const canvas = document.createElement('canvas');
                        const ctx = canvas.getContext('2d');
                        const maxW = 300; // Forte compressão para não estourar Firebase (Documento tem limite 1MB)
                        const scale = maxW / img.width;
                        canvas.width = maxW; canvas.height = img.height * scale;
                        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                        const b64 = canvas.toDataURL('image/jpeg', 0.6);
                        
                        document.getElementById(imgId).src = b64;
                        document.getElementById(imgId).style.display = 'block';
                        document.getElementById(txtId).style.display = 'none';
                        currentPhotos[key] = b64;
                    };
                    img.src = e.target.result;
                };
                reader.readAsDataURL(input.files[0]);
            }
        },
        renderHistory: () => {
            window.UI.filterHistory();
            
            // Gerar Gráfico Geral (Médias de peso dos últimos 15)
            const recent = historyArray.slice(0, 15).reverse();
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
        },
        filterHistory: () => {
            const termo = (document.getElementById('search-history')?.value || '').toLowerCase();
            const list = document.getElementById('lista-historico');
            list.innerHTML = '';
            
            const filtered = historyArray.filter(h => h.inputs.nome.toLowerCase().includes(termo));
            
            if(filtered.length === 0) { list.innerHTML = '<p>Nenhum paciente encontrado.</p>'; return; }

            filtered.forEach(item => {
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
                            <button class="btn-edit" onclick="window.App.editar('${item.id}')">✏️ Editar</button>
                            <button class="btn-print" onclick="window.App.imprimirAntigo('${item.id}')">🖨️ Relatório</button>
                            <button class="btn-delete" onclick="window.DB.deleteAvaliacao('${item.id}')">🗑️ Excluir</button>
                        </div>
                    </div>`;
            });
        }
    };

    // 8. CONTROLADOR PRINCIPAL (Aplicação)
    window.App = {
        salvarDadosAvaliador: () => {
            const prof = {
                n: document.getElementById('av_nome').value,
                c: document.getElementById('av_cref').value,
                e: document.getElementById('av_estado').value
            };
            localStorage.setItem('powfit_prof', JSON.stringify(prof));
        },
        carregarDadosAvaliador: () => {
            const prof = JSON.parse(localStorage.getItem('powfit_prof'));
            if(prof) {
                document.getElementById('av_nome').value = prof.n || '';
                document.getElementById('av_cref').value = prof.c || '';
                document.getElementById('av_estado').value = prof.e || '';
            }
            document.getElementById('in_data').valueAsDate = new Date();
        },
        
        getForm: () => {
            return {
                avaliador: { n: document.getElementById('av_nome').value, c: document.getElementById('av_cref').value, e: document.getElementById('av_estado').value },
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
                },
                fotos: currentPhotos
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

            // Fotos
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

            // IA Suggestions
            const ul = document.getElementById('res-dicas'); ul.innerHTML = '';
            if(resImc.type === 'danger') ul.innerHTML += `<li>⚠️ O IMC aponta excesso de peso considerável. É fundamental avaliar a composição estrutural (Massa Magra x Gorda) antes de dietas restritivas.</li>`;
            if(resRcq.type === 'danger') ul.innerHTML += `<li>🚨 <strong>Risco Cardiovascular:</strong> Relação Cintura/Quadril está elevada, indicando gordura visceral. Foco imediato em exercícios cardiovasculares!</li>`;
            if(i.objetivo === 'Hipertrofia' && resBf && parseFloat(resBf) > 20) ul.innerHTML += `<li>💡 Para hipertrofia efetiva, sugere-se reduzir levemente o % de gordura primeiro (melhorando sensibilidade à insulina).</li>`;
            if(i.sono === 'Ruim') ul.innerHTML += `<li>💤 <strong>Atenção:</strong> Sono ruim prejudica regeneração muscular e metabolismo de gordura. Trabalhar higiene do sono.</li>`;
            if(ul.innerHTML === '') ul.innerHTML = `<li>✅ Composição corporal equilibrada. Manter consistência no treinamento de força e ajustes dietéticos segundo o objetivo.</li>`;

            currentCalculatedData = { inputs: i, resultados: { imc: resImc.value, rcq: resRcq.value, bf: resBf, mg: mg, mm: mm } };
            document.getElementById('painel-resultados').scrollIntoView();
        },

        salvarNoBanco: () => window.DB.saveOrUpdate(currentCalculatedData),

        editar: (id) => {
            const item = historyArray.find(x => x.id === id);
            if(!item) return;
            currentEditingId = id;
            window.App.cancelarEdicaoOuLimpar(); // limpa antes
            currentEditingId = id; // restaura
            window.App.setForm(item.inputs);
            
            document.getElementById('edit-banner').style.display = 'block';
            document.getElementById('btn-gerar').innerText = "Atualizar Cálculos";
            document.getElementById('btn-salvar').innerText = "Salvar Alterações no Banco";
            window.UI.switchTab('form');
            window.App.processAvaliacao();
        },

        imprimirAntigo: (id) => {
            window.App.editar(id);
            setTimeout(() => window.App.imprimirPDF(), 500);
        },

        imprimirPDF: () => {
            const n = document.getElementById('in_nome').value;
            const t = document.title;
            document.title = `PowFit_Meds_Avaliacao_${n ? n.replace(/\s+/g,'_') : ''}`;
            window.print();
            document.title = t;
        },

        cancelarEdicaoOuLimpar: () => {
            currentEditingId = null; currentCalculatedData = null;
            document.getElementById('formAvaliacao').reset();
            window.App.carregarDadosAvaliador(); // Restaura dados do prof
            
            // Reseta imagens
            currentPhotos = { foto_frente: null, foto_lado: null, foto_costas: null };
            ['frente', 'lado', 'costas'].forEach(pos => {
                document.getElementById('preview-'+pos).style.display = 'none';
                document.getElementById('txt-foto-'+pos).style.display = 'block';
            });

            document.getElementById('edit-banner').style.display = 'none';
            document.getElementById('painel-resultados').style.display = 'none';
            document.getElementById('btn-gerar').innerText = "Processar Resultados";
            document.getElementById('btn-salvar').innerText = "Salvar no Banco de Dados";
            window.scrollTo(0,0);
        }
    };
</script>

</body>
</html>
