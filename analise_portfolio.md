# Relatório de Análise Técnica e Plano de Trabalho: Portfólio de Samuel Costa

Este documento serve como registo do estado atual do desenvolvimento do portfólio de Samuel Costa (localizado em [Portfolio](file:///C:/Users/samre/Ambiente%20de%20Trabalho/Projects/Portfolio)), detalhando o que já foi implementado e a lista de tarefas pendentes para futuras sessões de trabalho.

---

## 1. O que já foi concluído (Estado Atual)

### 🎨 Design Cinematográfico & UX (Inspirado em `andyyoungfilm.com`)
* **Redesenho Completo (SPA):** Website reestruturado para um visual minimalista, escuro e premium.
* **Grelha Panorâmica:** Os projetos são exibidos em formato retangular cinematográfico (16:9), com efeito de hover que revela o título e a indicação `— view —`.
* **Painel de Detalhes Superior (Split Layout):** Clicar num projeto abre a sua ficha de detalhes acima da grelha de projetos. Esta ficha exibe o leitor Plyr (vídeos) ou imagem do lado direito, e o título/descrição textual do lado esquerdo.
* **Otimização Mobile:** 
  * Inversão da ordem de elementos no ecrã móvel (o vídeo surge primeiro que a descrição para melhor UX).
  * Máscara de títulos da grelha permanentemente visível a 85% de opacidade em ecrãs táteis, uma vez que não existe hover no telemóvel.
  * Botão `✕ Close Project` fixado no topo do painel de detalhes.
  * Integração de um **Burger Menu (Menu de Hambúrguer)** minimalista com overlay em ecrã inteiro e efeito de desfocagem (`backdrop-blur-xl`).

### ⚙️ Engenharia e Dados
* **Fusão de Pastas do Git:** Consolidação de pastas duplicadas (`IMAGES/` e `images/`) em minúsculas para evitar erros no Windows, com a recuperação das imagens em falta e do PDF de currículo do repositório remoto.
* **Métricas Preservadas:** Restauro dos dados estatísticos ricos (`50M+ Views`, etc.), colaborações de marcas e links de canais de YouTube/TikTok nas descrições dinâmicas dos projetos de redes sociais (Trey24k e Tigz).

### 🐛 Correções de Bugs Efetuadas
* **Case-Sensitivity:** Correção do caminho do vídeo do "Joaquim" na página inicial (`JOAQUIM_VIDEO.webm` em maiúsculas), resolvendo o erro 404 em servidores de produção.

---

## 2. Tarefas Pendentes (Lista para a Próxima Sessão)

Para a próxima fase de desenvolvimento, devem ser implementadas as seguintes melhorias técnicas para garantir que o website indexa corretamente nos motores de busca (Google) e oferece navegação nativa:

### 🧭 Tarefa 1: Implementação de Hash Routing (SPA Semântica)
* **Objetivo:** Permitir indexação de secções, links partilháveis e uso dos botões de retroceder/avançar do browser.
* **Ações:**
  1. Converter todos os elementos `<button>` de navegação no menu principal e no menu mobile para links semânticos `<a>` com hashes (ex: `href="#campaigns"`, `href="#social"`, `href="#about"`, `href="#contact"`).
  2. Adicionar um escutador ao evento `hashchange` no JavaScript:
     ```javascript
     window.addEventListener('hashchange', () => {
         const hash = window.location.hash.substring(1) || 'campaigns';
         navigateTo(hash);
     });
     ```
  3. Atualizar a lógica de carregamento inicial para detetar o hash de entrada.

### 🌐 Tarefa 2: Configuração de SEO Completo (Metadados e Partilhas)
* **Objetivo:** Melhorar o ranking no Google e garantir pré-visualizações elegantes no LinkedIn e outras redes sociais.
* **Ações:**
  1. Adicionar as seguintes meta tags essenciais na `<head>` de `index.html`:
     * `<meta name="description" content="Samuel Costa - Marketing & Creative Strategist. Especialista em gestão de redes sociais, campanhas e análise de dados de performance digital.">`
     * `<meta name="keywords" content="...">`
  2. Implementar protocolos **Open Graph (OG)** para partilhas em redes sociais:
     * `og:title`, `og:description`, `og:image` (apontando para uma imagem de capa do portfólio), `og:url`.
  3. Adicionar tags **Twitter Cards** para compatibilidade com o X.

### 📁 Tarefa 3: Ficheiros de Indexação do Servidor
* **Objetivo:** Guiar os robôs de pesquisa do Google.
* **Ações:**
  1. Criar um ficheiro `robots.txt` na raiz com:
     ```text
     User-agent: *
     Allow: /
     Sitemap: https://samuell-costa.github.io/resume/sitemap.xml
     ```
  2. Criar um ficheiro `sitemap.xml` estruturado mapeando as secções e hashes do portfólio.
