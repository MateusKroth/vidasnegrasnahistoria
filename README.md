<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Livro Interativo</title>
<style>
  * { box-sizing: border-box; }
  body, html {
    margin: 0;
    height: 100vh;
    background: url("https://t4.ftcdn.net/jpg/12/56/34/05/360_F_1256340587_dxlDteanGixJBFiDJnIoUvZ1pbD9J4em.jpg");
    font-family: Arial, sans-serif;
    overflow: hidden;
  }

  /* Polaroids */
  #polaroid, .polaroid {
    width: 180px;
    height: 250px;
    cursor: pointer;
    user-select: none;
    position: fixed;
    background: #fff;
    padding: 10px;
    box-shadow: 0 8px 15px rgba(0,0,0,0.3);
    border-radius: 12px;
    text-align: center;
    transition: opacity 0.5s ease;
    z-index: 10;
  }
  #polaroid img, .polaroid img { 
    width: 100%; 
    height: 200px; 
    object-fit: cover; 
    border-radius: 5px; 
    display: block; 
  }
  #polaroid p, .polaroid p { 
    margin-top: 12px; 
    font-weight: bold; 
    font-size: 16px; 
    color: #333; 
  }

  /* Linha vermelha */
  svg#linha-conexao {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 5;
    pointer-events: none;
  }

  /* Livro / pasta */
  #livro {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 1000px;
    height: 800px;
    background-image: url("img/Pasta.png");
    background-repeat: no-repeat;
    background-position: center center;
    background-size: 100% 100%;
    color: #000;
    font-size: 24px;
    user-select: none;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.5s ease;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .conteudo-unificado {
    position: relative;
    background: #fff;
    width: 840px;
    height: 720px;
    box-shadow: inset 0 0 15px rgba(0,0,0,0.1);
    display: flex;
    flex-direction: row;
    overflow: hidden;
  }

  .linha-divisoria { width: 5px; background: #888; height: 100%; }

  .lado-esquerdo,
  .lado-direito {
    width: 50%;
    position: relative;
    display: flex;
    justify-content: flex-start;
    align-items: flex-start;
    padding: 10px;
    overflow-y: auto;
  }

  .lado-direito { text-align: left; flex-direction: column; }

  .foto-canto {
    width: 450px;
    height: 550px;
    border-radius: 8px;
    box-shadow: 0 5px 15px rgba(0,0,0,0.5);
    object-fit: cover;
    border: 2px solid #fff;
    background: #fff;
  }

  .navegacao {
    position: absolute;
    top: 10px;
    right: -130px;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .navegacao button {
    width: 120px;
    height: 45px;
    border: none;
    cursor: pointer;
    color: transparent;
    font-weight: bold;
    font-size: 14px;
    clip-path: polygon(0 0, 85% 0, 100% 50%, 85% 100%, 0 100%);
    transition: all 0.25s ease;
    box-shadow: 2px 0 6px rgba(0,0,0,0.3);
  }

  .navegacao button:hover { transform: scale(1.1); }
  .navegacao button.ativo { transform: scale(1.15); box-shadow: 3px 0 8px rgba(0,0,0,0.5); }

  #btn-fechar { background: #e74c3c; }
  #btn-azul { background: #3498db; }
  #btn-verde { background: #27ae60; }
  #btn-amarelo { background: #f1c40f; }
  #btn-roxo { background: #9b59b6; }

  @keyframes virar-pagina {
    0% { transform: rotateY(0deg); opacity: 1; }
    50% { transform: rotateY(90deg); opacity: 0.2; }
    100% { transform: rotateY(0deg); opacity: 1; }
  }

  .virando { animation: virar-pagina 0.6s ease; transform-origin: left center; }

  #texto-pagina h2 { margin-top: 0; }
  #texto-pagina p { margin-top: 5px; color: black; font-size: 17px; line-height: 1.3; }
</style>
</head>
<body>

<!-- Linha conectando as polaroids -->
<svg id="linha-conexao">
  <polyline points="160,180 440,480 720,180 1000,480" 
            stroke="red" stroke-width="8" fill="none" 
            stroke-linejoin="round" stroke-linecap="round" opacity="0.8"/>
</svg>

<!-- Polaroids reposicionadas em zigue-zague -->
<div id="polaroid" style="top:100px; left:100px;">
  <img src="https://riomemorias.com.br/wp-content/uploads/2025/02/Captura-de-Tela-2025-02-10-as-06.41.44-1-791x1024.png" alt="Machado de Assis">
  <p>Machado de Assis</p>
</div>

<div class="polaroid" style="top:400px; left:400px;">
  <img src="https://www.museudapelada.com/storage/2021/10/p1-15.jpg" alt="Pelé">
  <p>Pelé</p>
</div>

<div class="polaroid" style="top:100px; left:700px;">
  <img src="https://images.squarespace-cdn.com/content/v1/625e0a7f20bb3a13976a8f50/4b69a434-beaf-4420-a851-3428c2757f69/Marielle-Franco-2.jpg" alt="Marielle Franco">
  <p>Marielle Franco</p>
</div>

<div class="polaroid" style="top:400px; left:1000px;">
  <img src="https://isabelaboscov.com/wp-content/uploads/2020/08/blogib_chadwick-boseman_feat.jpg" alt="Chadwick Aaron Boseman">
  <p>Chadwick Aaron Boseman</p>
</div>

<!-- Livro / Pasta -->
<div id="livro">
  <div class="navegacao">
    <button id="btn-fechar">×</button>
    <button id="btn-azul" data-pagina="0">Página 1</button>
    <button id="btn-verde" data-pagina="1">Página 2</button>
    <button id="btn-amarelo" data-pagina="2">Página 3</button>
    <button id="btn-roxo" data-pagina="3">Página 4</button>
  </div>

  <div class="conteudo-unificado">
    <div class="lado-esquerdo" id="lado-esquerdo"></div>
    <div class="linha-divisoria"></div>
    <div class="lado-direito" id="lado-direito"></div>
  </div>
</div>

<script>
const polaroids = document.querySelectorAll('#polaroid, .polaroid');
const livro = document.getElementById('livro');
const btnFechar = document.getElementById('btn-fechar');
const ladoEsquerdo = document.getElementById('lado-esquerdo');
const ladoDireito = document.getElementById('lado-direito');
const botoes = document.querySelectorAll('.navegacao button[data-pagina]');
const linhas = document.getElementById('linha-conexao'); // CORRIGIDO: pegar o SVG pelo id

const autores = {
  "Machado de Assis": [
    {
      texto: `<h2>Machado de Assis</h2><p>Machado de Assis foi um dos maiores escritores do Brasil, mesmo tendo nascido pobre e enfrentado muito preconceito. Ele nasceu em 21 de junho de 1839, no Rio de Janeiro, filho de um pintor e de uma lavadeira.
Desde pequeno, gostava de ler e escrever, mas não teve muitas chances de estudar. Mesmo assim, aprendeu praticamente sozinho, lendo muito e observando as pessoas. Começou a trabalhar cedo, foi tipógrafo, revisor e jornalista, e logo passou a escrever suas próprias histórias.
Machado escreveu livros que até hoje são muito famosos, como Dom Casmurro, Memórias Póstumas de Brás Cubas e Quincas Borba. Ele falava sobre a vida, o amor, o ciúme e o comportamento das pessoas, sempre com um jeito inteligente e irônico.
Mesmo sendo negro, gago e epilético, ele nunca deixou nada disso impedir o seu sucesso. Foi respeitado por todos e se tornou o primeiro presidente da Academia Brasileira de Letras. Machado foi casado com Carolina Augusta, seu grande amor e companheira.
Machado de Assis morreu em 29 de setembro de 1908, no Rio de Janeiro.</p>`,
      foto: "https://s2-g1.glbimg.com/AeK06BC6Bo66vJ8ZL_B15H3Yt6M=/0x0:2000x2414/984x0/smart/filters:strip_icc()/i.s3.glbimg.com/v1/AUTH_59edd422c0c84a879bd37670ae4f538a/internal_photos/bs/2018/t/z/dAx8AkRQmNW76SdBkMKw/machado1907estudio.jpg",
      fotoPos: "esquerda"
    },
    {
      texto: `<h2>Machado de Assis (continuação)</h2><p>Até hoje, é lembrado como um gênio da literatura brasileira e um exemplo de superação e inteligência.
Machado de Assis nasceu em 21 de junho de 1839, no Rio de Janeiro, em uma família pobre. Autodidata, aprendeu lendo e observando as pessoas, já que teve pouca oportunidade de estudar. Trabalhou como tipógrafo, revisor e jornalista, até se tornar um dos maiores escritores do Brasil. Autor de obras famosas como Dom Casmurro, Memórias Póstumas de Brás Cubas e Quincas Borba, escreveu sobre temas como amor, ciúme e comportamento humano, com ironia e inteligência. Mesmo enfrentando preconceito por ser negro, gago e epilético, conquistou respeito e se tornou o primeiro presidente da Academia Brasileira de Letras. Casado com Carolina Augusta, morreu em 29 de setembro de 1908, sendo lembrado como um gênio da literatura e exemplo de superação.</p>`,
      foto: "https://riomemorias.com.br/wp-content/uploads/2025/02/Captura-de-Tela-2025-02-10-as-06.41.44-1-791x1024.png",
      fotoPos: "direita"
    }
  ],

  "Pelé": [
    {
      texto: `<h2>Pelé</h2><p>Conhecido mundialmente como Pelé, nasceu em 23 de outubro de 1940, em Três Corações, Minas Gerais. Filho de família humilde, destacou-se desde cedo no futebol, jogando em times amadores até ser descoberto por Waldemar de Brito, que o levou ao Santos Futebol Clube em 1956. Com apenas 16 anos, tornou-se titular e rapidamente virou ídolo da equipe, conquistando diversos títulos nacionais e internacionais, além de se tornar o maior artilheiro da história do clube
Na Seleção Brasileira, Pelé escreveu uma trajetória única, sendo o único jogador tricampeão da Copa do Mundo.
Após se aposentar dos gramados em 1977, atuou como embaixador do esporte, recebeu inúmeros prêmios internacionais e tornou-se ícone global
Pelé faleceu em 29 de dezembro de 2022, deixando um legado eterno.</p>`,
      foto: "https://www.museudapelada.com/storage/2021/10/p1-15.jpg",
      fotoPos: "esquerda"
    },
    {
      texto: `<h2>Pelé (continuação)</h2><p>É amplamente reconhecido como um dos maiores atletas do século e um símbolo do futebol mundo
Pelé, nascido em 23 de outubro de 1940 em Três Corações (MG), destacou-se desde jovem no futebol e foi levado ao Santos em 1956, onde se tornou ídolo e maior artilheiro do clube. Pela Seleção Brasileira, conquistou três Copas do Mundo (1958, 1962 e 1970), sendo o único jogador tricampeão mundial. Depois de encerrar a carreira no New York Cosmos, atuou como embaixador do esporte e recebeu diversos prêmios. Faleceu em 29 de dezembro de 2022, sendo lembrado como um dos maiores atletas da história e símbolo do futebol mundial.
</p>`,
      foto: "https://upload.wikimedia.org/wikipedia/commons/thumb/5/54/Pele_by_John_Mathew_Smith.jpg/250px-Pele_by_John_Mathew_Smith.jpg",
      fotoPos: "direita"
    }
  ],

  "Marielle Franco": [
    {
      texto: `<h2>Marielle Franco</h2><p>nasceu em 27 de julho de 1979, no Rio de Janeiro, em uma das maiores favelas da cidade,. Ela cresceu em um ambiente com muitas dificuldades, cercada por violência, pobreza e desigualdade social. Mesmo assim, Marielle sempre teve muita força, coragem e vontade de mudar a realidade das pessoas que viviam nas favelas.Desde jovem, ela se preocupava com as injustiças que via ao seu redor. Teve que trabalhar cedo para ajudar a família e, com muito esforço, conseguiu estudar. Entrou na faculdade e se formou em Ciências Sociais pela PUC-Rio, graças a uma bolsa de estudos. Depois, fez mestrado em Administração Pública. Nessa época, pesquisou sobre a violência que atinge os jovens nas favelas e a ação da polícia nessas áreas.Antes de entrar para a política, Marielle trabalhou como assistente social e ativista, lutando pelos direitos das mulheres, das pessoas negras, da população LGBTQIA+ e dos moradores de comunidades pobres. Ela também trabalhou na Comissão de Direitos Humanos da Assembleia Legislativa do Rio de Janeiro, onde ajudou a denunciar casos de violência policial e defender pessoas vítimas de abusos.Em 2016, Marielle foi eleita vereadora do Rio de Janeiro. Sua eleição foi histórica: uma mulher negra, nascida em favela, conquistando um dos cargos mais importantes da política municipal. Ela recebeu mais de 46 mil votos, sendo uma das vereadoras mais votadas da cidade.Na Câmara dos Vereadores, Marielle lutou contra o racismo, o machismo, a homofobia e a violência policial. Defendia os direitos humanos e sempre dava voz às pessoas que a sociedade muitas vezes ignorava. Era uma mulher firme, inteligente e corajosa, que acreditava na justiça e na igualdade para todos.Mas, por lutar contra injustiças e denunciar abusos, Marielle também enfrentou muito preconceito e ódio.</p>`,
      foto: "https://images.squarespace-cdn.com/content/v1/625e0a7f20bb3a13976a8f50/4b69a434-beaf-4420-a851-3428c2757f69/Marielle-Franco-2.jpg",
      fotoPos: "esquerda"
    },
    {
      texto: `<h2>Marielle Franco (continuação)</h2><p> Por ser mulher, negra, bissexual e vinda da favela, sofreu discriminação e ameaças, mas nunca deixou que isso a fizesse desistir.Tragicamente, no dia 14 de março de 2018, Marielle Franco foi assassinada a tiros no centro do Rio de Janeiro, junto com seu motorista,. Milhares de pessoas foram às ruas pedindo justiça por Marielle, e até hoje a sociedade cobra a punição dos responsáveis pelo assassinato.A morte de Marielle foi vista como um ataque à democracia e aos direitos humanos, pois ela representava as vozes das minorias e das pessoas que lutam contra a opressão.Mesmo depois de sua morte, Marielle continua viva na memória e na luta de muitos brasileiros. Seu nome virou símbolo de resistência, coragem e esperança. Escolas, projetos sociais e movimentos em todo o país e no mundo levam o seu nome.Marielle Franco mostrou que, mesmo com tantas dificuldades, uma mulher negra e pobre pode transformar o mundo com coragem, estudo e determinação. Ela deixou um legado de amor, luta e justiça, e será sempre lembrada como uma das maiores defensoras dos direitos humanos do Brasil.
</p>`,
      foto: "https://www.justicadesaia.com.br/wp-content/uploads/2019/03/marielle-franco.png",
      fotoPos: "direita"
    }
  ],

  "Chadwick Aaron Boseman": [
    {
      texto: `<h2>Chadwick Boseman</h2>
      <p>Chadwick Boseman (1976–2020) foi um aclamado ator, diretor e roteirista norte-americano, mais conhecido por seu icônico papel como o super-herói Pantera Negra no Universo Cinematográfico Marvel. Ele interpretou T'Challa em quatro filmes: Capitão América: Guerra Civil (2016), Pantera Negra (2018), Vingadores: Guerra Infinita (2018) e Vingadores: Ultimato (2019). Além do Pantera Negra, Boseman teve uma carreira notável, interpretando figuras históricas e inspiradoras, como Jackie Robinson em "42: A História de uma Lenda" (2013), James Brown em "Get on Up" (2014) e Thurgood Marshall em "Marshall: Igualdade e Justiça" (2017).</p>`,
      foto: "https://isabelaboscov.com/wp-content/uploads/2020/08/blogib_chadwick-boseman_feat.jpg",
      fotoPos: "esquerda"
    },
    {
      texto: `<h2>Chadwick Boseman (continuação)</h2>
      <p>Boseman foi diagnosticado com câncer colorretal em estágio III em 2016, mantendo a doença em segredo enquanto continuava atuando. Ele faleceu em 28 de agosto de 2020, aos 43 anos, em Los Angeles. Sua dedicação e talento foram reconhecidos com prêmios como o Globo de Ouro e o SAG Award póstumos. Seu legado permanece vivo, inspirando milhões e aumentando a conscientização sobre o câncer colorretal. Em sua homenagem, a Marvel decidiu não substituir o ator como T’Challa em “Wakanda Para Sempre” (2022), eternizando-o como o verdadeiro Pantera Negra.</p>`,
      foto: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRYJxJ8S---_s3S7H8lyO0ZDkkdxj5RfosWuA&s",
      fotoPos: "direita"
    }
  ]
};

let autorAtual = null;
let paginaAtual = 0;

function renderPagina() {
  if (!autorAtual) return;
  const paginasAutor = autores[autorAtual];
  if (!paginasAutor || !paginasAutor[paginaAtual]) return;
  const { texto, foto, fotoPos } = paginasAutor[paginaAtual];

  if (fotoPos === "esquerda") {
    ladoEsquerdo.innerHTML = `<img class="foto-canto" src="${foto}" alt="Imagem"/>`;
    ladoDireito.innerHTML = `<div id="texto-pagina">${texto}</div>`;
  } else {
    ladoDireito.innerHTML = `<img class="foto-canto" src="${foto}" alt="Imagem"/>`;
    ladoEsquerdo.innerHTML = `<div id="texto-pagina">${texto}</div>`;
  }
}

function abrirLivro(nomeAutor) {
  autorAtual = nomeAutor;
  paginaAtual = 0;

  // esconde polaroids e linhas (transição pela propriedade opacity)
  polaroids.forEach(p => p.style.opacity = 0);
  if (linhas) linhas.style.opacity = 0;

  setTimeout(() => {
    // depois da transição, remove do fluxo visual igual aos polaroids
    polaroids.forEach(p => p.style.display = 'none');
    if (linhas) linhas.style.display = 'none';

    livro.style.pointerEvents = 'auto';
    livro.style.opacity = 1;
    renderPagina();

    // selecionar botão inicial se existir
    const primeiro = document.querySelector('.navegacao button[data-pagina="0"]');
    if (primeiro) selecionarBotao(primeiro);
  }, 500);
}

function fecharLivro() {
  livro.style.opacity = 0;
  livro.style.pointerEvents = 'none';

  setTimeout(() => {
    // mostra polaroids e linhas novamente
    polaroids.forEach(p => p.style.display = 'block');
    if (linhas) linhas.style.display = 'block';

    // pequena pausa para garantir que display mudou, então animar opacidade
    setTimeout(() => {
      polaroids.forEach(p => p.style.opacity = 1);
      if (linhas) linhas.style.opacity = 1;
    }, 10);
  }, 500);
}

function animarVirada(callback) {
  const textoPagina = document.querySelector('#lado-esquerdo #texto-pagina, #lado-direito #texto-pagina');
  if (!textoPagina) return callback();
  textoPagina.classList.add('virando');
  setTimeout(() => {
    callback();
    textoPagina.classList.remove('virando');
  }, 600);
}

function trocarPagina(novaPagina) {
  if (!autorAtual) return;
  const paginasAutor = autores[autorAtual];
  if (!paginasAutor) return;
  if (novaPagina >= paginasAutor.length || novaPagina < 0 || novaPagina === paginaAtual) return;
  animarVirada(() => {
    paginaAtual = novaPagina;
    renderPagina();
  });
}

function selecionarBotao(botao) {
  botoes.forEach(b => b.classList.remove('ativo'));
  if (botao) botao.classList.add('ativo');
}

/* Event listeners */
polaroids.forEach(pol => {
  pol.addEventListener('click', () => {
    const nome = pol.querySelector('p') ? pol.querySelector('p').textContent.trim() : null;
    if (nome) abrirLivro(nome);
  });
});
if (btnFechar) btnFechar.addEventListener('click', fecharLivro);
botoes.forEach(btn => {
  btn.addEventListener('click', () => {
    const pagina = parseInt(btn.dataset.pagina, 10);
    selecionarBotao(btn);
    trocarPagina(pagina);
  });
});
</script>

</body>
</html>

