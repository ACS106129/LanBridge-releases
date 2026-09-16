# Registro de alterações

Todas as alterações relevantes do LanBridge ficam registradas aqui.

O formato segue o [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/) e a
numeração segue o [Versionamento Semântico](https://semver.org/lang/pt-BR/).

## [0.5.23] - 2026-09-16

### Added

- **A sessão anota para onde o alvo realmente foi e o que disso não passou pelo túnel.** Medir o
  endereço de saída diz se o túnel leva aquilo que foi roteado por ele; não diz nada sobre se o
  tráfego que importa está roteado — e rotear uma lista de nomes não ajuda em nada com um nome que
  ninguém colocou nela. Essa lacuna era o problema inteiro: o mesmo aplicativo funciona com uma VPN
  para a máquina toda e não com um punhado de rotas, e quais nomes entram nesse punhado era um
  palpite que precisa acertar, senão o recurso não faz nada.

  Então agora se observa. Cada destino que o alvo alcança é registrado uma vez, com a resposta do
  próprio sistema sobre se um pacote para lá sai pelo túnel, e o fim da sessão diz no que deu. Uma
  única execução agora nomeia exatamente o que está faltando.

- **Os endereços são relatados com o nome ao qual respondem.** Numa rede de distribuição o nome do
  nó carrega a localização dele, e a localização é a pergunta: o mesmo host respondeu
  `…nrt57.r.cloudfront.net` através de um túnel japonês e `…tpe53.r.cloudfront.net` a partir de
  Taipé. Narita contra Taipé se vê de relance num nome e não se vê de jeito nenhum numa lista de
  endereços.

### Fixed

- **Os nomes são acompanhados enquanto a sessão dura, em vez de fixados uma vez no início.** Eles
  respondem com TTL de sessenta segundos. Em uma hora de observação nada se moveu, então não era
  isso que estava errado — mas uma sessão dura horas, um nome pode se mover a qualquer minuto, e
  quando isso acontece o aplicativo alcança um endereço que nada roteia, o tráfego sai pelo caminho
  de sempre, e cada rota na tabela continua dizendo "em uso". A falha teria exatamente a cara do
  sucesso.

- **A busca por um resolvedor que responda para assim que um responde.** Ela era feita por nome, e
  num túnel real o primeiro candidato estourava o tempo por UDP e de novo por TCP para cada nome,
  seis segundos cada. Com quatro nomes eram vinte e quatro segundos antes de começar; com os vinte
  e nove que um aplicativo real acabou precisando teriam sido quase três minutos — e a conclusão
  fácil teria sido que a lista longa era inviável, em vez de a busca.

## [0.5.22] - 2026-09-16

### Added

- **O aplicativo agora mede, e deixa registrado, se o endereço de saída mudou de verdade.**
  Mandar o tráfego de um programa por um túnel vale a pena por um único motivo: a outra ponta o
  vê chegando de outro lugar. Todas as verificações que este recurso fazia até agora eram um
  passo em direção a isso e não isso — o comando que adicionou a rota, a rota na tabela com boa
  métrica, o nome resolvido através do túnel — e cada uma delas já foi verdadeira pelo menos uma
  vez enquanto o tráfego saía pelo adaptador de sempre o tempo todo.

  Então a mesma pergunta é feita à internet duas vezes, uma antes de mexer em qualquer rota e
  outra com todas já no lugar, e as duas respostas vão para o registro. "Não deu para saber" é
  escrito assim mesmo, nunca como "não mudou".

### Fixed

- **O registro deixava de fora justamente a metade da sessão para a qual ele serviria.** Os
  passos que a janela mostra — qual endereço de túnel chegou, se o isolamento por processo
  começou, qual processo foi adotado quando o alvo passou o bastão para outro — iam para a
  janela e para mais lugar nenhum. O mesmo valia para tudo o que o filtro de pacotes dizia e
  para tudo o que o próprio OpenVPN dizia. Relendo um registro depois, sobravam as rotas e quase
  nada em volta: já foram duas as vezes em que uma pergunta sobre uma sessão que deu errado não
  pôde ser respondida por ele, inclusive a de se o filtro chegou a iniciar. Agora tudo isso vai
  para o arquivo.

- **A verificação de que uma rota está mesmo sendo usada podia rejeitar uma que estava prestes a
  funcionar.** A 0.5.21 passou a verificar cada rota em vez de confiar no código de saída do comando,
  e a mudança estava certa; só que a pergunta era feita no instante exato em que a rota era
  adicionada. Uma rota que o sistema ainda não olhou é indistinguível de uma que ele recusou, então
  um momento de atraso bastava para jogar fora uma rota que ia funcionar — a verificação derrotando
  aquilo que verifica. Agora a tabela de rotas ganha algumas centenas de milissegundos para assentar.

- **O filtro de pacotes agora registra o filtro com o qual foi aberto.** Quando o programa continuava
  saindo para a internet por IPv6 apesar de estar bloqueado, o registro não permitia distinguir se a
  cláusula estava faltando ou se simplesmente nunca chegou a casar. Agora permite.

## [0.5.21] - 2026-09-14

### Fixed

- **Os sites que deviam ir pela VPN não iam, e tudo dizia que sim.** A 0.5.20 adicionou as
  rotas, relatou "ok" para cada uma e as deixou na tabela com boa métrica. O Windows ignorou
  todas elas.

  O próximo salto estava errado. Um túnel costuma dar um endereço ponto a ponto — este era
  um /30, com apenas quatro endereços — e o salto foi adivinhado como o .1 da rede, que num
  enlace desses não existe. O Windows não usa uma rota cujo próximo salto não alcança, então
  o tráfego saiu pelo adaptador de sempre. E nada disse isso: o comando aceitou a rota e
  devolveu sucesso.

  O próximo salto passa a ser deduzido do endereço que o túnel realmente recebeu. E
  "adicionada" já não significa "funcionando": depois de cada rota pergunta-se ao sistema
  por onde ele mandaria de fato um pacote, e a rota não escolhida é relatada e removida.

- **Um nome que não pôde ser resolvido pelo túnel era relatado como igual ao local.** Sem
  resposta não é a mesma resposta.

- **A consulta pelo túnel agora recorre a TCP.** No túnel onde isto foi encontrado, o
  resolvedor nada respondeu por UDP enquanto a conexão à mesma porta funcionava.

## [0.5.20] - 2026-09-13

### Added

- **Sites que você manda pela VPN sem mandar a máquina inteira.** Até agora, um programa que
  precisava que um site o *visse* chegando do outro lado — em vez de precisar alcançar uma
  máquina lá — só tinha uma opção: entregar tudo.

  Dê os nomes dos sites e seus endereços são resolvidos e roteados pelo túnel. Resolvê-los
  pelo túnel é o que importa: uma rede de conteúdo responde conforme de onde veio a
  pergunta, e um destes nomes respondeu com um nó de Taipé daqui e com endereços diferentes
  duas horas depois.

  O registro diz o que encontrou para cada nome, dos dois lados, concordem ou não.

  Duas coisas antes de ligar: muda a tabela de rotas da máquina, então vem vazio; e durante
  a sessão esses sites só são alcançados pelo aplicativo de destino. Ao parar, cada rota
  adicionada é removida.

## [0.5.19] - 2026-09-13

### Added

- **Um lugar para usuário e senha.** Alguns servidores pedem login e não havia onde informar.
  Um `auth-user-pass` sem arquivo depois significa "pergunte no console", e aqui o openvpn
  sobe sem janela e com a saída redirecionada: faz uma pergunta que ninguém ouve e depois
  relata falha de autenticação. Gerenciar perfis agora tem um botão por perfil.

  A senha fica sem criptografia e o diálogo diz isso em vez de sugerir o contrário. Está na
  pasta daquele perfil, que só você, o SYSTEM e os Administradores podem abrir, e nunca é
  lida de volta para exibição.

### Fixed

- **O aplicativo de destino saía para a internet por IPv6, contornando o túnel.** Descoberto
  observando uma sessão real: quatro minutos, quatro destinos, um deles por IPv6. Esta
  máquina tem endereço IPv6 global do provedor e o túnel é IPv4.

  Era um vazamento em todos os modos, inclusive o que entrega a máquina inteira à VPN. O IPv6
  do destino agora é descartado durante a sessão; o dos demais não é tocado.

- **"Atualizado" sem ter perguntado.** Com o limite horário esgotado, a verificação informava
  a versão que você já tinha como se tivesse olhado. Agora diz que não conseguiu verificar e
  quando tentará de novo.

## [0.5.18] - 2026-09-13

### Fixed

- **A janela Sobre agradecia ao WinDivert sem dizer sob quais termos ele é usado.** É a GNU
  LGPL v3, que pede ao programa que o diga, nomeie a licença e aponte para a cópia que
  distribui; um agradecimento não é nenhuma das três. Agora diz as três. Também fica claro
  que o OpenVPN é baixado de openvpn.net em vez de distribuído aqui.
- **As vagas livres de uma sala de Warcraft III nunca mudavam na outra máquina.** Abra um
  lugar onde havia um computador e o outro continuava vendo a sala como estava, até sair da
  lista de partidas e voltar.

  Quem já tem a sala na lista não relê o anúncio completo. Pega os números de um pequeno
  pacote que o anfitrião transmite sempre que a sala muda, e só refaz a entrada quando a
  lista é reaberta. Esse pacote é transmitido em difusão, e a difusão é justamente o que
  não dá para capturar aqui: o jogo já ocupa a porta em que seria preciso escutar. Por isso
  ele agora é derivado do anúncio e enviado quando os números mudam.

  Os números são conferidos antes: são lidos de uma posição fixa no fim de um pacote cujo
  layout foi deduzido, e uma partida tem de uma a vinte e quatro vagas e não pode ter mais
  livres do que possui. Qualquer outra coisa significa leitura errada, e aí nada é enviado.

- **O download da atualização ainda prendia a janela.** O 0.5.16 dizia ter corrigido isso.
  A transferência em segundo plano, a barra de progresso e o botão de cancelar estavam
  escritos e nada os chamava.

  Agora acontece de fato em segundo plano, e ao começar o diálogo oferece **Continuar em
  segundo plano**: a janela fecha, a transferência segue e reporta na barra da janela
  principal, onde também pode ser cancelada. Cancelar e falhar agora se distinguem — antes
  os dois abriam a página da versão no navegador.

## [0.5.17] - 2026-09-13

### Fixed

- **Um jogador sair de uma sala de Warcraft III fechava-a para todos.** Relatado assim:
  ponha a vaga de alguém em computador, aberta ou fechada e ele nunca mais consegue entrar.
  São três maneiras de derrubar a conexão dele, e o jogador sair por conta própria faz o
  mesmo.

  Um socket em escuta e cada conexão aceita nele compartilham uma porta local. O registro
  de quais portas são do jogo era por porta, então a escuta e as conexões dividiam uma
  entrada, e a primeira conexão a fechar levava-a embora. O Warcraft continua escutando e
  continua se anunciando, de modo que a sala permanece na lista de todos — mas cada pacote
  que chega naquela porta já não é de ninguém aos olhos do filtro, e é descartado. Visível
  e impossível de entrar, para todos, até o anfitrião criar outra partida.

  Agora cada socket é registrado separadamente, e uma porta deixa de ser do jogo quando o
  último fecha, não o primeiro.

- **O aplicativo de destino ainda era executado como administrador.** O 0.5.16 dizia ter
  corrigido isso e não tinha. Dar a um processo a identidade do usuário conectado pode ser
  feito de duas formas, que pedem permissões diferentes: a usada exige um privilégio que um
  administrador elevado não tem nem pode obter, então falhava sempre e o comportamento
  antigo assumia em silêncio. Agora usa aquela cuja permissão o auxiliar de fato possui.

## [0.5.16] - 2026-09-13

### Adicionado

- **Um instalador em cada idioma que o aplicativo fala.** Ele falava onze e o instalador
  falava dois. Agora são onze, cada um com a página de código ANSI correta.
- **A janela abre onde você a deixou.** Tamanho, posição e se estava maximizada. Guarda-se
  o tamanho restaurado, e uma posição que não cai mais em nenhuma tela é descartada.
- **Um ícone novo.** O anterior era uma barra com dois pontos e não dizia nada sobre o que
  isto faz. Agora é uma seta saindo pela abertura de um anel: o túnel, e o único aplicativo
  que passa por ele. Desenhado separadamente em cada tamanho. O anel é aberto do lado por
  onde a seta sai, porque um fechado com um traço é o sinal de proibido.

### Corrigido

- **Iniciar ficava abaixo da dobra.** Os dois botões eram a última coisa na coluna de
  cartões de configuração, e essa coluna rola. Assim que os cartões bastavam para
  preenchê-la — e a uma altura de janela comum bastam — a ação principal do aplicativo
  passava a ser algo que era preciso rolar para achar. Agora os botões ficam fixos abaixo
  dos cartões, e são os cartões que rolam atrás deles.
- **O aplicativo alvo rodava como administrador.** O auxiliar que o inicia precisa ser, e
  um processo filho herda o token do pai. Um programa elevado fica isolado da área de
  trabalho não elevada — é assim que um jogo que faz login pelo navegador nunca recebe seu
  código de autorização. Agora ele é iniciado com o token do shell, como você.
- **Baixar uma atualização travava a janela inteira.** Agora acontece em segundo plano, com
  o progresso numa barra da janela principal.

## [0.5.15] - 2026-09-13

### Corrigido

- **Uma única recusa do servidor encerrava a tentativa.** O openvpn trata um login
  recusado como fatal e sai na primeira, o que está certo para um servidor seu e errado
  para um relé público: esses recusam porque estão cheios ou porque quem o mantinha sumiu,
  e o mesmo perfil conecta um minuto depois. Agora ele tenta de novo e para na terceira,
  para que uma senha realmente errada ainda seja relatada.
- **"EXITING auth-failure" não explicava nada.** Parece senha errada, e depois de um
  certificado já ter sido aceito normalmente não é. A mensagem agora diz qual etapa falhou
  e o que isso significa.

## [0.5.14] - 2026-09-12

### Adicionado

- **Um perfil importado agora pertence ao aplicativo.** Antes se guardava onde o arquivo
  estava e se lia de novo a cada execução, o que funciona até o arquivo mudar de lugar, o
  pendrive sair ou a pasta Downloads ser esvaziada. Agora ele é copiado para uma pasta
  própria, junto com cada certificado e chave a que se refere, e essas referências são
  reescritas para as cópias.
- **Um lugar para ver o que está guardado.** Um botão Gerenciar ao lado de Importar: o que
  está armazenado, qual está em uso, renomear, excluir e abrir a pasta.
- **OpenVPN, se você não tiver.** Este aplicativo conduz o cliente comunitário do OpenVPN;
  não o contém. Agora ele avisa antes de começar e oferece buscar a versão atual no
  próprio servidor da OpenVPN e instalar, recusando qualquer coisa que o Windows não
  aceite ou que não esteja assinada pela OpenVPN.
- **Testes para o instalador.** O que há dentro do pacote e um percurso pelas suas páginas
  nos dois idiomas. Ele para no resumo e cancela: rodar a suíte não instala nada.
- **Um teste que olha os pixels.** Cada linha de explicação é fotografada nos dois temas e
  medida contra o que está atrás dela.

### Corrigido

- **Três linhas do Sobre estavam invisíveis.** Eram pintadas com um pincel tirado dos
  recursos do aplicativo, que resolve pelo tema do próprio aplicativo — e um aplicativo
  WinUI sem empacotamento não pode mudá-lo depois de iniciar, enquanto as caixas de
  diálogo são desenhadas no tema que você escolheu.
- **A verificação de atualizações parou de bater na porta.** Sessenta por hora é
  exatamente o limite sem autenticação. Agora ela lê quando a cota volta e espera.

- **O instalador escrevia por cima da própria arte.** Esses bitmaps não são imagens ao lado
  do texto: são o fundo em que a caixa de diálogo escreve, na cor escura dela, e é ela quem
  escolhe onde. Preencher os 493 pixels com um degradê azul deixava cada título escuro sobre
  escuro. Agora a arte é uma faixa à esquerda e um bloco à direita do banner.
- **A atualização oferecia a todos o instalador em inglês.** Uma versão traz um MSI por
  idioma e o atualizador pegava o primeiro da lista, ou seja, o que foi enviado antes.
  Agora ele pede o que corresponde ao idioma da janela, e cai para o inglês quando aquele
  idioma não tem instalador próprio.

### Alterado

- Cada configuração tem abaixo uma linha dizendo o que muda e onde fica guardada.
- O Sobre explica como as atualizações são procuradas e credita OpenVPN e WinDivert.

## [0.5.13] - 2026-09-12

### Adicionado

- **Som.** O WinUI traz um sistema de som em cada controle — foco, invocação, caixas de
  diálogo abrindo e fechando — e fica calado a menos que o aplicativo peça. Este nunca
  pediu, então cada toque foi silencioso por omissão e não por escolha. Agora é escolha, é
  espacial, e há uma caixa nas configurações para quem prefere um utilitário calado.
- **Movimento onde algo aconteceu.** As duas colunas surgem enquanto a janela se monta, o
  texto de status sobe ao mudar, a contagem retransmitida dá um salto ao subir, a luz de
  status respira durante uma sessão, e uma linha de aviso empurra as vizinhas em vez de
  aparecer do nada.
- **O desenho relata em vez de encenar.** Ele fazia a mesma animação acontecendo algo ou
  não, o que é decoração vestida de instrumento. Sem sessão fica apagado e parado, com ela
  se move, e a célula bloqueada mostra quantos pacotes o guardião de fato descartou — um
  número que a interface nunca havia recebido, porque ninguém assinava o evento que o leva.
- **Um instalador com a cara deste produto**, com imagens geradas em vez das de espaço
  reservado do WiX.
- **Um instalador no seu idioma**: um MSI por idioma em vez de inglês para todos. Inglês e
  chinês tradicional para começar.

### Alterado

- O aviso do confinamento por processo aparece quando a caixa está **desmarcada**, que é o
  estado que merece aviso, e diz o que esse estado significa em vez de repetir o rótulo.

## [0.5.12] - 2026-09-12

### Corrigido

- **A pasta de instalação tinha oitenta e oito pastas de traduções para idiomas que este
  aplicativo não oferece** — af-ZA, sl-SI, fil-PH e as demais. São os textos do próprio
  Windows App SDK, distribuídos como recursos Win32 e não como assemblies satélite do
  .NET, então a configuração usual para removê-los não os alcança. Agora ficam só as
  correspondentes a um idioma que a interface fala: quinze em vez de oitenta e oito.

### Alterado

- A opção por processo se chama *Só o aplicativo alvo pode falar com a VPN*, que é o que
  ela faz. Nunca governou o túnel inteiro.

### Adicionado

- **Mais dez verificações que operam a janela de verdade**, sobre as caixas de diálogo e o
  que há nelas. Escrevê-las encontrou duas coisas sozinha: uma caixa de diálogo aqui não é
  uma janela nem se chama como parecia, então quatro testes procuravam algo que nunca
  existiu; e Iniciar fica indisponível sem um perfil, em vez de aceitar o clique e não
  fazer nada.

## [0.5.11] - 2026-09-12

### Corrigido

- **O modo escuro era texto branco em página branca.** A tentativa anterior pintava o fundo
  em um elemento e aplicava o tema ao de dentro, então o texto se resolvia no tema escuro e
  a superfície atrás dele no claro. O tema agora vai no elemento que pinta o fundo, onde os
  dois concordam.
- **O ícone da seção do alvo era desenhado como um quadrado vazio.** Um ponto de código
  estar no mapa de caracteres de uma fonte não significa que a fonte tenha um glifo para
  ele. As três marcas de seção agora são emoji.
- **Os controles de cada seção ficavam centralizados** em um cartão que já tinha a largura
  certa. Um expansor estica a si mesmo, não o seu conteúdo.
- **Escolher anexar e apertar Iniciar sem escolher um programa lançava uma exceção.** A
  verificação criada para um executável ausente não cobre o modo de anexar, ao qual falta
  outra coisa. Agora ela diz o que falta, e a mensagem não pede mais para digitar um
  identificador de processo em um controle que é uma lista.

### Adicionado

- **Testes que operam a janela de verdade.** Todos os defeitos visuais relatados até aqui
  passariam por qualquer teste de unidade do projeto, porque nenhum trata do que um método
  devolve. Dezenove verificações agora abrem a compilação publicada e olham.

## [0.5.10] - 2026-09-12

### Corrigido

- **A linha de aviso alargava a coluna em vez de quebrar.** Uma pilha horizontal mede seus
  filhos com largura ilimitada, então um bloco de texto com quebra dentro dela nunca quebra
  — ele alarga tudo ao lado, inclusive o botão acima. As duas notas agora ficam em uma
  grade que dá ao texto uma largura de verdade.
- **As configurações e as informações do aplicativo apareciam duas vezes.** O par que ficava
  ao lado dos cartões de status permaneceu quando elas subiram para o canto superior
  direito.
- **O seletor de processos oferecia o próprio aplicativo.** Anexar o túnel à janela que o
  configura não é a intenção de ninguém.

### Alterado

- **O desenho ocupa o espaço que recebeu.** As rotas se esticam com a janela em vez de ficar
  com largura fixa no canto de um cartão grande e vazio, e os pacotes percorrem essa largura
  inteira.
- **A nota do confinamento por processo só aparece enquanto ele está ligado**, com a mesma
  marca de aviso da de roteamento, e diz o que ele faz: os pacotes de todos os outros
  programas daqui são parados.
- O caminho do programa alvo aparece por inteiro ao passar o ponteiro, por mais estreita que
  seja a caixa.

## [0.5.9] - 2026-09-12

### Corrigido

- **O modo escuro era inutilizável.** A página nunca pintava um fundo próprio, então o texto
  acompanhava o tema e a superfície atrás dele não — texto claro sobre fundo claro. As
  caixas de diálogo também mantinham a aparência do sistema, porque uma caixa é hospedada
  pela raiz da janela e não pelo elemento em que o tema foi aplicado, e precisava ser
  avisada à parte.

### Adicionado

- **Os dois interruptores de confinamento agora são um desenho.** Uma grade de quem envia
  contra para onde, com tráfego percorrendo cada rota: pelo túnel, pela saída de sempre, ou
  parado. As quatro células são todas as combinações das duas configurações, e respondem de
  relance o que dois parágrafos de texto não conseguiam.
- **Anexar escolhe de uma lista de programas em execução** em vez de pedir um identificador
  de processo procurado em outro lugar, com um botão para atualizar.

### Alterado

- O aviso de roteamento só aparece enquanto essa opção está ligada, diz uma coisa só, e diz
  na cor de um aviso.
- As seções perderam a numeração; nunca foram passos a seguir em ordem.
- As configurações e as informações do aplicativo foram para o canto superior direito, com
  os cartões de status logo abaixo.

## [0.5.8] - 2026-09-12

### Corrigido

- **Limitar o túnel a um único aplicativo só funcionava em um sentido.** Descartava o tráfego
  que outros programas daqui mandavam para a VPN e não fazia nada com o que vinha dela —
  então todos os outros programas desta máquina continuavam alcançáveis do outro lado, que é
  a metade que importa quando não se sabe quem está lá. Agora vale nos dois sentidos, e a
  explicação diz isso em vez de prometer mais do que fazia.

### Alterado

- **A opção de roteamento está agora ao contrário.** Manter o resto da máquina fora do túnel
  é o estado seguro e o que quase todo mundo quer, então não deveria ser preciso ligá-lo. A
  caixa agora diz *Enviar todo o tráfego pela VPN*, vem desligada e explica o que ligá-la
  significa: tudo o que esta máquina envia passa primeiro pelo servidor VPN, então quem o
  administra vê tudo. Isso importa sobretudo com um perfil que outra pessoa lhe deu.

### Adicionado

- **Aparência clara e escura**, ou acompanhar o sistema, aplicada na hora e lembrada. Em
  Aparência, nas configurações.
- **Ícones pela interface** — em cada seção, em Iniciar e Parar e nas ações do registro — e
  uma luz de status verde enquanto houver uma sessão em andamento.

## [0.5.7] - 2026-09-12

### Corrigido

- **Não dava para cancelar um download.** O botão dizia *Cancelar* e não podia ser
  pressionado: o download rodava segurando o adiamento de clique da caixa de diálogo, e uma
  caixa com um adiamento pendente desativa os próprios botões — inclusive o único que
  poderia tê-lo parado. Agora a transferência corre ao lado da caixa em vez de dentro do
  manipulador de clique, então o botão fica ativo exatamente enquanto houver algo a
  cancelar.
- **Um download cancelado ou com falha deixava seu arquivo parcial**, um por tentativa,
  para sempre. O arquivo incompleto agora é descartado quando a transferência não termina, e
  um download concluído limpa os instaladores anteriores.
- **Apertar Iniciar sem nada para iniciar não fazia absolutamente nada**: nenhuma mensagem,
  nenhuma linha de registro, nenhuma mudança. Sem perfil, ou sem aplicativo escolhido, agora
  diz qual está faltando em vez de parecer quebrado.

### Alterado

- **Instalar uma atualização com uma sessão em andamento agora avisa antes**, e a resposta
  segura é a padrão. Instalar para o túnel e desconecta o aplicativo alvo, o que não é algo
  para se descobrir depois.

## [0.5.6] - 2026-09-12

### Corrigido

- **Uma versão nova passa a ser notada cerca de um minuto depois de publicada**, em vez de
  na próxima verificação agendada. Perguntar com essa frequência é viável porque o pedido é
  condicional: o validador da resposta anterior é devolvido e, enquanto a versão não muda, a
  resposta é "não modificado" — sem corpo e sem contar para o limite de pedidos. Só uma
  versão realmente nova custa um pedido. Isto continua a ser consulta e não aviso, então é
  um minuto e não um instante, mas não há nada para apertar nem para reiniciar.
- **Dispensar o aviso de atualização não deixava caminho de volta.** Fechá-lo valia para a
  sessão inteira e só um reinício o trazia de volta. Agora tanto a caixa de informações
  quanto as configurações oferecem *Atualizar agora* enquanto houver uma esperando, então
  dispensar o aviso dispensa apenas o aviso.
- **O botão dizia *Parar* quando já não havia nada para parar.** Quando o aplicativo alvo
  sai, a sessão espera até vinte segundos para ver se um inicializador passa o bastão a
  outro processo — e nesse intervalo aquilo por que a sessão existe já está morto. Nessa
  janela o botão diz *Forçar parada*, que é o que ele faz: encerrar a sessão agora em vez de
  esperar a passagem.

### Alterado

- **Tudo o que diz respeito a atualizar está agora na caixa de informações do
  aplicativo**, e o botão dela leva um marcador enquanto houver uma esperando. A
  verificação automática, verificar agora, quando foi a última verificação e a própria
  atualização ficam ao lado da versão com que são comparadas, em vez de divididos entre ali
  e as configurações.
- **O instalador baixado é mantido quando você escolhe *Mais tarde*.** Antes, não instalar
  na hora jogava fora o download; agora o mesmo botão oferece *Instalar agora* até que a
  versão a que ele pertence seja superada.

## [0.5.5] - 2026-09-12

### Corrigido

- **As verificações automáticas de atualização eram raras demais para parecerem
  automáticas.** Quatro horas entre elas significava que, na prática, só um reinício
  encontrava algo, o que deixava um botão nas configurações como mecanismo real — e
  ninguém quer apertar um botão para ouvir que não há nada novo. Agora a verificação ocorre
  a cada trinta minutos, e trazer a janela para a frente também verifica quando a última
  foi há mais de cinco minutos. As configurações mostram quando foi a última, para que se
  veja que ela acontece.
- **As duas opções de confinamento pareciam duplicadas.** Ambas eram redigidas como limitar
  o túnel, sem dizer que limitam coisas diferentes. Cada rótulo agora nomeia o próprio eixo
  — *Só endereços da VPN passam pelo túnel* contra *Só o aplicativo alvo pode usar o túnel*
  — e cada explicação começa dizendo a que pergunta responde: quais destinos, ou qual
  programa.

### Alterado

- **O botão do aviso se chama *Atualizar agora***, não *Novidades*. O que ele faz é
  instalar a atualização; mostrar as notas é o que acontece no caminho.
- **As configurações podem iniciar uma atualização**, não apenas procurar por uma.
- **A faixa vazia no topo da janela acabou.** As configurações e as informações do
  aplicativo desceram para junto dos cartões de status, que era tudo o que havia lá em
  cima.
- **A contagem de anúncios retransmitidos aparece só com o Warcraft III.** É o único
  protocolo cujas informações de partida precisam ser pedidas e encaminhadas; nos demais o
  contador ficaria em zero para sempre, o que se lê como defeito e não como "não se
  aplica".

## [0.5.4] - 2026-09-12

### Corrigido

- **A janela de atualização mostrava as notas como código Markdown** — cerquilhas,
  asteriscos e crases — em vez de formatá-las, o que tornava penoso ler justamente o que
  foi escrito para ser lido. Títulos, marcadores, ênfase e código embutido agora são
  formatados.
- **A atualização não fechava o aplicativo antes.** O instalador era iniciado enquanto o
  túnel e o auxiliar com privilégios ainda seguravam os arquivos que ele iria substituir.
  Agora a sessão é encerrada e este processo termina antes de o instalador rodar, e o
  instalador encerra uma instância remanescente em vez de deixá-la transformar uma
  atualização em um pedido de reinicialização.
- **A janela e a barra de tarefas mantinham um ícone genérico** enquanto a área de
  notificação e Adicionar ou remover programas mostravam o real. Uma janela sem empacotar
  não pega o ícone do executável sozinha.
- **O rótulo em chinês de "Manter o resto da máquina fora do túnel" descrevia a
  configuração errada.** Ele se lia como "manter o tráfego de outros aplicativos fora do
  túnel", que é o que faz o confinamento por aplicativo, deixando as duas opções parecendo
  duplicadas. Elas são ortogonais: uma limita quais destinos usam o túnel, a outra qual
  processo pode usá-lo.

### Alterado

- **O nome e o lema não ocupam mais o topo da janela.** A barra de título já diz o que isto
  é, e os detalhes foram para a caixa de informações do aplicativo.
- **A caixa de informações não repete mais o nome que a intitula** e deixa abrir a pasta de
  logs para o painel do registro, onde esse botão já ficava. A versão, que é o motivo de
  abri-la, agora aparece grande o bastante para ler de relance.

## [0.5.3] - 2026-09-12

### Corrigido

- **Uma partida hospedada em uma máquina era vista da outra, mas não dava para entrar.**
  Só a porta de descoberta era aberta de entrada, e ela não é necessariamente a porta em que
  o anfitrião escuta: o Warcraft III pega a 6112 quando pode e sobe até a 6119 quando não
  pode, anunciando a que conseguiu. Um anfitrião empurrado para fora da 6112 ficava visível
  e inalcançável — e só nesse sentido, o que fazia parecer problema de uma das máquinas.
  Agora toda a faixa de hospedagem é aberta, ainda assim apenas para a sub-rede da VPN.
- **Uma partida continuava na lista do outro jogador depois que o anfitrião saiu dela.**
  O Warcraft III anuncia o fechamento por difusão, e uma difusão pode sair pelo adaptador
  da VPN, onde o retransmissor deliberadamente não escuta — então o anúncio nunca era
  captado e a outra ponta seguia oferecendo uma partida que já não existia. Agora o
  retransmissor percebe que o anfitrião parou de responder às sondagens e retira a partida
  ele mesmo, usando o último anúncio que encaminhou para dizer qual era.
- **Trocar de idioma esvaziava as listas do aplicativo alvo e da descoberta na rede**, e
  escolher *Padrão do sistema* esvaziava a própria lista de idiomas. Retraduzir uma lista
  significa substituir os itens dentro dela, e uma lista suspensa trata a substituição do
  item selecionado como o desaparecimento dele: limpava a seleção, e a associação gravava
  esse vazio por cima da escolha. Agora os itens mantêm sua identidade e só o texto muda,
  então não sobra nada para limpar. Duas tentativas anteriores devolviam a seleção depois;
  esta remove a causa.
- **A janela de configurações mantinha o idioma antigo no próprio título e botão** quando o
  idioma era trocado de dentro dela. Todo o conteúdo da janela era reetiquetado, mas o
  título e o botão de fechar não fazem parte desse conteúdo.
- **O registro de atividade não acompanhava as linhas novas de forma confiável.** Ele rolava
  antes de a linha nova ter sido disposta, indo para onde o fim ficava antes e permanecendo
  sempre uma linha atrás. Agora rola depois da disposição e para de acompanhar assim que
  você sobe para ler algo, retomando quando você volta ao fim.
- **Atualizar reescrevia todos os arquivos, tendo mudado ou não.** A versão anterior era
  removida por inteiro antes de um único arquivo novo ser escrito, então cada atualização
  reescrevia a instalação toda. Agora a versão nova é escrita primeiro e a anterior removida
  depois, o que deixa o instalador pular os arquivos idênticos e sobra para escrever apenas
  o que de fato mudou.
- **Um dos botões do registro ficava posicionado e clicável, mas nunca era desenhado.**
  *Abrir a pasta de logs* ocupava seu espaço e respondia aos cliques sem mostrar nada. As
  ações do registro agora ficam em uma única fileira horizontal em vez de uma coluna cada,
  o que elimina a disposição por coluna que estava falhando.

### Adicionado

- **Um botão de informações do aplicativo** ao lado do de configurações: qual versão está
  em execução, os direitos autorais e um link para as notas e downloads dessa versão.
- **Uma caixa *Iniciar o LanBridge* na última página do instalador**, marcada por padrão.
  Ela inicia o aplicativo sem elevação, que é como o LanBridge deve rodar: o consentimento é
  pedido quando uma sessão começa, não antes.

### Alterado

- **Fechar o aplicativo alvo agora encerra a sessão apenas quando o túnel está atrelado a
  ele.** Com *Só este aplicativo pode usar a VPN* ligado, o túnel existe para aquele
  processo e cai junto com ele — caso contrário sobraria um túnel que nada na máquina tem
  permissão de usar. Sem essa opção, o túnel está limitado apenas por destino e pode ainda
  estar levando tráfego de outra coisa, então permanece até você parar.

## [0.5.2] - 2026-09-11

### Corrigido

- **As notas da versão na janela de atualização mostravam apenas o primeiro título.** Ao
  serem extraídas do registro de alterações, as notas são normalizadas para quebras de
  linha simples, e um controle de texto do Windows quebra linhas no retorno de carro —
  então tudo depois da primeira linha nunca era desenhado. O corpo de uma versão em inglês
  já vem com retornos de carro, e por isso só as notas traduzidas pareciam vazias. Agora
  elas são convertidas antes de aparecer e são mostradas em um bloco rolável e
  selecionável.

## [0.5.1] - 2026-09-11

### Corrigido

- **As partidas apareciam mas não dava para entrar, ou não apareciam de jeito nenhum.**
  Tudo o que torna uma partida acessível chega *de entrada* pelo túnel, e o Windows
  bloqueia isso tudo por padrão: o anúncio que o retransmissor do colega encaminha é UDP
  de entrada, e entrar na partida é uma conexão TCP de entrada. A versão de linha de
  comando abria os dois; o aplicativo nunca fez isso, então a máquina que não tivesse
  sobrado nenhuma regra dela ficava inalcançável em um dos sentidos ou nos dois. Agora
  cada sessão abre a porta de descoberta apenas para a sub-rede da VPN — não para todas as
  redes a que a máquina está ligada —, tira o adaptador do túnel da categoria *pública*
  que o Windows lhe atribui, e desfaz as duas coisas ao terminar.
- **A interface se desmontava com qualquer tamanho de texto acima do padrão, e reiniciar
  não trazia nada de volta.** A camada ampliada era centralizada primeiro e só então
  crescia a partir do próprio canto superior esquerdo, de modo que tudo começava mais
  abaixo e mais à direita do que devia e saía pelas bordas inferior e direita — levando
  junto o botão de configurações. Como o tamanho do texto é lembrado, cada reinicialização
  caía no mesmo estado, sem caminho até o ajuste que o causou.
- **Baixar uma atualização não mostrava progresso algum.** A janela fechava assim que
  *Baixar* era pressionado e a transferência acontecia sem nada na tela, o que é
  indistinguível de um download que nunca começou. As notas da versão agora continuam
  abertas, com barra de progresso, a quantidade transferida e um *Cancelar* que funciona.
- **Religar a verificação automática de atualizações não fazia nada por até quatro
  horas.** A verificação em segundo plano só reconsiderava a configuração na próxima
  execução agendada; agora ela olha na hora.
- **A escolha do que fazer ao fechar a janela parecia ter sido descartada** quando o idioma
  era trocado na mesma visita às configurações. Traduzir a lista substitui o item
  selecionado, o que limpa a seleção — as outras listas se restauram sozinhas, esta não.

### Adicionado

- **As verificações de atualização também rodam com o aplicativo na área de notificação**,
  a cada quatro horas em vez de apenas na inicialização. Uma versão nova é anunciada com um
  balão na área de notificação, e a dica do ícone continua indicando isso depois que o
  balão some.
- **Um botão *Verificar agora* nas configurações**, para quando esperar a próxima
  verificação agendada não é o objetivo.

## [0.5.0] - 2026-09-11

### Corrigido

- **A VPN caía assim que uma partida era aberta.** A busca pelo processo para o qual um
  inicializador passa o bastão só comparava executáveis com o mesmo nome, então um jogo que
  continua com outro nome nunca era encontrado e o túnel caía. Agora conta qualquer processo
  que ainda esteja rodando da mesma pasta de instalação, e a busca registra o que procurou.
- **Parar não fazia nada depois que a sessão já havia terminado.** Fechar o canal para o
  processo auxiliar lançava um erro quando a outra ponta já não existia, e esse erro escapava
  da limpeza — o aplicativo continuava achando que uma sessão encerrada ainda estava em
  andamento.
- **Trocar de idioma esvaziava todas as listas suspensas** em vez de traduzi-las. Substituir
  o conteúdo de uma lista limpa a seleção, e a associação gravava essa seleção vazia de volta.
- **O registro de atividade não acompanhava as linhas novas.** Agora rola até a mais recente
  e para de acompanhar assim que você sobe para ler algo.

### Adicionado

- **Notas da versão no seu idioma.** Os registros de alterações traduzidos são publicados
  junto com as compilações, e a caixa de atualização mostra a que corresponde ao idioma da
  interface.
- **Exportar relatório de erros**: um botão que aparece com um marcador assim que algo falha.
  Ele junta a atividade da tela com os arquivos de log dos dois processos, para que um
  problema possa ser relatado sem saber onde ficam os logs.
- A configuração de tamanho do texto agora dimensiona toda a interface, não apenas o registro.

## [0.4.0] - 2026-09-10

### Adicionado

- **Verificação automática de atualizações.** O aplicativo consulta se há uma versão mais
  recente no GitHub e mostra o que mudou antes de instalar. Ativada por padrão; pode ser
  desligada nas configurações.
- **Caixa de configurações** atrás do botão de engrenagem, com idioma, tamanho do texto da
  interface, comportamento ao fechar e verificação de atualizações — para que uma escolha
  lembrada nunca vire um beco sem saída.
- **Mais dez idiomas**: chinês tradicional, chinês simplificado, japonês, coreano,
  espanhol, francês, alemão, russo e italiano, além de inglês e português. Status, listas
  suspensas, caixas de diálogo e o menu da área de notificação estão traduzidos.
- **O registro de atividade agora é texto selecionável**, com os botões *Copiar tudo* e
  *Exportar…*.
- **Tamanho do texto da interface ajustável** (10 a 22 pt), mantido entre execuções.
- **Ícone do aplicativo**, usado pela janela, pela barra de tarefas, pela área de
  notificação e por Adicionar ou remover programas.
- **O instalador pergunta onde instalar** e oferece o atalho do menu Iniciar e o da área de
  trabalho como opções independentes.

### Corrigido

- **A VPN caía exatamente quando o jogo terminava de carregar.** O Warcraft III, como a
  maioria dos títulos com um inicializador ou atualizador, encerra seu primeiro processo e
  passa o bastão para outro. Esse encerramento era tratado como "o alvo fechou" e o túnel
  era desmontado no pior momento possível. Agora a sessão acompanha o aplicativo através
  dessa passagem.
- **O ícone da área de notificação nunca aparecia.** O identificador do ícone era destruído
  antes de o Windows usá-lo, e o `Shell_NotifyIcon` com um identificador destruído não
  mostra nada nem informa erro algum.
- **Reabrir pela área de notificação iniciava uma segunda cópia** em vez de restaurar a que
  já estava em execução. Agora roda apenas uma instância por usuário, e abrir de novo traz
  a janela existente para a frente.
- **O `WinDivert64.sys` continuava bloqueado depois de fechar o aplicativo.** Fechar os
  identificadores do driver não basta: abrir um registra um serviço de kernel que continua
  em execução, e o arquivo fica bloqueado até que ele pare. Agora o serviço é parado e
  removido no fim da sessão. Fechar a janela também encerra o processo auxiliar com
  privilégios, o que antes não acontecia.
- **Voltar o idioma para "Padrão do sistema" não fazia nada.** A mudança era anunciada com
  uma única notificação de "tudo mudou", à qual o WinUI não reage de forma confiável; agora
  cada texto é anunciado pelo nome.
- **A desinstalação pedia reinicialização.** O instalador fecha primeiro o aplicativo e seu
  auxiliar, então não sobram arquivos em uso.
- As linhas do registro tinham o preenchimento de um item de lista, deixando meia linha em
  branco entre as entradas.

### Alterado

- As ações de registro foram para o painel de atividade à direita, em vez de ficarem no pé
  da coluna de configuração.

## [0.3.0] - 2026-09-09

### Adicionado

- Registro de erros em `%LOCALAPPDATA%\LanBridge\logs\`, um arquivo por processo e por
  execução, com exceções não tratadas capturadas em três pontos e registradas em vez de
  derrubar o aplicativo em silêncio.
- Persistência das configurações: gravadas a cada mudança, sobrevivem a uma falha ou a um
  encerramento forçado.
- Suporte à área de notificação, com uma pergunta ao fechar entre sair e minimizar.
- Interface em chinês tradicional além do inglês.
- Instalador MSI com atalhos, informações de versão e entrada em Adicionar ou remover
  programas.

### Corrigido

- A versão publicada iniciava e morria em seguida dentro do runtime XAML: publicar um
  aplicativo WinUI sem empacotamento não leva junto sua marcação compilada, então o
  `InitializeComponent` não encontrava nada para carregar.

## [0.2.0] - 2026-09-09

### Corrigido

- **A janela nunca aparecia depois do aviso de elevação.** O WinUI 3 não roda com
  privilégios elevados: a ativação WinRT falha e o processo termina sem exibir nada. Agora
  a interface roda sem privilégios e entrega o trabalho privilegiado a um processo auxiliar
  separado, que pede consentimento uma vez por sessão. A interface não detém privilégio
  algum, e o auxiliar só os mantém enquanto há uma sessão em andamento.

## [0.1.0] - 2026-09-09

### Adicionado

- VPN por aplicativo: importa um perfil OpenVPN e recusa a rota padrão e o DNS enviados
  pelo servidor, de modo que apenas a sub-rede da VPN atravessa o túnel e o restante da
  máquina mantém seu caminho normal.
- Confinamento opcional por processo via WinDivert, descartando o tráfego para a sub-rede
  da VPN vindo de qualquer processo que não seja o alvo.
- Retransmissão de descoberta na LAN para túneis que não carregam broadcast, incluindo o
  protocolo W3GS do Warcraft III, cujas informações de partida só são enviadas como
  resposta unicast e nunca por broadcast, precisando ser solicitadas ao jogo local e
  encaminhadas.
- Retransmissão genérica de broadcast UDP para outros jogos, configurada por porta.
- Versão de linha de comando do mesmo motor.

[0.5.13]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.13
[0.5.12]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.12
[0.5.11]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.11
[0.5.10]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.10
[0.5.9]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.9
[0.5.8]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.8
[0.5.7]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.7
[0.5.6]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.6
[0.5.5]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.5
[0.5.4]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.4
[0.5.3]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.3
[0.5.2]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.2
[0.5.1]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.1
[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0
