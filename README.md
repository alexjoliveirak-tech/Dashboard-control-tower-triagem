# 📦 Control Tower de Triagem: Dashboard Operacional de Performance Logística
 
Dashboard interativo em **HTML, Tailwind CSS e Chart.js** para acompanhamento da performance de um centro de triagem, com foco em **OOT (Out Of Time)**: pacotes que perderam o prazo de despacho, suas causas e a evolução mensal dos SLAs.
 
> ⚠️ **Dados fictícios.** Todos os valores, nomes de unidade e volumes foram alterados e são apenas ilustrativos. Não correspondem a nenhuma operação real. Projeto independente, feito para estudo e portfólio.
 
## 📊 Visão geral
 
<img width="1166" height="595" alt="Control Tower 2" src="https://github.com/user-attachments/assets/04b2a7bb-09cb-484a-8250-bbeba221d1a3" />

<img width="1193" height="682" alt="Control Tower" src="https://github.com/user-attachments/assets/94019a03-861d-4a66-a8ba-6160f2879192" />

 
🔗 **Demo online: [Control Tower de Triagem - Dashboard Operacional.html](https://github.com/user-attachments/files/34104095/Control.Tower.de.Triagem.-.Dashboard.Operacional.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Control Tower de Triagem - Dashboard Operacional (dados fictícios)</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com/3.4.17"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            accent: '#2DD4BF',
                            blue: '#2D3277',
                            navy: '#1A1D4E',
                            green: '#16A34A',
                            lightgreen: '#16A34A',
                            red: '#F04438',
                            orange: '#F79009',
                            graybg: '#F5F5F7',
                            card: '#FFFFFF',
                            border: '#E4E4E7',
                            darktext: '#2D3277',
                            subtext: '#6B7280'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #F5F5F7;
            font-family: 'Inter', sans-serif;
            color: #1F2937;
        }
        .brand-shadow {
            box-shadow: 0 4px 20px -2px rgba(45, 50, 119, 0.08), 0 2px 6px -1px rgba(0, 0, 0, 0.04);
        }
        .brand-gradient-header {
            background: linear-gradient(135deg, #5EEAD4 0%, #2DD4BF 100%);
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #F1F1F1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb {
            background: #CBD5E1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94A3B8;
        }
        .tab-btn.active {
            border-bottom: 3px solid #2D3277;
            color: #2D3277;
            font-weight: 700;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- HEADER -->
    <header class="brand-gradient-header border-b border-teal-500/30 sticky top-0 z-50">
        <div class="max-w-[1700px] mx-auto px-4 py-3 flex flex-wrap items-center justify-between gap-4">
            <!-- Brand & Title -->
            <div class="flex items-center gap-3">
                <div class="bg-brand-blue text-white p-2.5 rounded-xl shadow-md flex items-center justify-center">
                    <i class="fa-solid font-bold text-xl fa-plane-departure"></i>
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <h1 class="text-xl md:text-2xl font-extrabold text-brand-navy tracking-tight">Control Tower de Triagem</h1>
                        <span class="bg-brand-blue text-white text-xs font-bold px-2.5 py-0.5 rounded-full shadow-sm">CT-A</span>
                    </div>
                    <p class="text-xs font-semibold text-brand-navy/80">DOT GERAL & Performance Operacional • Operação Logística</p>
                </div>
            </div>

            <!-- Date Range & Info -->
            <div class="flex items-center gap-3">
                <div class="bg-white/80 backdrop-blur-md px-3.5 py-1.5 rounded-lg border border-teal-600/20 text-xs font-medium text-brand-navy flex items-center gap-2 shadow-sm">
                    <i class="fa-regular fa-calendar-days text-brand-blue"></i>
                    <span>Período: <strong>1 de set. de 2026 - 30 de set. de 2026</strong></span>
                </div>
                <div class="bg-brand-navy text-white text-xs px-3 py-1.5 rounded-lg font-medium flex items-center gap-2 shadow-sm">
                    <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                    <span>Status: Operativo</span>
                </div>
            </div>
        </div>

        <!-- TABS DE NAVEGAÇÃO -->
        <div class="max-w-[1700px] mx-auto px-4 mt-2 flex gap-6 text-sm font-medium text-gray-700">
            <button onclick="switchTab('dot-geral')" id="tab-dot-geral" class="tab-btn active pb-2 flex items-center gap-2 transition-all">
                <i class="fa-solid fa-chart-pie"></i> DOT GERAL (CT-A)
            </button>
            <button onclick="switchTab('mensal-operacional')" id="tab-mensal-operacional" class="tab-btn pb-2 flex items-center gap-2 transition-all">
                <i class="fa-solid fa-chart-column"></i> Acompanhamento Mensal & Motivos Operacionais
            </button>
        </div>
    </header>

    <!-- BARRA DE FILTROS INTERATIVOS -->
    <section class="bg-white border-b border-brand-border shadow-sm py-2.5 px-4">
        <div class="max-w-[1700px] mx-auto">
            <div class="flex items-center justify-between mb-2">
                <span class="text-xs font-bold text-brand-blue uppercase tracking-wider flex items-center gap-1.5">
                    <i class="fa-solid fa-filter text-brand-blue"></i> Filtros de Operação
                </span>
                <button onclick="resetFilters()" class="text-xs text-brand-subtext hover:text-brand-blue font-medium flex items-center gap-1 transition-colors">
                    <i class="fa-solid fa-rotate-left"></i> Limpar Filtros
                </button>
            </div>
            
            <!-- Filter Selects Grid -->
            <div class="grid grid-cols-2 sm:grid-cols-4 md:grid-cols-6 lg:grid-cols-9 gap-2 text-xs">
                <div>
                    <label class="block text-[10px] text-gray-500 font-semibold mb-0.5">CPT_TIME</label>
                    <select class="filter-select w-full bg-gray-50 border border-gray-300 rounded-md p-1.5 focus:ring-1 focus:ring-brand-blue focus:border-brand-blue">
                        <option value="">Todos</option>
                        <option>14:00</option>
                        <option>18:00</option>
                        <option>22:00</option>
                    </select>
                </div>
                <div>
                    <label class="block text-[10px] text-gray-500 font-semibold mb-0.5">Tipo</label>
                    <select class="filter-select w-full bg-gray-50 border border-gray-300 rounded-md p-1.5 focus:ring-1 focus:ring-brand-blue focus:border-brand-blue">
                        <option value="">Todos</option>
                        <option>Outbound</option>
                        <option>Inbound</option>
                    </select>
                </div>
                <div>
                    <label class="block text-[10px] text-gray-500 font-semibold mb-0.5">Origem</label>
                    <select class="filter-select w-full bg-gray-50 border border-gray-300 rounded-md p-1.5 focus:ring-1 focus:ring-brand-blue focus:border-brand-blue">
                        <option value="">Todas</option>
                        <option>Sortation Center</option>
                        <option>Fulfillment</option>
                    </select>
                </div>
                <div>
                    <label class="block text-[10px] text-gray-500 font-semibold mb-0.5">Área Resp. (1)</label>
                    <select class="filter-select w-full bg-gray-50 border border-gray-300 rounded-md p-1.5 focus:ring-1 focus:ring-brand-blue focus:border-brand-blue">
                        <option value="">Todas</option>
                        <option>Esteira</option>
                        <option>Armazenagem</option>
                        <option>Recebimento</option>
                        <option>Expedição</option>
                    </select>
                </div>
                <div>
                    <label class="block text-[10px] text-gray-500 font-semibold mb-0.5">Sub Área</label>
                    <select class="filter-select w-full bg-gray-50 border border-gray-300 rounded-md p-1.5 focus:ring-1 focus:ring-brand-blue focus:border-brand-blue">
                        <option value="">Todas</option>
                        <option>Triagem</option>
                        <option>Doca</option>
                    </select>
                </div>
                <div>
                    <label class="block text-[10px] text-gray-500 font-semibold mb-0.5">Canal Origem</label>
                    <select class="filter-select w-full bg-gray-50 border border-gray-300 rounded-md p-1.5 focus:ring-1 focus:ring-brand-blue focus:border-brand-blue">
                        <option value="">Todos</option>
                        <option>Direct</option>
                        <option>Transfer</option>
                    </select>
                </div>
                <div>
                    <label class="block text-[10px] text-gray-500 font-semibold mb-0.5">Canalização</label>
                    <select class="filter-select w-full bg-gray-50 border border-gray-300 rounded-md p-1.5 focus:ring-1 focus:ring-brand-blue focus:border-brand-blue">
                        <option value="">Todas</option>
                        <option>Crossdock</option>
                    </select>
                </div>
                <div>
                    <label class="block text-[10px] text-gray-500 font-semibold mb-0.5">Tipo Transp.</label>
                    <select class="filter-select w-full bg-gray-50 border border-gray-300 rounded-md p-1.5 focus:ring-1 focus:ring-brand-blue focus:border-brand-blue">
                        <option value="">Todos</option>
                        <option>Linehaul</option>
                        <option>Last Mile</option>
                    </select>
                </div>
                <div>
                    <label class="block text-[10px] text-gray-500 font-semibold mb-0.5">Flag Cheapest</label>
                    <select class="filter-select w-full bg-gray-50 border border-gray-300 rounded-md p-1.5 focus:ring-1 focus:ring-brand-blue focus:border-brand-blue">
                        <option value="">Sim / Não</option>
                        <option>Sim</option>
                        <option>Não</option>
                    </select>
                </div>
            </div>
        </div>
    </section>

    <!-- CONTEÚDO PRINCIPAL -->
    <main class="flex-1 max-w-[1700px] w-full mx-auto p-4 space-y-6">

        <!-- ================= SEÇÃO 1: DOT GERAL ================= -->
        <div id="section-dot-geral" class="space-y-6">
            
            <!-- GRID: ESQUERDA (GRÁFICOS PARETO) | DIREITA (KPIs RING) -->
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                
                <!-- COLUNA ESQUERDA: ESTRATIFICAÇÃO DAS PERDAS OOT (8 Colunas) -->
                <div class="lg:col-span-8 space-y-6">
                    
                    <!-- PARETO 1: PERDAS POR ÁREA -->
                    <div class="bg-white rounded-xl p-5 brand-shadow border border-brand-border">
                        <div class="flex items-center justify-between mb-4">
                            <div>
                                <h3 class="text-base font-bold text-brand-navy flex items-center gap-2">
                                    <i class="fa-solid fa-chart-bar text-brand-green"></i>
                                    Estratificação das Perdas OOT por Área
                                </h3>
                                <p class="text-xs text-brand-subtext">Volume de pacotes atrasados e percentual acumulado</p>
                            </div>
                            <span class="text-xs bg-amber-50 text-amber-700 font-bold px-2.5 py-1 rounded-md border border-amber-200">
                                Total Delay: 5.485 pacotes
                            </span>
                        </div>
                        <div class="h-64 relative">
                            <canvas id="chartParetoArea"></canvas>
                        </div>
                    </div>

                    <!-- PARETO 2: DETALHAMENTO DOS MOTIVOS -->
                    <div class="bg-white rounded-xl p-5 brand-shadow border border-brand-border">
                        <div class="flex items-center justify-between mb-4">
                            <div>
                                <h3 class="text-base font-bold text-brand-navy flex items-center gap-2">
                                    <i class="fa-solid fa-list-check text-brand-green"></i>
                                    Detalhamento dos Motivos de Perda
                                </h3>
                                <p class="text-xs text-brand-subtext">Principais causas de atrito no fluxo de processamento</p>
                            </div>
                        </div>
                        <div class="h-64 relative">
                            <canvas id="chartParetoMotivos"></canvas>
                        </div>
                    </div>

                    <!-- PARETO 3: ATRELAMENTO EM DOCA / TEMPOS -->
                    <div class="bg-white rounded-xl p-5 brand-shadow border border-brand-border">
                        <div class="flex items-center justify-between mb-4">
                            <div>
                                <h3 class="text-base font-bold text-brand-navy flex items-center gap-2">
                                    <i class="fa-solid fa-clock-rotate-left text-brand-green"></i>
                                    Tempo de Atrelamento em Doca
                                </h3>
                                <p class="text-xs text-brand-subtext">Distribuição por faixa de tempo de atrelamento</p>
                            </div>
                        </div>
                        <div class="h-64 relative">
                            <canvas id="chartParetoDoca"></canvas>
                        </div>
                    </div>

                </div>

                <!-- COLUNA DIREITA: KPIs RING CHARTS (4 Colunas) -->
                <div class="lg:col-span-4 space-y-4">
                    <div class="bg-white rounded-xl p-4 brand-shadow border border-brand-border sticky top-24">
                        <div class="flex items-center justify-between border-b pb-3 mb-4">
                            <h3 class="text-base font-bold text-brand-navy">
                                Out Of Time - Hard KPIs
                            </h3>
                            <span class="text-xs bg-emerald-100 text-emerald-800 font-bold px-2 py-0.5 rounded">
                                SLA Objetivo: 98.5%
                            </span>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-1 gap-4">
                            
                            <!-- RING 1: % OOT GERAL -->
                            <div class="flex items-center gap-4 bg-amber-50/50 p-3 rounded-xl border border-amber-100">
                                <div class="w-20 h-20 relative flex-shrink-0">
                                    <canvas id="ringOotGeral"></canvas>
                                    <div class="absolute inset-0 flex items-center justify-center font-extrabold text-xs text-amber-600">
                                        97,69%
                                    </div>
                                </div>
                                <div class="space-y-1">
                                    <div class="text-xs font-bold text-gray-500 uppercase">
                                        % OOT GERAL
                                    </div>
                                    <div class="text-sm font-semibold text-gray-800">
                                        895.156 <span class="text-xs text-gray-500 font-normal">Pacotes</span>
                                    </div>
                                    <div class="text-xs text-red-600 font-bold">
                                        5.485 <span class="font-normal text-gray-500">Pacotes Delay</span>
                                    </div>
                                </div>
                            </div>

                            <!-- RING 2: PARCIAL % OOT V3 -->
                            <div class="flex items-center gap-4 bg-emerald-50/50 p-3 rounded-xl border border-emerald-100">
                                <div class="w-20 h-20 relative flex-shrink-0">
                                    <canvas id="ringOotV3"></canvas>
                                    <div class="absolute inset-0 flex items-center justify-center font-extrabold text-xs text-emerald-600">
                                        99,65%
                                    </div>
                                </div>
                                <div class="space-y-1">
                                    <div class="text-xs font-bold text-gray-500 uppercase">
                                        Parcial % OOT V3
                                    </div>
                                    <div class="text-sm font-semibold text-gray-800">
                                        621.001 <span class="text-xs text-gray-500 font-normal">Pacotes V3</span>
                                    </div>
                                    <div class="text-xs text-emerald-600 font-bold">
                                        1.443 <span class="font-normal text-gray-500">Pacotes Delay</span>
                                    </div>
                                </div>
                            </div>

                            <!-- RING 3: % OOT H&B -->
                            <div class="flex items-center gap-4 bg-emerald-50/50 p-3 rounded-xl border border-emerald-100">
                                <div class="w-20 h-20 relative flex-shrink-0">
                                    <canvas id="ringOotHB"></canvas>
                                    <div class="absolute inset-0 flex items-center justify-center font-extrabold text-xs text-emerald-600">
                                        98,94%
                                    </div>
                                </div>
                                <div class="space-y-1">
                                    <div class="text-xs font-bold text-gray-500 uppercase">
                                        % OOT H&B
                                    </div>
                                    <div class="text-sm font-semibold text-gray-800">
                                        71.147 <span class="text-xs text-gray-500 font-normal">Pacotes</span>
                                    </div>
                                    <div class="text-xs text-emerald-600 font-bold">
                                        2.576 <span class="font-normal text-gray-500">Pacotes Delay</span>
                                    </div>
                                </div>
                            </div>

                            <!-- RING 4: PARCIAL % OOT V4 -->
                            <div class="flex items-center gap-4 bg-emerald-50/50 p-3 rounded-xl border border-emerald-100">
                                <div class="w-20 h-20 relative flex-shrink-0">
                                    <canvas id="ringOotV4"></canvas>
                                    <div class="absolute inset-0 flex items-center justify-center font-extrabold text-xs text-emerald-600">
                                        98,51%
                                    </div>
                                </div>
                                <div class="space-y-1">
                                    <div class="text-xs font-bold text-gray-500 uppercase">
                                        Parcial % OOT V4
                                    </div>
                                    <div class="text-sm font-semibold text-gray-800">
                                        274.155 <span class="text-xs text-gray-500 font-normal">Pacotes V4</span>
                                    </div>
                                    <div class="text-xs text-emerald-600 font-bold">
                                        3.788 <span class="font-normal text-gray-500">Pacotes Delay</span>
                                    </div>
                                </div>
                            </div>

                            <!-- RING 5: % OOT FAST -->
                            <div class="flex items-center gap-4 bg-emerald-50/50 p-3 rounded-xl border border-emerald-100">
                                <div class="w-20 h-20 relative flex-shrink-0">
                                    <canvas id="ringOotFast"></canvas>
                                    <div class="absolute inset-0 flex items-center justify-center font-extrabold text-xs text-emerald-600">
                                        99,31%
                                    </div>
                                </div>
                                <div class="space-y-1">
                                    <div class="text-xs font-bold text-gray-500 uppercase">
                                        % OOT FAST
                                    </div>
                                    <div class="text-sm font-semibold text-gray-800">
                                        734.846 <span class="text-xs text-gray-500 font-normal">Pacotes</span>
                                    </div>
                                    <div class="text-xs text-emerald-600 font-bold">
                                        3.999 <span class="font-normal text-gray-500">Pacotes Delay</span>
                                    </div>
                                </div>
                            </div>

                            <!-- RING 6: PARCIAL % SLOW -->
                            <div class="flex items-center gap-4 bg-amber-50/50 p-3 rounded-xl border border-amber-100">
                                <div class="w-20 h-20 relative flex-shrink-0">
                                    <canvas id="ringOotSlow"></canvas>
                                    <div class="absolute inset-0 flex items-center justify-center font-extrabold text-xs text-amber-600">
                                        98,47%
                                    </div>
                                </div>
                                <div class="space-y-1">
                                    <div class="text-xs font-bold text-gray-500 uppercase">
                                        Parcial % SLOW
                                    </div>
                                    <div class="text-sm font-semibold text-gray-800">
                                        160.310 <span class="text-xs text-gray-500 font-normal">Pacotes V4</span>
                                    </div>
                                    <div class="text-xs text-amber-600 font-bold">
                                        3.922 <span class="font-normal text-gray-500">Pacotes Delay</span>
                                    </div>
                                </div>
                            </div>

                        </div>
                    </div>
                </div>

            </div>
        </div>

        <!-- ================= SEÇÃO 2: ACOMPANHAMENTO MENSAL E MOTIVOS OPERACIONAIS ================= -->
        <div id="section-mensal-operacional" class="space-y-6 hidden">
            
            <!-- BLENDED HEADER BANNER -->
            <div class="bg-brand-navy text-white p-4 rounded-xl shadow-md flex items-center justify-between">
                <div>
                    <h2 class="text-lg font-bold flex items-center gap-2">
                        <i class="fa-solid fa-calendar-check text-brand-accent"></i>
                        Acompanhamento Trimestral de SLAs Operacionais
                    </h2>
                    <p class="text-xs text-gray-300">Comparativo Histórico: Julho vs Agosto vs Setembro (2026)</p>
                </div>
                <div class="flex gap-2 text-xs">
                    <span class="bg-gray-800 px-3 py-1 rounded-md border border-gray-700">Julho</span>
                    <span class="bg-gray-600 px-3 py-1 rounded-md border border-gray-500">Agosto</span>
                    <span class="bg-brand-accent text-brand-navy font-bold px-3 py-1 rounded-md">Setembro</span>
                </div>
            </div>

            <!-- CARD 1: OOT GERAL COMPARATIVO MENSAL -->
            <div class="bg-white rounded-xl p-5 brand-shadow border border-brand-border">
                <div class="flex items-center justify-between mb-4">
                    <div>
                        <h3 class="text-base font-bold text-brand-navy flex items-center gap-2">
                            <i class="fa-solid fa-chart-column text-brand-blue"></i>
                            OOT GERAL (Histórico Mensal)
                        </h3>
                        <p class="text-xs text-brand-subtext">Taxa de pontualidade geral por categoria e mês</p>
                    </div>
                </div>
                <div class="h-72 relative">
                    <canvas id="chartOotGeralMensal"></canvas>
                </div>
            </div>

            <!-- CARD 2: OOT OPERACIONAL COMPARATIVO MENSAL -->
            <div class="bg-white rounded-xl p-5 brand-shadow border border-brand-border">
                <div class="flex items-center justify-between mb-4">
                    <div>
                        <h3 class="text-base font-bold text-brand-navy flex items-center gap-2">
                            <i class="fa-solid fa-gears text-brand-blue"></i>
                            OOT OPERACIONAL (Histórico Mensal)
                        </h3>
                        <p class="text-xs text-brand-subtext">Taxa de pontualidade expurgando fatores não-operacionais</p>
                    </div>
                </div>
                <div class="h-72 relative">
                    <canvas id="chartOotOperacionalMensal"></canvas>
                </div>
            </div>

            <!-- CARD 3: MOTIVOS OPERACIONAIS - PARETO DE VOLUME DE ERROS -->
            <div class="bg-white rounded-xl p-5 brand-shadow border border-brand-border">
                <div class="flex items-center justify-between mb-4">
                    <div>
                        <h3 class="text-base font-bold text-brand-navy flex items-center gap-2">
                            <i class="fa-solid fa-triangle-exclamation text-amber-500"></i>
                            MOTIVOS OPERACIONAIS (Pareto de Erros)
                        </h3>
                        <p class="text-xs text-brand-subtext">Volume absoluto de desvios operacionais e linha de impacto acumulado (%)</p>
                    </div>
                </div>
                <div class="h-80 relative">
                    <canvas id="chartMotivosOperacionais"></canvas>
                </div>
            </div>

        </div>

    </main>

    <!-- FOOTER -->
    <footer class="bg-white border-t border-brand-border mt-auto py-3 px-4">
        <div class="max-w-[1700px] mx-auto flex flex-col sm:flex-row items-center justify-between text-xs text-gray-500 gap-2">
            <div class="flex items-center gap-2">
                <span class="font-bold text-brand-navy">Operação Logística</span>
                <span>•</span>
                <span>Performance Operacional - CT-A</span>
            </div>
            <div class="flex items-center gap-4">
                <span>Última atualização: Setembro 2026</span>
                <span class="bg-emerald-100 text-emerald-800 font-semibold px-2 py-0.5 rounded text-[10px]">Dados fictícios</span>
            </div>
        </div>
    </footer>

    <!-- JAVASCRIPT & CHART CONFIGURATIONS -->
    <script>
        // Tab Switcher
        function switchTab(tabName) {
            const secDot = document.getElementById('section-dot-geral');
            const secMensal = document.getElementById('section-mensal-operacional');
            const tabDot = document.getElementById('tab-dot-geral');
            const tabMensal = document.getElementById('tab-mensal-operacional');

            if (tabName === 'dot-geral') {
                secDot.classList.remove('hidden');
                secMensal.classList.add('hidden');
                tabDot.classList.add('active');
                tabMensal.classList.remove('active');
            } else {
                secDot.classList.add('hidden');
                secMensal.classList.remove('hidden');
                tabDot.classList.remove('active');
                tabMensal.classList.add('active');
            }
        }

        // Reset Filters Dummy Function
        function resetFilters() {
            document.querySelectorAll('.filter-select').forEach(select => select.value = '');
        }

        // Global Chart Defaults
        Chart.defaults.font.family = "'Inter', sans-serif";
        Chart.defaults.color = '#4B5563';

        // Custom Pareto Plugin for Label Formatting
        const paretoLabelPlugin = {
            id: 'paretoLabelPlugin',
            afterDatasetsDraw(chart) {
                const { ctx } = chart;
                chart.data.datasets.forEach((dataset, datasetIndex) => {
                    const meta = chart.getDatasetMeta(datasetIndex);
                    if (!meta.hidden) {
                        meta.data.forEach((element, index) => {
                            const dataValue = dataset.data[index];
                            if (dataValue !== null && dataValue !== undefined) {
                                ctx.save();
                                ctx.font = 'bold 10px Inter';
                                if (dataset.type === 'line') {
                                    ctx.fillStyle = '#2D3277';
                                    ctx.fillText(`${dataValue.toFixed(2)}%`, element.x - 12, element.y - 10);
                                } else if (dataset.type === 'bar' || !dataset.type) {
                                    ctx.fillStyle = '#16A34A';
                                    ctx.fillText(dataValue.toLocaleString('pt-BR'), element.x - 12, element.y - 6);
                                }
                                ctx.restore();
                            }
                        });
                    }
                });
            }
        };

        // --- SECTION 1 CHARTS ---

        // PARETO 1: PERDAS POR ÁREA
        const ctxParetoArea = document.getElementById('chartParetoArea').getContext('2d');
        new Chart(ctxParetoArea, {
            type: 'bar',
            data: {
                labels: ['Esteira', 'Armazenagem', 'Recebimento', 'Expedição', 'Volumoso', 'Big & Frágil', 'Rework', 'Comercial C...'],
                datasets: [
                    {
                        label: 'Pacotes Delay',
                        data: [2351, 934, 593, 518, 407, 263, 258, 161],
                        backgroundColor: '#16A34A',
                        borderRadius: 6,
                        yAxisID: 'y'
                    },
                    {
                        label: '% Acumulado',
                        data: [42.86, 59.89, 70.70, 80.15, 87.57, 92.36, 97.06, 100.00],
                        type: 'line',
                        borderColor: '#2D3277',
                        borderWidth: 2,
                        pointBackgroundColor: '#2D3277',
                        pointRadius: 4,
                        yAxisID: 'y1'
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: { display: false },
                    tooltip: { mode: 'index', intersect: false }
                },
                scales: {
                    y: {
                        type: 'linear',
                        position: 'left',
                        title: { display: true, text: 'Shipments (Qtd)' },
                        grid: { color: '#F3F4F6' }
                    },
                    y1: {
                        type: 'linear',
                        position: 'right',
                        max: 100,
                        title: { display: true, text: '% Acumulado' },
                        grid: { drawOnChartArea: false },
                        ticks: { callback: v => v + '%' }
                    },
                    x: { grid: { display: false } }
                }
            },
            plugins: [paretoLabelPlugin]
        });

        // PARETO 2: DETALHAMENTO DOS MOTIVOS
        const ctxParetoMotivos = document.getElementById('chartParetoMotivos').getContext('2d');
        new Chart(ctxParetoMotivos, {
            type: 'bar',
            data: {
                labels: [
                    'EST - Atraso proc. Auto',
                    'ARM - HU V3 não atrelada',
                    'EST - Pacote Não Localizado',
                    'EST - Sem Atrelamento V4',
                    'RW - Rework - Auto',
                    'EST - Pote rec. não Proc.',
                    'OUT - Atraso lib. veículo'
                ],
                datasets: [
                    {
                        label: 'Pacotes Delay',
                        data: [916, 617, 490, 357, 242, 229, 168],
                        backgroundColor: '#16A34A',
                        borderRadius: 6,
                        yAxisID: 'y'
                    },
                    {
                        label: '% Acumulado',
                        data: [16.70, 27.95, 36.88, 43.39, 47.80, 51.98, 55.04],
                        type: 'line',
                        borderColor: '#2D3277',
                        borderWidth: 2,
                        pointBackgroundColor: '#2D3277',
                        pointRadius: 4,
                        yAxisID: 'y1'
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: { legend: { display: false } },
                scales: {
                    y: { position: 'left', title: { display: true, text: 'Shipments' }, grid: { color: '#F3F4F6' } },
                    y1: { position: 'right', max: 100, ticks: { callback: v => v + '%' }, grid: { drawOnChartArea: false } },
                    x: { grid: { display: false } }
                }
            },
            plugins: [paretoLabelPlugin]
        });

        // PARETO 3: ATRELAMENTO EM DOCA
        const ctxParetoDoca = document.getElementById('chartParetoDoca').getContext('2d');
        new Chart(ctxParetoDoca, {
            type: 'bar',
            data: {
                labels: [
                    'Até 20 min',
                    'Não Atrelado em Doca',
                    'Mais de 3h',
                    'Até 1h20',
                    'Até 1h',
                    'Até 2h40',
                    'Até 40 min'
                ],
                datasets: [
                    {
                        label: 'Pacotes',
                        data: [2577, 2325, 169, 126, 116, 71, 60],
                        backgroundColor: '#16A34A',
                        borderRadius: 6,
                        yAxisID: 'y'
                    },
                    {
                        label: '% Acumulado',
                        data: [46.98, 89.37, 92.45, 94.75, 96.86, 98.16, 99.25],
                        type: 'line',
                        borderColor: '#2D3277',
                        borderWidth: 2,
                        pointBackgroundColor: '#2D3277',
                        pointRadius: 4,
                        yAxisID: 'y1'
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: { legend: { display: false } },
                scales: {
                    y: { position: 'left', title: { display: true, text: 'Atrelam. em Doca' }, grid: { color: '#F3F4F6' } },
                    y1: { position: 'right', max: 100, ticks: { callback: v => v + '%' }, grid: { drawOnChartArea: false } },
                    x: { grid: { display: false } }
                }
            },
            plugins: [paretoLabelPlugin]
        });

        // --- HELPER PARA DESENHAR RING CHARTS (KPIs) ---
        function createRingChart(canvasId, value, color) {
            const ctx = document.getElementById(canvasId).getContext('2d');
            new Chart(ctx, {
                type: 'doughnut',
                data: {
                    datasets: [{
                        data: [value, 100 - value],
                        backgroundColor: [color, '#E5E7EB'],
                        borderWidth: 0
                    }]
                },
                options: {
                    cutout: '78%',
                    responsive: true,
                    maintainAspectRatio: true,
                    plugins: { tooltip: { enabled: false }, legend: { display: false } }
                }
            });
        }

        createRingChart('ringOotGeral', 97.69, '#F79009');
        createRingChart('ringOotV3', 99.65, '#16A34A');
        createRingChart('ringOotHB', 98.94, '#16A34A');
        createRingChart('ringOotV4', 98.51, '#16A34A');
        createRingChart('ringOotFast', 99.31, '#16A34A');
        createRingChart('ringOotSlow', 98.47, '#F79009');


        // --- SECTION 2 CHARTS (ACOMPANHAMENTO MENSAL) ---

        // CARD 1: OOT GERAL MENSAL
        const ctxOotGeralMensal = document.getElementById('chartOotGeralMensal').getContext('2d');
        new Chart(ctxOotGeralMensal, {
            type: 'bar',
            data: {
                labels: ['OOT GERAL', 'OOT V3', 'OOT V4'],
                datasets: [
                    {
                        label: 'JULHO',
                        data: [98.61, 99.04, 97.80],
                        backgroundColor: '#374151',
                        borderRadius: 4
                    },
                    {
                        label: 'AGOSTO',
                        data: [97.70, 98.29, 96.88],
                        backgroundColor: '#9CA3AF',
                        borderRadius: 4
                    },
                    {
                        label: 'SETEMBRO',
                        data: [97.69, 98.47, 96.74],
                        backgroundColor: '#2DD4BF',
                        borderRadius: 4
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: { position: 'bottom' },
                    tooltip: { callbacks: { label: c => `${c.dataset.label}: ${c.raw}%` } }
                },
                scales: {
                    y: { min: 90, max: 100, ticks: { callback: v => v + '%' }, grid: { color: '#F3F4F6' } },
                    x: { grid: { display: false } }
                }
            }
        });

        // CARD 2: OOT OPERACIONAL MENSAL
        const ctxOotOperacionalMensal = document.getElementById('chartOotOperacionalMensal').getContext('2d');
        new Chart(ctxOotOperacionalMensal, {
            type: 'bar',
            data: {
                labels: ['OOT GERAL', 'OOT V3', 'OOT V4'],
                datasets: [
                    {
                        label: 'JULHO',
                        data: [99.63, 99.85, 99.37],
                        backgroundColor: '#374151',
                        borderRadius: 4
                    },
                    {
                        label: 'AGOSTO',
                        data: [99.14, 99.41, 98.73],
                        backgroundColor: '#9CA3AF',
                        borderRadius: 4
                    },
                    {
                        label: 'SETEMBRO',
                        data: [99.05, 99.38, 98.61],
                        backgroundColor: '#2DD4BF',
                        borderRadius: 4
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: { position: 'bottom' },
                    tooltip: { callbacks: { label: c => `${c.dataset.label}: ${c.raw}%` } }
                },
                scales: {
                    y: { min: 95, max: 100, ticks: { callback: v => v + '%' }, grid: { color: '#F3F4F6' } },
                    x: { grid: { display: false } }
                }
            }
        });

        // CARD 3: MOTIVOS OPERACIONAIS
        const ctxMotivosOperacionais = document.getElementById('chartMotivosOperacionais').getContext('2d');
        new Chart(ctxMotivosOperacionais, {
            type: 'bar',
            data: {
                labels: [
                    'ARM - HU V3 não atrelada na Armazenagem - Auto',
                    'EST - Atraso no processamento - Auto',
                    'EST - Pacote Não Localizado - Auto',
                    'OUT - Atraso na liberação do veículo',
                    'EST - Sem Atrelamento V4 - Auto',
                    'OUT - T3 HU em Doca Sem Shipped - Auto',
                    'OUT - Falha no Carregamento'
                ],
                datasets: [
                    {
                        label: 'Volume de Erros',
                        data: [3756, 2709, 2081, 1561, 1434, 1246, 962],
                        backgroundColor: '#16A34A',
                        borderRadius: 6,
                        yAxisID: 'y'
                    },
                    {
                        label: '% Acumulado',
                        data: [14.54, 25.02, 33.07, 39.12, 44.67, 49.49, 53.21],
                        type: 'line',
                        borderColor: '#2D3277',
                        borderWidth: 2,
                        pointBackgroundColor: '#2D3277',
                        pointRadius: 4,
                        yAxisID: 'y1'
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: { legend: { display: false } },
                scales: {
                    y: { position: 'left', title: { display: true, text: 'Volume de Pacotes' }, grid: { color: '#F3F4F6' } },
                    y1: { position: 'right', max: 100, ticks: { callback: v => v + '%' }, grid: { drawOnChartArea: false } },
                    x: {
                        grid: { display: false },
                        ticks: {
                            callback: function(value, index) {
                                const label = this.getLabelForValue(value);
                                return label.length > 25 ? label.substring(0, 22) + '...' : label;
                            }
                        }
                    }
                }
            },
            plugins: [paretoLabelPlugin]
        });
    </script>
</body>
</html>
 
## 🎯 Objetivo
 
Dar à operação uma visão rápida de:
 
- Quanto do volume está saindo dentro do prazo (OOT) em relação à meta de SLA
- Em quais áreas e por quais motivos os pacotes estão atrasando
- Como a performance evoluiu ao longo dos meses
## 🧩 Estrutura do dashboard
 
### Aba 1: DOT GERAL
 
- **Hard KPIs de OOT** comparados com a meta de SLA (98,5%): OOT Geral, V3, H&B, V4, FAST e SLOW, cada um com volume de pacotes e pacotes em atraso
- **Estratificação das perdas por área:** Pareto com volume de pacotes atrasados e percentual acumulado
- **Detalhamento dos motivos de perda:** Pareto com as principais causas de atrito no fluxo
- **Tempo de atrelamento em doca:** distribuição por faixa de tempo
### Aba 2: Acompanhamento Mensal e Motivos Operacionais
 
- **OOT Geral:** comparativo mensal (julho, agosto e setembro) por categoria
- **OOT Operacional:** taxa de pontualidade descontando fatores não operacionais
- **Pareto de motivos operacionais:** volume absoluto de desvios e linha de impacto acumulado
### Barra de filtros
 
Filtros por horário de corte (CPT), tipo, origem, área responsável, sub área, canal de origem, canalização, tipo de transporte e flag cheapest.
 
## 🔎 Principais leituras
 
- O **OOT Geral ficou em 97,69%**, abaixo da meta de 98,5%, com 5.485 pacotes em atraso sobre 895.156
- A área **Esteira** concentra a maior parte das perdas: 2.351 pacotes, cerca de 43% do total de atrasos
- Esteira, Armazenagem e Recebimento juntas respondem por cerca de 71% dos pacotes atrasados, o que mostra onde agir primeiro
- A faixa "Até 20 min" e os pacotes "Não Atrelados em Doca" concentram quase 90% dos casos de atrelamento
- Na comparação mensal, o OOT Geral caiu de julho (98,61%) para agosto (97,70%) e ficou estável em setembro
## 🛠️ Tecnologias
 
- HTML5
- [Tailwind CSS](https://tailwindcss.com/)
- [Chart.js](https://www.chartjs.org/)
- [Font Awesome](https://fontawesome.com/)
- Google Fonts (Inter)
## ▶️ Como executar
 
O projeto é um único arquivo, sem instalação.
 
1. Baixe ou clone o repositório
2. Abra o `index.html` no navegador
As bibliotecas são carregadas por CDN, então é preciso ter conexão com a internet.
 
## 📚 Sobre os dados
 
Os dados são fictícios e estão definidos diretamente no código (arrays do Chart.js e textos do HTML). Foram construídos para reproduzir a **forma** de uma operação de triagem: concentração das perdas em poucas áreas (Pareto), queda de performance ao longo dos meses e indicadores abaixo e acima da meta.
 
## ⚠️ Limitações
 
- Os **filtros são apenas visuais**: ainda não alteram os gráficos nem os indicadores
- Os dados são **estáticos**, escritos no próprio arquivo
- Não há conexão com banco de dados ou arquivo externo
## 🚀 Próximos passos
 
- Tornar os filtros funcionais, recalculando KPIs e gráficos
- Separar os dados em um arquivo JSON ou CSV
- Alimentar o dashboard a partir de um banco SQL
- Documentar a fórmula de cada indicador (OOT, OOT Operacional, V3, V4)
## 📁 Arquivos
 
- `index.html`: dashboard completo (estrutura, estilos e gráficos)
- `imagens/`: prints do dashboard
 
