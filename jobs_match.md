EDESAFIO APP DE CURRÍCULO ATS FRIENDLY - JOBSMATCH

Ferramenta do Desafio: Open Code

Nas Aulas, o Expert Bruno constrói o JobMatch ATS do zero, do primeiro prompt até o site publicado. Agora é a sua vez, com o seu tema e as suas escolhas.

O Que Criar
Uma aplicação web que compara um currículo com uma vaga e devolve uma versão ATS friendly desse currículo. ATS é o Application Tracking System, o sistema que ranqueia candidatos antes de qualquer pessoa do RH ler alguma coisa.

O núcleo é curto e precisa funcionar:

A pessoa cola a descrição da vaga;
A pessoa cola o próprio currículo;
A aplicação mostra o match, as palavras-chave encontradas e as que faltam;
A aplicação gera a versão ajustada do currículo, pronta para exportar.
Uma regra vale para a aplicação inteira: ela melhora como a pessoa se apresenta e nunca inventa experiência que a pessoa não tem. Bruno deixa isso escrito na própria interface, e vale trazer a ideia para a sua.


Ideias para Evoluir
Exportar o currículo também em .docx, para a pessoa editar antes de enviar;
Guardar o histórico das análises em um banco de dados;
Criar login, com e-mail de confirmação pelo conector Resend;
Montar um dashboard que mostre a evolução do match entre as versões;
Especializar a saída em um nicho, como vagas de tecnologia ou primeiro emprego;
Preparar a aplicação para ser encontrada, com SEO e com GEO, a otimização para buscas dentro das IAs.
Se você ainda está começando, tudo bem. Uma aplicação pequena, no ar e bem explicada vale mais que uma ambiciosa que ninguém consegue abrir.


--



1. O problema que a aplicação resolve
Pessoas candidatas a emprego perdem oportunidades por dois motivos invisíveis:

Match invisível. O candidato não sabe se o próprio currículo "conversa" com a descrição da vaga. Sistemas ATS (Applicant Tracking Systems) filtram por palavras-chave e estrutura — e o candidato só descobre que faltou um termo quando é reprovado.
Currículo mal apresentado. Mesmo quem tem a experiência certa perde chance porque o documento está em formato que o ATS não lê (tabelas, colunas, fontes exóticas, seções com nomes não canônicos) ou porque os resultados mais relevantes estão enterrados no fim do documento.
A solução: um aplicativo web onde a pessoa cola a descrição da vaga e cola o próprio currículo. O app calcula o percentual de match, mostra as palavras-chave encontradas e as que faltam, e gera uma versão ajustada do currículo — na estrutura exata que os sistemas ATS leem — pronta para exportar em PDF e .docx. O app é especializado no nicho de Marinha Mercante (vagas de embarque: marinheiro auxiliar de convés/máquinas, moço, eletricista marítimo, cozinheiro/taifeiro a bordo, condutor de máquinas, contramestre e primeiro emprego no mar).

Regra de ouro (escrita na própria interface):

"Melhoramos como você se apresenta — e nunca inventamos experiência, cargo ou habilidade que você não tenha. Tudo que entra no currículo vem do texto que você colou."

Essa regra vale para a aplicação inteira: a IA melhora a apresentação, nunca cria conteúdo novo.

2. O mega prompt original e o que mudou até a versão final
Prompt original (primeira geração)
Crie um aplicativo web de matchmaking de vagas de emprego e geração automatizada
de currículos ATS-friendly.

Stack: React (Vite) + TypeScript, Tailwind CSS v4, Shadcn UI (componentes próprios
no padrão shadcn: Button, Card, Badge, Progress, Tabs, Dialog, Separator, Input,
Textarea, Label, Select) sobre Radix UI, Lucide Icons e React Router.

Paleta obrigatória:
- Fundo e superfícies: branco puro / tons pastéis (máximo contraste);
- Cor primária (botões, links, ações): azul claro (bg-blue-400/500, text-blue-500);
- Cor secundária (badges, avisos, % de match): amarelo claro (bg-yellow-100..300,
  text-yellow-800);
- Design minimalista, clean, com bastante white space, acessível e fácil de ler.

Telas:
1. Dashboard de vagas (feed em grid de cards com % de match em anel amarelo,
   botões "Ver Detalhes" outline e "Aplicar" azul sólido);
2. Página de detalhes da vaga (descrição, requisitos, comparação "Suas habilidades
   vs. requisitos", CTA "Gerar Currículo ATS para esta Vaga");
3. Workspace do currículo (split screen: editor + sugestões de IA à esquerda,
   preview em tempo real à direita; botões "Baixar PDF" e "Salvar Versão");
4. Perfil do usuário (formulários Shadcn: Input, Textarea, Select, DatePicker).

Gere o código inicial completo, focado em UI e estruturação, pronto para renderizar.
O que mudou até a versão final (e por quê)
#	Mudança	Por quê
1	O núcleo passou a ser "colar vaga + colar currículo" (era só um feed de vagas). Nova tela Analisar virou a home.	O feed de vagas não resolvia o problema central: o candidato precisava colar os próprios textos para saber o match. O core de 4 passos (colar → analisar → gerar → exportar) é o coração do produto.
2	Análise real em TypeScript puro (src/lib/analysis.ts): extração de palavras-chave da vaga (stopwords PT+EN, frases compostas, frequência ponderada), detecção de presença no currículo (plurais, prefixos, radicais) e score ponderado.	Sem backend, a análise precisa ser determinística e auditável — e nunca pode depender de "achismo".
3	Parser de currículo colado (seções RESUMO/EXPERIÊNCIA/HABILIDADES/FORMAÇÃO/IDIOMAS, bullets, datas, contato).	Para gerar a versão ajustada é preciso entender a estrutura do texto que a pessoa colou.
4	Geração honesta: a versão ajustada só reordena e normaliza o texto do usuário (frases com keywords primeiro, habilidades ordenadas por relevância, seções canônicas ATS). Zero conteúdo inventado.	A regra de ouro. Palavras-chave ausentes aparecem na interface como "não incluídos" — nunca entram no documento.
5	Regra escrita na interface (banner "Nosso compromisso" na home, inline no workspace, rodapé do preview).	O usuário pediu explicitamente: a regra deve estar visível na própria interface.
6	Exportação PDF via @media print com #resume-print (só o currículo é impresso).	Evita dependência de biblioteca de PDF e garante que o arquivo gerado seja idêntico ao preview ATS.
7	Feed movido para /vagas e links de "Aplicar" passaram a levar ao núcleo (/?vaga=id, com pré-preenchimento da vaga).	Toda a jornada converge para o core; o feed virou vitrine, não o destino.
8	Ajustes finos de matching: stopwords de ruído de anúncio (CLT, PJ, benefícios, níveis), plural em frases compostas ("design system" → "design systems"), radical para flexões verbais ("desenvolver" ↔ "desenvolvimento").	Reduzir falsos negativos/positivos e deixar o match mais próximo do que um ATS real faria.
Evolução posterior (segunda rodada de pedidos)
#	Pedido	Como foi feito
9	Exportar também em .docx e em PDF com download direto	src/lib/exporters/resumeExport.ts com exportPdf() (jsPDF) e exportDocx() (pacote docx), ambos carregados por import() dinâmico (fora do bundle inicial). Botões Baixar PDF, Baixar DOCX e Imprimir no núcleo e no workspace. O conteúdo exportado é exatamente o do preview ATS — nada inventado.
10	Login com confirmação de e-mail pelo Resend, serverless na Vercel	Pasta api/ com auth/request, auth/verify e auth/session: código de 6 dígitos (validade 10 min), tokens stateless assinados por HMAC (AUTH_SECRET) — sem banco. A chave RESEND_API_KEY só existe no servidor. Em dev local, sem chave, o código aparece na tela (Modo desenvolvimento). Middleware no vite.config.ts reaproveita os mesmos handlers. Sessão de 30 dias no localStorage + botão Entrar/Sair no cabeçalho (AuthButton).
11	Dashboard da evolução do match entre as versões	src/lib/matchHistory.ts grava cada análise e cada versão salva em localStorage; a rota /evolucao mostra gráfico de linha SVG (área azul, pontos amarelos), cards de estatística (último, melhor, média, variação desde a 1ª) e tabela com delta por registro.
12	Especialização 100% em Marinha Mercante	Vagas, perfil, sugestões e exemplos reescritos para as carreiras do nicho (marinheiro auxiliar de convés/máquinas, moço, eletricista marítimo, cozinheiro/taifeiro a bordo, condutor de máquinas, contramestre, primeiro emprego); vocabulário de analysis.ts trocado por termos de bordo ("amarração e atracação", "combate a incêndio", "trabalho em altura", "escala 14x14"…); copy de todas as telas, filtros por carreira e rodapé marítimos.
13	SEO + GEO (buscas em IAs)	index.html com title/description/keywords/canonical/OG/Twitter + JSON-LD (WebApplication, FAQPage), shell estático crawlável sem JS, public/robots.txt liberando GPTBot/ClaudeBot/PerplexityBot/etc., public/sitemap.xml, public/llms.txt (contexto factual para IAs) e src/lib/seo.ts (ROUTE_SEO + useSeo) aplicando title/description canônicos por rota.
3. Como a análise funciona (da vaga colada ao currículo ajustado)
┌─────────────────────────────┐     ┌─────────────────────────────┐
│  INPUT 1: descrição da vaga │     │  INPUT 2: currículo (texto)  │
└──────────────┬──────────────┘     └──────────────┬──────────────┘
               │                                    │
               ▼                                    ▼
   extractKeywords(jobText)              parseResume(resumeText)
   • normaliza (minúsculas, sem acento)  • identifica seções por títulos
   • remove stopwords PT+EN              • extrai nome, cargo, contato
   • detecta frases compostas            • separa experiências/bullets
     ("power bi", "front end"…)          • extrai skills, formação, idiomas
   • ranqueia por frequência             • avisa se não achou seções
               │                                    │
               └──────────────┬─────────────────────┘
                              ▼
                  analyzeMatch(jobText, resumeText)
                  • para cada keyword, testa presença no
                    currículo (plural, prefixo, radical)
                  • score = Σ peso das encontradas ÷ Σ peso total
                              │
                              ▼
              ┌───────────────────────────────┐
              │ RESULTADO NA TELA             │
              │ • % de match (anel amarelo)   │
              │ • chips azuis: encontradas    │
              │ • chips amarelos: faltantes   │
              │ • aviso honesto sobre lacunas │
              └───────────────┬───────────────┘
                              ▼
              buildAdjustedResume(parsed, analysis)
              • reordena frases do resumo com keywords primeiro
              • reordena bullets por relevância
              • skills ordenadas (match primeiro)
              • seções renomeadas para o padrão ATS
              • NADA de novo é escrito
                              │
                              ▼
              ┌───────────────────────────────┐
              │ VERSÃO AJUSTADA (preview)     │
              │ • coluna única, Arial         │
              │ • seções canônicas ATS        │
              │ • "Baixar PDF" (impressão)    │
              └───────────────────────────────┘
Exemplo de uso (textos reais da aplicação — nicho Marinha Mercante):

Vaga colada: Marinheiro Auxiliar de Convés — Navegação Costeira S.A. (Suape · embarcação · escala 14x14): arrumação de convés, amarração e atracação, pintura, rondas, manobras, registro na Marinha Mercante, certificados de sobrevivência e combate a incêndio, NR-35, embarque imediato.
Currículo colado: Lucas Mendes Oliveira — Marinheiro de Convés / Auxiliar de Máquinas, com experiências honestas a bordo, certificados e competências.
Resultado: 84% de match — 20 de 24 palavras-chave encontradas e 4 faltantes (adicional de insalubridade, alimentacao a bordo, costeira…), estas não inseridas no documento.
Versão ajustada gerada: resumo com as frases-chave primeiro, bullets reordenados por relevância, habilidades com os termos da vaga no topo, seções canônicas (RESUMO PROFISSIONAL → EXPERIÊNCIA PROFISSIONAL → HABILIDADES → FORMAÇÃO ACADÊMICA → IDIOMAS), pronta para Baixar PDF (jsPDF) ou Baixar DOCX.
4. Ajustes pedidos depois da primeira geração (e por quê)
"O núcleo é curto e precisa funcionar" — a primeira versão era só um feed de vagas com cards. Pedi para o core ser: colar vaga → colar currículo → match com palavras-chave encontradas/faltantes → versão ajustada exportável. Por quê: sem isso o app era vitrine, não ferramenta.
"A regra vale para a aplicação inteira… e vale trazer a ideia para a sua" — o compromisso de nunca inventar experiência passou a aparecer escrito na interface (banner, inline no workspace, rodapé do preview) e virou invariante do código de geração. Por quê: confiança do candidato é o produto.
Refinamentos de matching — stopwords de ruído de anúncio, plural em frases compostas e radical verbal. Por quê: o match inicial tinha ruído ("lado", "time") e falsos negativos ("desenvolver" não casava com "desenvolvimento").
Layout da linha "Não incluímos" — o flex quebrava o texto em itens separados. Por quê: legibilidade e profissionalismo do documento de mudanças.
"Quero exportar em .docx e em PDF" — a exportação em PDF passou a baixar o arquivo direto (jsPDF) e o .docx foi adicionado, mantendo o Imprimir como alternativa fiel. Por quê: recrutador de navigação/cozinha pede anexo em Word, e abrir janela de impressão para salvar PDF é atrito.
"Login com e-mail de confirmação pelo Resend" — API serverless stateless (código de 6 dígitos assinado por HMAC), chave só no servidor, fluxo de dev sem chave para testar. Por quê: identidade real sem banco e sem expor segredo.
"Dashboard mostrando a evolução do match entre as versões" — histórico local gravado automaticamente a cada análise/versão + gráfico de linha do tempo em /evolucao. Por quê: provar que o currículo está ficando mais competitivo é a métrica do produto.
"Especializar em Marinha Mercante" (100%) — dados, vocabulário e toda a copy reposicionados para as carreiras de bordo. Por quê: nicho definido pelo usuário como foco exclusivo do app.
"Preparar para SEO e GEO" — metadados + JSON-LD, shell estático crawlável, robots.txt com robôs de IA permitidos, sitemap, llms.txt e título/ descrição por rota. Por quê: ser encontrado tanto no Google quanto nas buscas dentro de IAs.
5. Endereço da aplicação
Rodando localmente (desenvolvimento): http://localhost:5173 (npm run dev — Vite, porta 5173; login por e-mail roda em modo desenvolvimento sem chave do Resend).
Build de produção: npm run build gera dist/ (funções da API em api/ ficam na Vercel; o JS principal ~170 kB gzip, com jsPDF/docx em chunks separados carregados sob demanda).
Publicar (Vercel — recomendado, pois o login depende das funções):
Suba o repositório e importe o projeto na Vercel;
Configure as envs RESEND_API_KEY, AUTH_SECRET e RESEND_FROM (o .env.example lista todas);
Build command npm run build, output dist/ — o vercel.json já faz o rewrite de SPA preservando /api/*. O endereço resultante vira o SITE_URL de src/lib/seo.ts (hoje https://matchjobs.vercel.app, marcado com // TODO: trocar pelo domínio real).
Alternativa estática (sem login): qualquer host de dist/ (ex.: npx netlify-cli deploy --dir=dist --prod).
6. Evidências (prints)
As capturas abaixo estão em docs/screenshots/:

Arquivo	Conteúdo
01-nucleo-analise.png	Núcleo: vaga + currículo colados, match, keywords encontradas/faltantes e versão ajustada gerada
02-nucleo-resultado-gerado.png	Fluxo completo: análise + documento ATS pronto para exportar
03-feed-vagas.png	Feed de vagas com cards e anel de match
04-detalhes-vaga.png	Detalhes da vaga + comparação de habilidades
05-workspace-curriculo.png	Workspace split-screen com preview ATS
06-perfil.png	Perfil & Skills com formulários Shadcn
07-nucleo-match-84.png	Núcleo na versão Marinha Mercante: 84% de match, 20/24 keywords encontradas
08-nucleo-gerado-export.png	Versão ajustada gerada + "não incluímos" honesto + botões Baixar PDF / Baixar DOCX
09-feed-marinha.png	Feed 100% Marinha Mercante (chips de carreira, anel amarelo)
10-detalhe-vaga.png	Detalhe da vaga de embarque + comparação "Suas habilidades vs. requisitos do embarque"
11-workspace-export.png	Workspace com Salvar Versão / Imprimir / Baixar DOCX / Baixar PDF
12-evolucao-match.png	Dashboard de evolução do match (/evolucao): gráfico SVG + estatísticas
13-login-entrar.png	Login por e-mail com código de confirmação (/entrar)
14-perfil-maritimo.png	Perfil do marítimo + card de anexo (abas Arquivo / Colar texto)
Prints 01–06 registram a estrutura original; 07–14 registram a versão atual (100% Marinha Mercante, exportação PDF/DOCX, evolução do match e login). Verificação feita em navegador: análise com a amostra marítima em 84%, downloads curriculo-marcelo-andrade-costa-ats.docx e .pdf, login completo em modo desenvolvimento e histórico de 6 registros no gráfico de evolução.

Evidência de que a aplicação roda é o que mais pesa em um portfólio: os prints acima mostram o fluxo completo funcionando de ponta a ponta.

