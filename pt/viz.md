---
layout: default
title: Visualizações
lang: pt
---

<link rel="stylesheet" href="style.css">

<br>

<h1 class="title-about">Visualizações</h1>

<br>
<br>

<div style="max-width:570px; margin:0 auto;">
  <h2 class="selecao_por_tema">SELEÇÃO POR TEMA DA VISUALIZAÇÃO</h2>
    <div class="botoes-container">
      <a href="{{ site.baseurl }}/pt/viz/comercio-exterior" class="botao">COMÉRCIO EXTERIOR</a>
      <a href="{{ site.baseurl }}/pt/viz/desenvolvimento" class="botao">DESENVOLVIMENTO</a>
      <a href="{{ site.baseurl }}/pt/viz/desmatamento" class="botao">DESMATAMENTO</a>
      <a href="{{ site.baseurl }}/pt/viz/energia" class="botao">ENERGIA</a>
      <a href="{{ site.baseurl }}/pt/viz/saude" class="botao">SAÚDE</a>
      <a href="{{ site.baseurl }}/pt/viz/mercado-de-trabalho" class="botao">MERCADO DE TRABALHO</a>
      <a href="{{ site.baseurl }}/pt/viz/agropecuaria" class="botao">AGROPECUÁRIA</a>
    </div>
</div>

  <br>


   <h2 class="selecao_por_tema">VISUALIZAÇÕES MAIS ACESSADAS</h2>
<br>

  <div class="imagens-container">
  <button class="carousel-btn prev" aria-label="Anterior">&#10094;</button>
  
  <div class="imagens-track">
   <div class="icone-bloco">
    <a href="{{ site.baseurl }}/pt/viz/ranking-atividades-economicas-mais-dinamicas" target="_blank" rel="noopener noreferrer">
      <img src="{{ site.baseurl }}/assets/img/icons_viz/icon_rk_atividades_dinamicas.png" alt="ícone ranking das atividades mais dinâmicas">
    </a><br>
    <p>Ranking das Atividades Econômicas mais Dinâmicas</p>
   </div>
   <div class="icone-bloco">
    <a href="{{ site.baseurl }}/pt/viz/series-temporais-da-producao-consumo-e-consumidores-de-energia" target="_blank" rel="noopener noreferrer">
      <img src="{{ site.baseurl }}/assets/img/icons_viz/icon_ts_prod_con.jpg" alt="ícone séries temporais produção e consumo de energia 2">
    </a><br>
    <p>Séries Temporais de Produção, Consumo e Consumidores de Energia</p>
   </div>
   <div class="icone-bloco">
    <a href="{{ site.baseurl }}/pt/viz/ranking-da-potencia-outorgada-dos-estados-da-amazonia-legal" target="_blank" rel="noopener noreferrer">
      <img src="{{ site.baseurl }}/assets/img/icons_viz/icon_pot_outorgada.jpg" alt="ícone ranking potência outorgada">
    </a><br>
    <p>Ranking de Potência Outorgada</p>
   </div>
    <div class="icone-bloco">
    <a href="{{ site.baseurl }}/pt/viz/series-temporais-dos-sistemas-isolados" target="_blank" rel="noopener noreferrer">
      <img src="{{ site.baseurl }}/assets/img/icons_viz/icon_ts_sis_isolados.jpg" alt="ícone séries temporais de sistemas isolados">
    </a><br>
    <p>Séries Temporais dos Sistemas Isolados de Energia</p>
   </div>
   <div class="icone-bloco">
    <a href="{{ site.baseurl }}/pt/viz/series-temporais-da-geracao-distribuida" target="_blank" rel="noopener noreferrer">
      <img src="{{ site.baseurl }}/assets/img/icons_viz/icon_ts_ger_distribuida.jpg" alt="ícone séries temporais da geração distribuída">
    </a><br>
    <p>Séries Temporais da Geração Distribuída</p>
   </div>
   <div class="icone-bloco">
    <a href="{{ site.baseurl }}/pt/viz/mapa-floresta-desmatamento" target="_blank" rel="noopener noreferrer">
      <img src="{{ site.baseurl }}/assets/img/icons_viz/icon_ts_mapa_evolucao_desmatamento.png" alt="ícone mapa de área de floresta ou desmatada">
    </a><br>
    <p>Mapa de Área de Floresta ou Desmatada</p>
   </div>
  </div>

  <button class="carousel-btn next" aria-label="Próximo">&#10095;</button>
</div>

<br>
<br>
<br>
<br>


<script>
(function () {
  const AUTOPLAY_MS = 6000;
  const RESPECTS_REDUCED_MOTION = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  const container = document.querySelector('.imagens-container');
  if (!container) return;

  const track = container.querySelector('.imagens-track');
  const originals = Array.from(track.children); // guarda os cards originais
  const prev = container.querySelector('.carousel-btn.prev');
  const next = container.querySelector('.carousel-btn.next');

  let index = 0;        // índice no trilho COM clones
  let vis = 1;          // quantos cards cabem na viewport
  let autoplayId = null;
  let isHoveringOrFocusing = false;

  // utilidades
  function stepSize() {
    const first = track.children[0];
    const rect = first.getBoundingClientRect();
    const style = getComputedStyle(first);
    const ml = parseFloat(style.marginLeft) || 0;
    const mr = parseFloat(style.marginRight) || 0;
    const gap = parseFloat(getComputedStyle(track).gap || 0);
    return rect.width + ml + mr + gap;
  }
  function visibleCount() {
    const s = stepSize();
    if (s <= 0) return 1;
    return Math.max(1, Math.floor(container.clientWidth / s));
  }
  function setTransform(noAnim = false) {
    if (noAnim) track.style.transition = 'none';
    track.style.transform = `translateX(${-index * stepSize()}px)`;
    if (noAnim) {
      // força reflow e reativa animação
      track.offsetHeight;
      track.style.transition = 'transform 0.4s ease-in-out';
    }
  }

  // constrói/Reconstrói o trilho infinito com clones
  function build() {
    const currentVis = visibleCount();
    // salva posição relativa se quiser manter “grupo” atual ao reconstruir
    const logicalPos = (index - vis + originals.length) % originals.length || 0;

    track.innerHTML = '';
    vis = currentVis;

    // clones: últimos vis no início, originais, primeiros vis no fim
    const head = originals.slice(-vis).map(n => n.cloneNode(true));
    const tail = originals.slice(0, vis).map(n => n.cloneNode(true));

    head.forEach(n => track.appendChild(n));
    originals.forEach(n => track.appendChild(n));
    tail.forEach(n => track.appendChild(n));

    // começamos no primeiro original (após os clones de cabeça)
    index = vis + logicalPos;    // preserva grupo na troca de layout
    setTransform(true);
    updateButtons();             // nos infinitos, os botões nunca desabilitam
  }

  function updateButtons() {
    // em loop infinito não desabilitamos setas
    prev.disabled = false;
    next.disabled = false;
  }

  function goNext() {
    index++;
    setTransform();
  }

  function goPrev() {
    index--;
    setTransform();
  }

  // quando a transição termina, checa se estamos nos clones e "teleporta"
  track.addEventListener('transitionend', () => {
    const total = originals.length;
    // faixa de originais: [vis, vis + total - 1]
    if (index >= vis + total) {
      // saiu pelo fim -> volta para início dos originais
      index = vis;
      setTransform(true);
    } else if (index < vis) {
      // saiu pelo começo -> vai para final dos originais
      index = vis + total - 1;
      setTransform(true);
    }
  });

  // eventos de controle
  next.addEventListener('click', () => { goNext(); restartAutoplay(); });
  prev.addEventListener('click', () => { goPrev(); restartAutoplay(); });

  // teclado
  container.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowRight') { goNext(); restartAutoplay(); }
    if (e.key === 'ArrowLeft')  { goPrev();  restartAutoplay(); }
  });
  container.tabIndex = 0;

  // pausa em hover/focus
  container.addEventListener('mouseenter', () => { isHoveringOrFocusing = true; stopAutoplay(); });
  container.addEventListener('mouseleave', () => { isHoveringOrFocusing = false; startAutoplay(); });
  container.addEventListener('focusin',  () => { isHoveringOrFocusing = true; stopAutoplay(); });
  container.addEventListener('focusout', () => { isHoveringOrFocusing = false; startAutoplay(); });

  // pausa quando a aba perde foco
  document.addEventListener('visibilitychange', () => {
    if (document.hidden) stopAutoplay(); else startAutoplay();
  });

  // autoplay
  function startAutoplay() {
    if (RESPECTS_REDUCED_MOTION) return;
    if (isHoveringOrFocusing) return;
    if (autoplayId) return;
    autoplayId = setInterval(goNext, AUTOPLAY_MS);
  }
  function stopAutoplay() {
    if (autoplayId) { clearInterval(autoplayId); autoplayId = null; }
  }
  function restartAutoplay() { stopAutoplay(); startAutoplay(); }

  // rebuild em resize (com debounce simples)
  let resizeTimer;
  window.addEventListener('resize', () => {
    clearTimeout(resizeTimer);
    resizeTimer = setTimeout(() => { build(); restartAutoplay(); }, 120);
  });

  // boot
  build();
  startAutoplay();
})();
</script>
