<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="color-scheme" content="dark">
    <title>PowFit Med's - Avaliação Física</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        dark: { 900: '#0B0F19', 800: '#151A2D', 700: '#1F2937' },
                        neon: { blue: '#3B82F6', dark: '#2563EB' }
                    },
                    fontFamily: { sans: ['Inter', 'sans-serif'] }
                }
            }
        }
    </script>
    
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <!-- Icons (Lucide) -->
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <!-- Chart.js & html2pdf -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <style>
        :root { color-scheme: dark; }
        
        body {
            background-color: #0B0F19; 
            color: #F3F4F6;
            -webkit-tap-highlight-color: transparent;
        }
        
        .neon-text { color: #3B82F6; }
        .neon-button {
            background: #3B82F6;
            transition: all 0.2s ease;
        }
        .neon-button:hover { background: #2563EB; }
        
        /* Inputs Base - ESPAÇOSOS */
        .input-base {
            @apply w-full bg-dark-900 border border-gray-700 rounded-xl px-4 py-4 text-white focus:outline-none focus:border-neon-blue focus:ring-1 focus:ring-neon-blue transition-colors;
        }
        .label-base {
            @apply block text-xs font-semibold text-gray-400 uppercase tracking-wider mb-2;
        }

        /* Correção do Autofill */
        input:-webkit-autofill,
        input:-webkit-autofill:hover, 
        input:-webkit-autofill:focus, 
        input:-webkit-autofill:active,
        select:-webkit-autofill,
        select:-webkit-autofill:hover,
        select:-webkit-autofill:focus {
            -webkit-box-shadow: 0 0 0 30px #0B0F19 inset !important;
            -webkit-text-fill-color: #F3F4F6 !important;
            transition: background-color 5000s ease-in-out 0s;
        }
        select option { background-color: #151A2D; color: #F3F4F6; }

        /* --- BLINDAGEM DO HISTÓRICO CONTRA FUNDO BRANCO --- */
        #dash-table-container table,
        #dash-table-container tbody,
        #dash-table-container tr,
        #dash-table-container td {
            background-color: #151A2D !important; /* Cor dark-800 */
            color: #E5E7EB !important; /* Texto claro */
            border-bottom: 1px solid #374151 !important; /* border-gray-700 */
        }
        #dash-table-container thead,
        #dash-table-container th {
            background-color: #0B0F19 !important; /* Cor dark-900 */
            color: #9CA3AF !important; /* Texto cinza */
            border-bottom: 1px solid #374151 !important;
        }
        #dash-table-container tr:hover td {
            background-color: #1F2937 !important; /* Hover dark-700 */
        }
        /* Cores específicas dos botões na tabela blidada */
        #dash-table-container td button.btn-print { background-color: rgba(59, 130, 246, 0.1) !important; color: #3B82F6 !important; }
        #dash-table-container td button.btn-edit { background-color: rgba(234, 179, 8, 0.1) !important; color: #EAB308 !important; }
        #dash-table-container td button.btn-delete { background-color: rgba(239, 68, 68, 0.1) !important; color: #EF4444 !important; }
        
        #dash-table-container td button.btn-print:hover { background-color: rgba(59, 130, 246, 0.2) !important; }
        #dash-table-container td button.btn-edit:hover { background-color: rgba(234, 179, 8, 0.2) !important; }
        #dash-table-container td button.btn-delete:hover { background-color: rgba(239, 68, 68, 0.2) !important; }
        /* --------------------------------------------------- */

        /* Scrollbar customizada */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #0B0F19; }
        ::-webkit-scrollbar-thumb { background: #1F2937; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #3B82F6; }

        /* Loader */
        .loader {
            border: 3px solid rgba(255,255,255,0.1);
            border-left-color: #3B82F6;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Abas */
        .tab-content { display: none; }
        .tab-content.active { display: block; animation: fadeIn 0.3s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="antialiased font-sans min-h-screen flex flex-col relative">

    <!-- OVERLAY DE CARREGAMENTO -->
    <div id="global-loader" class="fixed inset-0 bg-dark-900 bg-opacity-90 z-50 flex flex-col items-center justify-center hidden">
        <div class="loader mb-4" style="width: 48px; height: 48px;"></div>
        <p class="text-neon-blue font-semibold animate-pulse">Processando...</p>
    </div>

    <!-- MODAL DE EXCLUSÃO -->
    <div id="modal-delete" class="fixed inset-0 bg-black/80 z-[60] flex items-center justify-center hidden px-4 backdrop-blur-sm transition-opacity">
        <div class="bg-dark-800 border border-gray-700 p-6 rounded-2xl w-full max-w-sm shadow-2xl text-center">
            <div class="mx-auto w-12 h-12 bg-red-500/20 text-red-500 flex items-center justify-center rounded-full mb-4">
                <i data-lucide="alert-triangle" class="w-6 h-6"></i>
            </div>
            <h3 class="text-xl font-bold text-white mb-2">Excluir Avaliação?</h3>
            <p class="text-gray-400 text-sm mb-6">Esta ação não pode ser desfeita. A ficha será removida permanentemente do histórico.</p>
            <div class="flex gap-3">
                <button onclick="window.closeDeleteModal()" class="flex-1 py-3 bg-dark-700 hover:bg-dark-900 text-white font-semibold rounded-xl transition">Cancelar</button>
                <button id="btn-confirm-delete" class="flex-1 py-3 bg-red-600 hover:bg-red-700 text-white font-semibold rounded-xl transition shadow-lg shadow-red-600/20">Excluir</button>
            </div>
        </div>
    </div>

    <!-- TELA DE LOGIN -->
    <div id="screen-login" class="flex-1 flex flex-col items-center justify-center p-6">
        <div class="max-w-md w-full bg-dark-800 p-8 rounded-3xl shadow-2xl border border-gray-800 text-center">
            <h1 class="text-4xl font-bold tracking-tighter mb-2"><span class="neon-text">PowFit</span> Med's</h1>
            <p class="text-gray-400 text-sm mb-8">Sistema de Avaliação Física</p>
            
            <button id="btn-google-login" class="w-full flex items-center justify-center gap-3 bg-white text-gray-900 font-semibold py-4 px-4 rounded-xl hover:bg-gray-100 transition-colors shadow-lg">
                <svg class="w-6 h-6" viewBox="0 0 24 24"><path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/><path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/><path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/><path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/><path fill="none" d="M1 1h22v22H1z"/></svg>
                Entrar com Google
            </button>
        </div>
    </div>

    <!-- TELA DE SETUP DE PERFIL -->
    <div id="screen-profile-setup" class="hidden flex-1 flex flex-col items-center justify-center p-6">
        <div class="max-w-md w-full bg-dark-800 p-8 rounded-3xl shadow-2xl border border-gray-800">
            <h2 class="text-2xl font-bold mb-6 text-white border-b border-gray-700 pb-2">Complete seu Perfil</h2>
            <form id="form-profile" class="space-y-6">
                <div>
                    <label class="label-base">Nome Completo (RT)</label>
                    <input type="text" id="prof-nome" class="input-base" placeholder="Ex: Luiz André">
                </div>
                <div>
                    <label class="label-base">Tipo de Atuação</label>
                    <select id="prof-tipo" class="input-base">
                        <option value="Treinador Esportivo">Treinador Esportivo</option>
                        <option value="Profissional de Educação Física">Profissional de Educação Física</option>
                    </select>
                </div>
                <div id="div-cref" class="hidden">
                    <label class="label-base">Registro CREF</label>
                    <input type="text" id="prof-cref" class="input-base" placeholder="Ex: 008094-G/RN">
                </div>
                <div>
                    <label class="label-base">Estado (UF)</label>
                    <input type="text" id="prof-uf" class="input-base" placeholder="Ex: RN" maxlength="2">
                </div>
                <button type="submit" class="w-full neon-button text-white font-bold py-4 rounded-xl mt-4 text-lg">Salvar Perfil</button>
            </form>
        </div>
    </div>

    <!-- NAVBAR SECUNDÁRIA -->
    <nav id="navbar" class="hidden bg-dark-800 border-b border-gray-800 sticky top-0 z-40">
        <div class="max-w-4xl mx-auto px-4 py-4 flex justify-between items-center">
            <div class="font-bold text-xl cursor-pointer" onclick="navigate('dashboard')">
                <span class="neon-text">PowFit</span>
            </div>
            <button id="btn-logout" class="p-2 text-gray-400 hover:text-red-400 transition"><i data-lucide="log-out"></i></button>
        </div>
    </nav>

    <!-- TELA DASHBOARD -->
    <div id="screen-dashboard" class="hidden flex-1 max-w-4xl w-full mx-auto p-6 flex flex-col">
        
        <div class="mb-8 mt-2 border-b border-gray-800 pb-6">
            <h1 class="text-3xl font-bold text-white mb-2" id="dash-greeting">Olá, ...</h1>
            <p class="text-gray-400 text-base mb-8">Gerencie as suas avaliações e clientes.</p>
            
            <button onclick="window.newEvaluation()" class="neon-button text-white px-6 py-3 rounded-xl font-bold flex items-center gap-2 shadow-lg w-max hover:scale-105 transition">
                <i data-lucide="plus" class="w-5 h-5"></i> Nova Avaliação
            </button>
        </div>
        
        <div class="bg-dark-800 border border-gray-700/50 rounded-2xl p-6 w-56 mb-8 shadow-sm">
            <p class="text-gray-400 text-sm mb-3 font-semibold uppercase tracking-wider">Total de Fichas</p>
            <p class="text-5xl font-black text-white" id="dash-total-evals">0</p>
        </div>

        <div class="bg-dark-800 border border-gray-700/50 rounded-2xl p-6 flex-1 shadow-sm">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6 gap-4">
                <h2 class="text-xl font-bold text-white flex items-center gap-2">
                    <i data-lucide="history" class="w-5 h-5 text-neon-blue"></i> Histórico Recente
                </h2>
                <input type="text" id="dash-search" placeholder="Procurar cliente..." class="bg-dark-900 border border-gray-700 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-neon-blue w-full sm:w-64">
            </div>
            
            <!-- CONTÊINER BLINDADO DA TABELA -->
            <div id="dash-table-container" class="overflow-x-auto rounded-xl border border-gray-700/50">
                <table class="w-full text-left border-collapse min-w-[650px]">
                    <thead>
                        <tr>
                            <th class="py-4 px-4 font-semibold text-sm">Data</th>
                            <th class="py-4 px-4 font-semibold text-sm">Cliente</th>
                            <th class="py-4 px-4 font-semibold text-sm">Objetivo</th>
                            <th class="py-4 px-4 font-semibold text-sm text-center">Ações</th>
                        </tr>
                    </thead>
                    <tbody id="evaluations-list" class="text-sm">
                        <tr><td colspan="4" class="text-center py-10">Carregando...</td></tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- TELA FORMULÁRIO DE AVALIAÇÃO -->
    <div id="screen-evaluation-form" class="hidden flex-1 max-w-2xl w-full mx-auto p-4 flex flex-col">
        <div class="flex items-center gap-3 mb-8 mt-2">
            <button onclick="navigate('dashboard')" class="text-gray-400 hover:text-white p-2 rounded-full hover:bg-dark-800"><i data-lucide="arrow-left"></i></button>
            <h2 class="text-2xl font-bold" id="form-title">Nova Avaliação</h2>
        </div>

        <!-- Abas Navegação -->
        <div class="flex overflow-x-auto mb-8 bg-dark-800 rounded-xl p-1 border border-gray-700 shadow-sm">
            <button class="flex-1 py-3 px-2 text-sm font-bold rounded-lg text-neon-blue bg-dark-700 shadow eval-tab-btn transition" data-target="tab-perfil">Perfil</button>
            <button class="flex-1 py-3 px-2 text-sm font-bold rounded-lg text-gray-400 hover:text-white eval-tab-btn transition" data-target="tab-anamnese">Anamnese</button>
            <button class="flex-1 py-3 px-2 text-sm font-bold rounded-lg text-gray-400 hover:text-white eval-tab-btn transition" data-target="tab-dobras">Dobras</button>
            <button class="flex-1 py-3 px-2 text-sm font-bold rounded-lg text-gray-400 hover:text-white eval-tab-btn transition" data-target="tab-perimetros">Medidas</button>
        </div>

        <!-- Form: Tudo opcional -->
        <form id="form-evaluation" class="bg-dark-800 p-6 pb-36 rounded-2xl border border-gray-700 shadow-xl relative mb-24">
            
            <!-- ABA 1: PERFIL -->
            <div id="tab-perfil" class="tab-content active space-y-8">
                <div>
                    <label class="label-base text-neon-blue">Nome do Avaliado</label>
                    <input type="text" id="eval-nome" class="input-base border-neon-blue bg-dark-900/50" placeholder="Nome do cliente">
                </div>
                <div class="grid grid-cols-2 gap-6">
                    <div>
                        <label class="label-base">Idade</label>
                        <input type="number" id="eval-idade" class="input-base" min="1" placeholder="Ex: 30">
                    </div>
                    <div>
                        <label class="label-base">Sexo</label>
                        <select id="eval-sexo" class="input-base">
                            <option value="Masculino">Masculino</option>
                            <option value="Feminino">Feminino</option>
                        </select>
                    </div>
                    <div>
                        <label class="label-base">Massa Corporal (kg)</label>
                        <input type="number" step="0.1" id="eval-peso" class="input-base" placeholder="Ex: 80.5">
                    </div>
                    <div>
                        <label class="label-base">Estatura (cm)</label>
                        <input type="number" step="0.1" id="eval-estatura" class="input-base" placeholder="Ex: 175">
                    </div>
                </div>
                <div class="pt-2 border-t border-gray-700/50 mt-4">
                    <label class="label-base text-gray-300">Protocolo de Composição Corporal</label>
                    <select id="eval-protocolo" class="input-base bg-dark-800">
                        <option value="pollock3">Pollock 3 Dobras</option>
                        <option value="pollock7">Pollock 7 Dobras</option>
                        <option value="guedes">Guedes</option>
                        <option value="imc">Apenas IMC (Sem dobras)</option>
                    </select>
                </div>
            </div>

            <!-- ABA 2: ANAMNESE -->
            <div id="tab-anamnese" class="tab-content space-y-8">
                <div>
                    <label class="label-base">Objetivo Principal</label>
                    <select id="ana-objetivo" class="input-base">
                        <option value="Emagrecimento">Emagrecimento</option>
                        <option value="Hipertrofia">Hipertrofia</option>
                        <option value="Saúde/Qualidade de Vida">Saúde / Qualidade de Vida</option>
                        <option value="">Não informado</option>
                    </select>
                </div>
                <div>
                    <label class="label-base">Nível de Atividade</label>
                    <select id="ana-atividade" class="input-base">
                        <option value="Sedentário">Sedentário</option>
                        <option value="Levemente Ativo">Levemente Ativo (1-2x/sem)</option>
                        <option value="Moderadamente Ativo">Moderadamente Ativo (3-4x/sem)</option>
                        <option value="Muito Ativo">Muito Ativo (5+x/sem)</option>
                        <option value="">Não informado</option>
                    </select>
                </div>
                <div>
                    <label class="label-base">Histórico de Lesões / Dores</label>
                    <textarea id="ana-lesoes" class="input-base h-28 resize-none" placeholder="Opcional. Relate histórico de dores..."></textarea>
                </div>
                <div>
                    <label class="label-base">Medicamentos / Suplementos</label>
                    <input type="text" id="ana-meds" class="input-base" placeholder="Ex: Creatina, Whey...">
                </div>
                <div>
                    <label class="label-base">Qualidade do Sono</label>
                    <select id="ana-sono" class="input-base">
                        <option value="Boa">Boa (7-8h reparadoras)</option>
                        <option value="Regular">Regular (Despertares)</option>
                        <option value="Ruim">Ruim (Insônia, <5h)</option>
                        <option value="">Não informado</option>
                    </select>
                </div>
            </div>

            <!-- ABA 3: DOBRAS CUTÂNEAS (Modelo Ficha 3 Medidas) -->
            <div id="tab-dobras" class="tab-content">
                <div class="bg-blue-900/10 border border-blue-900/50 text-blue-300 text-sm p-4 rounded-xl mb-6 flex items-start gap-3">
                    <i data-lucide="info" class="w-5 h-5 flex-shrink-0 mt-0.5"></i>
                    <p>Preencha até 3 medições em milímetros (mm). O sistema calculará a <strong>Média</strong> automaticamente no campo "Res".</p>
                </div>
                
                <div class="space-y-4" id="dobras-container">
                    <!-- Gerado via JS para manter o HTML limpo -->
                </div>
            </div>

            <!-- ABA 4: PERÍMETROS (Adaptado para Ficha Antropométrica) -->
            <div id="tab-perimetros" class="tab-content">
                <h3 class="text-sm font-bold text-gray-400 border-b border-gray-700 pb-2 mb-6 uppercase tracking-wider">Tronco & Geral (cm)</h3>
                <div class="grid grid-cols-2 gap-6 mb-8">
                    <div><label class="label-base">Cintura</label><input type="number" step="0.1" id="per-cin" class="input-base"></div>
                    <div><label class="label-base">Abdómen</label><input type="number" step="0.1" id="per-abd" class="input-base"></div>
                    <div><label class="label-base">Quadril</label><input type="number" step="0.1" id="per-qua" class="input-base"></div>
                    <div><label class="label-base">Pescoço</label><input type="number" step="0.1" id="per-pes" class="input-base"></div>
                    <div><label class="label-base text-gray-500">Tórax</label><input type="number" step="0.1" id="per-tor" class="input-base"></div>
                </div>

                <h3 class="text-sm font-bold text-gray-400 border-b border-gray-700 pb-2 mb-6 mt-10 uppercase tracking-wider">Membros Superiores (cm)</h3>
                <div class="grid grid-cols-2 gap-6 mb-8">
                    <div><label class="label-base">Bíceps Relax. (E)</label><input type="number" step="0.1" id="per-brre-e" class="input-base"></div>
                    <div><label class="label-base">Bíceps Relax. (D)</label><input type="number" step="0.1" id="per-brre-d" class="input-base"></div>
                    <div><label class="label-base">Bíceps Cont. (E)</label><input type="number" step="0.1" id="per-brco-e" class="input-base"></div>
                    <div><label class="label-base">Bíceps Cont. (D)</label><input type="number" step="0.1" id="per-brco-d" class="input-base"></div>
                    <div><label class="label-base text-gray-500">Antebraço (E)</label><input type="number" step="0.1" id="per-ante-e" class="input-base"></div>
                    <div><label class="label-base text-gray-500">Antebraço (D)</label><input type="number" step="0.1" id="per-ante-d" class="input-base"></div>
                </div>

                <h3 class="text-sm font-bold text-gray-400 border-b border-gray-700 pb-2 mb-6 mt-10 uppercase tracking-wider">Membros Inferiores (cm)</h3>
                <div class="grid grid-cols-2 gap-6 mb-8">
                    <div><label class="label-base">Coxa Superior (E)</label><input type="number" step="0.1" id="per-cxpr-e" class="input-base"></div>
                    <div><label class="label-base">Coxa Superior (D)</label><input type="number" step="0.1" id="per-cxpr-d" class="input-base"></div>
                    <div><label class="label-base">Panturrilha (E)</label><input type="number" step="0.1" id="per-pan-e" class="input-base"></div>
                    <div><label class="label-base">Panturrilha (D)</label><input type="number" step="0.1" id="per-pan-d" class="input-base"></div>
                    <div><label class="label-base text-gray-500">Coxa Medial (E)</label><input type="number" step="0.1" id="per-cxme-e" class="input-base"></div>
                    <div><label class="label-base text-gray-500">Coxa Medial (D)</label><input type="number" step="0.1" id="per-cxme-d" class="input-base"></div>
                </div>

                <h3 class="text-sm font-bold text-gray-400 border-b border-gray-700 pb-2 mb-6 mt-10 uppercase tracking-wider">Outras Medidas / Observações</h3>
                <div>
                    <textarea id="per-outras" class="input-base h-28 resize-none" placeholder="Anote aqui outras circunferências ou observações corporais do avaliado..."></textarea>
                </div>
            </div>

            <!-- Botão Fixo Bottom -->
            <div class="fixed bottom-0 left-0 w-full bg-[#0B0F19]/90 backdrop-blur-md border-t border-gray-800 p-5 pb-8 z-20 flex gap-4 shadow-[0_-10px_30px_rgba(0,0,0,0.5)]">
                <button type="button" onclick="navigate('dashboard')" class="flex-1 bg-dark-700 text-white font-bold py-4 rounded-xl hover:bg-dark-600 transition shadow-sm">Cancelar</button>
                <button type="submit" class="flex-1 neon-button text-white font-bold py-4 rounded-xl flex items-center justify-center gap-2 shadow-lg">
                    <i data-lucide="save" class="w-5 h-5"></i> Salvar Avaliação
                </button>
            </div>
        </form>
    </div>

    <!-- TELA RESULTADOS / RELATÓRIO PDF -->
    <div id="screen-results" class="hidden flex-1 w-full max-w-4xl mx-auto p-4 flex flex-col pb-10">
        
        <div class="flex justify-between items-center mb-6 no-print mt-2">
            <button onclick="navigate('dashboard')" class="text-gray-400 hover:text-white flex items-center gap-2 font-medium bg-dark-800 px-4 py-2 rounded-lg"><i data-lucide="arrow-left" class="w-4 h-4"></i> Voltar</button>
            
            <div class="flex gap-3">
                <button onclick="window.editCurrentEvaluation()" class="bg-yellow-500/20 text-yellow-500 hover:bg-yellow-500 hover:text-white px-4 py-2 rounded-lg font-bold flex items-center gap-2 text-sm transition">
                    <i data-lucide="edit" class="w-4 h-4"></i> Editar
                </button>
                <button onclick="window.generatePDF()" class="bg-blue-600 hover:bg-blue-700 text-white px-5 py-2 rounded-lg font-bold flex items-center gap-2 text-sm shadow-lg transition">
                    <i data-lucide="printer" class="w-4 h-4"></i> Imprimir
                </button>
            </div>
        </div>

        <!-- CONTEÚDO PARA PDF (Fundo Branco para impressão Limpa) -->
        <div id="pdf-content" class="bg-white text-gray-900 p-8 rounded-xl shadow-2xl relative">
            
            <!-- Header Relatório -->
            <div class="border-b-4 border-dark-900 pb-4 mb-6 flex justify-between items-end">
                <div>
                    <h1 class="text-3xl font-black tracking-tighter text-dark-900">PowFit <span class="text-blue-600">Med's</span></h1>
                    <p class="text-[10px] text-gray-500 uppercase font-bold tracking-widest mt-1">Relatório de Composição Corporal</p>
                </div>
                <div class="text-right">
                    <p class="text-sm font-bold text-gray-600" id="res-data">DD/MM/YYYY</p>
                </div>
            </div>

            <!-- Dados Cliente -->
            <div class="bg-gray-50 p-5 rounded-xl mb-6 grid grid-cols-2 md:grid-cols-4 gap-y-4 gap-x-2 text-sm border border-gray-200 shadow-sm">
                <div class="col-span-2"><span class="text-gray-400 block text-[10px] uppercase font-bold tracking-wider">Avaliador</span><strong id="res-rt-nome" class="text-gray-800">-</strong></div>
                <div class="col-span-2"><span class="text-gray-400 block text-[10px] uppercase font-bold tracking-wider">Avaliado</span><strong id="res-nome" class="text-xl text-dark-900">-</strong></div>
                <div><span class="text-gray-400 block text-[10px] uppercase font-bold tracking-wider">Idade</span><strong id="res-idade" class="text-gray-800">-</strong></div>
                <div><span class="text-gray-400 block text-[10px] uppercase font-bold tracking-wider">Sexo</span><strong id="res-sexo" class="text-gray-800">-</strong></div>
                <div><span class="text-gray-400 block text-[10px] uppercase font-bold tracking-wider">Massa Corporal</span><strong id="res-peso" class="text-gray-800">- kg</strong></div>
                <div><span class="text-gray-400 block text-[10px] uppercase font-bold tracking-wider">Estatura</span><strong id="res-estatura" class="text-gray-800">- cm</strong></div>
            </div>

            <!-- Cards de Resultados Principais -->
            <h3 class="text-lg font-black text-dark-900 mb-3 border-b-2 border-gray-100 pb-1 uppercase tracking-tight">Análise Quantitativa</h3>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
                
                <div class="border rounded-2xl p-5 text-center shadow-sm" id="card-imc">
                    <span class="text-[10px] text-gray-500 font-bold uppercase tracking-wider block mb-1">IMC</span>
                    <div class="text-3xl font-black text-dark-900" id="res-val-imc">-</div>
                    <span class="text-xs font-bold px-2 py-1 rounded mt-2 inline-block" id="res-class-imc">-</span>
                </div>

                <div class="border rounded-2xl p-5 text-center shadow-sm border-blue-200 bg-blue-50/50">
                    <span class="text-[10px] text-blue-600 font-bold uppercase tracking-wider block mb-1">% Gordura (<span id="res-prot-label" class="text-[8px]"></span>)</span>
                    <div class="text-3xl font-black text-blue-700" id="res-val-bf">-</div>
                    <span class="text-[11px] text-gray-600 mt-2 block font-bold" id="res-peso-gordo">Massa Gorda: - kg</span>
                </div>

                <div class="border rounded-2xl p-5 text-center shadow-sm border-green-200 bg-green-50/50">
                    <span class="text-[10px] text-green-700 font-bold uppercase tracking-wider block mb-1">Massa Magra</span>
                    <div class="text-3xl font-black text-green-800" id="res-val-lbm">- %</div>
                    <span class="text-[11px] text-gray-600 mt-2 block font-bold" id="res-peso-magro">Peso Magro: - kg</span>
                </div>

                <div class="border rounded-2xl p-5 text-center shadow-sm" id="card-rcq">
                    <span class="text-[10px] text-gray-500 font-bold uppercase tracking-wider block mb-1">Risco Cardio (RCQ)</span>
                    <div class="text-3xl font-black text-dark-900" id="res-val-rcq">-</div>
                    <span class="text-xs font-bold px-2 py-1 rounded mt-2 inline-block" id="res-class-rcq">-</span>
                </div>
            </div>

            <!-- Gráfico Evolução -->
            <div class="mb-8 hidden" id="chart-container">
                <h3 class="text-lg font-black text-dark-900 mb-3 border-b-2 border-gray-100 pb-1 uppercase tracking-tight">Evolução Histórica</h3>
                <div class="h-64 w-full">
                    <canvas id="evolutionChart"></canvas>
                </div>
            </div>

            <!-- Recomendações App -->
            <div class="bg-gray-50 border border-gray-200 p-5 rounded-2xl mb-8 shadow-sm">
                <h3 class="text-xs font-black text-dark-900 mb-2 flex items-center gap-2 uppercase tracking-widest">
                    <i data-lucide="brain" class="w-4 h-4 text-purple-600"></i> Parecer Automatizado
                </h3>
                <p id="res-recomendacao" class="text-sm text-gray-700 leading-relaxed font-medium"></p>
            </div>

            <!-- Disclaimer Legal -->
            <div class="mt-12 border-t border-gray-300 pt-6 text-center">
                <p class="text-xs font-black text-dark-900 mb-1">
                    Responsável Técnico: <span id="legal-rt-name" class="text-blue-700"></span>
                </p>
                <p class="text-[9px] text-gray-500 max-w-2xl mx-auto leading-tight">
                    Documento gerado eletronicamente via PowFit Med's. A atuação de Treinador Esportivo e Profissional de Educação Física segue as diretrizes da Lei nº 9.696/1998. Os valores apresentados são estimativas indiretas baseadas em protocolos preditivos.
                </p>
            </div>
            
        </div>
    </div>

    <!-- ================= LÓGICA DA APLICAÇÃO ================= -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, GoogleAuthProvider, signInWithPopup, onAuthStateChanged, signOut, signInWithCustomToken, signInAnonymously } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, enableIndexedDbPersistence, doc, setDoc, getDoc, collection, addDoc, updateDoc, deleteDoc, query, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
            apiKey: "AIzaSyA9icSOYHPq-5p-GJaFmKZ02DMMFpohk7g",
            authDomain: "powfit-med-s.firebaseapp.com",
            projectId: "powfit-med-s",
            storageBucket: "powfit-med-s.firebasestorage.app",
            messagingSenderId: "878219813171",
            appId: "1:878219813171:web:8fb80b8e1192aff1d55e46"
        };
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'powfit-med-s';

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // Habilitar Persistência Offline
        enableIndexedDbPersistence(db).catch((err) => console.warn("Offline desabilitado:", err.code));

        const AppState = { user: null, profile: null, evaluations: [], chartInstance: null, currentEvalId: null, editingId: null };

        const ui = {
            loader: document.getElementById('global-loader'),
            screens: {
                'login': document.getElementById('screen-login'),
                'profile-setup': document.getElementById('screen-profile-setup'),
                'dashboard': document.getElementById('screen-dashboard'),
                'evaluation-form': document.getElementById('screen-evaluation-form'),
                'results': document.getElementById('screen-results')
            },
            navbar: document.getElementById('navbar'),
            dashGreeting: document.getElementById('dash-greeting'),
            dashTotal: document.getElementById('dash-total-evals'),
            searchInput: document.getElementById('dash-search'),
            evalList: document.getElementById('evaluations-list'),
            formEval: document.getElementById('form-evaluation'),
            modalDelete: document.getElementById('modal-delete')
        };

        const showLoader = () => ui.loader.classList.remove('hidden');
        const hideLoader = () => ui.loader.classList.add('hidden');
        
        window.navigate = (screenName) => {
            Object.values(ui.screens).forEach(screen => screen.classList.add('hidden'));
            if(ui.screens[screenName]) {
                ui.screens[screenName].classList.remove('hidden');
                if(['login', 'profile-setup', 'dashboard'].includes(screenName)) {
                    ui.navbar.classList.add('hidden');
                } else {
                    ui.navbar.classList.remove('hidden');
                }
                window.scrollTo(0, 0);
            }
        };

        lucide.createIcons();

        // --- AUTH & SETUP ---
        document.getElementById('btn-google-login').addEventListener('click', async () => {
            try { showLoader(); const provider = new GoogleAuthProvider(); await signInWithPopup(auth, provider); } 
            catch (error) { alert("Falha no login."); hideLoader(); }
        });

        document.getElementById('btn-logout').addEventListener('click', () => signOut(auth));

        const initAuth = async () => {
            if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                await signInWithCustomToken(auth, __initial_auth_token);
            }
        };
        initAuth();

        onAuthStateChanged(auth, async (user) => {
            showLoader();
            if (user) { AppState.user = user; await checkProfileAndRoute(user.uid); } 
            else { AppState.user = null; AppState.profile = null; navigate('login'); hideLoader(); }
        });

        async function checkProfileAndRoute(uid) {
            try {
                const docRef = doc(db, 'artifacts', appId, 'users', uid, 'profile', 'data');
                const docSnap = await getDoc(docRef);

                if (docSnap.exists()) {
                    AppState.profile = docSnap.data();
                    ui.dashGreeting.textContent = `Olá, ${AppState.profile.nome.split(' ')[0]}`;
                    loadEvaluations(uid);
                    navigate('dashboard');
                } else {
                    document.getElementById('prof-nome').value = AppState.user.displayName || '';
                    navigate('profile-setup');
                }
            } catch (e) { navigate('profile-setup'); } finally { hideLoader(); }
        }

        document.getElementById('prof-tipo').addEventListener('change', (e) => {
            const divCref = document.getElementById('div-cref');
            const profCref = document.getElementById('prof-cref');
            if (e.target.value === 'Profissional de Educação Física') { divCref.classList.remove('hidden'); } 
            else { divCref.classList.add('hidden'); profCref.value = ''; }
        });

        document.getElementById('form-profile').addEventListener('submit', async (e) => {
            e.preventDefault();
            showLoader();
            try {
                const tipo = document.getElementById('prof-tipo').value;
                const cref = document.getElementById('prof-cref').value;
                const profileData = {
                    nome: document.getElementById('prof-nome').value || 'Treinador',
                    tipo: tipo,
                    cref: tipo === 'Profissional de Educação Física' ? cref : null,
                    uf: document.getElementById('prof-uf').value.toUpperCase(),
                    email: AppState.user.email
                };
                await setDoc(doc(db, 'artifacts', appId, 'users', AppState.user.uid, 'profile', 'data'), profileData);
                await checkProfileAndRoute(AppState.user.uid);
            } catch (err) { alert("Erro: " + err.message); hideLoader(); }
        });

        // --- DASHBOARD E TABELA ---
        let unsubscribeEvals = null;
        
        function renderTable(evals) {
            ui.evalList.innerHTML = '';
            ui.dashTotal.textContent = evals.length;

            if (evals.length === 0) {
                ui.evalList.innerHTML = '<tr><td colspan="4" class="text-center py-12 font-medium">Nenhuma ficha salva no histórico.</td></tr>';
                return;
            }

            evals.forEach((data) => {
                const dateStr = data.data?.toDate ? data.data.toDate().toLocaleDateString('pt-BR') : 'Hoje';
                const obj = data.anamnese?.objetivo || '-';
                const nomeVisivel = data.nomeAvaliado && data.nomeAvaliado.trim() !== '' ? data.nomeAvaliado : 'Cliente Sem Nome';
                
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="py-5 px-4 font-medium whitespace-nowrap">${dateStr}</td>
                    <td class="py-5 px-4 font-bold text-base whitespace-nowrap">${nomeVisivel}</td>
                    <td class="py-5 px-4">${obj}</td>
                    <td class="py-5 px-4 text-center">
                        <div class="flex items-center justify-center gap-2">
                            <button onclick="window.renderResultsById('${data.id}')" class="btn-print p-2.5 rounded-lg transition" title="Ver / Imprimir PDF"><i data-lucide="printer" class="w-4 h-4"></i></button>
                            <button onclick="window.editEvaluation('${data.id}')" class="btn-edit p-2.5 rounded-lg transition" title="Editar"><i data-lucide="edit-3" class="w-4 h-4"></i></button>
                            <button onclick="window.confirmDelete('${data.id}')" class="btn-delete p-2.5 rounded-lg transition" title="Excluir"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                        </div>
                    </td>
                `;
                ui.evalList.appendChild(tr);
            });
            lucide.createIcons();
        }

        function loadEvaluations(uid) {
            if (unsubscribeEvals) unsubscribeEvals();
            const q = query(collection(db, 'artifacts', appId, 'users', uid, 'evaluations'));
            unsubscribeEvals = onSnapshot(q, (snapshot) => {
                AppState.evaluations = [];
                snapshot.forEach((doc) => {
                    const data = doc.data();
                    data.id = doc.id;
                    AppState.evaluations.push(data);
                });
                
                AppState.evaluations.sort((a, b) => {
                    const timeA = a.data?.toMillis ? a.data.toMillis() : 0;
                    const timeB = b.data?.toMillis ? b.data.toMillis() : 0;
                    return timeB - timeA;
                });
                renderTable(AppState.evaluations);
            }, (error) => {
                ui.evalList.innerHTML = '<tr><td colspan="4" class="text-red-400 text-center py-6">Erro ao carregar avaliações.</td></tr>';
            });
        }

        ui.searchInput.addEventListener('input', (e) => {
            const term = e.target.value.toLowerCase();
            const filtered = AppState.evaluations.filter(ev => (ev.nomeAvaliado || '').toLowerCase().includes(term));
            renderTable(filtered);
        });

        // --- SISTEMA DE DOBRAS ---
        const dobrasList = [
            { id: 'bic', nome: 'Bicipital (Bíceps)' },
            { id: 'tri', nome: 'Tricipital (Tríceps)' },
            { id: 'sub', nome: 'Subescapular' },
            { id: 'pei', nome: 'Peitoral' },
            { id: 'axi', nome: 'Axilar Média' },
            { id: 'sup', nome: 'Suprailíaca' },
            { id: 'abd', nome: 'Abdómen' },
            { id: 'cox', nome: 'Coxa' },
            { id: 'pan', nome: 'Panturrilha' }
        ];

        function initDobrasUI() {
            const container = document.getElementById('dobras-container');
            container.innerHTML = dobrasList.map(d => `
                <div class="bg-dark-900/40 p-4 rounded-xl border border-gray-700/30">
                    <label class="label-base text-gray-300 mb-3">${d.nome}</label>
                    <div class="flex gap-2">
                        <div class="flex-1"><input type="number" step="0.1" id="dob-${d.id}-1" oninput="window.calcDobraMedia('${d.id}')" class="input-base py-3 px-1 text-center text-sm" placeholder="1ª"></div>
                        <div class="flex-1"><input type="number" step="0.1" id="dob-${d.id}-2" oninput="window.calcDobraMedia('${d.id}')" class="input-base py-3 px-1 text-center text-sm" placeholder="2ª"></div>
                        <div class="flex-1"><input type="number" step="0.1" id="dob-${d.id}-3" oninput="window.calcDobraMedia('${d.id}')" class="input-base py-3 px-1 text-center text-sm" placeholder="3ª"></div>
                        <div class="flex-1 relative">
                            <input type="number" step="0.1" id="dob-${d.id}" class="input-base py-3 px-1 text-center text-sm border-neon-blue text-neon-blue bg-dark-800 font-bold" placeholder="Res." readonly>
                            <span class="absolute -top-2 -right-1 bg-neon-blue text-white text-[8px] font-bold px-1 rounded">MÉDIA</span>
                        </div>
                    </div>
                </div>
            `).join('');
        }
        initDobrasUI();

        window.calcDobraMedia = (id) => {
            const v1 = parseFloat(document.getElementById(`dob-${id}-1`).value);
            const v2 = parseFloat(document.getElementById(`dob-${id}-2`).value);
            const v3 = parseFloat(document.getElementById(`dob-${id}-3`).value);

            let vals = [];
            if(!isNaN(v1)) vals.push(v1);
            if(!isNaN(v2)) vals.push(v2);
            if(!isNaN(v3)) vals.push(v3);

            if(vals.length === 0) {
                document.getElementById(`dob-${id}`).value = '';
                return;
            }
            const avg = vals.reduce((a, b) => a + b, 0) / vals.length;
            document.getElementById(`dob-${id}`).value = avg.toFixed(1);
        };

        // --- SISTEMA DE AÇÕES ---
        window.newEvaluation = () => {
            AppState.editingId = null;
            document.getElementById('form-title').textContent = "Nova Avaliação";
            ui.formEval.reset();
            tabs[0].click();
            navigate('evaluation-form');
        };

        window.editEvaluation = (id) => {
            const data = AppState.evaluations.find(e => e.id === id);
            if(!data) return;
            
            AppState.editingId = id;
            document.getElementById('form-title').textContent = "Editar Avaliação";
            
            document.getElementById('eval-nome').value = data.nomeAvaliado || '';
            document.getElementById('eval-idade').value = data.idade || '';
            document.getElementById('eval-sexo').value = data.sexo || 'Masculino';
            document.getElementById('eval-peso').value = data.peso || '';
            document.getElementById('eval-estatura').value = data.estatura || '';
            document.getElementById('eval-protocolo').value = data.protocolo || 'pollock3';

            if(data.anamnese) {
                document.getElementById('ana-objetivo').value = data.anamnese.objetivo || '';
                document.getElementById('ana-atividade').value = data.anamnese.atividade || '';
                document.getElementById('ana-lesoes').value = data.anamnese.lesoes || '';
                document.getElementById('ana-meds').value = data.anamnese.meds || '';
                document.getElementById('ana-sono').value = data.anamnese.sono || '';
            }
            
            dobrasList.forEach(d => {
                const arr = data.dobras && data.dobras[`${d.id}_m`] ? data.dobras[`${d.id}_m`] : [];
                document.getElementById(`dob-${d.id}-1`).value = arr[0] || '';
                document.getElementById(`dob-${d.id}-2`).value = arr[1] || '';
                document.getElementById(`dob-${d.id}-3`).value = arr[2] || '';
                window.calcDobraMedia(d.id); 
            });

            const setIfExist = (id, val) => {
                const el = document.getElementById(id);
                if (el) el.value = val !== undefined && val !== null ? val : '';
            };

            if(data.perimetros) {
                setIfExist('per-pes', data.perimetros.pes);
                setIfExist('per-omb', data.perimetros.omb);
                setIfExist('per-tor', data.perimetros.tor);
                setIfExist('per-cin', data.perimetros.cin);
                setIfExist('per-abd', data.perimetros.abd);
                setIfExist('per-qua', data.perimetros.qua);
                setIfExist('per-brre-e', data.perimetros['brre-e']);
                setIfExist('per-brre-d', data.perimetros['brre-d']);
                setIfExist('per-brco-e', data.perimetros['brco-e']);
                setIfExist('per-brco-d', data.perimetros['brco-d']);
                setIfExist('per-ante-e', data.perimetros['ante-e']);
                setIfExist('per-ante-d', data.perimetros['ante-d']);
                setIfExist('per-cxpr-e', data.perimetros['cxpr-e']);
                setIfExist('per-cxpr-d', data.perimetros['cxpr-d']);
                setIfExist('per-cxme-e', data.perimetros['cxme-e']);
                setIfExist('per-cxme-d', data.perimetros['cxme-d']);
                setIfExist('per-pan-e', data.perimetros['pan-e']);
                setIfExist('per-pan-d', data.perimetros['pan-d']);
                setIfExist('per-outras', data.perimetros.outras);
            }
            
            tabs[0].click();
            navigate('evaluation-form');
        };

        window.editCurrentEvaluation = () => {
            if(AppState.currentEvalId) window.editEvaluation(AppState.currentEvalId);
        };

        let itemToDelete = null;
        window.confirmDelete = (id) => {
            itemToDelete = id;
            ui.modalDelete.classList.remove('hidden');
        };
        window.closeDeleteModal = () => {
            itemToDelete = null;
            ui.modalDelete.classList.add('hidden');
        };
        document.getElementById('btn-confirm-delete').addEventListener('click', async () => {
            if(!itemToDelete) return;
            try {
                showLoader();
                await deleteDoc(doc(db, 'artifacts', appId, 'users', AppState.user.uid, 'evaluations', itemToDelete));
                window.closeDeleteModal();
            } catch (e) { console.error("Erro ao excluir", e); } finally { hideLoader(); }
        });

        // --- NAVEGAÇÃO DE ABAS ---
        const tabs = document.querySelectorAll('.eval-tab-btn');
        const tabContents = document.querySelectorAll('.tab-content');
        tabs.forEach(tab => {
            tab.addEventListener('click', (e) => {
                e.preventDefault();
                const target = tab.getAttribute('data-target');
                tabs.forEach(t => t.classList.replace('text-neon-blue', 'text-gray-400') || t.classList.remove('bg-dark-700', 'shadow'));
                tabContents.forEach(c => c.classList.remove('active'));
                tab.classList.add('text-neon-blue', 'bg-dark-700', 'shadow');
                tab.classList.remove('text-gray-400');
                document.getElementById(target).classList.add('active');
            });
        });

        const Calculators = {
            imc: (p, a) => (a > 0 && p > 0) ? p / ((a/100)**2) : 0,
            rcq: (c, q) => (q > 0 && c > 0) ? c / q : 0,
            siri: (d) => ((4.95 / d) - 4.50) * 100,
            pollock3: (sexo, idade, data) => {
                if(idade <= 0) return null;
                let soma, densidade;
                if(sexo === 'Masculino') {
                    const pei=parseFloat(data.pei)||0; const abd=parseFloat(data.abd)||0; const cox=parseFloat(data.cox)||0;
                    if(pei*abd*cox === 0) return null;
                    soma = pei + abd + cox;
                    densidade = 1.10938 - (0.0008267*soma) + (0.0000016*(soma**2)) - (0.0002574*idade);
                } else {
                    const tri=parseFloat(data.tri)||0; const sup=parseFloat(data.sup)||0; const cox=parseFloat(data.cox)||0;
                    if(tri*sup*cox === 0) return null;
                    soma = tri + sup + cox;
                    densidade = 1.0994921 - (0.0009929*soma) + (0.0000023*(soma**2)) - (0.0001392*idade);
                }
                return Calculators.siri(densidade);
            },
            pollock7: (sexo, idade, data) => {
                if(idade <= 0) return null;
                const sub=parseFloat(data.sub)||0; const tri=parseFloat(data.tri)||0; const pei=parseFloat(data.pei)||0;
                const axi=parseFloat(data.axi)||0; const sup=parseFloat(data.sup)||0; const abd=parseFloat(data.abd)||0; const cox=parseFloat(data.cox)||0;
                if(sub*tri*pei*axi*sup*abd*cox === 0) return null;
                const soma = sub+tri+pei+axi+sup+abd+cox;
                const densidade = sexo === 'Masculino' 
                    ? 1.112 - (0.00043499*soma) + (0.00000055*(soma**2)) - (0.00028826*idade)
                    : 1.097 - (0.00046971*soma) + (0.00000056*(soma**2)) - (0.00012828*idade);
                return Calculators.siri(densidade);
            }
        };

        ui.formEval.addEventListener('submit', async (e) => {
            e.preventDefault();
            showLoader();

            try {
                const getVal = (id) => {
                    const el = document.getElementById(id);
                    if (!el) return '';
                    const val = parseFloat(el.value);
                    return isNaN(val) ? '' : val;
                };
                
                const getDobraData = (id) => {
                    return {
                        res: getVal(`dob-${id}`),
                        m: [getVal(`dob-${id}-1`), getVal(`dob-${id}-2`), getVal(`dob-${id}-3`)]
                    };
                };

                const evalData = {
                    nomeAvaliado: document.getElementById('eval-nome').value || 'Cliente não identificado',
                    idade: parseInt(document.getElementById('eval-idade').value) || 0,
                    sexo: document.getElementById('eval-sexo').value,
                    peso: parseFloat(document.getElementById('eval-peso').value) || 0,
                    estatura: parseFloat(document.getElementById('eval-estatura').value) || 0,
                    protocolo: document.getElementById('eval-protocolo').value,
                    anamnese: {
                        objetivo: document.getElementById('ana-objetivo').value, atividade: document.getElementById('ana-atividade').value,
                        lesoes: document.getElementById('ana-lesoes').value, meds: document.getElementById('ana-meds').value, sono: document.getElementById('ana-sono').value
                    },
                    dobras: {
                        sub: getDobraData('sub').res, sub_m: getDobraData('sub').m,
                        tri: getDobraData('tri').res, tri_m: getDobraData('tri').m,
                        pei: getDobraData('pei').res, pei_m: getDobraData('pei').m,
                        axi: getDobraData('axi').res, axi_m: getDobraData('axi').m,
                        sup: getDobraData('sup').res, sup_m: getDobraData('sup').m,
                        abd: getDobraData('abd').res, abd_m: getDobraData('abd').m,
                        cox: getDobraData('cox').res, cox_m: getDobraData('cox').m,
                        bic: getDobraData('bic').res, bic_m: getDobraData('bic').m,
                        pan: getDobraData('pan').res, pan_m: getDobraData('pan').m
                    },
                    perimetros: {
                        pes: getVal('per-pes'), omb: getVal('per-omb'), tor: getVal('per-tor'), cin: getVal('per-cin'),
                        abd: getVal('per-abd'), qua: getVal('per-qua'), 'brre-e': getVal('per-brre-e'), 'brre-d': getVal('per-brre-d'),
                        'brco-e': getVal('per-brco-e'), 'brco-d': getVal('per-brco-d'), 'ante-e': getVal('per-ante-e'), 'ante-d': getVal('per-ante-d'),
                        'cxpr-e': getVal('per-cxpr-e'), 'cxpr-d': getVal('per-cxpr-d'), 'cxme-e': getVal('per-cxme-e'), 'cxme-d': getVal('per-cxme-d'),
                        'pan-e': getVal('per-pan-e'), 'pan-d': getVal('per-pan-d'),
                        outras: document.getElementById('per-outras').value
                    }
                };

                const imcVal = Calculators.imc(evalData.peso, evalData.estatura);
                let bfVal = null;
                
                if (evalData.protocolo === 'pollock3' || evalData.protocolo === 'guedes') bfVal = Calculators.pollock3(evalData.sexo, evalData.idade, evalData.dobras);
                else if (evalData.protocolo === 'pollock7') bfVal = Calculators.pollock7(evalData.sexo, evalData.idade, evalData.dobras);
                
                let rcqVal = null;
                if(parseFloat(evalData.perimetros.cin) > 0 && parseFloat(evalData.perimetros.qua) > 0) {
                    rcqVal = Calculators.rcq(parseFloat(evalData.perimetros.cin), parseFloat(evalData.perimetros.qua));
                }

                let massaGorda = null, massaMagra = null;
                if(bfVal !== null && bfVal > 0 && evalData.peso > 0) {
                    massaGorda = (evalData.peso * bfVal) / 100; massaMagra = evalData.peso - massaGorda;
                }

                evalData.resultados = { imc: imcVal, bf: bfVal, rcq: rcqVal, massaGorda, massaMagra };

                if(AppState.editingId) {
                    const originalData = AppState.evaluations.find(e => e.id === AppState.editingId);
                    evalData.data = originalData.data || serverTimestamp();
                    await updateDoc(doc(db, 'artifacts', appId, 'users', AppState.user.uid, 'evaluations', AppState.editingId), evalData);
                } else {
                    evalData.data = serverTimestamp();
                    await addDoc(collection(db, 'artifacts', appId, 'users', AppState.user.uid, 'evaluations'), evalData);
                }

                ui.formEval.reset();
                AppState.editingId = null;
                navigate('dashboard');
                
            } catch (err) { console.error(err); } finally { hideLoader(); }
        });

        // --- RENDERIZAR RESULTADOS PDF ---
        window.renderResultsById = (id) => {
            const data = AppState.evaluations.find(e => e.id === id);
            if(data) renderResults(data);
        };

        function classificarIMC(imc) {
            if(imc <= 0) return { t: "Indisponível", c: "bg-gray-100 text-gray-500" };
            if(imc < 18.5) return { t: "Baixo Peso", c: "bg-blue-100 text-blue-800" };
            if(imc < 24.9) return { t: "Normal", c: "bg-green-100 text-green-800" };
            if(imc < 29.9) return { t: "Sobrepeso", c: "bg-yellow-100 text-yellow-800" };
            return { t: "Obesidade", c: "bg-red-100 text-red-800" };
        }
        function classificarRCQ(rcq, sx) {
            if(rcq <= 0) return { t: "Indisponível", c: "bg-gray-100 text-gray-500" };
            let r = "Baixo", c = "bg-green-100 text-green-800";
            if(sx === 'Masculino') {
                if(rcq >= 0.90 && rcq < 0.95) { r = "Moderado"; c = "bg-yellow-100 text-yellow-800"; }
                if(rcq >= 0.95) { r = "Alto"; c = "bg-red-100 text-red-800"; }
            } else {
                if(rcq >= 0.80 && rcq < 0.85) { r = "Moderado"; c = "bg-yellow-100 text-yellow-800"; }
                if(rcq >= 0.85) { r = "Alto"; c = "bg-red-100 text-red-800"; }
            }
            return { t: r, c };
        }

        function renderResults(data) {
            navigate('results');
            AppState.currentEvalClientName = data.nomeAvaliado || 'Cliente';
            AppState.currentEvalId = data.id;
            
            document.getElementById('res-data').textContent = data.data?.toDate ? data.data.toDate().toLocaleDateString('pt-BR') : new Date().toLocaleDateString('pt-BR');
            document.getElementById('res-rt-nome').textContent = AppState.profile.nome;
            document.getElementById('legal-rt-name').textContent = `${AppState.profile.nome}${AppState.profile.cref ? ` – CREF: ${AppState.profile.cref}` : ''}`;
            
            document.getElementById('res-nome').textContent = data.nomeAvaliado || 'Não Identificado';
            document.getElementById('res-idade').textContent = data.idade ? `${data.idade} anos` : '-';
            document.getElementById('res-sexo').textContent = data.sexo;
            document.getElementById('res-peso').textContent = data.peso ? `${data.peso.toFixed(1)} kg` : '-';
            document.getElementById('res-estatura').textContent = data.estatura ? `${data.estatura.toFixed(1)} cm` : '-';

            // IMC
            const iCls = classificarIMC(data.resultados.imc);
            document.getElementById('res-val-imc').textContent = data.resultados.imc > 0 ? data.resultados.imc.toFixed(1) : '-';
            document.getElementById('res-class-imc').className = `text-[10px] font-black uppercase px-2 py-1 rounded mt-2 inline-block ${iCls.c}`;
            document.getElementById('res-class-imc').textContent = iCls.t;
            document.getElementById('card-imc').className = `border rounded-2xl p-5 text-center shadow-sm ${data.resultados.imc > 25 ? 'border-yellow-200 bg-yellow-50' : 'border-green-200 bg-green-50'}`;

            // Gordura
            document.getElementById('res-prot-label').textContent = data.protocolo.toUpperCase();
            if(data.resultados.bf) {
                document.getElementById('res-val-bf').textContent = `${data.resultados.bf.toFixed(1)}%`;
                document.getElementById('res-peso-gordo').textContent = `Massa Gorda: ${data.resultados.massaGorda.toFixed(1)} kg`;
                document.getElementById('res-val-lbm').textContent = `${(100 - data.resultados.bf).toFixed(1)}%`;
                document.getElementById('res-peso-magro').textContent = `Peso Magro: ${data.resultados.massaMagra.toFixed(1)} kg`;
            } else {
                document.getElementById('res-val-bf').textContent = '-';
                document.getElementById('res-peso-gordo').textContent = 'Faltam Dados / Peso';
                document.getElementById('res-val-lbm').textContent = '-';
                document.getElementById('res-peso-magro').textContent = 'Faltam Dados / Peso';
            }

            // RCQ
            if(data.resultados.rcq) {
                const rCls = classificarRCQ(data.resultados.rcq, data.sexo);
                document.getElementById('res-val-rcq').textContent = data.resultados.rcq.toFixed(2);
                document.getElementById('res-class-rcq').className = `text-[10px] font-black uppercase px-2 py-1 rounded mt-2 inline-block ${rCls.c}`;
                document.getElementById('res-class-rcq').textContent = rCls.t;
                document.getElementById('card-rcq').className = `border rounded-2xl p-5 text-center shadow-sm ${rCls.t === 'Alto' ? 'border-red-200 bg-red-50/50' : (rCls.t === 'Moderado' ? 'border-yellow-200 bg-yellow-50/50' : 'border-green-200 bg-green-50/50')}`;
            } else {
                document.getElementById('res-val-rcq').textContent = '-';
                document.getElementById('res-class-rcq').textContent = 'Falta cintura/quadril';
                document.getElementById('res-class-rcq').className = 'text-[10px] uppercase font-bold text-gray-400 mt-2 block';
                document.getElementById('card-rcq').className = 'border rounded-2xl p-5 text-center shadow-sm bg-gray-50';
            }

            let rec = `Objetivo principal: ${data.anamnese?.objetivo || 'Não informado'}. `;
            if(data.resultados.imc >= 30) rec += "Alerta IMC. ";
            if(data.resultados.rcq && classificarRCQ(data.resultados.rcq, data.sexo).t === 'Alto') rec += "Risco Cardiovascular ALTO. ";
            if(data.resultados.bf > 25 && data.sexo === 'Masculino') rec += "%G elevado. ";
            if(data.resultados.bf > 32 && data.sexo === 'Feminino') rec += "%G elevado. ";
            document.getElementById('res-recomendacao').textContent = rec || "Perfil dentro dos padrões.";

            renderChart(data.nomeAvaliado);
        }

        function renderChart(clientName) {
            const clientEvals = AppState.evaluations.filter(e => e.nomeAvaliado === clientName && e.resultados?.bf).sort((a,b) => a.data - b.data);
            const chartContainer = document.getElementById('chart-container');
            if(clientEvals.length < 2) { chartContainer.classList.add('hidden'); return; }
            
            chartContainer.classList.remove('hidden');
            const ctx = document.getElementById('evolutionChart').getContext('2d');
            if(AppState.chartInstance) AppState.chartInstance.destroy();

            AppState.chartInstance = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: clientEvals.map(e => e.data?.toDate ? e.data.toDate().toLocaleDateString('pt-BR').substring(0,5) : ''),
                    datasets: [
                        { label: 'Peso Total (kg)', data: clientEvals.map(e => e.peso), borderColor: '#4B5563', yAxisID: 'y' },
                        { label: '% Gordura', data: clientEvals.map(e => e.resultados.bf), borderColor: '#2563EB', backgroundColor: '#3B82F633', fill: true, yAxisID: 'y1' }
                    ]
                },
                options: { responsive: true, maintainAspectRatio: false, scales: { y: { position: 'left' }, y1: { position: 'right' } } }
            });
        }

        window.generatePDF = () => {
            const el = document.getElementById('pdf-content');
            const btn = document.querySelector('[onclick="window.generatePDF()"]');
            const oldHtml = btn.innerHTML;
            btn.innerHTML = `<div class="loader inline-block align-middle" style="width:14px;height:14px;border-width:2px;"></div>`;
            
            html2pdf().set({
                margin: [10, 10, 10, 10], filename: `Avaliacao_${(AppState.currentEvalClientName||'Cliente').replace(/\s+/g, '_')}.pdf`,
                image: { type: 'jpeg', quality: 0.98 }, html2canvas: { scale: 2, useCORS: true }, jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
            }).from(el).save().then(() => btn.innerHTML = oldHtml);
        };
    </script>
</body>
</html>
