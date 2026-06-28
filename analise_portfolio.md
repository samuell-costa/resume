# Relatório de Análise Técnica: Portfólio de Samuel Costa

Este documento apresenta uma análise detalhada do estado atual do portfólio do Samuel Costa (localizado em [Portfolio](file:///C:/Users/samre/Ambiente%20de%20Trabalho/Projects/Portfolio)) e sugere melhorias organizadas por categorias, incluindo bugs críticos que comprometem o funcionamento imediato do website.

---

## 1. O que está feito (Arquitetura e Funcionalidades)

O portfólio foi construído como uma **SPA (Single Page Application)** leve e moderna.

### 🎨 Design e Interface (UI/UX)
- **Tema Escuro Moderno:** Uso consistente de tons escuros (`#121212`, `#0a0a0a`), conferindo um aspeto premium e minimalista.
- **Glassmorphism:** O cabeçalho fixo global utiliza fundo semi-transparente com blur (`backdrop-filter: blur(12px)`) e bordas subtis, o que proporciona um efeito visual muito elegante ao fazer scroll.
- **Galeria Acordeão (Expansível):** Na vista de Projetos, os itens da galeria expandem-se de forma fluida no hover (`transition: flex 0.7s cubic-bezier(0.25, 1, 0.5, 1)`) em ecrãs desktop, criando uma experiência interativa dinâmica.
- **Feedback Visual:** Efeitos de micro-interações nos botões e cartões de projeto (elevação com transição suave, alteração de opacidade na navegação).

### ⚙️ Engenharia e Funcionalidades Core
- **Navegação Single Page (SPA):** Transição de páginas controlada por JavaScript (`window.navigateTo(viewId)`). As secções são carregadas em contentores dinâmicos, aplicando animações de fade-in e ocultando/mostrando as vistas ativas.
- **Internacionalização (i18n):** Tradução nativa entre Inglês (EN) e Português (PT) controlada por um dicionário local (`translations` no JS). O estado do idioma é persistido em `localStorage` para manter a escolha do utilizador entre sessões.
- **Leitor de Vídeo Customizado (Plyr):** Integração da biblioteca [Plyr](https://plyr.io/) para um controlo elegante sobre os vídeos do portfólio, configurando controlos personalizados e comportamento responsivo.
- **Modo Cinema (Expansor de Média):** Funcionalidade inovadora no modal de detalhes que oculta o painel de texto e expande o vídeo/imagem para cobrir quase a totalidade do ecrã, maximizando o foco na média.
- **Formulário de Contacto AJAX:** Integração com o serviço [Formspree](https://formspree.io/) via `fetch` para envio de mensagens sem recarregar a página, apresentando estados de loading e sucesso/erro ao utilizador.

---

## 2. Erros Críticos (Bugs Identificados)

Existem problemas que impedem o correto funcionamento do site e que devem ser corrigidos imediatamente:

### ⚠️ Case-Sensitivity no Vídeo do Projeto "Joaquim"
- **O Problema:** No ficheiro [index.html](file:///C:/Users/samre/Ambiente%20de%20Trabalho/Projects/Portfolio/index.html#L290), o cartão de projeto "Joaquim" na página inicial está mapeado com:
  `data-media="./videos/JOAQUIM_video.webm"` (com `video` em minúsculas).
  Contudo, o ficheiro físico na pasta de [videos](file:///C:/Users/samre/Ambiente%20de%20Trabalho/Projects/Portfolio/videos) chama-se `JOAQUIM_VIDEO.webm` (em maiúsculas).
- **Impacto:** Em servidores Linux de produção (como GitHub Pages, Vercel ou Netlify), que são estritamente sensíveis a maiúsculas/minúsculas, **o vídeo falhará com erro 404** na página inicial, embora funcione na página de projetos (onde o link está correto).

### 📁 Pasta de Imagens e CV em Falta no Repositório
- **O Problema:** O código de [index.html](file:///C:/Users/samre/Ambiente%20de%20Trabalho/Projects/Portfolio/index.html) referencia várias imagens locais (ex: `./images/APAV.png`, `./images/OPERA.png`, `./images/JOAQUIM.png`, `./images/CNO.png`) e um ficheiro de currículo (`CV_Samuel_Costa.pdf`). Contudo, **a pasta `/images` e o PDF não existem** no diretório do projeto.
- **Impacto:** As miniaturas dos projetos não carregam (apresentando ícones de imagem partida). Apenas os projetos de Social Media (Trey24k e Tigz) funcionam porque as suas imagens são carregadas externamente a partir de um URL do GitHub. O botão de download do CV também gera um erro.

---

## 3. Oportunidades de Melhoria e Boas Práticas

Para elevar o portfólio a um nível verdadeiramente profissional e produtivo, recomendam-se as seguintes alterações:

### 🚀 Performance e Otimização
1. **Remover o Tailwind CDN em Produção:**
   - Atualmente, o Tailwind CSS é injetado via script do CDN: `<script src="https://cdn.tailwindcss.com"></script>`.
   - **Desvantagem:** O navegador precisa de descarregar e compilar as classes em runtime. Isto atrasa a renderização e pode causar FOUC (Flash of Unstyled Content).
   - **Solução:** Configurar um ambiente build com Vite ou compilar o Tailwind via CLI para gerar um ficheiro CSS estático e minificado apenas com as classes utilizadas.
2. **Compressão e Distribuição dos Vídeos:**
   - Os vídeos na pasta `/videos` são pesados (ex: `OPERA_VIDEO.webm` tem ~25MB e `CNO_VIDEO.webm` tem ~24.6MB).
   - **Desvantagem:** Utilizadores móveis ou com ligações mais lentas sofrerão com paragens no buffering e tempos longos de carregamento.
   - **Solução:** Utilizar ferramentas como o `FFmpeg` ou `Handbrake` para comprimir agressivamente os ficheiros, ou alojá-los em serviços de streaming (Vimeo Pro, YouTube não listado, Cloudflare Stream) e consumi-los via API/Embed.
3. **Divisão de Ficheiros (Modularização):**
   - O ficheiro `index.html` (com mais de 1000 linhas e 80KB) é um "monólito".
   - **Solução:** Separar a folha de estilos CSS customizada para um ficheiro `style.css`, o motor JavaScript e lógica do modal para `app.js` e, idealmente, externalizar o dicionário de traduções para ficheiros JSON individuais (`en.json` e `pt.json`) carregados sob demanda.

### 🌐 SEO (Otimização para Motores de Busca)
1. **Metadados em Falta:**
   - O site não possui tags `<meta>` essenciais para partilhas sociais e indexação de motores de busca.
   - **O que adicionar:**
     - `<meta name="description" content="...">`
     - Tags Open Graph para o Facebook/LinkedIn (`og:title`, `og:description`, `og:image`, `og:url`)
     - Tags Twitter Cards (`twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`)
     - Um ficheiro `favicon.ico` e tags de ícone para dispositivos móveis.
2. **Uso de Semântica HTML e Links Reais:**
   - A barra de navegação usa elementos `<button>` para transitar entre vistas.
   - **Desvantagem:** Motores de busca (Googlebot) não seguem facilmente eventos `onclick` de JavaScript para descobrir e indexar conteúdo, e os utilizadores não conseguem usar a funcionalidade nativa do browser "Abrir num novo separador" (Ctrl+Click).
   - **Solução:** Substituir por `<a>` com `href="#home"`, `href="#projects"`, etc., e usar a lógica SPA escutando o evento `hashchange` do URL.

### ♿ Acessibilidade (a11y)
1. **Links Vazios e Redes Sociais:**
   - Vários links de redes sociais no cabeçalho e rodapé têm `href="#"` (como o Instagram). Além disso, ícones como `<i class="fab fa-instagram"></i>` necessitam de descritores de texto adequados.
   - **Solução:** Adicionar atributos `aria-label="Instagram de Samuel Costa"` a todos os links com ícones puros para que os leitores de ecrã saibam o seu propósito.
2. **Falta de Estados de Foco Claros:**
   - Para utilizadores que navegam usando o teclado (Tab), os links de menu e botões devem possuir um contorno visível (`focus-visible`) para indicação clara do foco.

### 🧭 Histórico e Routing do Navegador
- **O Problema:** Como a SPA troca as classes ativas via JS, o URL no topo do navegador nunca muda.
- **Desvantagem:** Se um utilizador estiver na página "About" ou "Contacts" e atualizar o browser (F5), ele é imediatamente redirecionado para a "Home". Adicionalmente, os botões "Avançar" e "Retroceder" do próprio navegador ficam desativados.
- **Solução:** Implementar um roteador simples baseado em **Hash Routing** (ex: `window.location.hash`). Desta forma, navegar para Contactos mudará o URL para `index.html#contact`. Ao carregar a página ou mudar o hash, o JS lê o valor e ativa a vista correspondente de forma limpa.

### ✍️ Conteúdo e Placeholders
- **Lorem Ipsum no HTML:**
  - O parágrafo `<p data-i18n="bio2">` contém texto estático em Lorem Ipsum no código HTML de base.
  - **Solução:** Colocar o texto em Inglês (ou Português) como o conteúdo por omissão diretamente no HTML. Isto garante que mesmo que o JavaScript demore a carregar ou falhe, o utilizador nunca verá o Lorem Ipsum de teste.

---

## 4. Próximos Passos Recomendados

Para ajudar a colocar estas melhorias em prática, proponho a seguinte sequência de intervenções:

1. **Correção de Bugs Críticos:** Corrigir o path do vídeo do "Joaquim" e restabelecer os ficheiros locais de imagens na pasta correspondente.
2. **Implementação de Router por Hash:** Atualizar o JavaScript para usar `window.location.hash`, permitindo links partilháveis (ex: `site.com/#projects`) e funcionamento correto dos botões de retroceder/avançar.
3. **Melhorias de Acessibilidade e SEO:** Adicionar metadados à `<head>` e converter botões de navegação em links semânticos.
