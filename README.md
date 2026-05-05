<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Med's - Avaliação Física</title>
    
    <!-- Tailwind CSS (via CDN) -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        primary: '#3b82f6', // Azul principal
                        primaryDark: '#2563eb',
                        darkBg: '#111827', // Fundo principal
                        darkPanel: '#1f2937', // Paineis/Cards
                        darkInput: '#374151'
                    },
                    animation: {
                        'fade-in': 'fadeIn 0.3s ease-out',
                        'slide-up': 'slideUp 0.4s ease-out'
                    },
                    keyframes: {
                        fadeIn: {
                            '0%': { opacity: '0' },
                            '100%': { opacity: '1' },
                        },
                        slideUp: {
                            '0%': { transform: 'translateY(20px)', opacity: '0' },
                            '100%': { transform: 'translateY(0)', opacity: '1' },
                        }
                    }
                }
            }
        }
    </script>

    <!-- FontAwesome (Ícones) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <!-- html2pdf (Para geração de PDF) -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <style>
        /* Estilos adicionais e customizações do scrollbar */
        body {
            background-color: #111827;
            color: #f3f4f6;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow-x: hidden;
        }
        
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1f2937; 
        }
        ::-webkit-scrollbar-thumb {
            background: #4b5563; 
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #3b82f6; 
        }

        /* Esconder setas de input number */
        input[type=number]::-webkit-inner-spin-button, 
        input[type=number]::-webkit-outer-spin-button { 
            -webkit-appearance: none; 
            margin: 0; 
        }
        input[type=number] {
            -moz-appearance: textfield;
        }

        .glass-panel {
            background: rgba(31, 41, 55, 0.7);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(75, 85, 99, 0.4);
        }

        .badge {
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 600;
        }
        .badge-green { background-color: rgba(16, 185, 129, 0.2); color: #34d399; border: 1px solid #10b981; }
        .badge-yellow { background-color: rgba(245, 158, 11, 0.2); color: #fbbf24; border: 1px solid #f59e0b; }
        .badge-red { background-color: rgba(239, 68, 68, 0.2); color: #f87171; border: 1px solid #ef4444; }

        /* Esconder telas inativas */
        .screen { display: none; }
        .screen.active { display: block; animation: fadeIn 0.4s; }

        /* Estilo para impressão (PDF) */
        @media print {
            body { background-color: white !important; color: black !important; }
            .no-print { display: none !important; }
            .glass-panel { background: white !important; border: 1px solid #ccc !important; box-shadow: none !important; }
            .text-white { color: black !important; }
            .text-gray-400 { color: #4b5563 !important; }
        }
    </style>
</head>
<body class="antialiased flex flex-col min-h-screen">

    <!-- Navbar -->
    <nav class="bg-darkPanel border-b border-gray-700 sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-16">
                <div class="flex items-center">
                    <i class="fa-solid fa-dumbbell text-primary text-2xl mr-2"></i>
                    <span class="font-bold text-xl tracking-tight">PowFit <span class="text-primary">Med's</span></span>
                </div>
                <div class="flex items-center space-x-4" id="nav-actions">
                    <!-- Dinâmico via JS -->
                </div>
            </div>
        </div>
    </nav>

    <!-- Main Content Area -->
    <main class="flex-grow w-full max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- ==================== TELA 1: LOGIN ==================== -->
        <section id="screen-login" class="screen active flex flex-col items-center justify-center min-h-[70vh]">
            <div class="glass-panel p-8 rounded-2xl w-full max-w-md text-center shadow-xl animate-slide-up">
                <div class="w-20 h-20 bg-primary/20 rounded-full flex items-center justify-center mx-auto mb-6">
                    <i class="fa-solid fa-user-shield text-4xl text-primary"></i>
                </div>
                <h1 class="text-2xl font-bold mb-2">Acesso Restrito</h1>
                <p class="text-gray-400 mb-8 text-sm">Faça login para acessar seu ambiente profissional e gerenciar seus clientes.</p>
                
                <button id="btn-login" class="w-full bg-primary hover:bg-primaryDark text-white font-bold py-3 px-4 rounded-lg flex items-center justify-center transition duration-300">
                    <i class="fa-brands fa-google mr-2 text-xl"></i>
                    Entrar com Conta Google
                </button>
                <p id="login-status" class="mt-4 text-sm text-gray-500"></p>
            </div>
        </section>

        <!-- ==================== TELA 2: PERFIL DO PROFISSIONAL ==================== -->
        <section id="screen-profile" class="screen max-w-2xl mx-auto">
            <div class="mb-6">
                <h2 class="text-2xl font-bold border-l-4 border-primary pl-3">Configurar Perfil</h2>
                <p class="text-gray-400 text-sm mt-1">Defina seus dados profissionais antes de começar.</p>
            </div>

            <div class="glass-panel p-6 rounded-xl animate-fade-in">
                <form id="form-profile" class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium mb-1">Nome Completo *</label>
                        <input type="text" id="prof-nome" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-3 text-white focus:outline-none focus:border-primary transition">
                    </div>
                    
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Tipo de Profissional *</label>
                            <select id="prof-tipo" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-3 text-white focus:outline-none focus:border-primary transition">
                                <option value="">Selecione...</option>
                                <option value="educador">Profissional de Educação Física</option>
                                <option value="treinador">Treinador Esportivo</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Estado (UF) *</label>
                            <select id="prof-uf" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-3 text-white focus:outline-none focus:border-primary transition">
                                <option value="AC">AC</option><option value="AL">AL</option><option value="AP">AP</option>
                                <option value="AM">AM</option><option value="BA">BA</option><option value="CE">CE</option>
                                <option value="DF">DF</option><option value="ES">ES</option><option value="GO">GO</option>
                                <option value="MA">MA</option><option value="MT">MT</option><option value="MS">MS</option>
                                <option value="MG">MG</option><option value="PA">PA</option><option value="PB">PB</option>
                                <option value="PR">PR</option><option value="PE">PE</option><option value="PI">PI</option>
                                <option value="RJ">RJ</option><option value="RN" selected>RN</option><option value="RS">RS</option>
                                <option value="RO">RO</option><option value="RR">RR</option><option value="SC">SC</option>
                                <option value="SP">SP</option><option value="SE">SE</option><option value="TO">TO</option>
                            </select>
                        </div>
                    </div>

                    <div id="cref-container" class="hidden animate-fade-in">
                        <label class="block text-sm font-medium mb-1 text-primary">Número do CREF *</label>
                        <input type="text" id="prof-cref" placeholder="Ex: 000000-G/UF" class="w-full bg-darkInput border border-primary rounded-lg p-3 text-white focus:outline-none focus:ring-1 focus:ring-primary transition">
                        <p class="text-xs text-gray-400 mt-1"><i class="fa-solid fa-circle-info mr-1"></i> Obrigatório por lei (nº 9.696/1998) para prescrição de treinos.</p>
                    </div>

                    <div class="mt-8 pt-4 border-t border-gray-700">
                        <button type="submit" class="w-full bg-primary hover:bg-primaryDark text-white font-bold py-3 px-4 rounded-lg transition duration-300">
                            Salvar Perfil e Continuar
                        </button>
                    </div>
                </form>
            </div>
        </section>

        <!-- ==================== TELA 3: DASHBOARD ==================== -->
        <section id="screen-dashboard" class="screen">
            <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-4">
                <div>
                    <h2 class="text-3xl font-bold" id="dash-greeting">Olá, Profissional</h2>
                    <p class="text-gray-400">Gerencie suas avaliações e clientes.</p>
                </div>
                <button onclick="ui.navigateTo('screen-form')" class="bg-primary hover:bg-primaryDark text-white font-bold py-2 px-6 rounded-lg shadow-lg flex items-center transition transform hover:scale-105">
                    <i class="fa-solid fa-plus mr-2"></i> Nova Avaliação
                </button>
            </div>

            <!-- Cards de Estatísticas -->
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
                <div class="glass-panel p-4 rounded-xl text-center border-l-4 border-l-primary">
                    <p class="text-sm text-gray-400">Total de Avaliações</p>
                    <p class="text-3xl font-bold mt-1" id="stat-total">0</p>
                </div>
                <div class="glass-panel p-4 rounded-xl text-center border-l-4 border-l-green-500">
                    <p class="text-sm text-gray-400">Neste Mês</p>
                    <p class="text-3xl font-bold mt-1" id="stat-month">0</p>
                </div>
            </div>

            <div class="glass-panel rounded-xl overflow-hidden">
                <div class="p-4 border-b border-gray-700 bg-gray-800 flex justify-between items-center">
                    <h3 class="font-bold"><i class="fa-solid fa-history mr-2"></i>Histórico Recente</h3>
                    <input type="text" id="search-client" placeholder="Buscar cliente..." class="bg-darkInput border border-gray-600 rounded p-1 px-3 text-sm focus:outline-none focus:border-primary w-48">
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-gray-800 text-gray-400 text-sm">
                                <th class="p-3 font-medium">Data</th>
                                <th class="p-3 font-medium">Cliente</th>
                                <th class="p-3 font-medium">Objetivo</th>
                                <th class="p-3 font-medium text-center">Ação</th>
                            </tr>
                        </thead>
                        <tbody id="assessments-list" class="text-sm divide-y divide-gray-700">
                            <!-- Preenchido via JS -->
                            <tr><td colspan="4" class="p-4 text-center text-gray-500">Carregando histórico...</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- ==================== TELA 4: FORMULÁRIO DE AVALIAÇÃO ==================== -->
        <section id="screen-form" class="screen max-w-4xl mx-auto">
            <div class="flex items-center justify-between mb-6">
                <div>
                    <h2 class="text-2xl font-bold border-l-4 border-primary pl-3">Nova Avaliação</h2>
                    <p class="text-gray-400 text-sm mt-1">Preencha os dados com atenção.</p>
                </div>
                <button onclick="ui.navigateTo('screen-dashboard')" class="text-gray-400 hover:text-white transition">
                    <i class="fa-solid fa-arrow-left mr-1"></i> Voltar
                </button>
            </div>

            <form id="form-assessment" class="space-y-6">
                <!-- SEÇÃO: DADOS BÁSICOS -->
                <div class="glass-panel p-6 rounded-xl border-t-4 border-t-primary">
                    <h3 class="font-bold text-lg mb-4 text-primary"><i class="fa-solid fa-user mr-2"></i>Dados do Avaliado</h3>
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                        <div class="md:col-span-4">
                            <label class="block text-sm font-medium mb-1">Nome Completo *</label>
                            <input type="text" id="av-nome" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white focus:border-primary outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Idade *</label>
                            <input type="number" id="av-idade" required min="1" class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white focus:border-primary outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Sexo *</label>
                            <select id="av-sexo" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white focus:border-primary outline-none">
                                <option value="M">Masculino</option>
                                <option value="F">Feminino</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Peso (kg) *</label>
                            <input type="number" step="0.1" id="av-peso" required placeholder="Ex: 75.5" class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white focus:border-primary outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Estatura (cm) *</label>
                            <input type="number" id="av-altura" required placeholder="Ex: 175" class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white focus:border-primary outline-none">
                        </div>
                    </div>
                </div>

                <!-- SEÇÃO: ANAMNESE -->
                <div class="glass-panel p-6 rounded-xl border-t-4 border-t-purple-500">
                    <h3 class="font-bold text-lg mb-4 text-purple-400"><i class="fa-solid fa-notes-medical mr-2"></i>Anamnese</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Objetivo Principal *</label>
                            <select id="av-objetivo" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white outline-none">
                                <option value="Emagrecimento">Emagrecimento</option>
                                <option value="Hipertrofia">Hipertrofia</option>
                                <option value="Saude">Saúde / Condicionamento</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Nível de Atividade</label>
                            <select id="av-atividade" class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white outline-none">
                                <option value="Sedentario">Sedentário</option>
                                <option value="Leve">Leve (1-2x semana)</option>
                                <option value="Moderado">Moderado (3-4x semana)</option>
                                <option value="Intenso">Intenso (5+ semana)</option>
                            </select>
                        </div>
                        <div class="md:col-span-2">
                            <label class="block text-sm font-medium mb-1">Lesões / Dores Articulares</label>
                            <input type="text" id="av-lesoes" placeholder="Nenhuma ou descreva..." class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Uso de Medicamentos</label>
                            <input type="text" id="av-meds" placeholder="Descreva se houver" class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Qualidade do Sono</label>
                            <select id="av-sono" class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white outline-none">
                                <option value="Bom">Bom (7-8h reparadoras)</option>
                                <option value="Regular">Regular</option>
                                <option value="Ruim">Ruim (Insônia/Acorda muito)</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- SEÇÃO: PERÍMETROS -->
                <div class="glass-panel p-6 rounded-xl border-t-4 border-t-yellow-500">
                    <h3 class="font-bold text-lg mb-4 text-yellow-400"><i class="fa-solid fa-ruler mr-2"></i>Perímetros (cm)</h3>
                    <p class="text-xs text-gray-400 mb-4">Medidas em centímetros. Preencha os campos com valores.</p>
                    
                    <!-- Foco em RCQ primeiro -->
                    <div class="grid grid-cols-2 gap-4 mb-4 pb-4 border-b border-gray-700">
                        <div>
                            <label class="block text-sm font-bold text-yellow-200 mb-1">Cintura * <span class="text-xs font-normal text-gray-400">(menor diâmetro)</span></label>
                            <input type="number" step="0.1" id="per-cintura" required class="w-full bg-darkInput border border-yellow-700 rounded-lg p-2 text-white outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-bold text-yellow-200 mb-1">Quadril * <span class="text-xs font-normal text-gray-400">(maior proeminência)</span></label>
                            <input type="number" step="0.1" id="per-quadril" required class="w-full bg-darkInput border border-yellow-700 rounded-lg p-2 text-white outline-none">
                        </div>
                    </div>

                    <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                        <div><label class="block text-xs mb-1 text-gray-300">Pescoço</label><input type="number" step="0.1" id="per-pescoco" class="w-full bg-darkInput border border-gray-600 rounded p-2 text-sm outline-none"></div>
                        <div><label class="block text-xs mb-1 text-gray-300">Ombros</label><input type="number" step="0.1" id="per-ombros" class="w-full bg-darkInput border border-gray-600 rounded p-2 text-sm outline-none"></div>
                        <div><label class="block text-xs mb-1 text-gray-300">Tórax</label><input type="number" step="0.1" id="per-torax" class="w-full bg-darkInput border border-gray-600 rounded p-2 text-sm outline-none"></div>
                        <div><label class="block text-xs mb-1 text-gray-300">Abdominal</label><input type="number" step="0.1" id="per-abd" class="w-full bg-darkInput border border-gray-600 rounded p-2 text-sm outline-none"></div>
                        
                        <div><label class="block text-xs mb-1 text-gray-300">Braço Relaxado (D)</label><input type="number" step="0.1" id="per-braco-rel-d" class="w-full bg-darkInput border border-gray-600 rounded p-2 text-sm outline-none"></div>
                        <div><label class="block text-xs mb-1 text-gray-300">Braço Contraído (D)</label><input type="number" step="0.1" id="per-braco-con-d" class="w-full bg-darkInput border border-gray-600 rounded p-2 text-sm outline-none"></div>
                        
                        <div><label class="block text-xs mb-1 text-gray-300">Coxa Medial (D)</label><input type="number" step="0.1" id="per-coxa-d" class="w-full bg-darkInput border border-gray-600 rounded p-2 text-sm outline-none"></div>
                        <div><label class="block text-xs mb-1 text-gray-300">Panturrilha (D)</label><input type="number" step="0.1" id="per-pant-d" class="w-full bg-darkInput border border-gray-600 rounded p-2 text-sm outline-none"></div>
                    </div>
                </div>

                <!-- SEÇÃO: COMPOSIÇÃO CORPORAL (DOBRAS) -->
                <div class="glass-panel p-6 rounded-xl border-t-4 border-t-green-500">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="font-bold text-lg text-green-400"><i class="fa-solid fa-layer-group mr-2"></i>Dobras Cutâneas (mm)</h3>
                        <select id="protocolo-dobras" class="bg-darkInput border border-gray-600 rounded-lg p-1 text-sm text-white outline-none">
                            <option value="pollock3">Pollock 3 Dobras</option>
                            <option value="pollock7">Pollock 7 Dobras</option>
                            <option value="nenhum">Não realizar</option>
                        </select>
                    </div>

                    <div id="container-dobras" class="grid grid-cols-2 md:grid-cols-4 gap-4 transition-all">
                        <!-- Campos gerados via JS dependendo do protocolo -->
                    </div>
                </div>

                <!-- Botões -->
                <div class="flex gap-4 pt-4">
                    <button type="button" onclick="ui.navigateTo('screen-dashboard')" class="w-1/3 bg-gray-700 hover:bg-gray-600 text-white font-bold py-3 rounded-lg transition">Cancelar</button>
                    <button type="submit" class="w-2/3 bg-primary hover:bg-primaryDark text-white font-bold py-3 rounded-lg shadow-lg flex justify-center items-center transition">
                        <i class="fa-solid fa-calculator mr-2"></i> Calcular e Salvar Resultados
                    </button>
                </div>
            </form>
        </section>

        <!-- ==================== TELA 5: RESULTADOS E RELATÓRIO ==================== -->
        <section id="screen-results" class="screen">
            <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 no-print">
                <button onclick="ui.navigateTo('screen-dashboard')" class="text-gray-400 hover:text-white transition mb-2 md:mb-0">
                    <i class="fa-solid fa-arrow-left mr-1"></i> Voltar ao Painel
                </button>
                <div class="flex gap-2">
                    <button onclick="actions.generatePDF()" class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded shadow flex items-center">
                        <i class="fa-solid fa-file-pdf mr-2"></i> Exportar PDF
                    </button>
                </div>
            </div>

            <!-- ÁREA A SER IMPRESSA -->
            <div id="pdf-content" class="bg-darkBg text-white p-2 md:p-6 rounded-xl relative">
                <!-- Cabeçalho do Relatório -->
                <div class="border-b-2 border-primary pb-4 mb-6 text-center flex flex-col items-center">
                    <h1 class="text-3xl font-bold tracking-wider uppercase mb-1">Avaliação Física</h1>
                    <p class="text-primary font-medium" id="res-prof-nome">Profissional Responsável</p>
                    <p class="text-xs text-gray-400" id="res-prof-cref"></p>
                    <p class="text-sm mt-2 text-gray-300">Data: <span id="res-data">--/--/----</span></p>
                </div>

                <!-- Dados do Cliente -->
                <div class="glass-panel p-4 rounded-lg mb-6 flex flex-wrap justify-between items-center">
                    <div>
                        <p class="text-xl font-bold" id="res-cli-nome">Nome do Cliente</p>
                        <p class="text-sm text-gray-400"><span id="res-cli-idade">00</span> anos | Sexo: <span id="res-cli-sexo">-</span></p>
                    </div>
                    <div class="text-right">
                        <p class="text-sm text-gray-400">Objetivo</p>
                        <p class="font-bold text-primary" id="res-cli-objetivo">Emagrecimento</p>
                    </div>
                </div>

                <!-- Painel Principal de Indicadores -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
                    
                    <!-- Box IMC -->
                    <div class="glass-panel p-5 rounded-xl border-l-4" id="box-imc">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="text-sm text-gray-400 uppercase tracking-wider font-bold">Índice de Massa Corporal</h3>
                                <div class="flex items-baseline gap-2 mt-1">
                                    <span class="text-4xl font-black" id="res-imc-val">0.0</span>
                                    <span class="text-sm text-gray-400">kg/m²</span>
                                </div>
                            </div>
                            <i class="fa-solid fa-weight-scale text-3xl text-gray-500 opacity-50"></i>
                        </div>
                        <div class="mt-4 flex items-center justify-between">
                            <span class="badge" id="res-imc-class">Classificação</span>
                            <span class="text-xs text-gray-400">Peso: <span id="res-peso" class="text-white font-bold">0</span>kg | Alt: <span id="res-altura" class="text-white font-bold">0</span>cm</span>
                        </div>
                    </div>

                    <!-- Box Gordura -->
                    <div class="glass-panel p-5 rounded-xl border-l-4" id="box-bf">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="text-sm text-gray-400 uppercase tracking-wider font-bold">Gordura Corporal</h3>
                                <div class="flex items-baseline gap-2 mt-1">
                                    <span class="text-4xl font-black" id="res-bf-val">0.0</span>
                                    <span class="text-xl font-bold text-gray-300">%</span>
                                </div>
                            </div>
                            <i class="fa-solid fa-droplet text-3xl text-gray-500 opacity-50"></i>
                        </div>
                        <div class="mt-4 flex items-center justify-between">
                            <span class="badge" id="res-bf-class">Classificação</span>
                            <span class="text-xs text-gray-400" id="res-protocolo-nome">Protocolo: -</span>
                        </div>
                    </div>

                </div>

                <!-- Detalhamento da Composição -->
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
                    <!-- Gráfico -->
                    <div class="glass-panel p-4 rounded-xl col-span-1 md:col-span-1 flex flex-col items-center justify-center">
                        <h3 class="text-sm font-bold text-gray-400 uppercase mb-2">Composição (Kg)</h3>
                        <div class="w-40 h-40 relative">
                            <canvas id="compChart"></canvas>
                        </div>
                    </div>
                    
                    <!-- Massa Magra / Gorda -->
                    <div class="glass-panel p-4 rounded-xl col-span-1 md:col-span-1 flex flex-col justify-center">
                        <div class="mb-4">
                            <div class="flex justify-between items-center mb-1">
                                <span class="text-gray-300 text-sm"><i class="fa-solid fa-cube text-primary mr-2"></i>Massa Magra</span>
                                <span class="font-bold text-lg" id="res-massamagra">0.0 kg</span>
                            </div>
                            <div class="w-full bg-gray-700 rounded-full h-2">
                                <div class="bg-primary h-2 rounded-full" id="bar-magra" style="width: 0%"></div>
                            </div>
                        </div>
                        <div>
                            <div class="flex justify-between items-center mb-1">
                                <span class="text-gray-300 text-sm"><i class="fa-solid fa-fire text-yellow-500 mr-2"></i>Massa Gorda</span>
                                <span class="font-bold text-lg" id="res-massagorda">0.0 kg</span>
                            </div>
                            <div class="w-full bg-gray-700 rounded-full h-2">
                                <div class="bg-yellow-500 h-2 rounded-full" id="bar-gorda" style="width: 0%"></div>
                            </div>
                        </div>
                    </div>

                    <!-- Risco Cardiovascular -->
                    <div class="glass-panel p-4 rounded-xl col-span-1 md:col-span-1 border-l-4" id="box-rcq">
                        <h3 class="text-sm font-bold text-gray-400 uppercase mb-2"><i class="fa-solid fa-heart-pulse mr-2"></i>Risco Cardiovascular</h3>
                        <p class="text-xs text-gray-400 mb-2">Relação Cintura-Quadril (RCQ)</p>
                        <div class="text-3xl font-black mb-2" id="res-rcq-val">0.00</div>
                        <span class="badge w-full block text-center" id="res-rcq-class">Risco</span>
                        <div class="mt-4 text-xs text-gray-400 border-t border-gray-700 pt-2">
                            Cintura: <span id="res-cintura">0</span>cm | Quadril: <span id="res-quadril">0</span>cm
                        </div>
                    </div>
                </div>

                <!-- Inteligência Artificial / Recomendações -->
                <div class="glass-panel p-5 rounded-xl border border-primary/50 bg-primary/5 mb-6 relative overflow-hidden">
                    <div class="absolute top-0 right-0 p-4 opacity-10">
                        <i class="fa-solid fa-robot text-6xl"></i>
                    </div>
                    <h3 class="font-bold text-primary mb-3 flex items-center">
                        <i class="fa-solid fa-lightbulb text-yellow-400 mr-2"></i> Análise e Recomendações
                    </h3>
                    <ul id="res-recomendacoes" class="list-disc pl-5 space-y-2 text-sm text-gray-200 relative z-10">
                        <!-- Gerado via JS -->
                    </ul>
                </div>

                <!-- Rodapé Legal -->
                <div class="mt-8 pt-4 border-t border-gray-700 text-center text-xs text-gray-500">
                    <p id="footer-legal">As recomendações clínicas e prescrições de treinamento são de exclusiva responsabilidade do Responsável Técnico.</p>
                    <p class="mt-1 font-bold text-gray-400" id="footer-rt">RT: -</p>
                    <p class="mt-4 opacity-50">Gerado por PowFit Med's Software</p>
                </div>
            </div>
        </section>

    </main>

    <!-- Modal de Loading Overlay -->
    <div id="loading-overlay" class="fixed inset-0 bg-black/80 z-[100] hidden items-center justify-center backdrop-blur-sm transition-opacity">
        <div class="text-center">
            <i class="fa-solid fa-circle-notch fa-spin text-primary text-5xl mb-4"></i>
            <p class="font-bold tracking-widest uppercase text-sm animate-pulse">Sincronizando Dados...</p>
        </div>
    </div>

    <!-- SCRIPT PRINCIPAL (MÓDULOS INTEGRADOS) -->
    <script type="module">
        // Importando módulos do Firebase diretamente do CDN conforme regras do sistema Canvas
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithCustomToken, signInAnonymously, onAuthStateChanged, GoogleAuthProvider, signInWithPopup, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, query, onSnapshot, addDoc, serverTimestamp, orderBy } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // ==========================================
        // 1. CONFIGURAÇÃO & ESTADO GLOBAL
        // ==========================================
        const state = {
            user: null,
            profile: null,
            assessments: [],
            currentAppId: typeof __app_id !== 'undefined' ? __app_id : 'powfit-meds-v1',
            chartInstance: null
        };

        const DOM = {
            screens: document.querySelectorAll('.screen'),
            loading: document.getElementById('loading-overlay'),
            btnLogin: document.getElementById('btn-login'),
            loginStatus: document.getElementById('login-status'),
            navActions: document.getElementById('navActions'),
            
            // Formulários
            formProfile: document.getElementById('form-profile'),
            formAssessment: document.getElementById('form-assessment'),
            
            // Perfil
            profTipo: document.getElementById('prof-tipo'),
            crefContainer: document.getElementById('cref-container'),
            profCref: document.getElementById('prof-cref'),
            
            // Form Avaliação
            protocoloDobras: document.getElementById('protocolo-dobras'),
            containerDobras: document.getElementById('container-dobras')
        };

        // ==========================================
        // 2. FIREBASE INICIALIZAÇÃO (Padrão Canvas Estrito)
        // ==========================================
        
        // Tenta pegar a config injetada pelo ambiente, ou usa um mock para rodar offline puramente visual
        let firebaseConfig = {};
        try {
            if (typeof __firebase_config !== 'undefined') {
                firebaseConfig = JSON.parse(__firebase_config);
            } else {
                // Fallback para uso local/teste caso o usuário abra o HTML direto
                console.warn("Executando sem __firebase_config real. Modos restritos podem falhar.");
                firebaseConfig = {
                    apiKey: "fake-key", projectId: "demo-project", appId: "1:123"
                };
            }
        } catch(e) { console.error("Erro na config do Firebase", e); }

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // Função de Inicialização de Auth (Obrigatória antes de queries)
        const initAuth = async (forceGoogle = false) => {
            ui.showLoading(true);
            try {
                if (forceGoogle) {
                    const provider = new GoogleAuthProvider();
                    await signInWithPopup(auth, provider);
                } else {
                    // Fluxo padrão do Canvas
                    if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                        await signInWithCustomToken(auth, __initial_auth_token);
                    } else {
                        await signInAnonymously(auth);
                    }
                }
            } catch (error) {
                console.error("Erro na Autenticação:", error);
                DOM.loginStatus.innerText = "Erro ao conectar. Executando em modo local.";
                DOM.loginStatus.classList.add('text-red-500');
            } finally {
                ui.showLoading(false);
            }
        };

        // ==========================================
        // 3. UI & NAVEGAÇÃO
        // ==========================================
        const ui = {
            showScreen: (screenId) => {
                DOM.screens.forEach(s => s.classList.remove('active'));
                document.getElementById(screenId).classList.add('active');
                window.scrollTo(0,0);
            },
            navigateTo: (screenId) => {
                ui.showScreen(screenId);
                if(screenId === 'screen-form') formHandlers.renderDobrasInputs();
            },
            showLoading: (show) => {
                if(show) {
                    DOM.loading.style.display = 'flex';
                    DOM.loading.classList.remove('hidden');
                } else {
                    DOM.loading.style.display = 'none';
                    DOM.loading.classList.add('hidden');
                }
            },
            renderNavActions: () => {
                const nav = document.getElementById('nav-actions');
                if (state.user && state.profile) {
                    nav.innerHTML = `
                        <div class="text-right mr-3 hidden md:block">
                            <p class="text-sm font-bold leading-tight">${state.profile.nome.split(' ')[0]}</p>
                            <p class="text-xs text-primary leading-tight">${state.profile.tipo === 'educador' ? 'Profissional Ed. Física' : 'Treinador'}</p>
                        </div>
                        <div class="w-10 h-10 rounded-full bg-gray-700 flex items-center justify-center text-xl cursor-pointer hover:bg-gray-600 border-2 border-primary" onclick="ui.navigateTo('screen-profile')">
                            <i class="fa-solid fa-user-gear"></i>
                        </div>
                        <button onclick="actions.logout()" class="text-gray-400 hover:text-red-400 ml-2" title="Sair"><i class="fa-solid fa-right-from-bracket"></i></button>
                    `;
                } else {
                    nav.innerHTML = ``;
                }
            },
            updateDashboard: () => {
                document.getElementById('dash-greeting').innerText = `Olá, ${state.profile?.nome?.split(' ')[0] || 'Profissional'}`;
                
                const list = document.getElementById('assessments-list');
                if (state.assessments.length === 0) {
                    list.innerHTML = `<tr><td colspan="4" class="p-4 text-center text-gray-500"><i class="fa-regular fa-folder-open text-2xl block mb-2"></i>Nenhuma avaliação encontrada.</td></tr>`;
                    document.getElementById('stat-total').innerText = '0';
                    document.getElementById('stat-month').innerText = '0';
                    return;
                }

                document.getElementById('stat-total').innerText = state.assessments.length;
                
                // Calculo do mês
                const currentMonth = new Date().getMonth();
                const monthCount = state.assessments.filter(a => new Date(a.data).getMonth() === currentMonth).length;
                document.getElementById('stat-month').innerText = monthCount;

                list.innerHTML = state.assessments.map(a => {
                    const dateObj = new Date(a.data);
                    const formattedDate = `${dateObj.getDate().toString().padStart(2,'0')}/${(dateObj.getMonth()+1).toString().padStart(2,'0')}/${dateObj.getFullYear()}`;
                    return `
                        <tr class="hover:bg-gray-700/50 transition">
                            <td class="p-3 whitespace-nowrap text-gray-300">${formattedDate}</td>
                            <td class="p-3 font-medium">${a.cliente.nome}</td>
                            <td class="p-3"><span class="bg-gray-700 px-2 py-1 rounded text-xs">${a.anamnese.objetivo}</span></td>
                            <td class="p-3 text-center">
                                <button onclick="actions.viewResult('${a.id}')" class="text-primary hover:text-white bg-primary/10 hover:bg-primary/30 p-2 rounded transition">
                                    <i class="fa-solid fa-eye"></i> Ver
                                </button>
                            </td>
                        </tr>
                    `;
                }).join('');
            }
        };

        // ==========================================
        // 4. LÓGICA DE DADOS & FIREBASE
        // ==========================================
        const dbActions = {
            checkProfile: async (uid) => {
                try {
                    // REGRA 1 E 3: Path correto e usuário logado
                    const docRef = doc(db, 'artifacts', state.currentAppId, 'users', uid, 'profile');
                    const docSnap = await getDoc(docRef);
                    
                    if (docSnap.exists()) {
                        state.profile = docSnap.data();
                        ui.renderNavActions();
                        dbActions.listenAssessments(uid);
                        ui.showScreen('screen-dashboard');
                    } else {
                        ui.showScreen('screen-profile');
                    }
                } catch (error) {
                    console.error("Erro ao checar perfil:", error);
                    // Fallback para localStorage se Firebase falhar por permissões no ambiente
                    const localProfile = localStorage.getItem('powfit_profile');
                    if (localProfile) {
                        state.profile = JSON.parse(localProfile);
                        ui.renderNavActions();
                        dbActions.listenAssessments(uid); // Tenta carregar do storage local
                        ui.showScreen('screen-dashboard');
                    } else {
                        ui.showScreen('screen-profile');
                    }
                }
            },
            saveProfile: async (profileData) => {
                ui.showLoading(true);
                try {
                    if (state.user) {
                        const docRef = doc(db, 'artifacts', state.currentAppId, 'users', state.user.uid, 'profile');
                        await setDoc(docRef, profileData);
                    }
                    localStorage.setItem('powfit_profile', JSON.stringify(profileData));
                    state.profile = profileData;
                    ui.renderNavActions();
                    dbActions.listenAssessments(state.user?.uid || 'local');
                    ui.showScreen('screen-dashboard');
                } catch (error) {
                    console.error("Erro ao salvar perfil:", error);
                    alert("Aviso: Salvo apenas localmente devido a restrições do ambiente.");
                } finally {
                    ui.showLoading(false);
                }
            },
            listenAssessments: (uid) => {
                if (uid && uid !== 'local') {
                    // REGRA 2: Query simples
                    const q = collection(db, 'artifacts', state.currentAppId, 'users', uid, 'assessments');
                    onSnapshot(q, (snapshot) => {
                        const assessments = [];
                        snapshot.forEach((doc) => {
                            assessments.push({ id: doc.id, ...doc.data() });
                        });
                        // Ordenar localmente (Regra 2)
                        state.assessments = assessments.sort((a,b) => new Date(b.data) - new Date(a.data));
                        ui.updateDashboard();
                    }, (error) => {
                        console.error("Erro listener avaliações:", error);
                        dbActions.loadLocalAssessments();
                    });
                } else {
                    dbActions.loadLocalAssessments();
                }
            },
            loadLocalAssessments: () => {
                const localData = localStorage.getItem('powfit_assessments');
                state.assessments = localData ? JSON.parse(localData) : [];
                ui.updateDashboard();
            },
            saveAssessment: async (data) => {
                ui.showLoading(true);
                try {
                    const assessmentData = {
                        ...data,
                        data: new Date().toISOString()
                    };

                    if (state.user) {
                        const colRef = collection(db, 'artifacts', state.currentAppId, 'users', state.user.uid, 'assessments');
                        const docRef = await addDoc(colRef, assessmentData);
                        assessmentData.id = docRef.id;
                    } else {
                        assessmentData.id = 'loc_' + Date.now();
                        const local = JSON.parse(localStorage.getItem('powfit_assessments') || '[]');
                        local.unshift(assessmentData);
                        localStorage.setItem('powfit_assessments', JSON.stringify(local));
                        state.assessments = local;
                    }
                    
                    ui.showLoading(false);
                    actions.viewResult(assessmentData.id, assessmentData); // Mostra o resultado recém calculado
                    DOM.formAssessment.reset();
                } catch (error) {
                    console.error("Erro ao salvar avaliação:", error);
                    ui.showLoading(false);
                }
            }
        };

        // Monitor de Autenticação (Regra 3: sempre após Init)
        onAuthStateChanged(auth, (user) => {
            if (user) {
                state.user = user;
                dbActions.checkProfile(user.uid);
            } else {
                state.user = null;
                state.profile = null;
                ui.showScreen('screen-login');
            }
        });

        // ==========================================
        // 5. LÓGICA DE FORMULÁRIOS E CÁLCULOS
        // ==========================================
        const calcConfig = {
            pollock3M: ['peito', 'abdomen', 'coxa'],
            pollock3F: ['triceps', 'suprailiaca', 'coxa'],
            pollock7: ['peito', 'axilar', 'triceps', 'subescapular', 'abdomen', 'suprailiaca', 'coxa']
        };

        const formHandlers = {
            renderDobrasInputs: () => {
                const protocolo = DOM.protocoloDobras.value;
                const sexo = document.getElementById('av-sexo').value;
                const container = DOM.containerDobras;
                container.innerHTML = '';

                if (protocolo === 'nenhum') {
                    container.innerHTML = '<p class="col-span-4 text-sm text-gray-400 italic">Cálculo de percentual de gordura desativado.</p>';
                    return;
                }

                let fields = [];
                if (protocolo === 'pollock7') {
                    fields = [
                        {id: 'dc-peito', label: 'Peitoral'}, {id: 'dc-axilar', label: 'Axilar Média'},
                        {id: 'dc-triceps', label: 'Tríceps'}, {id: 'dc-sub', label: 'Subescapular'},
                        {id: 'dc-abdomen', label: 'Abdominal'}, {id: 'dc-supra', label: 'Suprailíaca'},
                        {id: 'dc-coxa', label: 'Coxa'}
                    ];
                } else if (protocolo === 'pollock3') {
                    if (sexo === 'M') {
                        fields = [{id: 'dc-peito', label: 'Peitoral'}, {id: 'dc-abdomen', label: 'Abdominal'}, {id: 'dc-coxa', label: 'Coxa'}];
                    } else {
                        fields = [{id: 'dc-triceps', label: 'Tríceps'}, {id: 'dc-supra', label: 'Suprailíaca'}, {id: 'dc-coxa', label: 'Coxa'}];
                    }
                }

                fields.forEach(f => {
                    container.innerHTML += `
                        <div>
                            <label class="block text-xs mb-1 font-medium text-gray-300">${f.label}</label>
                            <input type="number" step="0.1" id="${f.id}" required class="w-full bg-darkInput border border-green-700 rounded p-2 text-sm text-white outline-none focus:border-green-400">
                        </div>
                    `;
                });
            }
        };

        const calculators = {
            getIMC: (peso, alturaCm) => {
                const altM = alturaCm / 100;
                const imc = peso / (altM * altM);
                let classe = ''; let color = '';
                if (imc < 18.5) { classe = 'Baixo Peso'; color = 'yellow'; }
                else if (imc < 24.9) { classe = 'Normal'; color = 'green'; }
                else if (imc < 29.9) { classe = 'Sobrepeso'; color = 'yellow'; }
                else { classe = 'Obesidade'; color = 'red'; }
                return { value: imc.toFixed(1), classe, color };
            },
            getRCQ: (cintura, quadril, sexo) => {
                const rcq = cintura / quadril;
                let classe = 'Baixo'; let color = 'green';
                if (sexo === 'M') {
                    if (rcq >= 0.90 && rcq < 0.95) { classe = 'Moderado'; color = 'yellow'; }
                    else if (rcq >= 0.95) { classe = 'Alto'; color = 'red'; }
                } else {
                    if (rcq >= 0.80 && rcq < 0.85) { classe = 'Moderado'; color = 'yellow'; }
                    else if (rcq >= 0.85) { classe = 'Alto'; color = 'red'; }
                }
                return { value: rcq.toFixed(2), classe, color };
            },
            getBodyFat: (protocolo, sexo, idade, dataObj) => {
                if(protocolo === 'nenhum') return null;
                
                let soma = 0;
                let d = 0;
                
                if (protocolo === 'pollock7') {
                    soma = ['peito', 'axilar', 'triceps', 'sub', 'abdomen', 'supra', 'coxa']
                           .reduce((acc, val) => acc + parseFloat(dataObj[`dc-${val}`] || 0), 0);
                    
                    if (sexo === 'M') d = 1.112 - (0.00043499 * soma) + (0.00000055 * Math.pow(soma, 2)) - (0.00028826 * idade);
                    else d = 1.097 - (0.00046971 * soma) + (0.00000056 * Math.pow(soma, 2)) - (0.00012828 * idade);
                } 
                else if (protocolo === 'pollock3') {
                    if (sexo === 'M') {
                        soma = parseFloat(dataObj['dc-peito']||0) + parseFloat(dataObj['dc-abdomen']||0) + parseFloat(dataObj['dc-coxa']||0);
                        d = 1.109380 - (0.0008267 * soma) + (0.0000016 * Math.pow(soma, 2)) - (0.0002574 * idade);
                    } else {
                        soma = parseFloat(dataObj['dc-triceps']||0) + parseFloat(dataObj['dc-supra']||0) + parseFloat(dataObj['dc-coxa']||0);
                        d = 1.0994921 - (0.0009929 * soma) + (0.0000023 * Math.pow(soma, 2)) - (0.0001392 * idade);
                    }
                }

                const fatPercent = ((4.95 / d) - 4.5) * 100;
                
                // Classificação simplificada de gordura
                let classe = 'Normal'; let color = 'green';
                if (sexo === 'M') {
                    if (fatPercent < 8) { classe = 'Essencial/Atleta'; color = 'blue'; }
                    else if (fatPercent > 20) { classe = 'Acima da Média'; color = 'yellow'; }
                    if (fatPercent > 25) { classe = 'Obesidade'; color = 'red'; }
                } else {
                    if (fatPercent < 15) { classe = 'Essencial/Atleta'; color = 'blue'; }
                    else if (fatPercent > 30) { classe = 'Acima da Média'; color = 'yellow'; }
                    if (fatPercent > 35) { classe = 'Obesidade'; color = 'red'; }
                }

                return { value: fatPercent.toFixed(1), classe, color, protocoloName: protocolo === 'pollock7' ? 'Pollock 7 Dobras' : 'Pollock 3 Dobras' };
            },
            generateInsights: (imc, rcq, bf, obj) => {
                const insights = [];
                if (obj === 'Emagrecimento') {
                    insights.push("O foco primário do treinamento deve ser o déficit calórico aliado ao treino de força para manutenção de massa magra.");
                } else if (obj === 'Hipertrofia') {
                    insights.push("Focar em superávit calórico e progressão de carga no treinamento resistido.");
                }

                if (rcq.color === 'red') {
                    insights.push("<span class='text-red-400 font-bold'>Atenção:</span> O Risco Cardiovascular está elevado. Priorizar exercícios aeróbicos e controle dietético da gordura visceral.");
                }

                if (bf && bf.color === 'red') {
                    insights.push("O percentual de gordura indica obesidade. Protocolos de emagrecimento estruturado e avaliação médica são recomendados.");
                } else if (bf && bf.color === 'blue') {
                    insights.push("Excelente condicionamento percentual. Foco em manutenção e performance esportiva.");
                }

                if(insights.length === 0) insights.push("Parâmetros dentro da normalidade. Manter rotina de exercícios regulares e alimentação balanceada.");

                return insights;
            }
        };

        // ==========================================
        // 6. CONTROLADORES DE EVENTOS (Actions)
        // ==========================================
        const actions = {
            login: () => initAuth(true), // Tenta forçar Google Auth
            logout: () => {
                signOut(auth).then(() => {
                    localStorage.removeItem('powfit_profile');
                    localStorage.removeItem('powfit_assessments');
                });
            },
            viewResult: (id, rawData = null) => {
                const data = rawData || state.assessments.find(a => a.id === id);
                if(!data) return;

                // Preencher Header Legal
                document.getElementById('res-prof-nome').innerText = state.profile.nome;
                document.getElementById('footer-rt').innerText = `RT: ${state.profile.nome} ${state.profile.cref ? '- CREF: ' + state.profile.cref : ''}`;
                document.getElementById('res-prof-cref').innerText = state.profile.cref ? `CREF: ${state.profile.cref}` : 'Treinador Esportivo';
                
                const d = new Date(data.data);
                document.getElementById('res-data').innerText = `${d.getDate().toString().padStart(2,'0')}/${(d.getMonth()+1).toString().padStart(2,'0')}/${d.getFullYear()}`;

                // Preencher Cliente
                document.getElementById('res-cli-nome').innerText = data.cliente.nome;
                document.getElementById('res-cli-idade').innerText = data.cliente.idade;
                document.getElementById('res-cli-sexo').innerText = data.cliente.sexo === 'M' ? 'Masculino' : 'Feminino';
                document.getElementById('res-cli-objetivo').innerText = data.anamnese.objetivo;

                // Variáveis Base
                const peso = parseFloat(data.cliente.peso);
                document.getElementById('res-peso').innerText = peso;
                document.getElementById('res-altura').innerText = data.cliente.altura;

                // Cálculos
                const imc = calculators.getIMC(peso, data.cliente.altura);
                const rcq = calculators.getRCQ(data.perimetros.cintura, data.perimetros.quadril, data.cliente.sexo);
                const bf = calculators.getBodyFat(data.dobras.protocolo, data.cliente.sexo, data.cliente.idade, data.dobras);

                // Renderizar IMC
                document.getElementById('res-imc-val').innerText = imc.value;
                const elImcClass = document.getElementById('res-imc-class');
                elImcClass.innerText = imc.classe;
                elImcClass.className = `badge badge-${imc.color}`;
                document.getElementById('box-imc').style.borderColor = getComputedStyle(document.documentElement).getPropertyValue(`--tw-colors-${imc.color}-500`) || (imc.color==='green'?'#10b981':imc.color==='yellow'?'#f59e0b':'#ef4444');

                // Renderizar RCQ
                document.getElementById('res-rcq-val').innerText = rcq.value;
                document.getElementById('res-cintura').innerText = data.perimetros.cintura;
                document.getElementById('res-quadril').innerText = data.perimetros.quadril;
                const elRcqClass = document.getElementById('res-rcq-class');
                elRcqClass.innerText = `Risco: ${rcq.classe}`;
                elRcqClass.className = `badge badge-${rcq.color} mt-2 w-full block text-center`;
                document.getElementById('box-rcq').style.borderColor = imc.color==='green'?'#10b981':imc.color==='yellow'?'#f59e0b':'#ef4444';

                // Renderizar Gordura e Gráfico
                let valGorda = 0; let valMagra = peso;
                
                if (bf) {
                    document.getElementById('res-bf-val').innerText = bf.value;
                    const elBfClass = document.getElementById('res-bf-class');
                    elBfClass.innerText = bf.classe;
                    elBfClass.className = `badge badge-${bf.color}`;
                    document.getElementById('res-protocolo-nome').innerText = bf.protocoloName;
                    
                    valGorda = peso * (parseFloat(bf.value) / 100);
                    valMagra = peso - valGorda;
                } else {
                    document.getElementById('res-bf-val').innerText = '--';
                    document.getElementById('res-bf-class').innerText = 'Não Avaliado';
                    document.getElementById('res-bf-class').className = 'badge badge-gray';
                    document.getElementById('res-protocolo-nome').innerText = '-';
                }

                document.getElementById('res-massamagra').innerText = valMagra.toFixed(1) + ' kg';
                document.getElementById('res-massagorda').innerText = valGorda.toFixed(1) + ' kg';
                document.getElementById('bar-magra').style.width = `${(valMagra/peso)*100}%`;
                document.getElementById('bar-gorda').style.width = `${(valGorda/peso)*100}%`;

                // Render Chart.js
                if(state.chartInstance) state.chartInstance.destroy();
                const ctx = document.getElementById('compChart').getContext('2d');
                Chart.defaults.color = '#9ca3af';
                state.chartInstance = new Chart(ctx, {
                    type: 'doughnut',
                    data: {
                        labels: ['Massa Magra', 'Massa Gorda'],
                        datasets: [{
                            data: [valMagra.toFixed(1), valGorda.toFixed(1)],
                            backgroundColor: ['#3b82f6', '#f59e0b'],
                            borderWidth: 0,
                            hoverOffset: 4
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        cutout: '70%',
                        plugins: {
                            legend: { display: false }
                        }
                    }
                });

                // Insights
                const insights = calculators.generateInsights(imc, rcq, bf, data.anamnese.objetivo);
                document.getElementById('res-recomendacoes').innerHTML = insights.map(i => `<li>${i}</li>`).join('');

                ui.showScreen('screen-results');
            },
            generatePDF: () => {
                const element = document.getElementById('pdf-content');
                // Adiciona classe para forçar cores light na impressão
                document.body.classList.remove('dark');
                
                const opt = {
                    margin:       0.5,
                    filename:     `Avaliacao_${document.getElementById('res-cli-nome').innerText.replace(/\s+/g, '_')}.pdf`,
                    image:        { type: 'jpeg', quality: 0.98 },
                    html2canvas:  { scale: 2, useCORS: true, logging: false },
                    jsPDF:        { unit: 'in', format: 'a4', orientation: 'portrait' }
                };

                ui.showLoading(true);
                html2pdf().set(opt).from(element).save().then(() => {
                    document.body.classList.add('dark'); // Volta ao tema dark
                    ui.showLoading(false);
                });
            }
        };

        // Tornar 'actions' e 'ui' globais para os botões do HTML
        window.actions = actions;
        window.ui = ui;

        // ==========================================
        // 7. EVENT LISTENERS
        // ==========================================
        DOM.btnLogin.addEventListener('click', actions.login);

        DOM.profTipo.addEventListener('change', (e) => {
            if (e.target.value === 'educador') {
                DOM.crefContainer.classList.remove('hidden');
                DOM.profCref.setAttribute('required', 'true');
            } else {
                DOM.crefContainer.classList.add('hidden');
                DOM.profCref.removeAttribute('required');
            }
        });

        DOM.formProfile.addEventListener('submit', (e) => {
            e.preventDefault();
            const profile = {
                nome: document.getElementById('prof-nome').value,
                tipo: document.getElementById('prof-tipo').value,
                uf: document.getElementById('prof-uf').value,
                cref: document.getElementById('prof-tipo').value === 'educador' ? document.getElementById('prof-cref').value : null,
                updatedAt: new Date().toISOString()
            };
            dbActions.saveProfile(profile);
        });

        DOM.protocoloDobras.addEventListener('change', formHandlers.renderDobrasInputs);
        DOM.avSexo = document.getElementById('av-sexo');
        DOM.avSexo.addEventListener('change', formHandlers.renderDobrasInputs);

        DOM.formAssessment.addEventListener('submit', (e) => {
            e.preventDefault();
            
            // Coletar dados do form
            const assessmentData = {
                cliente: {
                    nome: document.getElementById('av-nome').value,
                    idade: document.getElementById('av-idade').value,
                    sexo: document.getElementById('av-sexo').value,
                    peso: document.getElementById('av-peso').value,
                    altura: document.getElementById('av-altura').value,
                },
                anamnese: {
                    objetivo: document.getElementById('av-objetivo').value,
                    atividade: document.getElementById('av-atividade').value,
                    lesoes: document.getElementById('av-lesoes').value,
                    meds: document.getElementById('av-meds').value,
                    sono: document.getElementById('av-sono').value,
                },
                perimetros: {
                    cintura: document.getElementById('per-cintura').value,
                    quadril: document.getElementById('per-quadril').value,
                    pescoco: document.getElementById('per-pescoco').value || null,
                    ombros: document.getElementById('per-ombros').value || null,
                    torax: document.getElementById('per-torax').value || null,
                    abd: document.getElementById('per-abd').value || null,
                    bracoRelD: document.getElementById('per-braco-rel-d').value || null,
                    bracoConD: document.getElementById('per-braco-con-d').value || null,
                    coxaD: document.getElementById('per-coxa-d').value || null,
                    pantD: document.getElementById('per-pant-d').value || null,
                },
                dobras: {
                    protocolo: document.getElementById('protocolo-dobras').value,
                }
            };

            // Coletar dobras dinâmicas
            if(assessmentData.dobras.protocolo !== 'nenhum') {
                const inputs = DOM.containerDobras.querySelectorAll('input');
                inputs.forEach(input => {
                    assessmentData.dobras[input.id] = input.value;
                });
            }

            dbActions.saveAssessment(assessmentData);
        });

        // Search simple filter
        document.getElementById('search-client').addEventListener('keyup', (e) => {
            const term = e.target.value.toLowerCase();
            const rows = document.getElementById('assessments-list').querySelectorAll('tr');
            rows.forEach(row => {
                if(row.innerText.toLowerCase().includes(term)) row.style.display = '';
                else row.style.display = 'none';
            });
        });

        // Init
        // Dispara o login anônimo automático se estiver rodando no Canvas para habilitar persistência
        if(typeof __initial_auth_token !== 'undefined') {
             initAuth(false);
        }

    </script>
</body>
</html>
