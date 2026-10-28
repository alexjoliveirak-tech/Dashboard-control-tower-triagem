# 📦 Control Tower de Triagem: Dashboard Operacional de Performance Logística
 
Dashboard interativo em **HTML, Tailwind CSS e Chart.js** para acompanhamento da performance de um centro de triagem, com foco em **OOT (Out Of Time)**: pacotes que perderam o prazo de despacho, suas causas e a evolução mensal dos SLAs.
 
> ⚠️ **Dados fictícios.** Todos os valores, nomes de unidade e volumes foram alterados e são apenas ilustrativos. Não correspondem a nenhuma operação real. Projeto independente, feito para estudo e portfólio.
 
## 📊 Visão geral
 
<img width="1166" height="595" alt="Control Tower 2" src="https://github.com/user-attachments/assets/04b2a7bb-09cb-484a-8250-bbeba221d1a3" />

<img width="1193" height="682" alt="Control Tower" src="https://github.com/user-attachments/assets/94019a03-861d-4a66-a8ba-6160f2879192" />

 
🔗 **Demo online:** [COLE-AQUI-O-LINK-DO-GITHUB-PAGES]
 
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
 
