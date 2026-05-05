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
        body {
            background-color: #111827;
            color: #f3f4f6;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow-x: hidden;
        }
        
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #1f2937; }
        ::-webkit-scrollbar-thumb { background: #4b5563; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #3b82f6; }

        input[type=number]::-webkit-inner-spin-button, 
        input[type=number]::-webkit-outer-spin-button { -webkit-appearance: none; margin: 0; }
        input[type=number] { -moz-appearance: textfield; }

        .glass-panel { background: rgba(31, 41, 55, 0.7); backdrop-filter: blur(10px); border: 1px solid rgba(75, 85, 99, 0.4); }

        .badge { padding: 0.25rem 0.75rem; border-radius: 9999px; font-size: 0.75rem; font-weight: 600; }
        .badge-green { background-color: rgba(16, 185, 129, 0.2); color: #34d399; border: 1px solid #10b981; }
        .badge-yellow { background-color: rgba(245, 158, 11, 0.2); color: #fbbf24; border: 1px solid #f59e0b; }
        .badge-red { background-color: rgba(239, 68, 68, 0.2); color: #f87171; border: 1px solid #ef4444; }
        .badge-blue { background-color: rgba(59, 130, 246, 0.2); color: #60a5fa; border: 1px solid #3b82f6; }

        .screen { display: none; }
        .screen.active { display: block; animation: fadeIn 0.4s; }

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
        
        <!-- TELA 1: LOGIN -->
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

        <!-- TELA 2: PERFIL DO PROFISSIONAL -->
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

        <!-- TELA 3: DASHBOARD -->
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

        <!-- TELA 4: FORMULÁRIO DE AVALIAÇÃO -->
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
                    <p class="text-xs text-gray-400 mb-4">Medidas em centímetros.</p>
                    
                    <div class="grid grid-cols-2 gap-4 mb-4 pb-4 border-b border-gray-700">
                        <div>
                            <label class="block text-sm font-bold text-yellow-200 mb-1">Cintura *</label>
                            <input type="number" step="0.1" id="per-cintura" required class="w-full bg-darkInput border border-yellow-700 rounded-lg p-2 text-white outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-bold text-yellow-200 mb-1">Quadril *</label>
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

                <!-- SEÇÃO: COMPOSIÇÃO CORPORAL -->
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
                        <!-- Gerado via JS -->
                    </div>
                </div>

                <div class="flex gap-4 pt-4">
                    <button type="button" onclick="ui.navigateTo('screen-dashboard')" class="w-1/3 bg-gray-700 hover:bg-gray-600 text-white font-bold py-3 rounded-lg transition">Cancelar</button>
                    <button type="submit" class="w-2/3 bg-primary hover:bg-primaryDark text-white font-bold py-3 rounded-lg shadow-lg flex justify-center items-center transition">
                        <i class="fa-solid fa-calculator mr-2"></i> Calcular e Salvar
                    </button>
                </div>
            </form>
        </section>

        <!-- TELA 5: RESULTADOS E RELATÓRIO -->
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

            <!-- ÁREA DE IMPRESSÃO -->
            <div id="pdf-content" class="bg-darkBg text-white p-2 md:p-6 rounded-xl relative">
                <div class="border-b-2 border-primary pb-4 mb-6 text-center flex flex-col items-center">
                    <h1 class="text-3xl font-bold tracking-wider uppercase mb-1">Avaliação Física</h1>
                    <p class="text-primary font-medium" id="res-prof-nome">Profissional Responsável</p>
                    <p class="text-xs text-gray-400" id="res-prof-cref"></p>
                    <p class="text-sm mt-2 text-gray-300">Data: <span id="res-data">--/--/----</span></p>
                </div>

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

                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
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

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
                    <div class="glass-panel p-4 rounded-xl col-span-1 flex flex-col items-center justify-center">
                        <h3 class="text-sm font-bold text-gray-400 uppercase mb-2">Composição (Kg)</h3>
                        <div class="w-40 h-40 relative">
                            <canvas id="compChart"></canvas>
                        </div>
                    </div>
                    
                    <div class="glass-panel p-4 rounded-xl col-span-1 flex flex-col justify-center">
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

                    <div class="glass-panel p-4 rounded-xl col-span-1 border-l-4" id="box-rcq">
                        <h3 class="text-sm font-bold text-gray-400 uppercase mb-2"><i class="fa-solid fa-heart-pulse mr-2"></i>Risco Cardíaco</h3>
                        <p class="text-xs text-gray-400 mb-2">Relação Cintura-Quadril</p>
                        <div class="text-3xl font-black mb-2" id="res-rcq-val">0.00</div>
                        <span class="badge w-full block text-center" id="res-rcq-class">Risco</span>
                        <div class="mt-4 text-xs text-gray-400 border-t border-gray-700 pt-2">
                            Cintura: <span id="res-cintura">0</span>cm | Quadril: <span id="res-quadril">0</span>cm
                        </div>
                    </div>
                </div>

                <div class="glass-panel p-5 rounded-xl border border-primary/50 bg-primary/5 mb-6 relative overflow-hidden">
                    <div class="absolute top-0 right-0 p-4 opacity-10">
                        <i class="fa-solid fa-robot text-6xl"></i>
                    </div>
                    <h3 class="font-bold text-primary mb-3 flex items-center">
                        <i class="fa-solid fa-lightbulb text-yellow-400 mr-2"></i> Análise e Recomendações
                    </h3>
                    <ul id="res-recomendacoes" class="list-disc pl-5 space-y-2 text-sm text-gray-200 relative z-10">
                    </ul>
                </div>

                <div class="mt-8 pt-4 border-t border-gray-700 text-center text-xs text-gray-500">
                    <p id="footer-legal">As recomendações clínicas e prescrições de treinamento são de exclusiva responsabilidade do Responsável Técnico.</p>
                    <p class="mt-1 font-bold text-gray-400" id="footer-rt">RT: -</p>
                    <p class="mt-4 opacity-50">Gerado por PowFit Med's Software</p>
                </div>
            </div>
        </section>

    </main>

    <div id="loading-overlay" class="fixed inset-0 bg-black/80 z-[100] hidden items-center justify-center backdrop-blur-sm transition-opacity">
        <div class="text-center">
            <i class="fa-solid fa-circle-notch fa-spin text-primary text-5xl mb-4"></i>
            <p class="font-bold tracking-widest uppercase text-sm animate-pulse">Carregando...</p>
        </div>
    </div>

    <!-- SCRIPT PRINCIPAL -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, GoogleAuthProvider, signInWithPopup, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, onSnapshot, addDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // SUA CONFIGURAÇÃO OFICIAL DO FIREBASE (INSERIDA AQUI)
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

        const state = {
            user: null,
            profile: null,
            assessments: [],
            appId: 'powfit-meds', // Identificador da coleção principal
            chartInstance: null
        };

        const DOM = {
            screens: document.querySelectorAll('.screen'),
            loading: document.getElementById('loading-overlay'),
            btnLogin: document.getElementById('btn-login'),
            loginStatus: document.getElementById('login-status'),
            formProfile: document.getElementById('form-profile'),
            formAssessment: document.getElementById('form-assessment'),
            protocoloDobras: document.getElementById('protocolo-dobras'),
            containerDobras: document.getElementById('container-dobras')
        };

        const ui = {
            showScreen: (id) => {
                DOM.screens.forEach(s => s.classList.remove('active'));
                document.getElementById(id).classList.add('active');
                window.scrollTo(0,0);
            },
            navigateTo: (id) => {
                ui.showScreen(id);
                if(id === 'screen-form') formHandlers.renderDobrasInputs();
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
                } else { nav.innerHTML = ''; }
            },
            updateDashboard: () => {
                document.getElementById('dash-greeting').innerText = `Olá, ${state.profile?.nome?.split(' ')[0] || 'Profissional'}`;
                const list = document.getElementById('assessments-list');
                
                if (state.assessments.length === 0) {
                    list.innerHTML = `<tr><td colspan="4" class="p-4 text-center text-gray-500">Nenhuma avaliação encontrada.</td></tr>`;
                    document.getElementById('stat-total').innerText = '0';
                    document.getElementById('stat-month').innerText = '0';
                    return;
                }

                document.getElementById('stat-total').innerText = state.assessments.length;
                const currentMonth = new Date().getMonth();
                document.getElementById('stat-month').innerText = state.assessments.filter(a => new Date(a.data).getMonth() === currentMonth).length;

                list.innerHTML = state.assessments.map(a => {
                    const d = new Date(a.data);
                    return `
                        <tr class="hover:bg-gray-700/50 transition border-b border-gray-700">
                            <td class="p-3 text-gray-300 text-xs sm:text-sm">${d.getDate().toString().padStart(2,'0')}/${(d.getMonth()+1).toString().padStart(2,'0')}/${d.getFullYear()}</td>
                            <td class="p-3 font-medium text-xs sm:text-sm">${a.cliente.nome}</td>
                            <td class="p-3 hidden sm:table-cell"><span class="bg-gray-700 px-2 py-1 rounded text-xs">${a.anamnese.objetivo}</span></td>
                            <td class="p-3 text-center">
                                <button onclick="actions.viewResult('${a.id}')" class="text-primary hover:text-white bg-primary/10 hover:bg-primary/30 px-3 py-1 rounded transition text-sm">Ver</button>
                            </td>
                        </tr>
                    `;
                }).join('');
            }
        };

        const dbActions = {
            checkProfile: async (uid) => {
                try {
                    const docRef = doc(db, 'artifacts', state.appId, 'users', uid, 'profile');
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
                    console.error("Erro perfil:", error);
                    alert("Erro ao ler dados do Firestore. Verifique as regras do banco de dados no Firebase.");
                }
            },
            saveProfile: async (profileData) => {
                ui.showLoading(true);
                try {
                    const docRef = doc(db, 'artifacts', state.appId, 'users', state.user.uid, 'profile');
                    await setDoc(docRef, profileData);
                    state.profile = profileData;
                    ui.renderNavActions();
                    dbActions.listenAssessments(state.user.uid);
                    ui.showScreen('screen-dashboard');
                } catch (error) {
                    console.error("Erro salvar perfil:", error);
                    alert("Falha ao salvar no banco. Verifique as permissões (Rules) do Firestore.");
                } finally {
                    ui.showLoading(false);
                }
            },
            listenAssessments: (uid) => {
                const q = collection(db, 'artifacts', state.appId, 'users', uid, 'assessments');
                onSnapshot(q, (snapshot) => {
                    const assessments = [];
                    snapshot.forEach((doc) => assessments.push({ id: doc.id, ...doc.data() }));
                    state.assessments = assessments.sort((a,b) => new Date(b.data) - new Date(a.data));
                    ui.updateDashboard();
                }, (error) => {
                    console.error("Erro listener avaliações:", error);
                });
            },
            saveAssessment: async (data) => {
                ui.showLoading(true);
                try {
                    data.data = new Date().toISOString();
                    const colRef = collection(db, 'artifacts', state.appId, 'users', state.user.uid, 'assessments');
                    const docRef = await addDoc(colRef, data);
                    data.id = docRef.id;
                    ui.showLoading(false);
                    actions.viewResult(data.id, data);
                    DOM.formAssessment.reset();
                } catch (error) {
                    console.error("Erro ao salvar avaliação:", error);
                    ui.showLoading(false);
                }
            }
        };

        // Escuta o Status da Autenticação do Firebase
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

        // AÇÕES DA INTERFACE
        const actions = {
            login: async () => {
                ui.showLoading(true);
                try {
                    // Abre o Popup de Login do Google Oficialmente
                    await signInWithPopup(auth, provider);
                } catch (error) {
                    console.error("Erro Login:", error);
                    DOM.loginStatus.innerText = "Erro: " + error.message;
                    DOM.loginStatus.classList.add('text-red-500');
                    ui.showLoading(false);
                }
            },
            logout: () => {
                ui.showLoading(true);
                signOut(auth).then(() => ui.showLoading(false));
            },
            viewResult: (id, rawData = null) => {
                const data = rawData || state.assessments.find(a => a.id === id);
                if(!data) return;

                // Preencher tela (Mesma lógica anterior, resumida para a exibição)
                document.getElementById('res-prof-nome').innerText = state.profile.nome;
                document.getElementById('footer-rt').innerText = `RT: ${state.profile.nome} ${state.profile.cref ? '- CREF: ' + state.profile.cref : ''}`;
                document.getElementById('res-prof-cref').innerText = state.profile.cref ? `CREF: ${state.profile.cref}` : 'Treinador Esportivo';
                
                const d = new Date(data.data);
                document.getElementById('res-data').innerText = `${d.getDate().toString().padStart(2,'0')}/${(d.getMonth()+1).toString().padStart(2,'0')}/${d.getFullYear()}`;
                
                document.getElementById('res-cli-nome').innerText = data.cliente.nome;
                document.getElementById('res-cli-idade').innerText = data.cliente.idade;
                document.getElementById('res-cli-sexo').innerText = data.cliente.sexo === 'M' ? 'Masculino' : 'Feminino';
                document.getElementById('res-cli-objetivo').innerText = data.anamnese.objetivo;

                const peso = parseFloat(data.cliente.peso);
                document.getElementById('res-peso').innerText = peso;
                document.getElementById('res-altura').innerText = data.cliente.altura;

                // Call Calculators
                const imc = calculators.getIMC(peso, data.cliente.altura);
                const rcq = calculators.getRCQ(data.perimetros.cintura, data.perimetros.quadril, data.cliente.sexo);
                const bf = calculators.getBodyFat(data.dobras.protocolo, data.cliente.sexo, data.cliente.idade, data.dobras);

                // Render IMC
                document.getElementById('res-imc-val').innerText = imc.value;
                document.getElementById('res-imc-class').innerText = imc.classe;
                document.getElementById('res-imc-class').className = `badge badge-${imc.color}`;

                // Render RCQ
                document.getElementById('res-rcq-val').innerText = rcq.value;
                document.getElementById('res-cintura').innerText = data.perimetros.cintura;
                document.getElementById('res-quadril').innerText = data.perimetros.quadril;
                document.getElementById('res-rcq-class').innerText = `Risco: ${rcq.classe}`;
                document.getElementById('res-rcq-class').className = `badge badge-${rcq.color} mt-2 w-full block text-center`;

                // Render BF & Chart
                let valGorda = 0; let valMagra = peso;
                if (bf) {
                    document.getElementById('res-bf-val').innerText = bf.value;
                    document.getElementById('res-bf-class').innerText = bf.classe;
                    document.getElementById('res-bf-class').className = `badge badge-${bf.color}`;
                    document.getElementById('res-protocolo-nome').innerText = bf.protocoloName;
                    valGorda = peso * (parseFloat(bf.value) / 100);
                    valMagra = peso - valGorda;
                }

                document.getElementById('res-massamagra').innerText = valMagra.toFixed(1) + ' kg';
                document.getElementById('res-massagorda').innerText = valGorda.toFixed(1) + ' kg';
                document.getElementById('bar-magra').style.width = `${(valMagra/peso)*100}%`;
                document.getElementById('bar-gorda').style.width = `${(valGorda/peso)*100}%`;

                if(state.chartInstance) state.chartInstance.destroy();
                state.chartInstance = new Chart(document.getElementById('compChart').getContext('2d'), {
                    type: 'doughnut',
                    data: { labels: ['Massa Magra', 'Massa Gorda'], datasets: [{ data: [valMagra.toFixed(1), valGorda.toFixed(1)], backgroundColor: ['#3b82f6', '#f59e0b'], borderWidth: 0 }] },
                    options: { responsive: true, maintainAspectRatio: false, cutout: '70%', plugins: { legend: { display: false } } }
                });

                document.getElementById('res-recomendacoes').innerHTML = calculators.generateInsights(imc, rcq, bf, data.anamnese.objetivo).map(i => `<li>${i}</li>`).join('');
                ui.showScreen('screen-results');
            },
            generatePDF: () => {
                const element = document.getElementById('pdf-content');
                document.body.classList.remove('dark');
                const opt = { margin: 0.5, filename: `Avaliacao_${document.getElementById('res-cli-nome').innerText.replace(/\s+/g, '_')}.pdf`, image: { type: 'jpeg', quality: 0.98 }, html2canvas: { scale: 2 }, jsPDF: { unit: 'in', format: 'a4', orientation: 'portrait' } };
                ui.showLoading(true);
                html2pdf().set(opt).from(element).save().then(() => {
                    document.body.classList.add('dark');
                    ui.showLoading(false);
                });
            }
        };

        const formHandlers = {
            renderDobrasInputs: () => {
                const p = DOM.protocoloDobras.value;
                const s = document.getElementById('av-sexo').value;
                DOM.containerDobras.innerHTML = '';
                if (p === 'nenhum') { DOM.containerDobras.innerHTML = '<p class="col-span-4 text-gray-400">Cálculo desativado.</p>'; return; }
                let fields = p === 'pollock7' ? [{id: 'dc-peito', label: 'Peitoral'}, {id: 'dc-axilar', label: 'Axilar Média'}, {id: 'dc-triceps', label: 'Tríceps'}, {id: 'dc-sub', label: 'Subescapular'}, {id: 'dc-abdomen', label: 'Abdominal'}, {id: 'dc-supra', label: 'Suprailíaca'}, {id: 'dc-coxa', label: 'Coxa'}] : (s === 'M' ? [{id: 'dc-peito', label: 'Peitoral'}, {id: 'dc-abdomen', label: 'Abdominal'}, {id: 'dc-coxa', label: 'Coxa'}] : [{id: 'dc-triceps', label: 'Tríceps'}, {id: 'dc-supra', label: 'Suprailíaca'}, {id: 'dc-coxa', label: 'Coxa'}]);
                fields.forEach(f => DOM.containerDobras.innerHTML += `<div><label class="block text-xs mb-1 text-gray-300">${f.label}</label><input type="number" step="0.1" id="${f.id}" required class="w-full bg-darkInput border border-green-700 rounded p-2 text-white outline-none"></div>`);
            }
        };

        const calculators = {
            getIMC: (p, a) => { const i = p/((a/100)*(a/100)); return { value: i.toFixed(1), classe: i<18.5?'Baixo Peso':i<24.9?'Normal':i<29.9?'Sobrepeso':'Obesidade', color: i<18.5?'yellow':i<24.9?'green':i<29.9?'yellow':'red' }; },
            getRCQ: (c, q, s) => { const r = c/q; return { value: r.toFixed(2), classe: s==='M'?(r<0.90?'Baixo':r<0.95?'Moderado':'Alto'):(r<0.80?'Baixo':r<0.85?'Moderado':'Alto'), color: s==='M'?(r<0.90?'green':r<0.95?'yellow':'red'):(r<0.80?'green':r<0.85?'yellow':'red') }; },
            getBodyFat: (p, s, i, d) => {
                if(p==='nenhum') return null;
                let sm=0, dt=0;
                if(p==='pollock7') { sm = ['peito','axilar','triceps','sub','abdomen','supra','coxa'].reduce((a,v)=>a+parseFloat(d[`dc-${v}`]||0),0); dt = s==='M'?1.112-(0.00043499*sm)+(0.00000055*(sm**2))-(0.00028826*i):1.097-(0.00046971*sm)+(0.00000056*(sm**2))-(0.00012828*i); }
                else { if(s==='M') { sm=parseFloat(d['dc-peito']||0)+parseFloat(d['dc-abdomen']||0)+parseFloat(d['dc-coxa']||0); dt=1.109380-(0.0008267*sm)+(0.0000016*(sm**2))-(0.0002574*i); } else { sm=parseFloat(d['dc-triceps']||0)+parseFloat(d['dc-supra']||0)+parseFloat(d['dc-coxa']||0); dt=1.0994921-(0.0009929*sm)+(0.0000023*(sm**2))-(0.0001392*i); } }
                const f = ((4.95/dt)-4.5)*100;
                let c = 'Normal', cl = 'green';
                if(s==='M') { if(f<8){c='Atleta';cl='blue'}else if(f>20){c='Acima';cl='yellow'} if(f>25){c='Obesidade';cl='red'} } else { if(f<15){c='Atleta';cl='blue'}else if(f>30){c='Acima';cl='yellow'} if(f>35){c='Obesidade';cl='red'} }
                return { value: f.toFixed(1), classe: c, color: cl, protocoloName: p==='pollock7'?'Pollock 7':'Pollock 3' };
            },
            generateInsights: (i, r, b, o) => {
                const l = [];
                if(o==='Emagrecimento') l.push("Foco primário: Déficit calórico com treino de força para manutenção de massa magra.");
                else if(o==='Hipertrofia') l.push("Foco primário: Superávit calórico e progressão de carga no treinamento resistido.");
                if(r.color==='red') l.push("<span class='text-red-400 font-bold'>Atenção:</span> Risco Cardiovascular elevado. Priorizar aeróbicos e controle da gordura visceral.");
                if(b&&b.color==='red') l.push("O percentual de gordura indica obesidade. Protocolos de emagrecimento são recomendados.");
                if(l.length===0) l.push("Parâmetros dentro da normalidade. Manter rotina de exercícios regulares.");
                return l;
            }
        };

        window.actions = actions;
        window.ui = ui;

        DOM.btnLogin.addEventListener('click', actions.login);
        document.getElementById('prof-tipo').addEventListener('change', (e) => {
            if (e.target.value === 'educador') { document.getElementById('cref-container').classList.remove('hidden'); document.getElementById('prof-cref').setAttribute('required', 'true'); }
            else { document.getElementById('cref-container').classList.add('hidden'); document.getElementById('prof-cref').removeAttribute('required'); }
        });
        DOM.formProfile.addEventListener('submit', (e) => {
            e.preventDefault();
            dbActions.saveProfile({
                nome: document.getElementById('prof-nome').value, tipo: document.getElementById('prof-tipo').value, uf: document.getElementById('prof-uf').value,
                cref: document.getElementById('prof-tipo').value === 'educador' ? document.getElementById('prof-cref').value : null
            });
        });
        DOM.protocoloDobras.addEventListener('change', formHandlers.renderDobrasInputs);
        document.getElementById('av-sexo').addEventListener('change', formHandlers.renderDobrasInputs);

        DOM.formAssessment.addEventListener('submit', (e) => {
            e.preventDefault();
            const data = {
                cliente: { nome: document.getElementById('av-nome').value, idade: document.getElementById('av-idade').value, sexo: document.getElementById('av-sexo').value, peso: document.getElementById('av-peso').value, altura: document.getElementById('av-altura').value },
                anamnese: { objetivo: document.getElementById('av-objetivo').value, atividade: document.getElementById('av-atividade').value, lesoes: document.getElementById('av-lesoes').value, meds: document.getElementById('av-meds').value, sono: document.getElementById('av-sono').value },
                perimetros: { cintura: document.getElementById('per-cintura').value, quadril: document.getElementById('per-quadril').value, pescoco: document.getElementById('per-pescoco').value||null, ombros: document.getElementById('per-ombros').value||null, torax: document.getElementById('per-torax').value||null, abd: document.getElementById('per-abd').value||null, bracoRelD: document.getElementById('per-braco-rel-d').value||null, bracoConD: document.getElementById('per-braco-con-d').value||null, coxaD: document.getElementById('per-coxa-d').value||null, pantD: document.getElementById('per-pant-d').value||null },
                dobras: { protocolo: document.getElementById('protocolo-dobras').value }
            };
            if(data.dobras.protocolo !== 'nenhum') {
                DOM.containerDobras.querySelectorAll('input').forEach(i => data.dobras[i.id] = i.value);
            }
            dbActions.saveAssessment(data);
        });

        document.getElementById('search-client').addEventListener('keyup', (e) => {
            const t = e.target.value.toLowerCase();
            document.getElementById('assessments-list').querySelectorAll('tr').forEach(r => r.style.display = r.innerText.toLowerCase().includes(t) ? '' : 'none');
        });
    </script>
</body>
</html>
