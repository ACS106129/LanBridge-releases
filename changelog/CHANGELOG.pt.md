# Registro de alterações

Todas as alterações relevantes do LanBridge ficam registradas aqui.

O formato segue o [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/) e a
numeração segue o [Versionamento Semântico](https://semver.org/lang/pt-BR/).

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
