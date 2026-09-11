# Registro de alterações

Todas as alterações relevantes do LanBridge ficam registradas aqui.

O formato segue o [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/) e a
numeração segue o [Versionamento Semântico](https://semver.org/lang/pt-BR/).

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

[0.5.2]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.2
[0.5.1]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.1
[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0
