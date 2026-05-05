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
                        primary: '#3b82f6',
                        primaryDark: '#2563eb',
                        darkBg: '#111827',
                        darkPanel: '#1f2937',
                        darkInput: '#374151'
                    }
                }
            }
        }
    </script>

    <!-- FontAwesome (Ícones) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <!-- html2pdf -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <style>
        body { background-color: #111827; color: #f3f4f6; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; overflow-x: hidden; }
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #1f2937; }
        ::-webkit-scrollbar-thumb { background: #4b5563; border-radius: 4px; }
        input[type=number]::-webkit-inner-spin-button, input[type=number]::-webkit-outer-spin-button { -webkit-appearance: none; margin: 0; }
        input[type=number] { -moz-appearance: textfield; }
        .glass-panel { background: rgba(31, 41, 55, 0.7); backdrop-filter: blur(10px); border: 1px solid rgba(75, 85, 99, 0.4); }
        .badge { padding: 0.25rem 0.75rem; border-radius: 9999px; font-size: 0.75rem; font-weight: 600; }
        .badge-green { background-color: rgba(16, 185, 129, 0.2); color: #34d399; border: 1px solid #10b981; }
        .badge-yellow { background-color: rgba(245, 158, 11, 0.2); color: #fbbf24; border: 1px solid #f59e0b; }
        .badge-red { background-color: rgba(239, 68, 68, 0.2); color: #f87171; border: 1px solid #ef4444; }
        .badge-blue { background-color: rgba(59, 130, 246, 0.2); color: #60a5fa; border: 1px solid #3b82f6; }
        .screen { display: none; }
        .screen.active { display: block; }
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
                <div class="flex items-center space-x-4" id="nav-actions"></div>
            </div>
        </div>
    </nav>

    <!-- Main Content -->
    <main class="flex-grow w-full max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- TELA 1: LOGIN -->
        <section id="screen-login" class="screen active flex flex-col items-center justify-center min-h-[70vh]">
            <div class="glass-panel p-8 rounded-2xl w-full max-w-md text-center shadow-xl">
                <div class="w-20 h-20 bg-primary/20 rounded-full flex items-center justify-center mx-auto mb-6">
                    <i class="fa-solid fa-user-shield text-4xl text-primary"></i>
                </div>
                <h1 class="text-2xl font-bold mb-2">Acesso Restrito</h1>
                <p class="text-gray-400 mb-8 text-sm">Faça login para aceder ao seu ambiente profissional.</p>
                
                <button id="btn-login" class="w-full bg-primary hover:bg-primaryDark text-white font-bold py-3 px-4 rounded-lg flex items-center justify-center transition duration-300">
                    <i class="fa-brands fa-google mr-2 text-xl"></i>
                    Entrar com Conta Google
                </button>
                <p id="login-status" class="mt-4 text-sm text-gray-500 font-medium"></p>
            </div>
        </section>

        <!-- TELA 2: PERFIL DO PROFISSIONAL -->
        <section id="screen-profile" class="screen max-w-2xl mx-auto">
            <div class="mb-6">
                <h2 class="text-2xl font-bold border-l-4 border-primary pl-3">Configurar Perfil</h2>
                <p class="text-gray-400 text-sm mt-1">Defina os seus dados profissionais.</p>
            </div>
            <div class="glass-panel p-6 rounded-xl">
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
                            <input type="text" id="prof-uf" placeholder="Ex: SP, RJ, RN" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-3 text-white focus:outline-none focus:border-primary transition">
                        </div>
                    </div>
                    <div id="cref-container" class="hidden">
                        <label class="block text-sm font-medium mb-1 text-primary">Número do CREF *</label>
                        <input type="text" id="prof-cref" placeholder="Ex: 000000-G/UF" class="w-full bg-darkInput border border-primary rounded-lg p-3 text-white focus:outline-none focus:ring-1 focus:ring-primary transition">
                    </div>
                    <div class="mt-8 pt-4 border-t border-gray-700">
                        <button type="submit" class="w-full bg-primary hover:bg-primaryDark text-white font-bold py-3 px-4 rounded-lg transition duration-300">Salvar Perfil</button>
                    </div>
                </form>
            </div>
        </section>

        <!-- TELA 3: DASHBOARD -->
        <section id="screen-dashboard" class="screen">
            <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-4">
                <div>
                    <h2 class="text-3xl font-bold" id="dash-greeting">Olá, Profissional</h2>
                    <p class="text-gray-400">Gira as suas avaliações e clientes.</p>
                </div>
                <button onclick="ui.navigateTo('screen-form')" class="bg-primary hover:bg-primaryDark text-white font-bold py-2 px-6 rounded-lg shadow-lg flex items-center transition">
                    <i class="fa-solid fa-plus mr-2"></i> Nova Avaliação
                </button>
            </div>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
                <div class="glass-panel p-4 rounded-xl text-center border-l-4 border-l-primary">
                    <p class="text-sm text-gray-400">Total de Avaliações</p>
                    <p class="text-3xl font-bold mt-1" id="stat-total">0</p>
                </div>
            </div>
            <div class="glass-panel rounded-xl overflow-hidden">
                <div class="p-4 border-b border-gray-700 bg-gray-800 flex justify-between items-center">
                    <h3 class="font-bold"><i class="fa-solid fa-history mr-2"></i>Histórico Recente</h3>
                    <input type="text" id="search-client" placeholder="Procurar cliente..." class="bg-darkInput border border-gray-600 rounded p-1 px-3 text-sm focus:outline-none focus:border-primary w-48">
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
                            <tr><td colspan="4" class="p-4 text-center text-gray-500">A carregar histórico...</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- TELA 4: FORMULÁRIO -->
        <section id="screen-form" class="screen max-w-4xl mx-auto">
            <div class="flex items-center justify-between mb-6">
                <div>
                    <h2 class="text-2xl font-bold border-l-4 border-primary pl-3">Nova Avaliação</h2>
                </div>
                <button onclick="ui.navigateTo('screen-dashboard')" class="text-gray-400 hover:text-white transition">
                    <i class="fa-solid fa-arrow-left mr-1"></i> Voltar
                </button>
            </div>

            <form id="form-assessment" class="space-y-6">
                <!-- Dados Básicos -->
                <div class="glass-panel p-6 rounded-xl border-t-4 border-t-primary">
                    <h3 class="font-bold text-lg mb-4 text-primary">Dados do Avaliado</h3>
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                        <div class="md:col-span-4"><label class="block text-sm mb-1">Nome *</label><input type="text" id="av-nome" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white"></div>
                        <div><label class="block text-sm mb-1">Idade *</label><input type="number" id="av-idade" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white"></div>
                        <div><label class="block text-sm mb-1">Sexo *</label><select id="av-sexo" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white"><option value="M">Masculino</option><option value="F">Feminino</option></select></div>
                        <div><label class="block text-sm mb-1">Peso (kg) *</label><input type="number" step="0.1" id="av-peso" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white"></div>
                        <div><label class="block text-sm mb-1">Estatura (cm) *</label><input type="number" id="av-altura" required class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white"></div>
                    </div>
                </div>

                <!-- Anamnese -->
                <div class="glass-panel p-6 rounded-xl border-t-4 border-t-purple-500">
                    <h3 class="font-bold text-lg mb-4 text-purple-400">Anamnese</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div><label class="block text-sm mb-1">Objetivo *</label><select id="av-objetivo" class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white"><option>Emagrecimento</option><option>Hipertrofia</option><option>Saúde</option></select></div>
                        <div><label class="block text-sm mb-1">Nível de Atividade</label><select id="av-atividade" class="w-full bg-darkInput border border-gray-600 rounded-lg p-2 text-white"><option>Sedentário</option><option>Ativo</option></select></div>
                    </div>
                </div>

                <!-- Perímetros -->
                <div class="glass-panel p-6 rounded-xl border-t-4 border-t-yellow-500">
                    <h3 class="font-bold text-lg mb-4 text-yellow-400">Perímetros (cm)</h3>
                    <div class="grid grid-cols-2 gap-4 mb-4">
                        <div><label class="block text-sm font-bold text-yellow-200 mb-1">Cintura *</label><input type="number" step="0.1" id="per-cintura" required class="w-full bg-darkInput border border-yellow-700 rounded-lg p-2 text-white"></div>
                        <div><label class="block text-sm font-bold text-yellow-200 mb-1">Quadril *</label><input type="number" step="0.1" id="per-quadril" required class="w-full bg-darkInput border border-yellow-700 rounded-lg p-2 text-white"></div>
                    </div>
                </div>

                <!-- Dobras -->
                <div class="glass-panel p-6 rounded-xl border-t-4 border-t-green-500">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="font-bold text-lg text-green-400">Dobras Cutâneas (mm)</h3>
                        <select id="protocolo-dobras" class="bg-darkInput border border-gray-600 rounded-lg p-1 text-sm text-white"><option value="pollock3">Pollock 3</option><option value="nenhum">Nenhum</option></select>
                    </div>
                    <div id="container-dobras" class="grid grid-cols-2 md:grid-cols-3 gap-4"></div>
                </div>

                <div class="flex gap-4 pt-4">
                    <button type="submit" class="w-full bg-primary hover:bg-primaryDark text-white font-bold py-3 rounded-lg shadow-lg">Calcular e Guardar</button>
                </div>
            </form>
        </section>

        <!-- TELA 5: RESULTADOS -->
        <section id="screen-results" class="screen">
            <div class="flex justify-between mb-6 no-print">
                <button onclick="ui.navigateTo('screen-dashboard')" class="text-gray-400 hover:text-white"><i class="fa-solid fa-arrow-left"></i> Voltar</button>
                <button onclick="actions.generatePDF()" class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"><i class="fa-solid fa-file-pdf"></i> PDF</button>
            </div>
            <div id="pdf-content" class="bg-darkBg text-white p-6 rounded-xl">
                <h1 class="text-3xl font-bold text-center mb-6">Resultados da Avaliação</h1>
                <p id="res-cli-nome" class="text-xl font-bold mb-4"></p>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="glass-panel p-5 rounded-xl"><p>IMC</p><h2 id="res-imc-val" class="text-3xl font-bold"></h2></div>
                    <div class="glass-panel p-5 rounded-xl"><p>Gordura Corporal</p><h2 id="res-bf-val" class="text-3xl font-bold"></h2></div>
                </div>
            </div>
        </section>
    </main>

    <!-- SCRIPT FIREBASE OFICIAL (SEM CÓDIGO DE TESTE) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, GoogleAuthProvider, signInWithPopup, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, onSnapshot, addDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // As suas chaves oficiais do Firebase
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

        const state = { user: null, profile: null, assessments: [] };
        const DOM = { screens: document.querySelectorAll('.screen') };

        const ui = {
            showScreen: (id) => {
                DOM.screens.forEach(s => s.classList.remove('active'));
                document.getElementById(id).classList.add('active');
            },
            navigateTo: (id) => {
                ui.showScreen(id);
                if(id === 'screen-form') formHandlers.renderDobras();
            },
            updateDashboard: () => {
                document.getElementById('dash-greeting').innerText = `Olá, ${state.profile?.nome || 'Profissional'}`;
                document.getElementById('stat-total').innerText = state.assessments.length;
                const list = document.getElementById('assessments-list');
                if(state.assessments.length === 0) list.innerHTML = '<tr><td colspan="4" class="p-4 text-center">Sem avaliações.</td></tr>';
                else list.innerHTML = state.assessments.map(a => `<tr class="border-b border-gray-700"><td class="p-3">${new Date(a.data).toLocaleDateString()}</td><td class="p-3">${a.cliente.nome}</td><td class="p-3">${a.anamnese.objetivo}</td><td class="p-3"><button onclick="actions.viewResult('${a.id}')" class="text-primary">Ver</button></td></tr>`).join('');
            }
        };

        const dbActions = {
            checkProfile: async (uid) => {
                try {
                    const docSnap = await getDoc(doc(db, 'artifacts', 'powfit', 'users', uid, 'profile'));
                    if (docSnap.exists()) {
                        state.profile = docSnap.data();
                        dbActions.listenAssessments(uid);
                        ui.showScreen('screen-dashboard');
                    } else {
                        ui.showScreen('screen-profile');
                    }
                } catch (e) {
                    alert("Erro ao ler dados: " + e.message);
                }
            },
            saveProfile: async (data) => {
                await setDoc(doc(db, 'artifacts', 'powfit', 'users', state.user.uid, 'profile'), data);
                state.profile = data;
                dbActions.listenAssessments(state.user.uid);
                ui.showScreen('screen-dashboard');
            },
            listenAssessments: (uid) => {
                onSnapshot(collection(db, 'artifacts', 'powfit', 'users', uid, 'assessments'), (snap) => {
                    state.assessments = [];
                    snap.forEach(d => state.assessments.push({ id: d.id, ...d.data() }));
                    ui.updateDashboard();
                });
            },
            saveAssessment: async (data) => {
                data.data = new Date().toISOString();
                const docRef = await addDoc(collection(db, 'artifacts', 'powfit', 'users', state.user.uid, 'assessments'), data);
                data.id = docRef.id;
                actions.viewResult(data.id, data);
            }
        };

        onAuthStateChanged(auth, (user) => {
            if (user) {
                state.user = user;
                dbActions.checkProfile(user.uid);
            } else {
                ui.showScreen('screen-login');
            }
        });

        const actions = {
            login: async () => {
                const btnStatus = document.getElementById('login-status');
                try {
                    btnStatus.innerText = "A conectar com o Google...";
                    btnStatus.classList.remove('text-red-500');
                    await signInWithPopup(auth, provider);
                } catch (error) {
                    console.error(error);
                    btnStatus.innerText = "Erro do Firebase: " + error.message;
                    btnStatus.classList.add('text-red-500');
                }
            },
            viewResult: (id, rawData=null) => {
                const data = rawData || state.assessments.find(a => a.id === id);
                if(!data) return;
                document.getElementById('res-cli-nome').innerText = data.cliente.nome;
                const imc = data.cliente.peso / Math.pow(data.cliente.altura/100, 2);
                document.getElementById('res-imc-val').innerText = imc.toFixed(1);
                
                let bf = "--";
                if(data.dobras.protocolo === 'pollock3') {
                     const soma = parseFloat(data.dobras['dc-1']||0) + parseFloat(data.dobras['dc-2']||0) + parseFloat(data.dobras['dc-3']||0);
                     bf = (soma * 0.15).toFixed(1) + "% (Aprox)"; // Calculo simplificado para demonstração
                }
                document.getElementById('res-bf-val').innerText = bf;
                ui.showScreen('screen-results');
            },
            generatePDF: () => {
                document.body.classList.remove('dark');
                html2pdf().from(document.getElementById('pdf-content')).save('Avaliacao.pdf').then(() => document.body.classList.add('dark'));
            }
        };

        const formHandlers = {
            renderDobras: () => {
                const p = document.getElementById('protocolo-dobras').value;
                const c = document.getElementById('container-dobras');
                c.innerHTML = '';
                if(p === 'pollock3') {
                    c.innerHTML = `
                        <div><label class="block text-xs mb-1">Dobra 1 (mm)</label><input type="number" id="dc-1" class="w-full bg-darkInput rounded p-2" required></div>
                        <div><label class="block text-xs mb-1">Dobra 2 (mm)</label><input type="number" id="dc-2" class="w-full bg-darkInput rounded p-2" required></div>
                        <div><label class="block text-xs mb-1">Dobra 3 (mm)</label><input type="number" id="dc-3" class="w-full bg-darkInput rounded p-2" required></div>
                    `;
                }
            }
        };

        window.actions = actions;
        window.ui = ui;

        document.getElementById('btn-login').addEventListener('click', actions.login);
        document.getElementById('prof-tipo').addEventListener('change', (e) => {
            document.getElementById('cref-container').style.display = e.target.value === 'educador' ? 'block' : 'none';
        });
        document.getElementById('form-profile').addEventListener('submit', (e) => {
            e.preventDefault();
            dbActions.saveProfile({ nome: document.getElementById('prof-nome').value, tipo: document.getElementById('prof-tipo').value });
        });
        document.getElementById('protocolo-dobras').addEventListener('change', formHandlers.renderDobras);
        document.getElementById('form-assessment').addEventListener('submit', (e) => {
            e.preventDefault();
            const data = {
                cliente: { nome: document.getElementById('av-nome').value, peso: document.getElementById('av-peso').value, altura: document.getElementById('av-altura').value },
                anamnese: { objetivo: document.getElementById('av-objetivo').value },
                dobras: { protocolo: document.getElementById('protocolo-dobras').value }
            };
            if(data.dobras.protocolo === 'pollock3') {
                data.dobras['dc-1'] = document.getElementById('dc-1').value;
                data.dobras['dc-2'] = document.getElementById('dc-2').value;
                data.dobras['dc-3'] = document.getElementById('dc-3').value;
            }
            dbActions.saveAssessment(data);
        });
    </script>
</body>
</html>
