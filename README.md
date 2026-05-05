<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <!-- Força o navegador e teclado do telemóvel a usarem o tema escuro nativo -->
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
        :root {
            color-scheme: dark;
        }
        
        body {
            background-color: #0B0F19; /* Fundo escuro real do app */
            color: #F3F4F6;
            -webkit-tap-highlight-color: transparent;
        }
        
        /* Estilos Neon & Customizados */
        .neon-text { color: #3B82F6; }
        .neon-button {
            background: #3B82F6;
            transition: all 0.2s ease;
        }
        .neon-button:hover { background: #2563EB; }
        
        /* Inputs Base */
        .input-base {
            @apply w-full bg-dark-900 border border-gray-700 rounded-lg px-4 py-3 text-white focus:outline-none focus:border-neon-blue focus:ring-1 focus:ring-neon-blue transition-colors;
        }
        .label-base {
            @apply block text-xs font-semibold text-gray-400 uppercase tracking-wider mb-1;
        }

        /* --- CORREÇÃO DO AUTOFILL E DROPDOWNS (MOBILE) --- */
        /* Impede que o navegador force o fundo branco no preenchimento automático */
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
        
        /* Garante fundo escuro nas opções de Select (Dropdown) */
        select option {
            background-color: #151A2D;
            color: #F3F4F6;
        }
        /* ------------------------------------------------ */

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

        /* Esconder abas inativas na avaliação */
        .tab-content { display: none; }
        .tab-content.active { display: block; animation: fadeIn 0.3s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="antialiased font-sans min-h-screen flex flex-col">

    <!-- OVERLAY DE CARREGAMENTO GLOBAL -->
    <div id="global-loader" class="fixed inset-0 bg-dark-900 bg-opacity-90 z-50 flex flex-col items-center justify-center hidden">
        <div class="loader mb-4" style="width: 48px; height: 48px;"></div>
        <p class="text-neon-blue font-semibold animate-pulse">Sincronizando sistema...</p>
    </div>

    <!-- TELA DE LOGIN -->
    <div id="screen-login" class="flex-1 flex flex-col items-center justify-center p-6">
        <div class="max-w-md w-full bg-dark-800 p-8 rounded-2xl shadow-2xl border border-gray-800 text-center">
            <h1 class="text-4xl font-bold tracking-tighter mb-2"><span class="neon-text">PowFit</span> Med's</h1>
            <p class="text-gray-400 text-sm mb-8">Sistema de Avaliação Física</p>
            
            <button id="btn-google-login" class="w-full flex items-center justify-center gap-3 bg-white text-gray-900 font-semibold py-3 px-4 rounded-xl hover:bg-gray-100 transition-colors shadow-lg">
                <svg class="w-6 h-6" viewBox="0 0 24 24"><path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/><path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/><path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/><path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/><path fill="none" d="M1 1h22v22H1z"/></svg>
                Entrar com Google
            </button>
        </div>
    </div>

    <!-- TELA DE SETUP DE PERFIL -->
    <div id="screen-profile-setup" class="hidden flex-1 flex flex-col items-center justify-center p-6">
        <div class="max-w-md w-full bg-dark-800 p-8 rounded-2xl shadow-2xl border border-gray-800">
            <h2 class="text-2xl font-bold mb-6 text-white border-b border-gray-700 pb-2">Complete seu Perfil</h2>
            <form id="form-profile" class="space-y-4">
                <div>
                    <label class="label-base">Nome Completo (RT)</label>
                    <input type="text" id="prof-nome" class="input-base" required placeholder="Ex: Luiz André">
                </div>
                <div>
                    <label class="label-base">Tipo de Atuação</label>
                    <select id="prof-tipo" class="input-base" required>
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
                    <input type="text" id="prof-uf" class="input-base" required placeholder="Ex: RN" maxlength="2">
                </div>
                <button type="submit" class="w-full neon-button text-white font-bold py-3 rounded-xl mt-4">Salvar Perfil</button>
            </form>
        </div>
    </div>

    <!-- NAVBAR SECUNDÁRIA -->
    <nav id="navbar" class="hidden bg-dark-800 border-b border-gray-800 sticky top-0 z-40">
        <div class="max-w-4xl mx-auto px-4 py-3 flex justify-between items-center">
            <div class="font-bold text-xl cursor-pointer" onclick="navigate('dashboard')">
                <span class="neon-text">PowFit</span>
            </div>
            <button id="btn-logout" class="p-2 text-gray-400 hover:text-red-400 transition"><i data-lucide="log-out"></i></button>
        </div>
    </nav>

    <!-- TELA DASHBOARD -->
    <div id="screen-dashboard" class="hidden flex-1 max-w-4xl w-full mx-auto p-6 flex flex-col">
        
        <!-- Header Principal -->
        <div class="mb-8 mt-2 border-b border-gray-800 pb-6">
            <h1 class="text-3xl font-bold text-white mb-2" id="dash-greeting">Olá, ...</h1>
            <p class="text-gray-400 text-base mb-6">Gerencie as suas avaliações e clientes.</p>
            
            <button onclick="navigate('evaluation-form')" class="neon-button text-white px-5 py-2.5 rounded-lg font-semibold flex items-center gap-2 text-sm shadow-md w-max">
                <i data-lucide="plus" class="w-4 h-4"></i> Nova Avaliação
            </button>
        </div>
        
        <!-- Card Total -->
        <div class="bg-dark-800 border border-gray-700/50 rounded-2xl p-6 w-56 mb-8 shadow-sm">
            <p class="text-gray-400 text-sm mb-3">Total de Avaliações</p>
            <p class="text-4xl font-bold text-white" id="dash-total-evals">0</p>
        </div>

        <!-- Tabela Histórico -->
        <div class="bg-dark-800 border border-gray-700/50 rounded-2xl p-6 flex-1 shadow-sm">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6 gap-4">
                <h2 class="text-xl font-bold text-white flex items-center gap-2">
                    <i data-lucide="history" class="w-5 h-5 text-gray-400"></i> Histórico Recente
                </h2>
                <input type="text" id="dash-search" placeholder="Procurar cliente..." class="bg-dark-900 border border-gray-700 rounded-lg px-4 py-2.5 text-sm text-white focus:outline-none focus:border-neon-blue w-full sm:w-64">
            </div>
            
            <div class="overflow-x-auto">
                <table class="w-full text-left border-collapse min-w-[500px]">
                    <thead>
                        <tr class="border-b border-gray-700 text-gray-400 text-sm">
                            <th class="pb-4 px-2 font-semibold">Data</th>
                            <th class="pb-4 px-2 font-semibold">Cliente</th>
                            <th class="pb-4 px-2 font-semibold">Objetivo</th>
                            <th class="pb-4 px-2 font-semibold text-center">Ação</th>
                        </tr>
                    </thead>
                    <tbody id="evaluations-list" class="text-sm">
                        <tr><td colspan="4" class="text-center text-gray-500 py-10">Carregando...</td></tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- TELA FORMULÁRIO DE AVALIAÇÃO -->
    <div id="screen-evaluation-form" class="hidden flex-1 max-w-2xl w-full mx-auto p-4 flex flex-col pb-24">
        <div class="flex items-center gap-3 mb-6">
            <button onclick="navigate('dashboard')" class="text-gray-400 hover:text-white"><i data-lucide="arrow-left"></i></button>
            <h2 class="text-2xl font-bold">Nova Avaliação</h2>
        </div>

        <!-- Abas Navegação -->
        <div class="flex overflow-x-auto mb-6 bg-dark-800 rounded-lg p-1 border border-gray-700">
            <button class="flex-1 py-2 text-sm font-semibold rounded-md text-neon-blue bg-dark-700 shadow eval-tab-btn" data-target="tab-perfil">Perfil</button>
            <button class="flex-1 py-2 text-sm font-semibold rounded-md text-gray-400 hover:text-white eval-tab-btn" data-target="tab-anamnese">Anamnese</button>
            <button class="flex-1 py-2 text-sm font-semibold rounded-md text-gray-400 hover:text-white eval-tab-btn" data-target="tab-dobras">Dobras</button>
            <button class="flex-1 py-2 text-sm font-semibold rounded-md text-gray-400 hover:text-white eval-tab-btn" data-target="tab-perimetros">Medidas</button>
        </div>

        <form id="form-evaluation" class="bg-dark-800 p-5 rounded-xl border border-gray-700 shadow-xl relative">
            
            <!-- ABA 1: PERFIL -->
            <div id="tab-perfil" class="tab-content active space-y-4">
                <div>
                    <label class="label-base text-neon-blue">Nome do Avaliado *</label>
                    <input type="text" id="eval-nome" class="input-base border-neon-blue" required>
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="label-base">Idade *</label>
                        <input type="number" id="eval-idade" class="input-base" required min="1">
                    </div>
                    <div>
                        <label class="label-base">Sexo *</label>
                        <select id="eval-sexo" class="input-base" required>
                            <option value="Masculino">Masculino</option>
                            <option value="Feminino">Feminino</option>
                        </select>
                    </div>
                    <div>
                        <label class="label-base">Peso (kg) *</label>
                        <input type="number" step="0.1" id="eval-peso" class="input-base" required>
                    </div>
                    <div>
                        <label class="label-base">Estatura (cm) *</label>
                        <input type="number" step="0.1" id="eval-estatura" class="input-base" required>
                    </div>
                </div>
                <div>
                    <label class="label-base text-yellow-500">Protocolo de Composição Corporal *</label>
                    <select id="eval-protocolo" class="input-base border-yellow-700 focus:border-yellow-500" required>
                        <option value="pollock3">Pollock 3 Dobras</option>
                        <option value="pollock7">Pollock 7 Dobras</option>
                        <option value="guedes">Guedes</option>
                        <option value="imc">Apenas IMC (Sem dobras)</option>
                    </select>
                </div>
            </div>

            <!-- ABA 2: ANAMNESE -->
            <div id="tab-anamnese" class="tab-content space-y-4">
                <div>
                    <label class="label-base">Objetivo Principal</label>
                    <select id="ana-objetivo" class="input-base">
                        <option value="Emagrecimento">Emagrecimento</option>
                        <option value="Hipertrofia">Hipertrofia</option>
                        <option value="Saúde/Qualidade de Vida">Saúde / Qualidade de Vida</option>
                    </select>
                </div>
                <div>
                    <label class="label-base">Nível de Atividade</label>
                    <select id="ana-atividade" class="input-base">
                        <option value="Sedentário">Sedentário</option>
                        <option value="Levemente Ativo">Levemente Ativo (1-2x/sem)</option>
                        <option value="Moderadamente Ativo">Moderadamente Ativo (3-4x/sem)</option>
                        <option value="Muito Ativo">Muito Ativo (5+x/sem)</option>
                    </select>
                </div>
                <div>
                    <label class="label-base">Histórico de Lesões / Dores</label>
                    <textarea id="ana-lesoes" class="input-base h-20" placeholder="Nenhuma relatada..."></textarea>
                </div>
                <div>
                    <label class="label-base">Uso de Medicamentos / Suplementos</label>
                    <input type="text" id="ana-meds" class="input-base" placeholder="Ex: Losartana, Creatina...">
                </div>
                <div>
                    <label class="label-base">Qualidade do Sono</label>
                    <select id="ana-sono" class="input-base">
                        <option value="Boa">Boa (7-8h reparadoras)</option>
                        <option value="Regular">Regular (Despertares)</option>
                        <option value="Ruim">Ruim (Insônia, <5h)</option>
                    </select>
                </div>
            </div>

            <!-- ABA 3: DOBRAS CUTÂNEAS -->
            <div id="tab-dobras" class="tab-content">
                <div class="bg-blue-900/20 border border-blue-800 text-blue-200 text-xs p-3 rounded-lg mb-4">
                    Preencha as dobras em milímetros (mm). O sistema ignorará dobras não necessárias para o protocolo selecionado.
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div><label class="label-base">Subescapular</label><input type="number" step="0.1" id="dob-sub" class="input-base"></div>
                    <div><label class="label-base">Tríceps</label><input type="number" step="0.1" id="dob-tri" class="input-base"></div>
                    <div><label class="label-base">Peitoral</label><input type="number" step="0.1" id="dob-pei" class="input-base"></div>
                    <div><label class="label-base">Axilar Medial</label><input type="number" step="0.1" id="dob-axi" class="input-base"></div>
                    <div><label class="label-base">Suprailíaca</label><input type="number" step="0.1" id="dob-sup" class="input-base"></div>
                    <div><label class="label-base">Abdominal</label><input type="number" step="0.1" id="dob-abd" class="input-base"></div>
                    <div><label class="label-base">Coxa Medial</label><input type="number" step="0.1" id="dob-cox" class="input-base"></div>
                    <div><label class="label-base">Bíceps</label><input type="number" step="0.1" id="dob-bic" class="input-base"></div>
                    <div><label class="label-base">Panturrilha</label><input type="number" step="0.1" id="dob-pan" class="input-base"></div>
                </div>
            </div>

            <!-- ABA 4: PERÍMETROS -->
            <div id="tab-perimetros" class="tab-content">
                <h3 class="text-sm font-semibold text-neon-blue border-b border-gray-700 pb-1 mb-3">Tronco (cm)</h3>
                <div class="grid grid-cols-2 gap-3 mb-4">
                    <div><label class="label-base">Pescoço</label><input type="number" step="0.1" id="per-pes" class="input-base"></div>
                    <div><label class="label-base">Ombros</label><input type="number" step="0.1" id="per-omb" class="input-base"></div>
                    <div><label class="label-base">Tórax</label><input type="number" step="0.1" id="per-tor" class="input-base"></div>
                    <div><label class="label-base text-yellow-500">Cintura *</label><input type="number" step="0.1" id="per-cin" class="input-base" required></div>
                    <div><label class="label-base">Abdominal</label><input type="number" step="0.1" id="per-abd" class="input-base"></div>
                    <div><label class="label-base text-yellow-500">Quadril *</label><input type="number" step="0.1" id="per-qua" class="input-base" required></div>
                </div>

                <h3 class="text-sm font-semibold text-neon-blue border-b border-gray-700 pb-1 mb-3 mt-6">Membros Superiores (cm)</h3>
                <div class="grid grid-cols-2 gap-3 mb-4">
                    <div><label class="label-base">Braço Relax. (E)</label><input type="number" step="0.1" id="per-brre-e" class="input-base"></div>
                    <div><label class="label-base">Braço Relax. (D)</label><input type="number" step="0.1" id="per-brre-d" class="input-base"></div>
                    <div><label class="label-base">Braço Contr. (E)</label><input type="number" step="0.1" id="per-brco-e" class="input-base"></div>
                    <div><label class="label-base">Braço Contr. (D)</label><input type="number" step="0.1" id="per-brco-d" class="input-base"></div>
                    <div><label class="label-base">Antebraço (E)</label><input type="number" step="0.1" id="per-ante-e" class="input-base"></div>
                    <div><label class="label-base">Antebraço (D)</label><input type="number" step="0.1" id="per-ante-d" class="input-base"></div>
                </div>

                <h3 class="text-sm font-semibold text-neon-blue border-b border-gray-700 pb-1 mb-3 mt-6">Membros Inferiores (cm)</h3>
                <div class="grid grid-cols-2 gap-3 mb-6">
                    <div><label class="label-base">Coxa Prox. (E)</label><input type="number" step="0.1" id="per-cxpr-e" class="input-base"></div>
                    <div><label class="label-base">Coxa Prox. (D)</label><input type="number" step="0.1" id="per-cxpr-d" class="input-base"></div>
                    <div><label class="label-base">Coxa Med. (E)</label><input type="number" step="0.1" id="per-cxme-e" class="input-base"></div>
                    <div><label class="label-base">Coxa Med. (D)</label><input type="number" step="0.1" id="per-cxme-d" class="input-base"></div>
                    <div><label class="label-base">Panturrilha (E)</label><input type="number" step="0.1" id="per-pan-e" class="input-base"></div>
                    <div><label class="label-base">Panturrilha (D)</label><input type="number" step="0.1" id="per-pan-d" class="input-base"></div>
                </div>
            </div>

            <!-- Botão Fixo Bottom Mobile -->
            <div class="fixed bottom-0 left-0 w-full bg-dark-900 border-t border-gray-800 p-4 z-10 flex gap-4">
                <button type="button" onclick="navigate('dashboard')" class="flex-1 bg-dark-700 text-white font-semibold py-3 rounded-xl hover:bg-dark-800 transition">Cancelar</button>
                <button type="submit" class="flex-1 neon-button text-white font-bold py-3 rounded-xl flex items-center justify-center gap-2">
                    <i data-lucide="calculator" class="w-5 h-5"></i> Salvar
                </button>
            </div>
        </form>
    </div>

    <!-- TELA RESULTADOS / RELATÓRIO -->
    <div id="screen-results" class="hidden flex-1 w-full max-w-4xl mx-auto p-4 flex flex-col pb-10">
        
        <div class="flex justify-between items-center mb-6 no-print">
            <button onclick="navigate('dashboard')" class="text-gray-400 hover:text-white"><i data-lucide="arrow-left"></i> Voltar</button>
            <div class="flex gap-2">
                <button onclick="generatePDF()" class="bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded-lg font-semibold flex items-center gap-2 text-sm shadow-lg transition">
                    <i data-lucide="file-down" class="w-4 h-4"></i> Exportar PDF
                </button>
            </div>
        </div>

        <!-- CONTEÚDO PARA PDF (Fundo Branco para impressão Limpa) -->
        <div id="pdf-content" class="bg-white text-gray-900 p-8 rounded-xl shadow-2xl relative">
            
            <!-- Header Relatório -->
            <div class="border-b-4 border-dark-900 pb-4 mb-6 flex justify-between items-end">
                <div>
                    <h1 class="text-3xl font-bold tracking-tighter text-dark-900">PowFit <span class="text-blue-600">Med's</span></h1>
                    <p class="text-xs text-gray-500 uppercase font-bold tracking-widest mt-1">Relatório de Composição Corporal</p>
                </div>
                <div class="text-right">
                    <p class="text-sm font-bold" id="res-data">DD/MM/YYYY</p>
                </div>
            </div>

            <!-- Dados Cliente -->
            <div class="bg-gray-100 p-4 rounded-lg mb-6 grid grid-cols-2 md:grid-cols-4 gap-4 text-sm border border-gray-200">
                <div class="col-span-2"><span class="text-gray-500 block text-xs uppercase font-bold">Avaliador</span><strong id="res-rt-nome">-</strong></div>
                <div class="col-span-2"><span class="text-gray-500 block text-xs uppercase font-bold">Avaliado</span><strong id="res-nome" class="text-lg">-</strong></div>
                <div><span class="text-gray-500 block text-xs uppercase font-bold">Idade</span><strong id="res-idade">-</strong></div>
                <div><span class="text-gray-500 block text-xs uppercase font-bold">Sexo</span><strong id="res-sexo">-</strong></div>
                <div><span class="text-gray-500 block text-xs uppercase font-bold">Peso Total</span><strong id="res-peso">- kg</strong></div>
                <div><span class="text-gray-500 block text-xs uppercase font-bold">Estatura</span><strong id="res-estatura">- cm</strong></div>
            </div>

            <!-- Cards de Resultados Principais -->
            <h3 class="text-lg font-bold text-dark-900 mb-3 border-b pb-1">Análise Quantitativa</h3>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
                
                <!-- IMC -->
                <div class="border rounded-xl p-4 text-center shadow-sm" id="card-imc">
                    <span class="text-xs text-gray-500 font-bold uppercase block mb-1">IMC</span>
                    <div class="text-3xl font-black text-dark-900" id="res-val-imc">-</div>
                    <span class="text-xs font-semibold px-2 py-1 rounded-full mt-2 inline-block" id="res-class-imc">-</span>
                </div>

                <!-- Gordura -->
                <div class="border rounded-xl p-4 text-center shadow-sm border-blue-200 bg-blue-50">
                    <span class="text-xs text-blue-600 font-bold uppercase block mb-1">% Gordura (<span id="res-prot-label" class="text-[10px]"></span>)</span>
                    <div class="text-3xl font-black text-blue-700" id="res-val-bf">-</div>
                    <span class="text-xs text-gray-600 mt-2 block font-medium" id="res-peso-gordo">Massa Gorda: - kg</span>
                </div>

                <!-- Massa Magra -->
                <div class="border rounded-xl p-4 text-center shadow-sm border-green-200 bg-green-50">
                    <span class="text-xs text-green-700 font-bold uppercase block mb-1">Massa Magra</span>
                    <div class="text-3xl font-black text-green-800" id="res-val-lbm">- %</div>
                    <span class="text-xs text-gray-600 mt-2 block font-medium" id="res-peso-magro">Peso Magro: - kg</span>
                </div>

                <!-- RCQ -->
                <div class="border rounded-xl p-4 text-center shadow-sm" id="card-rcq">
                    <span class="text-xs text-gray-500 font-bold uppercase block mb-1">Risco Cardio (RCQ)</span>
                    <div class="text-3xl font-black text-dark-900" id="res-val-rcq">-</div>
                    <span class="text-xs font-semibold px-2 py-1 rounded-full mt-2 inline-block" id="res-class-rcq">-</span>
                </div>
            </div>

            <!-- Gráfico Evolução -->
            <div class="mb-8 hidden" id="chart-container">
                <h3 class="text-lg font-bold text-dark-900 mb-3 border-b pb-1">Evolução Histórica</h3>
                <div class="h-64 w-full">
                    <canvas id="evolutionChart"></canvas>
                </div>
            </div>

            <!-- Recomendações App -->
            <div class="bg-gray-50 border border-gray-200 p-4 rounded-xl mb-8">
                <h3 class="text-sm font-bold text-dark-900 mb-2 flex items-center gap-2">
                    <i data-lucide="brain" class="w-4 h-4 text-purple-600"></i> Síntese e Direcionamento
                </h3>
                <p id="res-recomendacao" class="text-sm text-gray-800 leading-relaxed font-medium"></p>
            </div>

            <!-- Disclaimer Legal -->
            <div class="mt-12 border-t-2 border-gray-300 pt-6 text-center">
                <p class="text-sm font-bold text-dark-900 mb-1">
                    As recomendações clínicas e estruturação de treino são de responsabilidade do RT: <span id="legal-rt-name" class="text-blue-700"></span>
                </p>
                <p class="text-[10px] text-gray-500 max-w-2xl mx-auto leading-tight">
                    Documento gerado pelo sistema PowFit Med's. A atuação de Treinador Esportivo e Profissional de Educação Física segue as diretrizes da Lei nº 9.696/1998. A avaliação física é um parâmetro de acompanhamento e não substitui diagnóstico médico.
                </p>
            </div>
            
        </div>
    </div>

    <!-- ================= LÓGICA DA APLICAÇÃO ================= -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
        import { getAuth, GoogleAuthProvider, signInWithPopup, onAuthStateChanged, signOut, signInWithCustomToken, signInAnonymously } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
        import { getFirestore, enableIndexedDbPersistence, doc, setDoc, getDoc, collection, addDoc, query, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-firestore.js";

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

        const AppState = { user: null, profile: null, evaluations: [], chartInstance: null, currentEvalClientName: null };

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
            formEval: document.getElementById('form-evaluation')
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
            try {
                showLoader();
                const provider = new GoogleAuthProvider();
                await signInWithPopup(auth, provider);
            } catch (error) {
                console.error("Login Error:", error);
                alert("Falha no login.");
                hideLoader();
            }
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
            if (user) {
                AppState.user = user;
                await checkProfileAndRoute(user.uid);
            } else {
                AppState.user = null; AppState.profile = null;
                navigate('login');
                hideLoader();
            }
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
            } catch (e) {
                navigate('profile-setup');
            } finally { hideLoader(); }
        }

        document.getElementById('prof-tipo').addEventListener('change', (e) => {
            const divCref = document.getElementById('div-cref');
            const profCref = document.getElementById('prof-cref');
            if (e.target.value === 'Profissional de Educação Física') {
                divCref.classList.remove('hidden'); profCref.required = true;
            } else {
                divCref.classList.add('hidden'); profCref.required = false; profCref.value = '';
            }
        });

        document.getElementById('form-profile').addEventListener('submit', async (e) => {
            e.preventDefault();
            showLoader();
            try {
                const tipo = document.getElementById('prof-tipo').value;
                const cref = document.getElementById('prof-cref').value;
                const profileData = {
                    nome: document.getElementById('prof-nome').value,
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
                ui.evalList.innerHTML = '<tr><td colspan="4" class="text-center text-gray-500 py-10 font-medium">Nenhuma avaliação encontrada.</td></tr>';
                return;
            }

            evals.forEach((data) => {
                const dateStr = data.data?.toDate ? data.data.toDate().toLocaleDateString('pt-BR') : 'Hoje';
                const obj = data.anamnese?.objetivo || '-';
                
                const tr = document.createElement('tr');
                tr.className = "border-b border-gray-700/50 hover:bg-dark-700 transition duration-150 group cursor-pointer";
                tr.innerHTML = `
                    <td class="py-4 px-2 text-gray-400 font-medium">${dateStr}</td>
                    <td class="py-4 px-2 text-gray-100 font-semibold">${data.nomeAvaliado}</td>
                    <td class="py-4 px-2 text-gray-400">${obj}</td>
                    <td class="py-4 px-2 text-center">
                        <button class="text-neon-blue group-hover:text-blue-400 font-semibold text-sm transition">Ver PDF</button>
                    </td>
                `;
                tr.onclick = () => renderResults(data);
                ui.evalList.appendChild(tr);
            });
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
                    const timeA = a.data?.toMillis ? a.data.toMillis() : Date.now();
                    const timeB = b.data?.toMillis ? b.data.toMillis() : Date.now();
                    return timeB - timeA;
                });

                renderTable(AppState.evaluations);
            }, (error) => {
                console.error("Erro:", error);
                ui.evalList.innerHTML = '<tr><td colspan="4" class="text-red-400 text-center py-6">Erro ao carregar avaliações.</td></tr>';
            });
        }

        ui.searchInput.addEventListener('input', (e) => {
            const term = e.target.value.toLowerCase();
            const filtered = AppState.evaluations.filter(ev => ev.nomeAvaliado.toLowerCase().includes(term));
            renderTable(filtered);
        });

        // --- LÓGICA DO FORMULÁRIO ---
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
            imc: (p, a) => p / ((a/100)**2),
            rcq: (c, q) => c / q,
            siri: (d) => ((4.95 / d) - 4.50) * 100,
            pollock3: (sexo, idade, data) => {
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
                const evalData = {
                    nomeAvaliado: document.getElementById('eval-nome').value,
                    idade: parseInt(document.getElementById('eval-idade').value),
                    sexo: document.getElementById('eval-sexo').value,
                    peso: parseFloat(document.getElementById('eval-peso').value),
                    estatura: parseFloat(document.getElementById('eval-estatura').value),
                    protocolo: document.getElementById('eval-protocolo').value,
                    anamnese: {
                        objetivo: document.getElementById('ana-objetivo').value,
                        atividade: document.getElementById('ana-atividade').value,
                        lesoes: document.getElementById('ana-lesoes').value,
                        meds: document.getElementById('ana-meds').value,
                        sono: document.getElementById('ana-sono').value
                    },
                    dobras: {
                        sub: document.getElementById('dob-sub').value, tri: document.getElementById('dob-tri').value,
                        pei: document.getElementById('dob-pei').value, axi: document.getElementById('dob-axi').value,
                        sup: document.getElementById('dob-sup').value, abd: document.getElementById('dob-abd').value,
                        cox: document.getElementById('dob-cox').value, bic: document.getElementById('dob-bic').value,
                        pan: document.getElementById('dob-pan').value
                    },
                    perimetros: {
                        pes: document.getElementById('per-pes').value, omb: document.getElementById('per-omb').value,
                        tor: document.getElementById('per-tor').value, cin: document.getElementById('per-cin').value,
                        abd: document.getElementById('per-abd').value, qua: document.getElementById('per-qua').value
                    },
                    data: serverTimestamp()
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
                if(bfVal !== null && bfVal > 0) {
                    massaGorda = (evalData.peso * bfVal) / 100; massaMagra = evalData.peso - massaGorda;
                }

                evalData.resultados = { imc: imcVal, bf: bfVal, rcq: rcqVal, massaGorda, massaMagra };

                await addDoc(collection(db, 'artifacts', appId, 'users', AppState.user.uid, 'evaluations'), evalData);
                ui.formEval.reset();
                tabs[0].click(); 
                navigate('dashboard');
                
            } catch (err) {
                alert("Erro ao salvar! Preencha todos os campos obrigatórios corretamente.");
            } finally { hideLoader(); }
        });

        // --- RENDERIZAR RESULTADOS PDF ---
        function classificarIMC(imc) {
            if(imc < 18.5) return { t: "Baixo Peso", c: "bg-blue-100 text-blue-800" };
            if(imc < 24.9) return { t: "Normal", c: "bg-green-100 text-green-800" };
            if(imc < 29.9) return { t: "Sobrepeso", c: "bg-yellow-100 text-yellow-800" };
            return { t: "Obesidade", c: "bg-red-100 text-red-800" };
        }
        function classificarRCQ(rcq, sx) {
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
            AppState.currentEvalClientName = data.nomeAvaliado;
            
            document.getElementById('res-data').textContent = data.data?.toDate ? data.data.toDate().toLocaleDateString('pt-BR') : new Date().toLocaleDateString('pt-BR');
            document.getElementById('res-rt-nome').textContent = AppState.profile.nome;
            document.getElementById('legal-rt-name').textContent = `${AppState.profile.nome}${AppState.profile.cref ? ` – CREF: ${AppState.profile.cref}` : ''}`;
            
            document.getElementById('res-nome').textContent = data.nomeAvaliado;
            document.getElementById('res-idade').textContent = `${data.idade} anos`;
            document.getElementById('res-sexo').textContent = data.sexo;
            document.getElementById('res-peso').textContent = `${data.peso.toFixed(1)} kg`;
            document.getElementById('res-estatura').textContent = `${data.estatura.toFixed(1)} cm`;

            const iCls = classificarIMC(data.resultados.imc);
            document.getElementById('res-val-imc').textContent = data.resultados.imc.toFixed(1);
            document.getElementById('res-class-imc').className = `text-xs font-semibold px-2 py-1 rounded-full mt-2 inline-block ${iCls.c}`;
            document.getElementById('res-class-imc').textContent = iCls.t;
            document.getElementById('card-imc').className = `border rounded-xl p-4 text-center shadow-sm ${data.resultados.imc > 25 ? 'border-yellow-200 bg-yellow-50' : 'border-green-200 bg-green-50'}`;

            document.getElementById('res-prot-label').textContent = data.protocolo.toUpperCase();
            if(data.resultados.bf) {
                document.getElementById('res-val-bf').textContent = `${data.resultados.bf.toFixed(1)}%`;
                document.getElementById('res-peso-gordo').textContent = `Massa Gorda: ${data.resultados.massaGorda.toFixed(1)} kg`;
                document.getElementById('res-val-lbm').textContent = `${(100 - data.resultados.bf).toFixed(1)}%`;
                document.getElementById('res-peso-magro').textContent = `Peso Magro: ${data.resultados.massaMagra.toFixed(1)} kg`;
            } else {
                document.getElementById('res-val-bf').textContent = 'N/A';
                document.getElementById('res-peso-gordo').textContent = 'Faltam dobras';
                document.getElementById('res-val-lbm').textContent = 'N/A';
                document.getElementById('res-peso-magro').textContent = 'Faltam dobras';
            }

            if(data.resultados.rcq) {
                const rCls = classificarRCQ(data.resultados.rcq, data.sexo);
                document.getElementById('res-val-rcq').textContent = data.resultados.rcq.toFixed(2);
                document.getElementById('res-class-rcq').className = `text-xs font-semibold px-2 py-1 rounded-full mt-2 inline-block ${rCls.c}`;
                document.getElementById('res-class-rcq').textContent = rCls.t;
                document.getElementById('card-rcq').className = `border rounded-xl p-4 text-center shadow-sm ${rCls.t === 'Alto' ? 'border-red-200 bg-red-50' : (rCls.t === 'Moderado' ? 'border-yellow-200 bg-yellow-50' : 'border-green-200 bg-green-50')}`;
            } else {
                document.getElementById('res-val-rcq').textContent = '-';
                document.getElementById('res-class-rcq').textContent = 'Falta cintura/quadril';
                document.getElementById('res-class-rcq').className = 'text-xs text-gray-400 mt-2 block';
                document.getElementById('card-rcq').className = 'border rounded-xl p-4 text-center shadow-sm bg-gray-50';
            }

            let rec = `Objetivo principal: ${data.anamnese?.objetivo || 'Não informado'}. `;
            if(data.resultados.imc >= 30) rec += "Alerta para Obesidade (IMC). Acompanhamento estruturado é recomendado. ";
            if(data.resultados.rcq && classificarRCQ(data.resultados.rcq, data.sexo).t === 'Alto') rec += "Atenção: Risco Cardiovascular ALTO associado à relação cintura/quadril. Foco em redução de gordura abdominal. ";
            if(data.resultados.bf > 25 && data.sexo === 'Masculino') rec += "Percentual de gordura elevado para o sexo. Priorizar déficit calórico. ";
            if(data.resultados.bf > 32 && data.sexo === 'Feminino') rec += "Percentual de gordura elevado para o sexo. Priorizar déficit calórico. ";
            document.getElementById('res-recomendacao').textContent = rec || "Perfil físico dentro dos padrões esperados. Foco na progressão do treinamento baseada no objetivo principal.";

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
            const btn = document.querySelector('[onclick="generatePDF()"]');
            const oldHtml = btn.innerHTML;
            btn.innerHTML = `<div class="loader inline-block align-middle" style="width:14px;height:14px;border-width:2px;"></div>`;
            
            html2pdf().set({
                margin: [10, 10, 10, 10], filename: `Avaliacao_${AppState.currentEvalClientName.replace(/\s+/g, '_')}.pdf`,
                image: { type: 'jpeg', quality: 0.98 }, html2canvas: { scale: 2, useCORS: true }, jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
            }).from(el).save().then(() => btn.innerHTML = oldHtml);
        };
    </script>
</body>
</html>
