

### Da funcionalidade
* SC = Servidor de Configuração.
* _Grupos_ são estruturas de dados no SC.
  * Grupos são identificados por nomes.
    * Cada grupo corresponde a:
     * um conjunto de Serviços;
     * Arquivos de Configuração;
     * Script   de instalação; 
       * Chamado quando o cliente não estava no grupo antes.
     * Script   de remoção;
       * Chamado quando o cliente detecta que deixou de estar no grupo.
     * Scripts  de atualização de configuração;
       * São chamados após o final do download de configurações e (possivelmente) os de instalação a menos que a remoção tenha ocorrido.
  * Grupos distintos não devem escrever no mesmo arquivo
    * Por exemplo, não é aceitável um grupo colocar uma configuração num arquivo só para outro tirar.
    * Isso não é garantido pelo servidor ou cliente.
  * EXEMPLO:
    * `kerberos_client`:
      * serviços de cliente de kerberos
      * configurações de kerberos
      * `apt install` ...
      * script que move os arquivos de configuração baixados para os lugares apropriados, reinicia os serviços etc. 
 * Todos os clientes são do grupo `config_client`
   * `config_client` corresponde ao serviço de configuração cliente.
   * Configuração:
     * `config_client.conf`:
       * `GROUPS_FILE_PATH=/some/path/for/groups.file`
       * `REFRESH_RATE=3600000`
       * `SC_IP=0.0.0.0`
     * É o único arquivo de configuração do serviço cliente de configuração. Determina que envie pedido de 
     configurações dos nomes de grupos no arquivo `GROUPS_FILE_PATH` a cada `REFRESH_RATE` ms para a máquina 
     `SC_IP`.
 * O SC é do grupo `config_server`
   * `config_server` corresponde ao serviço do servidor de configuração.
   * Configuração:
     * Diretório de grupos:
       * `groups/`:
         * Arquivos em `json` descrevendo cada grupo.
 * Clientes devem periodicamente baixar suas configurações do SC.
 * O cliente deve ser usado para adicionar ou remover grupos e pedir atualizações.
 * O SC deve ser capaz de coagir os clientes a atualizar suas configurações.
   * De preferência, tanto individualmente como todas juntas.
   * Isto talvez seja difícil. Mas podemos considerar endereços que recentemente pediram atualização como “membros 
    da Rede” para o SC. Ele pode iterar pelas máquinas, rodando o script que requisição de atualização via ssh. 
    Se a máquina não tiver ticket ele vai pedir para o admin entrar com a credencial da root da máquina. Deveria
    funcionar.
 * Clientes devem poder pedir atualizações a qualquer hora.
 * Os arquivos de configurações no SC devem existir em um repositório de git.
   * Isso é bom para que possamos reverter configurações da rede toda.
   * Isso é bom pois podemos isolar erros.
   * Isso é bom pois `git blame`.
   * Isso é bom pois podemos ter dois SCs em branches diferentes e uma máquina apontando para o SC teste. Quando 
    estivermos satisfeitos podemos dar merge nas configurações e a mudança se propaga pela rede. 
   * Na eventualidade de um arquivo ser confidencial, colocar no `.gitignore` com comentário explicando o fim de sua adição lá.
   * Deve-se priorizar manter o repositório atualizado com o local.
