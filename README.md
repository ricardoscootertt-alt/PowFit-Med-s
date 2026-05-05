<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PowFit Med's - Avaliação Física Profissional</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        dark: '#0f172a',
                        darker: '#020617',
                        card: '#1e293b',
                        primary: '#2563eb',
                        primaryHover: '#1d4ed8',
                        accent: '#38bdf8'
                    }
                }
            }
        }
    </script>

    <!-- FontAwesome & Google Fonts -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- Libraries (Chart.js & html2pdf) -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0f172a;
            color: #f8fafc;
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #0f172a; }
        ::-webkit-scrollbar-thumb { background: #334155; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #475569; }

        .glass-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .input-dark {
            @apply w-full bg-slate-800 border border-slate-700 rounded-lg px-4 py-2 text-white focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent transition-all;
        }

        .label-dark {
            @apply block text-sm font-medium text-slate-400 mb-1;
        }

        .btn-primary {
            @apply bg-primary hover:bg-primaryHover text-white font-bold py-3 px-4 rounded-lg transition-all transform hover:scale-[1.02] shadow-lg shadow-blue-500/30 flex items-center justify-center gap-2 w-full;
        }
        
        .btn-secondary {
            @apply bg-slate-700 hover:bg-slate-600 text-white font-bold py-3 px-4 rounded-lg transition-all flex items-center justify-center gap-2 w-full;
        }

        /* PDF Print Styles */
        @media print {
            body { background: white !important; color: black !important; }
            .no-print { display: none !important; }
            .glass-card { background: white !important; border: 1px solid #ccc !important; box-shadow: none !important; page-break-inside: avoid;}
            .text-white, .text-slate-400 { color: black !important; }
            .bg-dark { background: white !important; }
            #pdf-content { padding: 0 !important; }
            .print-break { page-break-before: always; }
        }

        /* Loader */
        .loader {
            border: 3px solid #1e293b;
            border-top: 3px solid #38bdf8;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Photo Grid */
        .photo-preview {
            width: 100%; height: 150px; object-fit: cover; border-radius: 0.5rem; border: 2px dashed #334155;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col relative overflow-x-hidden">

    <!-- Toast Notification -->
    <div id="toast" class="fixed top-4 right-4 bg-green-500 text-white px-6 py-3 rounded-lg shadow-lg transform transition-transform translate-x-full z-50 opacity-0 font-medium flex items-center gap-2">
        <i class="fas fa-check-circle"></i> <span id="toast-msg">Sucesso!</span>
    </div>

    <!-- MAIN WRAPPER -->
    <div id="app-container" class="w-full max-w-4xl mx-auto flex-grow flex flex-col relative">
        
        <!-- HEADER -->
        <header class="bg-darker/90 backdrop-blur-md border-b border-slate-800 sticky top-0 z-40 px-4 py-3 flex justify-between items-center">
            <div class="flex items-center gap-2">
                <div class="bg-primary text-white p-2 rounded-lg">
                    <i class="fas fa-heartbeat text-xl"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-blue-400 to-accent">PowFit Med's</h1>
                    <p class="text-[10px] text-slate-400 uppercase tracking-wider">Avaliação Física Pro</p>
                </div>
            </div>
            <div class="flex gap-3">
                <button id="btn-fullscreen" class="text-slate-400 hover:text-white transition-colors p-2" title="Modo Totem">
                    <i class="fas fa-expand"></i>
                </button>
                <div id="user-menu" class="hidden items-center gap-3">
                    <div class="text-right hidden sm:block">
                        <p id="prof-name-display" class="text-sm font-semibold text-white"></p>
                        <p id="prof-type-display" class="text-xs text-accent"></p>
                    </div>
                    <button id="btn-logout" class="bg-red-500/20 text-red-400 hover:bg-red-500 hover:text-white p-2 rounded-lg transition-colors">
                        <i class="fas fa-sign-out-alt"></i>
                    </button>
                </div>
            </div>
        </header>

        <!-- SCREEN 1: LOGIN -->
        <div id="screen-login" class="flex-grow flex items-center justify-center p-4">
            <div class="glass-card p-8 rounded-2xl w-full max-w-md text-center shadow-2xl relative overflow-hidden">
                <div class="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-blue-500 to-accent"></div>
                <i class="fas fa-user-md text-5xl text-accent mb-4"></i>
                <h2 class="text-2xl font-bold mb-2">Acesso Restrito</h2>
                <p class="text-slate-400 mb-8 text-sm">Faça login para gerenciar suas avaliações físicas e pacientes.</p>
                
                <button id="btn-google-login" class="bg-white text-gray-800 hover:bg-gray-100 font-bold py-3 px-4 rounded-lg w-full flex items-center justify-center gap-3 transition-transform hover:scale-105 shadow-md">
                    <img src="https://upload.wikimedia.org/wikipedia/commons/5/53/Google_%22G%22_Logo.svg" alt="Google Logo" class="w-5 h-5">
                    Entrar com Google
                </button>
                <p id="login-error" class="text-red-400 text-sm mt-4 hidden"></p>
            </div>
        </div>

        <!-- SCREEN 2: ONBOARDING PROFISSIONAL -->
        <div id="screen-onboarding" class="hidden flex-grow flex items-center justify-center p-4">
            <div class="glass-card p-6 md:p-8 rounded-2xl w-full max-w-lg shadow-2xl">
                <h2 class="text-2xl font-bold mb-1 text-center text-accent">Cadastro Profissional</h2>
                <p class="text-slate-400 text-sm text-center mb-6">Configure seu perfil para assinar as avaliações.</p>

                <form id="form-onboarding" class="space-y-4">
                    <div>
                        <label class="label-dark">Nome Completo</label>
                        <input type="text" id="prof-name" class="input-dark" required>
                    </div>
                    
                    <div>
                        <label class="label-dark">Atuação Profissional</label>
                        <select id="prof-type" class="input-dark" required>
                            <option value="">Selecione...</option>
                            <option value="EDF">Profissional de Educação Física</option>
                            <option value="TREINADOR">Treinador Esportivo</option>
                        </select>
                    </div>

                    <div id="cref-container" class="hidden">
                        <label class="label-dark">Número do CREF</label>
                        <input type="text" id="prof-cref" class="input-dark" placeholder="000000-G/UF">
                    </div>

                    <div>
                        <label class="label-dark">Estado de Atuação (UF)</label>
                        <select id="prof-uf" class="input-dark" required>
                            <option value="SP">São Paulo</option>
                            <option value="RJ">Rio de Janeiro</option>
                            <option value="RN">Rio Grande do Norte</option>
                            <option value="MG">Minas Gerais</option>
                            <!-- Outros estados omitidos por brevidade, assumindo padrão -->
                            <option value="OUTRO">Outro</option>
                        </select>
                    </div>

                    <div class="bg-blue-900/30 border border-blue-800 rounded-lg p-3 text-xs text-blue-200 mt-4" id="legal-notice">
                        <i class="fas fa-info-circle mr-1"></i> Selecione sua atuação para ver a base legal.
                    </div>

                    <button type="submit" class="btn-primary mt-6">
                        Salvar Perfil <i class="fas fa-arrow-right"></i>
                    </button>
                </form>
            </div>
        </div>

        <!-- SCREEN 3: MAIN APP (DASHBOARD) -->
        <div id="screen-main" class="hidden flex-grow flex flex-col p-4">
            
            <!-- Tabs -->
            <div class="flex bg-slate-800 rounded-lg p-1 mb-6 no-print">
                <button id="tab-new" class="flex-1 py-2 text-sm font-medium rounded-md bg-primary text-white transition-all shadow">Nova Avaliação</button>
                <button id="tab-history" class="flex-1 py-2 text-sm font-medium rounded-md text-slate-400 hover:text-white transition-all">Histórico & Gráficos</button>
            </div>

            <!-- TAB: NOVA AVALIAÇÃO -->
            <div id="view-new-eval" class="space-y-6">
                
                <!-- CONTAINER FOR PDF EXPORT -->
                <div id="pdf-content" class="space-y-6">
                    
                    <!-- Header PDF (Hidden in screen, visible in print) -->
                    <div class="hidden print:flex justify-between items-center border-b-2 border-slate-800 pb-4 mb-6">
                        <div class="flex items-center gap-3">
                            <i class="fas fa-heartbeat text-3xl text-primary"></i>
                            <div>
                                <h1 class="text-2xl font-bold text-slate-800">PowFit Med's</h1>
                                <p class="text-xs text-slate-500 uppercase">Relatório de Avaliação Física</p>
                            </div>
                        </div>
                        <div class="text-right text-sm">
                            <p class="font-bold text-slate-800" id="print-prof-name"></p>
                            <p class="text-slate-500" id="print-prof-cref"></p>
                            <p class="text-slate-500" id="print-date"></p>
                        </div>
                    </div>

                    <!-- Formulário Principal -->
                    <form id="form-evaluation" class="space-y-4">
                        
                        <!-- Dados Pessoais -->
                        <div class="glass-card rounded-xl overflow-hidden shadow-lg border-l-4 border-l-primary">
                            <div class="bg-slate-800/50 p-3 font-semibold flex justify-between items-center cursor-pointer toggle-section">
                                <span><i class="fas fa-user mr-2 text-primary"></i> Dados Pessoais</span>
                                <i class="fas fa-chevron-down text-slate-400"></i>
                            </div>
                            <div class="p-4 grid grid-cols-1 md:grid-cols-3 gap-4">
                                <div class="md:col-span-3">
                                    <label class="label-dark">Nome do Avaliado *</label>
                                    <input type="text" id="eval-name" class="input-dark" required>
                                </div>
                                <div>
                                    <label class="label-dark">Idade *</label>
                                    <input type="number" id="eval-age" class="input-dark" required min="10" max="100">
                                </div>
                                <div>
                                    <label class="label-dark">Peso (kg) *</label>
                                    <input type="number" step="0.1" id="eval-weight" class="input-dark" required>
                                </div>
                                <div>
                                    <label class="label-dark">Altura (cm) *</label>
                                    <input type="number" id="eval-height" class="input-dark" required>
                                </div>
                                <div>
                                    <label class="label-dark">Sexo *</label>
                                    <select id="eval-gender" class="input-dark" required>
                                        <option value="M">Masculino</option>
                                        <option value="F">Feminino</option>
                                    </select>
                                </div>
                                <div class="md:col-span-2">
                                    <label class="label-dark">Protocolo *</label>
                                    <select id="eval-protocol" class="input-dark" required>
                                        <option value="pollock3">Pollock 3 Dobras</option>
                                        <option value="pollock7">Pollock 7 Dobras</option>
                                        <option value="imc">Apenas IMC & Medidas</option>
                                    </select>
                                </div>
                            </div>
                        </div>

                        <!-- Anamnese -->
                        <div class="glass-card rounded-xl overflow-hidden shadow-lg border-l-4 border-l-accent">
                            <div class="bg-slate-800/50 p-3 font-semibold flex justify-between items-center cursor-pointer toggle-section">
                                <span><i class="fas fa-clipboard-list mr-2 text-accent"></i> Anamnese</span>
                                <i class="fas fa-chevron-down text-slate-400"></i>
                            </div>
                            <div class="p-4 grid grid-cols-1 md:grid-cols-2 gap-4 hidden-section hidden">
                                <div>
                                    <label class="label-dark">Objetivo Principal</label>
                                    <select id="eval-objective" class="input-dark">
                                        <option value="emagrecimento">Emagrecimento</option>
                                        <option value="hipertrofia">Hipertrofia</option>
                                        <option value="saude">Qualidade de Vida / Saúde</option>
                                        <option value="performance">Performance Esportiva</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="label-dark">Nível de Atividade</label>
                                    <select id="eval-activity" class="input-dark">
                                        <option value="sedentario">Sedentário</option>
                                        <option value="leve">Leve (1-2x/sem)</option>
                                        <option value="moderado">Moderado (3-4x/sem)</option>
                                        <option value="intenso">Intenso (5+x/sem)</option>
                                    </select>
                                </div>
                                <div class="md:col-span-2">
                                    <label class="label-dark">Medicamentos Contínuos / Observações</label>
                                    <input type="text" id="eval-meds" class="input-dark" placeholder="Ex: Losartana, nenhum...">
                                </div>
                            </div>
                        </div>

                        <!-- Perímetros -->
                        <div class="glass-card rounded-xl overflow-hidden shadow-lg border-l-4 border-l-green-400">
                            <div class="bg-slate-800/50 p-3 font-semibold flex justify-between items-center cursor-pointer toggle-section">
                                <span><i class="fas fa-ruler mr-2 text-green-400"></i> Perímetros (cm)</span>
                                <i class="fas fa-chevron-down text-slate-400"></i>
                            </div>
                            <div class="p-4 grid grid-cols-2 md:grid-cols-4 gap-3 hidden-section hidden">
                                <div><label class="label-dark">Cintura *</label><input type="number" step="0.1" id="p-cintura" class="input-dark" required></div>
                                <div><label class="label-dark">Quadril *</label><input type="number" step="0.1" id="p-quadril" class="input-dark" required></div>
                                <div><label class="label-dark">Pescoço</label><input type="number" step="0.1" id="p-pescoco" class="input-dark"></div>
                                <div><label class="label-dark">Ombros</label><input type="number" step="0.1" id="p-ombros" class="input-dark"></div>
                                <div><label class="label-dark">Tórax</label><input type="number" step="0.1" id="p-torax" class="input-dark"></div>
                                <div><label class="label-dark">Abdominal</label><input type="number" step="0.1" id="p-abdominal" class="input-dark"></div>
                                <div><label class="label-dark">Braço Dir. (R)</label><input type="number" step="0.1" id="p-braco-dr" class="input-dark"></div>
                                <div><label class="label-dark">Braço Esq. (R)</label><input type="number" step="0.1" id="p-braco-er" class="input-dark"></div>
                                <div><label class="label-dark">Braço Dir. (C)</label><input type="number" step="0.1" id="p-braco-dc" class="input-dark"></div>
                                <div><label class="label-dark">Braço Esq. (C)</label><input type="number" step="0.1" id="p-braco-ec" class="input-dark"></div>
                                <div><label class="label-dark">Coxa Med. Dir</label><input type="number" step="0.1" id="p-coxa-md" class="input-dark"></div>
                                <div><label class="label-dark">Coxa Med. Esq</label><input type="number" step="0.1" id="p-coxa-me" class="input-dark"></div>
                                <div><label class="label-dark">Panturrilha Dir</label><input type="number" step="0.1" id="p-pant-d" class="input-dark"></div>
                                <div><label class="label-dark">Panturrilha Esq</label><input type="number" step="0.1" id="p-pant-e" class="input-dark"></div>
                            </div>
                        </div>

                        <!-- Dobras Cutâneas -->
                        <div class="glass-card rounded-xl overflow-hidden shadow-lg border-l-4 border-l-orange-400" id="section-dobras">
                            <div class="bg-slate-800/50 p-3 font-semibold flex justify-between items-center cursor-pointer toggle-section">
                                <span><i class="fas fa-compress-alt mr-2 text-orange-400"></i> Dobras Cutâneas (mm)</span>
                                <i class="fas fa-chevron-down text-slate-400"></i>
                            </div>
                            <div class="p-4 grid grid-cols-2 md:grid-cols-4 gap-3 hidden-section hidden">
                                <div><label class="label-dark">Tríceps</label><input type="number" step="0.1" id="d-triceps" class="input-dark dobra-input"></div>
                                <div><label class="label-dark">Subescapular</label><input type="number" step="0.1" id="d-subescapular" class="input-dark dobra-input"></div>
                                <div><label class="label-dark">Peitoral</label><input type="number" step="0.1" id="d-peitoral" class="input-dark dobra-input"></div>
                                <div><label class="label-dark">Axilar Média</label><input type="number" step="0.1" id="d-axilar" class="input-dark dobra-input"></div>
                                <div><label class="label-dark">Suprailíaca</label><input type="number" step="0.1" id="d-suprailiaca" class="input-dark dobra-input"></div>
                                <div><label class="label-dark">Abdominal</label><input type="number" step="0.1" id="d-abdominal" class="input-dark dobra-input"></div>
                                <div><label class="label-dark">Coxa Frontal</label><input type="number" step="0.1" id="d-coxa" class="input-dark dobra-input"></div>
                            </div>
                        </div>

                        <!-- Fotos -->
                        <div class="glass-card rounded-xl overflow-hidden shadow-lg border-l-4 border-l-purple-400 no-print">
                            <div class="bg-slate-800/50 p-3 font-semibold flex justify-between items-center cursor-pointer toggle-section">
                                <span><i class="fas fa-camera mr-2 text-purple-400"></i> Registro Fotográfico (Local)</span>
                                <i class="fas fa-chevron-down text-slate-400"></i>
                            </div>
                            <div class="p-4 hidden-section hidden">
                                <p class="text-xs text-slate-400 mb-3">Imagens são salvas apenas no seu dispositivo por privacidade.</p>
                                <div class="grid grid-cols-3 gap-2">
                                    <div class="relative">
                                        <label class="block text-center cursor-pointer">
                                            <input type="file" id="photo-frente" accept="image/*" class="hidden photo-upload">
                                            <img id="img-frente" src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100' height='150' fill='%23334155'><rect width='100' height='150'/><text x='50%' y='50%' dominant-baseline='middle' text-anchor='middle' fill='%2394a3b8' font-family='sans-serif' font-size='12'>Frente</text></svg>" class="photo-preview">
                                        </label>
                                    </div>
                                    <div class="relative">
                                        <label class="block text-center cursor-pointer">
                                            <input type="file" id="photo-perfil" accept="image/*" class="hidden photo-upload">
                                            <img id="img-perfil" src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100' height='150' fill='%23334155'><rect width='100' height='150'/><text x='50%' y='50%' dominant-baseline='middle' text-anchor='middle' fill='%2394a3b8' font-family='sans-serif' font-size='12'>Perfil</text></svg>" class="photo-preview">
                                        </label>
                                    </div>
                                    <div class="relative">
                                        <label class="block text-center cursor-pointer">
                                            <input type="file" id="photo-costas" accept="image/*" class="hidden photo-upload">
                                            <img id="img-costas" src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100' height='150' fill='%23334155'><rect width='100' height='150'/><text x='50%' y='50%' dominant-baseline='middle' text-anchor='middle' fill='%2394a3b8' font-family='sans-serif' font-size='12'>Costas</text></svg>" class="photo-preview">
                                        </label>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Actions no-print -->
                        <div class="flex gap-3 mt-6 no-print">
                            <button type="button" id="btn-calculate" class="btn-primary flex-1">
                                <i class="fas fa-calculator"></i> Calcular Resultados
                            </button>
                        </div>
                    </form>

                    <!-- RESULTADOS (Inicialmente ocultos) -->
                    <div id="results-container" class="hidden space-y-4 mt-8 print-break">
                        <h3 class="text-xl font-bold text-center border-b border-slate-700 pb-2"><i class="fas fa-chart-pie text-accent"></i> Diagnóstico Corporal</h3>
                        
                        <!-- Badges Grid -->
                        <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
                            <div class="bg-slate-800 p-4 rounded-xl text-center border border-slate-700 shadow-inner">
                                <p class="text-slate-400 text-xs uppercase tracking-wide">IMC</p>
                                <p class="text-2xl font-bold text-white" id="res-imc">--</p>
                                <span id="badge-imc" class="text-xs px-2 py-1 rounded-full bg-slate-600 text-white mt-1 inline-block">--</span>
                            </div>
                            <div class="bg-slate-800 p-4 rounded-xl text-center border border-slate-700 shadow-inner">
                                <p class="text-slate-400 text-xs uppercase tracking-wide">% Gordura</p>
                                <p class="text-2xl font-bold text-white" id="res-fat-pct">--</p>
                                <span id="badge-fat" class="text-xs px-2 py-1 rounded-full bg-slate-600 text-white mt-1 inline-block">--</span>
                            </div>
                            <div class="bg-slate-800 p-4 rounded-xl text-center border border-slate-700 shadow-inner">
                                <p class="text-slate-400 text-xs uppercase tracking-wide">Massa Magra</p>
                                <p class="text-2xl font-bold text-white" id="res-lean-mass">--</p>
                                <span class="text-xs text-slate-400 mt-1 inline-block">kg</span>
                            </div>
                            <div class="bg-slate-800 p-4 rounded-xl text-center border border-slate-700 shadow-inner">
                                <p class="text-slate-400 text-xs uppercase tracking-wide">Risco RCQ</p>
                                <p class="text-2xl font-bold text-white" id="res-rcq">--</p>
                                <span id="badge-rcq" class="text-xs px-2 py-1 rounded-full bg-slate-600 text-white mt-1 inline-block">--</span>
                            </div>
                        </div>

                        <!-- Recomendações Inteligentes -->
                        <div class="bg-blue-900/20 border border-blue-800 p-4 rounded-xl mt-4 relative overflow-hidden">
                            <div class="absolute top-0 left-0 w-1 h-full bg-blue-500"></div>
                            <h4 class="font-bold text-blue-300 mb-2"><i class="fas fa-brain"></i> Prescrição / Análise Inteligente</h4>
                            <p id="smart-text" class="text-sm text-slate-300 leading-relaxed italic"></p>
                        </div>

                        <!-- Nota do RT Obrigatória -->
                        <div class="bg-slate-800/80 p-3 rounded-lg border border-slate-700 text-xs text-slate-400 text-center mt-6 shadow-inner">
                            <i class="fas fa-user-md mr-1"></i> Responsável pelas recomendações de saúde Clínicas<br>
                            <span class="font-bold text-slate-300">É o RT Luiz André Cref:008094-G/RN</span>
                        </div>
                        
                        <div class="text-xs text-slate-500 text-center mt-2" id="legal-footer">
                            <!-- Inserido via JS baseado no perfil -->
                        </div>
                    </div>

                </div> <!-- End PDF Content -->

                <!-- Ações Pós-Cálculo -->
                <div id="actions-container" class="hidden flex-col md:flex-row gap-3 mt-6 no-print">
                    <button id="btn-save-eval" class="btn-primary bg-green-600 hover:bg-green-700 flex-1">
                        <i class="fas fa-save"></i> Salvar na Nuvem
                    </button>
                    <button id="btn-export-pdf" class="btn-secondary flex-1">
                        <i class="fas fa-file-pdf text-red-400"></i> Gerar PDF
                    </button>
                    <button id="btn-clear" class="bg-slate-800 text-slate-300 hover:bg-slate-700 font-bold py-3 px-4 rounded-lg flex-1">
                        Limpar
                    </button>
                </div>

            </div> <!-- Fim View Nova Avaliação -->


            <!-- TAB: HISTÓRICO & GRÁFICOS -->
            <div id="view-history" class="hidden space-y-6">
                
                <div class="glass-card p-4 rounded-xl">
                    <h3 class="text-lg font-bold mb-4 flex items-center gap-2"><i class="fas fa-users text-primary"></i> Seus Pacientes</h3>
                    <div class="mb-4">
                        <input type="text" id="search-patient" class="input-dark" placeholder="Buscar por nome...">
                    </div>
                    
                    <!-- Lista de Avaliações -->
                    <div id="history-list" class="space-y-3 max-h-60 overflow-y-auto pr-2">
                        <!-- Itens renderizados via JS -->
                        <div class="text-center text-slate-500 py-4"><div class="loader inline-block"></div> Carregando...</div>
                    </div>
                </div>

                <!-- Gráficos -->
                <div id="charts-container" class="hidden glass-card p-4 rounded-xl">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-lg font-bold" id="chart-patient-name">Evolução: ---</h3>
                        <button id="btn-close-charts" class="text-slate-400 hover:text-white"><i class="fas fa-times"></i></button>
                    </div>
                    <div class="w-full h-48 mb-4">
                        <canvas id="weightChart"></canvas>
                    </div>
                    <div class="w-full h-48">
                        <canvas id="fatChart"></canvas>
                    </div>
                </div>

            </div>

        </div> <!-- End Main App -->

    </div>

    <!-- Firebase e Lógica JS -->
    <script type="module">
        // 1. IMPORTAÇÕES FIREBASE (V11)
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithCustomToken, signInAnonymously, onAuthStateChanged, GoogleAuthProvider, signInWithPopup, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, onSnapshot, orderBy, serverTimestamp, getDocs } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // 2. CONFIGURAÇÃO FIREBASE (Canvas Environment Requirement)
        let firebaseConfig;
        let appId = 'powfit-meds-default';
        
        try {
            // Tenta pegar a config injetada do ambiente Canvas
            if (typeof __firebase_config !== 'undefined') {
                firebaseConfig = JSON.parse(__firebase_config);
                if (typeof __app_id !== 'undefined') appId = __app_id;
            } else {
                // Fallback para as credenciais fornecidas na prompt (Apenas para rodar externamente se exportado)
                firebaseConfig = {
                  apiKey: "AIzaSyA9icSOYHPq-5p-GJaFmKZ02DMMFpohk7g",
                  authDomain: "powfit-med-s.firebaseapp.com",
                  projectId: "powfit-med-s",
                  storageBucket: "powfit-med-s.firebasestorage.app",
                  messagingSenderId: "878219813171",
                  appId: "1:878219813171:web:8fb80b8e1192aff1d55e46"
                };
            }
        } catch (e) {
            console.error("Erro na config do Firebase:", e);
        }

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // Variáveis de Estado
        let currentUser = null;
        let professionalData = null;
        let currentEvaluations = [];
        let chartsInstance = { weight: null, fat: null };

        // 3. ELEMENTOS DA DOM
        const DOM = {
            screens: {
                login: document.getElementById('screen-login'),
                onboarding: document.getElementById('screen-onboarding'),
                main: document.getElementById('screen-main')
            },
            tabs: {
                btnNew: document.getElementById('tab-new'),
                btnHistory: document.getElementById('tab-history'),
                viewNew: document.getElementById('view-new-eval'),
                viewHistory: document.getElementById('view-history')
            },
            auth: {
                btnLogin: document.getElementById('btn-google-login'),
                btnLogout: document.getElementById('btn-logout'),
                error: document.getElementById('login-error')
            },
            profile: {
                form: document.getElementById('form-onboarding'),
                nameDisplay: document.getElementById('prof-name-display'),
                typeDisplay: document.getElementById('prof-type-display'),
                typeSelect: document.getElementById('prof-type'),
                crefContainer: document.getElementById('cref-container'),
                legalNotice: document.getElementById('legal-notice')
            },
            evalForm: {
                form: document.getElementById('form-evaluation'),
                btnCalc: document.getElementById('btn-calculate'),
                btnSave: document.getElementById('btn-save-eval'),
                btnExport: document.getElementById('btn-export-pdf'),
                btnClear: document.getElementById('btn-clear'),
                results: document.getElementById('results-container'),
                actions: document.getElementById('actions-container'),
                protocol: document.getElementById('eval-protocol'),
                sectionDobras: document.getElementById('section-dobras')
            }
        };

        // 4. LÓGICA DE AUTENTICAÇÃO (MANDATORY RULES)
        const initAuth = async () => {
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    // Se não estiver no canvas com token, prepara para login manual (Google)
                    // Não loga anonimamente pois o app requer dados profissionais do usuário real.
                }
            } catch (error) {
                console.error("Auth error:", error);
            }
        };

        DOM.auth.btnLogin.addEventListener('click', async () => {
            const provider = new GoogleAuthProvider();
            try {
                await signInWithPopup(auth, provider);
            } catch (error) {
                DOM.auth.error.textContent = "Erro ao fazer login: " + error.message;
                DOM.auth.error.classList.remove('hidden');
            }
        });

        DOM.auth.btnLogout.addEventListener('click', () => signOut(auth));

        onAuthStateChanged(auth, async (user) => {
            currentUser = user;
            if (user) {
                checkProfessionalProfile();
            } else {
                showScreen('login');
                DOM.profile.nameDisplay.textContent = '';
                document.getElementById('user-menu').classList.add('hidden');
            }
        });

        // 5. NAVEGAÇÃO DE TELAS
        function showScreen(screenName) {
            Object.values(DOM.screens).forEach(s => s.classList.add('hidden'));
            if(screenName === 'login') DOM.screens.login.classList.remove('hidden');
            if(screenName === 'onboarding') DOM.screens.onboarding.classList.remove('hidden');
            if(screenName === 'main') {
                DOM.screens.main.classList.remove('hidden');
                document.getElementById('user-menu').classList.remove('hidden');
                document.getElementById('user-menu').classList.add('flex');
            }
        }

        // 6. PERFIL PROFISSIONAL
        async function checkProfessionalProfile() {
            if (!currentUser) return;
            
            // Seguindo a Rule 1 do Firebase: Caminho estrito
            const profRef = doc(db, 'artifacts', appId, 'users', currentUser.uid, 'profile', 'data');
            
            try {
                const docSnap = await getDoc(profRef);
                if (docSnap.exists()) {
                    professionalData = docSnap.data();
                    updateUIWithProfile();
                    showScreen('main');
                    loadHistory();
                } else {
                    showScreen('onboarding');
                }
            } catch (error) {
                console.error("Erro ao buscar perfil", error);
                showToast("Erro ao conectar servidor. Verifique permissões.", true);
            }
        }

        DOM.profile.typeSelect.addEventListener('change', (e) => {
            const isEDF = e.target.value === 'EDF';
            DOM.profile.crefContainer.classList.toggle('hidden', !isEDF);
            document.getElementById('prof-cref').required = isEDF;
            
            if(isEDF) {
                DOM.profile.legalNotice.innerHTML = "<i class='fas fa-gavel mr-1'></i> <b>Profissional de Educação Física:</b> Atuação regulamentada pela Lei Federal nº 9.696/1998.";
            } else if (e.target.value === 'TREINADOR') {
                DOM.profile.legalNotice.innerHTML = "<i class='fas fa-gavel mr-1'></i> <b>Treinador Esportivo:</b> Classificação CBO 2241-20. O CREF não é obrigatório dependendo do escopo esportivo.";
            } else {
                DOM.profile.legalNotice.innerHTML = "<i class='fas fa-info-circle mr-1'></i> Selecione sua atuação para ver a base legal.";
            }
        });

        DOM.profile.form.addEventListener('submit', async (e) => {
            e.preventDefault();
            if (!currentUser) return;

            const profType = document.getElementById('prof-type').value;
            const data = {
                nome: document.getElementById('prof-name').value,
                tipo: profType,
                cref: profType === 'EDF' ? document.getElementById('prof-cref').value : null,
                uf: document.getElementById('prof-uf').value,
                updatedAt: serverTimestamp()
            };

            const profRef = doc(db, 'artifacts', appId, 'users', currentUser.uid, 'profile', 'data');
            try {
                await setDoc(profRef, data);
                professionalData = data;
                showToast("Perfil salvo com sucesso!");
                updateUIWithProfile();
                showScreen('main');
                loadHistory();
            } catch (error) {
                showToast("Erro ao salvar: " + error.message, true);
            }
        });

        function updateUIWithProfile() {
            DOM.profile.nameDisplay.textContent = professionalData.nome;
            DOM.profile.typeDisplay.textContent = professionalData.tipo === 'EDF' ? `CREF: ${professionalData.cref}` : 'Treinador Esportivo';
            
            // Para impressão
            document.getElementById('print-prof-name').textContent = "Avaliador: " + professionalData.nome;
            document.getElementById('print-prof-cref').textContent = professionalData.tipo === 'EDF' ? `CREF: ${professionalData.cref}` : 'Treinador Esportivo';
            
            const legalText = professionalData.tipo === 'EDF' ? 
                "Avaliação chancelada por Profissional de Ed. Física (Lei 9.696/98)" : 
                "Avaliação baseada na CBO 2241-20 (Treinamento Esportivo)";
            document.getElementById('legal-footer').textContent = legalText;
        }

        // 7. UX DA INTERFACE (Sanfonas, Tabs, Fotos)
        document.querySelectorAll('.toggle-section').forEach(header => {
            header.addEventListener('click', () => {
                const content = header.nextElementSibling;
                const icon = header.querySelector('.fa-chevron-down, .fa-chevron-up');
                content.classList.toggle('hidden');
                if(icon) {
                    icon.classList.toggle('fa-chevron-up');
                    icon.classList.toggle('fa-chevron-down');
                }
            });
        });

        DOM.tabs.btnNew.addEventListener('click', () => {
            DOM.tabs.viewNew.classList.remove('hidden');
            DOM.tabs.viewHistory.classList.add('hidden');
            DOM.tabs.btnNew.classList.replace('bg-slate-700', 'bg-primary');
            DOM.tabs.btnNew.classList.replace('text-slate-400', 'text-white');
            DOM.tabs.btnHistory.classList.replace('bg-primary', 'bg-transparent');
            DOM.tabs.btnHistory.classList.replace('text-white', 'text-slate-400');
        });

        DOM.tabs.btnHistory.addEventListener('click', () => {
            DOM.tabs.viewHistory.classList.remove('hidden');
            DOM.tabs.viewNew.classList.add('hidden');
            DOM.tabs.btnHistory.classList.replace('bg-transparent', 'bg-primary');
            DOM.tabs.btnHistory.classList.replace('text-slate-400', 'text-white');
            DOM.tabs.btnNew.classList.replace('bg-primary', 'bg-slate-700');
            DOM.tabs.btnNew.classList.replace('text-white', 'text-slate-400');
        });

        // Alternar visualização de dobras baseado no protocolo
        DOM.evalForm.protocol.addEventListener('change', (e) => {
            const isIMC = e.target.value === 'imc';
            DOM.evalForm.sectionDobras.classList.toggle('hidden', isIMC);
            // Requerimentos
            document.querySelectorAll('.dobra-input').forEach(input => {
                if(!isIMC && e.target.value === 'pollock7') input.required = true;
                else input.required = false; // simplificação
            });
        });

        // Handler de Fotos Locais (Convert to Base64 to show)
        document.querySelectorAll('.photo-upload').forEach(input => {
            input.addEventListener('change', function(e) {
                if (e.target.files && e.target.files[0]) {
                    const reader = new FileReader();
                    reader.onload = function(event) {
                        const imgId = input.id.replace('photo-', 'img-');
                        document.getElementById(imgId).src = event.target.result;
                    }
                    reader.readAsDataURL(e.target.files[0]);
                }
            });
        });

        // 8. MATEMÁTICA E CÁLCULOS FÍSICOS
        const BodyCalc = {
            calcIMC: (w, h_cm) => {
                const h_m = h_cm / 100;
                return (w / (h_m * h_m)).toFixed(1);
            },
            classificarIMC: (imc) => {
                if(imc < 18.5) return { text: "Baixo Peso", color: "bg-yellow-500" };
                if(imc < 24.9) return { text: "Normal", color: "bg-green-500" };
                if(imc < 29.9) return { text: "Sobrepeso", color: "bg-orange-500" };
                return { text: "Obesidade", color: "bg-red-500" };
            },
            calcRCQ: (cintura, quadril) => {
                if(!cintura || !quadril) return null;
                return (cintura / quadril).toFixed(2);
            },
            classificarRCQ: (rcq, gender, age) => {
                // Tabela simplificada padrão
                let limit = gender === 'M' ? 0.90 : 0.85;
                if (rcq < limit) return { text: "Baixo Risco", color: "bg-green-500" };
                if (rcq < limit + 0.05) return { text: "Risco Moderado", color: "bg-orange-500" };
                return { text: "Alto Risco", color: "bg-red-500" };
            },
            calcSiri: (densidade) => {
                return ((495 / densidade) - 450).toFixed(1);
            },
            calcPollock3: (gender, age, d) => {
                // d = array/object of skinfolds
                let sum = 0, densidade = 0;
                if (gender === 'M') {
                    sum = (Number(d.peitoral)||0) + (Number(d.abdominal)||0) + (Number(d.coxa)||0);
                    if(sum === 0) return 0;
                    densidade = 1.1093800 - 0.0008267 * sum + 0.0000016 * (sum * sum) - 0.0002574 * age;
                } else {
                    sum = (Number(d.triceps)||0) + (Number(d.suprailiaca)||0) + (Number(d.coxa)||0);
                    if(sum === 0) return 0;
                    densidade = 1.0994921 - 0.0009929 * sum + 0.0000023 * (sum * sum) - 0.0001392 * age;
                }
                return BodyCalc.calcSiri(densidade);
            },
            calcPollock7: (gender, age, d) => {
                const sum = (Number(d.triceps)||0) + (Number(d.subescapular)||0) + (Number(d.peitoral)||0) + 
                            (Number(d.axilar)||0) + (Number(d.suprailiaca)||0) + (Number(d.abdominal)||0) + (Number(d.coxa)||0);
                if(sum === 0) return 0;
                let densidade = 0;
                if (gender === 'M') {
                    densidade = 1.1120000 - 0.00043499 * sum + 0.00000055 * (sum * sum) - 0.00028826 * age;
                } else {
                    densidade = 1.0970 - 0.00046971 * sum + 0.00000056 * (sum * sum) - 0.00012828 * age;
                }
                return BodyCalc.calcSiri(densidade);
            },
            classificarGordura: (pct, gender) => {
                // Simplificação genérica para fins didáticos
                if(gender === 'M') {
                    if(pct < 8) return {text: "Atleta", color: "bg-blue-500"};
                    if(pct < 15) return {text: "Bom", color: "bg-green-500"};
                    if(pct < 20) return {text: "Normal", color: "bg-yellow-500"};
                    return {text: "Elevado", color: "bg-red-500"};
                } else {
                    if(pct < 15) return {text: "Atleta", color: "bg-blue-500"};
                    if(pct < 23) return {text: "Bom", color: "bg-green-500"};
                    if(pct < 30) return {text: "Normal", color: "bg-yellow-500"};
                    return {text: "Elevado", color: "bg-red-500"};
                }
            }
        };

        // 9. EVENTOS DE CÁLCULO
        let currentEvalData = {};

        DOM.evalForm.btnCalc.addEventListener('click', () => {
            if(!DOM.evalForm.form.checkValidity()) {
                DOM.evalForm.form.reportValidity();
                return;
            }

            const w = Number(document.getElementById('eval-weight').value);
            const h = Number(document.getElementById('eval-height').value);
            const age = Number(document.getElementById('eval-age').value);
            const gender = document.getElementById('eval-gender').value;
            const prot = document.getElementById('eval-protocol').value;

            // Medidas
            const cintura = Number(document.getElementById('p-cintura').value);
            const quadril = Number(document.getElementById('p-quadril').value);

            // Cálculos básicos
            const imc = BodyCalc.calcIMC(w, h);
            const classIMC = BodyCalc.classificarIMC(imc);
            
            let rcq = BodyCalc.calcRCQ(cintura, quadril);
            let classRCQ = rcq ? BodyCalc.classificarRCQ(rcq, gender, age) : {text:"N/A", color:"bg-slate-600"};

            // Gordura
            let fatPct = 0;
            if(prot !== 'imc') {
                const dobras = {
                    triceps: document.getElementById('d-triceps').value,
                    subescapular: document.getElementById('d-subescapular').value,
                    peitoral: document.getElementById('d-peitoral').value,
                    axilar: document.getElementById('d-axilar').value,
                    suprailiaca: document.getElementById('d-suprailiaca').value,
                    abdominal: document.getElementById('d-abdominal').value,
                    coxa: document.getElementById('d-coxa').value,
                };
                
                if(prot === 'pollock3') fatPct = BodyCalc.calcPollock3(gender, age, dobras);
                else if(prot === 'pollock7') fatPct = BodyCalc.calcPollock7(gender, age, dobras);
            }

            const classFat = fatPct > 0 ? BodyCalc.classificarGordura(fatPct, gender) : {text: "N/A", color: "bg-slate-600"};
            const fatMass = fatPct > 0 ? ((w * fatPct) / 100).toFixed(1) : "--";
            const leanMass = fatPct > 0 ? (w - fatMass).toFixed(1) : "--";

            // Atualiza UI
            document.getElementById('res-imc').textContent = imc;
            const bImc = document.getElementById('badge-imc');
            bImc.textContent = classIMC.text; bImc.className = `text-xs px-2 py-1 rounded-full text-white mt-1 inline-block ${classIMC.color}`;

            document.getElementById('res-fat-pct').textContent = fatPct > 0 ? fatPct + "%" : "--";
            const bFat = document.getElementById('badge-fat');
            bFat.textContent = classFat.text; bFat.className = `text-xs px-2 py-1 rounded-full text-white mt-1 inline-block ${classFat.color}`;

            document.getElementById('res-lean-mass').textContent = leanMass;

            document.getElementById('res-rcq').textContent = rcq || "--";
            const bRcq = document.getElementById('badge-rcq');
            bRcq.textContent = classRCQ.text; bRcq.className = `text-xs px-2 py-1 rounded-full text-white mt-1 inline-block ${classRCQ.color}`;

            // Inteligência / Recomendações
            let smart = "";
            const obj = document.getElementById('eval-objective').value;
            
            if(classIMC.text === "Obesidade" || classRCQ.text === "Alto Risco") {
                smart += "⚠️ <b>Atenção Clínica:</b> Há indicativos de risco cardiovascular aumentado (RCQ ou IMC elevados). O foco primário deve ser a melhoria do perfil metabólico. ";
            }
            if(obj === 'emagrecimento' && fatPct > 0) {
                smart += `Para emagrecimento efetivo, sugere-se déficit calórico aliado a treinos de força para preservação dos ${leanMass}kg de massa magra. `;
            } else if (obj === 'hipertrofia') {
                smart += `Foco em superávit calórico e progressão de carga. Manter o percentual de gordura controlado. `;
            } else {
                smart += "Manutenção da saúde geral através de rotina ativa e controle de medidas preventivas. ";
            }
            document.getElementById('smart-text').innerHTML = smart;

            // Mostrar resultados e botões
            DOM.evalForm.results.classList.remove('hidden');
            DOM.evalForm.actions.classList.remove('hidden');
            DOM.evalForm.actions.classList.add('flex');

            // Data impressao
            document.getElementById('print-date').textContent = new Date().toLocaleDateString('pt-BR');

            // Preparar objeto para salvar
            currentEvalData = {
                paciente: document.getElementById('eval-name').value.trim(),
                idade: age,
                peso: w,
                altura: h,
                imc: Number(imc),
                pctGordura: Number(fatPct) || 0,
                massaMagra: Number(leanMass) || 0,
                rcq: Number(rcq) || 0,
                data: new Date().toISOString()
            };
        });

        // 10. SALVAR NO FIREBASE
        DOM.evalForm.btnSave.addEventListener('click', async () => {
            if(!currentUser) return;
            if(!currentEvalData.paciente) return showToast("Calcule os resultados primeiro", true);

            const btn = DOM.evalForm.btnSave;
            btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Salvando...';
            btn.disabled = true;

            try {
                const evalCol = collection(db, 'artifacts', appId, 'users', currentUser.uid, 'evaluations');
                currentEvalData.createdAt = serverTimestamp();
                await addDoc(evalCol, currentEvalData);
                
                showToast("Avaliação salva com sucesso!");
                btn.innerHTML = '<i class="fas fa-check"></i> Salvo';
                setTimeout(() => {
                    btn.innerHTML = '<i class="fas fa-save"></i> Salvar na Nuvem';
                    btn.disabled = false;
                }, 2000);
            } catch(e) {
                console.error("Erro", e);
                showToast("Erro ao salvar.", true);
                btn.innerHTML = '<i class="fas fa-save"></i> Tentar Novamente';
                btn.disabled = false;
            }
        });

        // 11. HISTÓRICO E GRÁFICOS
        function loadHistory() {
            if(!currentUser) return;
            const evalCol = collection(db, 'artifacts', appId, 'users', currentUser.uid, 'evaluations');
            
            // Usando onSnapshot para atualizações em tempo real (Rule 2: queries simples)
            onSnapshot(evalCol, (snapshot) => {
                currentEvaluations = [];
                snapshot.forEach(doc => {
                    currentEvaluations.push({ id: doc.id, ...doc.data() });
                });
                
                // Ordenar no cliente (Evita erro de index no Firestore Rule 2)
                currentEvaluations.sort((a, b) => new Date(b.data) - new Date(a.data));
                renderHistory();
            }, (error) => {
                console.error("Erro histórico:", error);
            });
        }

        function renderHistory(filter = "") {
            const list = document.getElementById('history-list');
            list.innerHTML = '';

            // Agrupar por paciente
            const grouped = {};
            currentEvaluations.forEach(ev => {
                const name = ev.paciente.toLowerCase();
                if(filter && !name.includes(filter.toLowerCase())) return;
                
                if(!grouped[ev.paciente]) grouped[ev.paciente] = [];
                grouped[ev.paciente].push(ev);
            });

            if(Object.keys(grouped).length === 0) {
                list.innerHTML = '<p class="text-slate-500 text-center text-sm py-4">Nenhum paciente encontrado.</p>';
                return;
            }

            for(let paciente in grouped) {
                const evals = grouped[paciente];
                const lastEval = evals[0]; // mais recente
                
                const div = document.createElement('div');
                div.className = "bg-slate-700/50 p-3 rounded-lg flex justify-between items-center border border-slate-600";
                div.innerHTML = `
                    <div>
                        <p class="font-bold text-white text-sm">${paciente}</p>
                        <p class="text-xs text-slate-400">${evals.length} avaliações • Última: ${new Date(lastEval.data).toLocaleDateString()}</p>
                    </div>
                    <button class="bg-primary hover:bg-primaryHover text-white px-3 py-1 rounded text-xs font-bold transition" onclick="window.viewCharts('${paciente}')">
                        <i class="fas fa-chart-line"></i> Gráficos
                    </button>
                `;
                list.appendChild(div);
            }
        }

        document.getElementById('search-patient').addEventListener('input', (e) => {
            renderHistory(e.target.value);
        });

        window.viewCharts = (paciente) => {
            const evals = currentEvaluations.filter(e => e.paciente === paciente).sort((a,b) => new Date(a.data) - new Date(b.data));
            
            document.getElementById('charts-container').classList.remove('hidden');
            document.getElementById('chart-patient-name').textContent = `Evolução: ${paciente}`;

            const labels = evals.map(e => new Date(e.data).toLocaleDateString('pt-BR', {month:'short', year:'2-digit'}));
            const weights = evals.map(e => e.peso);
            const fats = evals.map(e => e.pctGordura);

            // Destroi antigos se existirem
            if(chartsInstance.weight) chartsInstance.weight.destroy();
            if(chartsInstance.fat) chartsInstance.fat.destroy();

            // Setup comum Chart.js para Dark Mode
            Chart.defaults.color = '#94a3b8';
            Chart.defaults.borderColor = '#334155';

            const ctxWeight = document.getElementById('weightChart').getContext('2d');
            chartsInstance.weight = new Chart(ctxWeight, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Peso (kg)',
                        data: weights,
                        borderColor: '#38bdf8',
                        backgroundColor: 'rgba(56, 189, 248, 0.1)',
                        borderWidth: 2,
                        tension: 0.3,
                        fill: true
                    }]
                },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: true } } }
            });

            const ctxFat = document.getElementById('fatChart').getContext('2d');
            chartsInstance.fat = new Chart(ctxFat, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: '% Gordura',
                        data: fats,
                        borderColor: '#ef4444',
                        backgroundColor: 'rgba(239, 68, 68, 0.1)',
                        borderWidth: 2,
                        tension: 0.3,
                        fill: true
                    }]
                },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: true } } }
            });
            
            // Rolar para gráficos (Mobile)
            document.getElementById('charts-container').scrollIntoView({behavior: 'smooth'});
        };

        document.getElementById('btn-close-charts').addEventListener('click', () => {
            document.getElementById('charts-container').classList.add('hidden');
        });

        // 12. EXPORTAÇÃO PDF (html2pdf)
        DOM.evalForm.btnExport.addEventListener('click', () => {
            if(!currentEvalData.paciente) return;
            
            const element = document.getElementById('pdf-content');
            const name = currentEvalData.paciente.replace(/\s+/g, '_');
            const opt = {
                margin:       10,
                filename:     `Avaliacao_${name}_PowFit.pdf`,
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  { scale: 2, useCORS: true, logging: false },
                jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' }
            };

            const btn = DOM.evalForm.btnExport;
            btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Gerando...';
            
            html2pdf().set(opt).from(element).save().then(() => {
                btn.innerHTML = '<i class="fas fa-file-pdf text-red-400"></i> Gerar PDF';
                showToast("PDF gerado com sucesso!");
            });
        });

        // 13. UTILITÁRIOS (Toast, Limpar, Fullscreen)
        function showToast(msg, isError = false) {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').textContent = msg;
            t.className = `fixed top-4 right-4 text-white px-6 py-3 rounded-lg shadow-lg transform transition-transform z-50 font-medium flex items-center gap-2 ${isError ? 'bg-red-500' : 'bg-green-500'}`;
            t.classList.remove('translate-x-full', 'opacity-0');
            setTimeout(() => t.classList.add('translate-x-full', 'opacity-0'), 3000);
        }

        DOM.evalForm.btnClear.addEventListener('click', () => {
            DOM.evalForm.form.reset();
            DOM.evalForm.results.classList.add('hidden');
            DOM.evalForm.actions.classList.add('hidden');
            // reset imagens local
            document.getElementById('img-frente').src = "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100' height='150' fill='%23334155'><rect width='100' height='150'/></svg>";
            document.getElementById('img-perfil').src = "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100' height='150' fill='%23334155'><rect width='100' height='150'/></svg>";
            document.getElementById('img-costas').src = "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100' height='150' fill='%23334155'><rect width='100' height='150'/></svg>";
            window.scrollTo({top: 0, behavior: 'smooth'});
        });

        document.getElementById('btn-fullscreen').addEventListener('click', () => {
            if (!document.fullscreenElement) {
                document.documentElement.requestFullscreen().catch(err => {
                    console.log(`Error attempting to enable fullscreen: ${err.message}`);
                });
            } else {
                document.exitFullscreen();
            }
        });

        // Init routine call (Call Auth first)
        initAuth();

    </script>
</body>
</html>
