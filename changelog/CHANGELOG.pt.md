# Registo de alterações

Todas as alterações relevantes do LanBridge são registadas aqui.

O formato segue [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/) e as versões
seguem o [versionamento semântico](https://semver.org/lang/pt-BR/).

## [Não lançado]

### Corrigido

- Uma verificação recusada já não aparece como atualizada.
- Perfis sem uso podem ser apagados durante a conexão.
- O modo de espera já não diz que está a abrir.
- Os sites não listados já não saem do túnel.
- As linhas repetidas a intervalos regulares ficam no registo.

## [0.5.29] - 2026-09-19

### Adicionado

- Baixar uma VPN pelo VPN Gate.
- Alterar a lista de sites conectado.

### Alterado

- No túnel só os sites do destino.
- A atualização traz um único runtime.

### Corrigido

- O rastreio sobrevive à reconexão.
- As verificações de atualização não são mais recusadas.

## [0.5.28] - 2026-09-19

### Corrigido

- **A atualização é 38,8 MB mais pequena**: a pilha de aprendizagem automática do Windows App SDK, onnxruntime e DirectML, seguia dentro de uma VPN por aplicação que nunca a chama.
- **Uma publicação deixa de ser visível até todos os seus instaladores estarem anexados**, razão pela qual a 0.5.27 ofereceu um instalador alemão a uma interface chinesa: foi publicada com um ficheiro carregado e os restantes ainda a subir, e a aplicação deteta uma versão nova em cerca de um minuto.

## [0.5.27] - 2026-09-19

### Corrigido

- **A aplicação alvo já não alcança nada a não ser através do túnel**: o primeiro pacote de uma ligação a um endereço que nenhuma rota cobre é descartado em vez de sair com o endereço real desta máquina, que era o que causava os 403 repetidos e o ecrã de carregamento preso.
- **Descartar esse pacote é o que acrescenta a rota**, pelo que a ligação vinga na primeira retransmissão em vez de falhar.
- **Um pacote IPv6 bloqueado já não é contado nem registado como fuga**, nem na linha do momento nem no veredicto final, que descrevia uma execução medida como «22 de 73 destinos não passaram pelo túnel» quando os 51 de IPv4 passaram e os 22 eram o guarda a funcionar como previsto.
- **Um túnel que se liga de novo com outro endereço é acompanhado**, e se o novo ficar fora da sub-rede à volta da qual o filtro de pacotes foi construído, a recusa para e di-lo, em vez de transformar em recusa cada pacote que o alvo envia.

### Alterado

- **As rotas voltam a sair do túnel**: um endereço é libertado quando nenhum nome seguido o devolve e o alvo não tem ligações abertas para ele, em vez de o conjunto crescer durante toda a sessão.
- **Uma rota adotada expira ao fim de cinco minutos sem uso** e devolve o seu lugar no limite da sessão.

## [0.5.26] - 2026-09-18

### Alterado

- **A lista de sites já não tem de estar certa**: é apenas um arranque a quente, e tudo o que o alvo alcança sem o túnel recebe uma rota própria, exceto o servidor VPN, as redes desta máquina, a difusão e o IPv6.
- **O resumo final já não chama perdido a um destino depois de o ter encaminhado**, porque volta a perguntar ao sistema em vez de reler as rotas adicionadas.

## [0.5.25] - 2026-09-17

### Corrigido

- **Os sites só passavam pelo túnel quando os dois lados coincidiam por acaso no endereço**: dezoito de vinte e sete nomes respondiam de forma diferente, só a resposta do túnel era encaminhada e a aplicação usava a local; agora encaminham-se ambas.
- **Os sites editam-se um por linha, numa janela própria**, em vez de uma caixa única com vinte e sete nomes.
- **Uma transferência cortada pelo servidor recomeça onde parou** em vez de ser deitada fora, pedindo cada parte até cinco vezes.
- **A 0.5.24 culpou disso um tempo-limite e enganou-se**: a falha já durava trinta minutos, pelo que um limite de quinze não pode tê-la terminado.

## [0.5.24] - 2026-09-17

### Corrigido

- **Uma transferência lenta era deitada fora mesmo antes de acabar**, porque o limite de quinze minutos cobria a leitura do ficheiro e não apenas chegar ao servidor; já não há limite global.
- **As atualizações transferem cerca de três vezes mais depressa**, porque o servidor de publicação limita cada ligação e não a linha: o instalador é obtido em quatro partes ao mesmo tempo.
- **O progresso é comunicado a cada 512 KB em vez de cada 80 KB**, um ritmo que a janela consegue aproveitar.

## [0.5.23] - 2026-09-16

### Adicionado

- **A sessão anota para onde o alvo foi realmente e o que disso falhou o túnel**, de modo que uma execução nomeia o que falta em vez de deixar a lista de nomes em suposições.
- **Os endereços são comunicados com o nome a que respondem**, porque o nome de um nó de uma rede de distribuição traz o local, e o local é toda a questão.

### Corrigido

- **Os nomes configurados são seguidos durante toda a sessão**, em vez de fixados uma vez no início, já que respondem com um TTL de sessenta segundos e uma sessão dura horas.
- **A procura de um resolvedor que funcione para assim que um responde**, sem esgotar tempos nome a nome e custar minutos antes de começar.

## [0.5.22] - 2026-09-16

### Adicionado

- **O endereço de saída é medido antes e depois de colocadas as rotas, e ambas as respostas vão para o registo**, porque toda a verificação anterior era um passo nessa direção e não isso mesmo — e «não foi possível saber» é escrito tal como é.

### Corrigido

- **O registo guarda enfim a metade da sessão para a qual seria preciso**: os passos da janela, cada mensagem do filtro de pacotes e tudo o que o OpenVPN disse vão para o ficheiro.
- **Uma rota já não é rejeitada um instante antes de funcionar**; dão-se algumas centenas de milissegundos à tabela de encaminhamento para assentar.
- **O filtro de pacotes anota o filtro com que foi aberto**, para distinguir uma cláusula ausente de uma que nunca correspondeu.

## [0.5.21] - 2026-09-14

### Corrigido

- **Os sites enviados pela VPN não iam por ela enquanto tudo dizia que sim**: o salto seguinte era adivinhado como o .1 da rede, que numa /30 não existe; passa a ser deduzido do endereço que o túnel obteve e cada rota é verificada como aquela que o sistema usaria mesmo.
- **Um nome que não se conseguia resolver pelo túnel era comunicado como coincidente com a resposta local**; nenhuma resposta não é a mesma resposta, e o registo diz qual o resolvedor e o transporte.
- **A consulta pelo túnel recorre a TCP**, porque um relé que transporta um e não o outro é comum em servidores voluntários.

## [0.5.20] - 2026-09-13

### Adicionado

- **Sites que pode enviar pela VPN sem lá enviar a máquina inteira**: os sites indicados são resolvidos pelo túnel e encaminhados por ele, e durante a sessão só a aplicação alvo lhes chega.

## [0.5.19] - 2026-09-13

### Adicionado

- **Um sítio para o nome de utilizador e a palavra-passe**, para servidores que pedem autenticação; a caixa de diálogo diz claramente que o openvpn só a consegue ler de um ficheiro, pelo que fica sem cifra na pasta própria desse perfil.

### Corrigido

- **O alvo alcançava a Internet por IPv6, contornando o túnel por completo**: uma fuga em todos os modos, pois um túnel que transporta IPv4 não pode transportar o que a máquina envia por IPv6; o IPv6 do alvo passa a ser descartado.
- **«Sem atualizações» quando nada tinha sido perguntado**: com a quota esgotada devolvia a versão já instalada, e o validador que torna a verificação gratuita passa a ser guardado entre execuções.

## [0.5.18] - 2026-09-13

### Corrigido

- **A caixa Acerca agradecia ao WinDivert sem dizer sob que termos é usado**, e passa a nomear a LGPL v3, a cópia distribuída junto ao programa e onde está o código.
- **Os lugares livres de uma sala de Warcraft III nunca mudavam na outra máquina**, porque o aviso que o par lê é difusão e não se consegue captar; passa a ser derivado do anúncio e verificado antes de ser enviado.
- **A transferência da atualização continuava a prender a janela**: a 0.5.16 dava isto por resolvido enquanto nada chamava aquele código; agora corre mesmo em segundo plano, com **Continuar em segundo plano** na caixa de diálogo e cancelamento na janela principal.

## [0.5.17] - 2026-09-13

### Corrigido

- **A saída de um jogador fechava a sala de Warcraft III para todos**, porque o ouvinte e cada ligação aceite partilhavam um único registo de porta e o primeiro fecho levava-o; agora cada socket é seguido à parte.
- **A aplicação alvo continuava a correr como administrador**: o caminho da 0.5.16 exigia um privilégio que um auxiliar elevado não pode ter, pelo que se usa o que ele tem e o registo diz qual.

## [0.5.16] - 2026-09-13

### Adicionado

- **Um instalador em cada idioma que a aplicação fala**, cada um com a página de códigos ANSI da sua cultura, e a atualização oferece o que corresponde ao idioma da janela.
- **A janela abre onde a deixou**, exceto se essa posição já não cair num ecrã.
- **Um ícone novo**: uma seta a sair pela abertura de um anel, desenhado separadamente em cada tamanho em vez de reduzido a partir de uma imagem grande.

### Corrigido

- **Iniciar ficava abaixo da dobra**; os botões estão agora fixos sob os cartões, que deslizam por trás deles.
- **A aplicação alvo corria como administrador**, pelo que uma autenticação no navegador nunca lhe conseguia entregar o código de autorização; passa a arrancar com o token da shell.
- **Transferir uma atualização tomava a janela inteira como refém**, e agora corre em segundo plano com barra de progresso e um botão de cancelar que funciona.

## [0.5.15] - 2026-09-13

### Corrigido

- **Uma única recusa do servidor terminava a tentativa**, o que é correto num servidor próprio e errado num relé público; passa a tentar três vezes antes de comunicar.
- **«EXITING auth-failure» não explicava nada** e passa a dizer que passo falhou e o que isso significa para o tipo de servidor que usa.

## [0.5.14] - 2026-09-12

### Adicionado

- **Um perfil importado passa a pertencer à aplicação**: o .ovpn e cada certificado e chave a que se refere são copiados para uma pasta própria, pelo que apagar o original não muda nada.
- **Um sítio para ver o que está guardado**, com mudar o nome, apagar e um acesso à pasta, confirmando dentro da própria linha.
- **O OpenVPN, se não o tiver**, obtido do servidor de transferências do próprio OpenVPN e recusando tudo em que o Windows não confie ou que o OpenVPN não tenha assinado.
- **Testes para o instalador**, que percorrem as suas páginas nos dois idiomas e cancelam no resumo, pelo que correr a suite não instala nada.
- **Um teste que olha para os píxeis**, medindo cada linha explicativa das Definições e do Acerca contra o seu fundo, nos dois temas.

### Corrigido

- **Três linhas do Acerca eram invisíveis**, porque um pincel vindo dos recursos da aplicação resolve-se com o tema da própria aplicação, que uma aplicação WinUI não empacotada não pode mudar depois de arrancar.
- **A verificação de atualizações deixou de pedir com educação**: passa a ler quando a quota regressa e espera, e di-lo uma vez em vez de sessenta.
- **O instalador escrevia por cima da sua própria arte**, que é o fundo sobre o qual a caixa de diálogo escreve e não uma imagem ao lado do texto.
- **A atualização dava a toda a gente o instalador inglês**, e passa a pedir o que corresponde ao idioma da janela.

### Alterado

- Sob cada definição há uma linha a dizer o que muda e onde as definições são guardadas.
- O Acerca diz de quanto em quanto tempo se procuram atualizações e credita o cliente comunitário do OpenVPN e o WinDivert.

## [0.5.13] - 2026-09-12

### Adicionado

- **Um piano de cauda**: os sons dos controlos passam pelo sintetizador General MIDI que o Windows já tem, numa escala pentatónica para que duas notas quaisquer combinem, com uma caixa para o calar.
- **Movimento onde algo aconteceu**, e não em todo o lado.
- **O diagrama informa em vez de mimar**: parado sem sessão, e a célula dos bloqueados mostra os pacotes que o guarda descartou mesmo.
- **Um instalador com a cara deste produto**, com arte gerada em vez dos marcadores do WiX.
- **Um instalador no seu idioma**, um MSI por idioma, para começar inglês e chinês tradicional.

### Alterado

- O aviso sob o confinamento por processo aparece agora quando a caixa está **vazia**, que é o estado sobre o qual vale a pena avisar.

## [0.5.12] - 2026-09-12

### Corrigido

- **A pasta de instalação tinha oitenta e oito pastas de traduções de idiomas que esta aplicação não oferece**, incluídas como recursos Win32 que a habitual definição de corte não alcança; ficam quinze.

### Alterado

- A opção por processo chama-se *Só a aplicação alvo pode falar com a VPN*, que é o que faz; nunca governou o túnel inteiro.

### Adicionado

- **Mais dez verificações que conduzem a janela real**, sobre as caixas de diálogo e as escolhas lá dentro — e escrevê-las encontrou quatro testes à procura de algo que nunca existiu.

## [0.5.11] - 2026-09-12

### Corrigido

- **O modo escuro era texto branco em página branca**, porque o tema era aplicado a um elemento dentro daquele que pinta o fundo.
- **O ícone da secção do alvo aparecia como um quadrado vazio**, pois um ponto de código no mapa de caracteres de um tipo de letra não significa que exista glifo.
- **Os controlos de cada secção ficavam centrados** num cartão que já tinha a largura certa.
- **Escolher anexar e carregar em iniciar sem escolher programa lançava uma exceção**, e passa a dizer que escolha falta.

### Adicionado

- **Testes que conduzem a janela real**: dezanove verificações abrem a compilação publicada e olham, porque todos os defeitos visuais comunicados até agora passavam todos os testes unitários do projeto.

## [0.5.10] - 2026-09-12

### Corrigido

- **A linha de aviso alargava a coluna em vez de mudar de linha**, porque uma pilha horizontal mede os filhos com largura ilimitada.
- **As definições e a informação da aplicação apareciam duas vezes**, ficando o par antigo para trás quando passaram para o canto superior direito.
- **O seletor de processos oferecia esta própria aplicação.**

### Alterado

- **O diagrama enche o espaço que lhe é dado**, em vez de ficar com largura fixa no canto de um cartão grande e vazio.
- **A nota sob o confinamento por processo só aparece enquanto está ativo**, e diz o que faz.
- O caminho do alvo mostra-se por inteiro ao passar o rato, por mais estreita que seja a caixa.

## [0.5.9] - 2026-09-12

### Corrigido

- **O modo escuro era inutilizável**, porque a página nunca pintava um fundo próprio e às caixas de diálogo o tema tinha de ser indicado à parte.

### Adicionado

- **Os dois interruptores de confinamento são agora uma imagem**, uma grelha de quem envia contra para onde, que responde num relance ao que dois parágrafos não conseguiam dizer.
- **Anexar escolhe de uma lista de programas em execução**, em vez de pedir um identificador de processo copiado de outro sítio.

### Alterado

- O aviso de encaminhamento aparece só enquanto essa opção está ativa, diz uma coisa e di-la na cor de um aviso.
- As secções perderam a numeração; nunca foram passos a seguir por ordem.
- Definições e informação da aplicação passaram para o canto superior direito, com os cartões de estado por baixo.

## [0.5.8] - 2026-09-12

### Corrigido

- **Confinar o túnel a uma aplicação só funcionava num sentido**, deixando todos os outros programas daqui alcançáveis do outro lado, que é a metade que conta quando não se sabe quem lá está.

### Alterado

- **A opção de encaminhamento está agora ao contrário**: *Enviar todo o tráfego pela VPN*, desligada por omissão, dizendo o que ligá-la significa para quem gere aquele servidor.

### Adicionado

- **Aspeto claro e escuro**, ou a seguir o sistema, aplicado de imediato e memorizado.
- **Ícones por toda a interface** e uma luz de estado verde enquanto há sessão.

## [0.5.7] - 2026-09-12

### Corrigido

- **Uma transferência não podia ser cancelada**, porque corria segurando o adiamento do clique da caixa de diálogo, o que a faz desativar os seus próprios botões.
- **Uma transferência cancelada ou falhada deixava o ficheiro parcial**, um por tentativa, para sempre.
- **Carregar em Iniciar sem nada para iniciar não fazia absolutamente nada**, e passa a dizer que escolha falta.

### Alterado

- **Instalar uma atualização com uma sessão a decorrer avisa primeiro**, com a resposta segura por omissão, já que instalar desliga a aplicação alvo.

## [0.5.6] - 2026-09-12

### Corrigido

- **Uma nova versão é notada cerca de um minuto depois de publicada**, com um pedido condicional que não custa nada enquanto nada muda.
- **Dispensar o aviso de atualização não deixava forma de voltar a ele**; as definições e a caixa de informação oferecem agora *Atualizar agora* enquanto uma espera.
- **Parar dizia *Parar* quando já não havia nada para parar**, e diz *Forçar paragem* durante a espera pela passagem de testemunho.

### Alterado

- **Tudo o que diz respeito a atualizar está na caixa de informação da aplicação**, ao lado da versão com que é comparada.
- **Um instalador transferido é guardado se escolher *Mais tarde***, até a sua versão ser ultrapassada.

## [0.5.5] - 2026-09-12

### Corrigido

- **As verificações automáticas eram demasiado raras para parecerem automáticas**: agora de trinta em trinta minutos, e também ao trazer a janela para a frente se a última tiver mais de cinco minutos.
- **As duas opções de confinamento liam-se como duplicados**, e cada etiqueta nomeia agora o seu eixo: que destinos, ou que programa.

### Alterado

- **O botão da faixa chama-se *Atualizar agora*** em vez de *Novidades*, porque instalar é o que faz.
- **As definições também podem iniciar uma atualização**, não só procurá-la.
- **A faixa vazia no topo da janela desapareceu.**
- **A contagem de anúncios retransmitidos só se mostra no Warcraft III**, o único protocolo a que se aplica.

## [0.5.4] - 2026-09-12

### Corrigido

- **A janela de atualização mostrava as notas como código Markdown** em vez de as formatar, tornando penoso ler o que foi escrito para ser lido.
- **Atualizar não fechava a aplicação primeiro**, pelo que o instalador substituía ficheiros ainda seguros pelo túnel e pelo auxiliar.
- **A janela e a barra de tarefas mantinham um ícone genérico**, porque uma janela não empacotada não tira o ícone do executável por si.
- **A etiqueta chinesa de «manter o resto da máquina fora do túnel» descrevia a outra definição**, fazendo duas opções ortogonais parecerem duplicados.

### Alterado

- **O nome e o lema já não ocupam o topo da janela**; a barra de título já diz o que isto é.
- **A caixa de informação já não repete o nome do seu próprio título**, e mostra a versão grande o bastante para se ler num relance.

## [0.5.3] - 2026-09-12

### Corrigido

- **Um jogo alojado numa máquina via-se mas não se conseguia entrar a partir da outra**, porque só a porta de deteção estava aberta enquanto o Warcraft III sobe até 6119 se a 6112 estiver ocupada; agora abre-se o intervalo todo, sempre só à sub-rede VPN.
- **Uma sala ficava na lista do outro jogador depois de o anfitrião a deixar**, porque o aviso de fecho é difusão e nunca chega; o relé nota que o anfitrião deixou de responder e retira-a ele próprio.
- **Mudar de idioma esvaziava as listas da aplicação alvo e da deteção LAN**, porque substituir a entrada selecionada lê-se como o seu desaparecimento; as entradas mantêm agora a sua identidade e só muda o texto.
- **A janela de definições mantinha o idioma antigo no título e no botão**, que não fazem parte do conteúdo reetiquetado.
- **O registo de atividade não seguia de forma fiável as linhas novas**, porque deslizava antes de a nova linha estar disposta.
- **Uma atualização reescrevia todos os ficheiros, alterados ou não**, porque a versão antiga era removida por inteiro antes de escrito um único ficheiro novo.
- **Um dos botões do registo estava disposto e clicável mas nunca era desenhado.**

### Adicionado

- **Um botão de informação da aplicação** ao lado do das definições, com a versão, os direitos de autor e uma ligação às notas dessa versão.
- **Uma caixa *Iniciar o LanBridge* na última página do instalador**, que o arranca sem elevação, como é suposto funcionar.

### Alterado

- **Fechar a aplicação alvo só termina a sessão quando o túnel lhe está ligado**, já que de outro modo poderia ainda transportar tráfego de outra coisa.

## [0.5.2] - 2026-09-11

### Corrigido

- **As notas de versão mostravam só o primeiro título**, porque o controlo quebra linhas num retorno de carro que as notas normalizadas já não traziam.

## [0.5.1] - 2026-09-11

### Corrigido

- **Os jogos viam-se mas não se conseguia entrar, ou não apareciam de todo**: tudo o que torna um jogo acessível chega de entrada e o Windows bloqueia-o por omissão, pelo que uma sessão abre a porta de deteção só para a sub-rede VPN e desfaz tudo no fim.
- **A interface desfazia-se em qualquer tamanho de texto acima do normal, e reiniciar nunca a repunha**, porque a camada escalada era primeiro centrada e depois ampliada a partir do seu próprio canto superior esquerdo.
- **Transferir uma atualização não mostrava progresso nenhum**, indistinguível de uma transferência que nunca começou.
- **Voltar a ligar as verificações automáticas não fazia nada durante até quatro horas.**
- **A escolha de fecho parecia descartada** se o idioma mudasse na mesma visita às definições.

### Adicionado

- **As verificações continuam enquanto a aplicação está na área de notificação**, anunciadas com um balão e mantidas na dica do ícone.
- **Um botão *Verificar agora* nas definições**, para quando esperar pela próxima não é o ponto.

## [0.5.0] - 2026-09-11

### Corrigido

- **A VPN caía assim que se abria uma sala**, porque a procura do processo a que um lançador passa o testemunho só comparava executáveis com o mesmo nome.
- **Parar não fazia nada com a sessão já terminada**, porque fechar o canal para um auxiliar desaparecido lançava uma exceção que escapava à limpeza.
- **Mudar de idioma esvaziava todas as listas pendentes** em vez de as traduzir.
- **O registo de atividade não seguia as linhas novas**, e agora deixa de seguir assim que se desliza para cima.

### Adicionado

- **Notas de versão no seu idioma**, publicadas junto às compilações e mostradas conforme o idioma da interface.
- **Exportar relatório de erro**, que junta a atividade no ecrã aos registos de ambos os processos.
- A definição de tamanho de texto escala agora toda a interface, não só o registo.

## [0.4.0] - 2026-09-10

### Adicionado

- **Verificação automática de atualizações**, mostrando o que mudou antes de instalar; ligada por omissão.
- **Caixa de definições** atrás do botão da roda dentada, com idioma, tamanho de letra, comportamento ao fechar e verificação de atualizações.
- **Mais dez idiomas de interface**, ao lado do inglês e do chinês tradicional.
- **O registo de atividade é texto selecionável**, com *Copiar tudo* e *Exportar…*.
- **Tamanho de letra ajustável** (10–22 pt), memorizado entre execuções.
- **Ícone da aplicação**, usado pela janela, barra de tarefas, área de notificação e lista de programas.
- **O instalador pergunta onde instalar** e oferece os dois atalhos como escolhas independentes.

### Corrigido

- **A VPN caía assim que o jogo acabava de carregar**, porque a saída do primeiro processo do lançador era lida como o fecho do alvo; a sessão passa a seguir a passagem.
- **O ícone da bandeja nunca aparecia**, porque o seu identificador era destruído antes de o Windows o usar.
- **Reabrir da bandeja iniciava uma segunda cópia** em vez de repor a que estava a correr.
- **O `WinDivert64.sys` ficava bloqueado depois de fechar**, porque abrir um identificador do controlador regista um serviço de núcleo que continua a correr.
- **Repor o idioma em «Predefinição do sistema» não fazia nada**, porque uma única notificação de «mudou tudo» não é tratada de forma fiável.
- **Desinstalar pedia reinício**, já que a aplicação e o auxiliar ainda seguravam ficheiros.
- As linhas do registo tinham um preenchimento de item de lista que deixava meia linha em branco entre entradas.

### Alterado

- As ações do registo passaram para o painel de atividade à direita, em vez do fundo da coluna de configuração.

## [0.3.0] - 2026-09-09

### Adicionado

- Registo de erros em `%LOCALAPPDATA%\LanBridge\logs\`, um ficheiro por processo e por execução, com as exceções não tratadas anotadas em vez de terminar em silêncio.
- Persistência das definições a cada alteração, de modo a sobreviverem a um bloqueio.
- Suporte da área de notificação com pergunta ao fechar.
- Interface em chinês tradicional ao lado do inglês.
- Instalador MSI com atalhos, metadados de versão e entrada na lista de programas.

### Corrigido

- A compilação publicada arrancava e morria dentro do motor XAML, porque uma aplicação WinUI não empacotada não leva consigo a sua marcação compilada.

## [0.2.0] - 2026-09-09

### Corrigido

- **A janela nunca aparecia depois do pedido de elevação**, porque o WinUI 3 não pode correr elevado; a interface corre sem elevação e entrega o trabalho privilegiado a um auxiliar que pede consentimento uma vez por sessão.

## [0.1.0] - 2026-09-09

### Adicionado

- VPN por aplicação que recusa a rota predefinida e o DNS empurrados, de modo que só a sub-rede VPN atravessa o túnel.
- Confinamento opcional por processo com WinDivert, descartando o tráfego para a sub-rede VPN de qualquer processo que não o alvo.
- Relé de deteção LAN para túneis que não transportam difusão, incluindo o protocolo W3GS do Warcraft III, cuja informação de jogo só é enviada como resposta unicast.
- Relé genérico de difusão UDP para outros jogos, configurado por porta.
- Versão de linha de comandos do mesmo motor, sem interface.

[0.5.27]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.27
[0.5.26]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.26
[0.5.25]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.25
[0.5.24]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.24
[0.5.23]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.23
[0.5.22]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.22
[0.5.21]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.21
[0.5.20]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.20
[0.5.19]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.19
[0.5.18]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.18
[0.5.17]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.17
[0.5.16]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.16
[0.5.15]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.15
[0.5.14]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.14
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
