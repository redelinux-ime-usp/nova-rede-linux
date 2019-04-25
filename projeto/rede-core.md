# Rede Core:
 * DHCP
 * DNS e DNS Externo
 * SSH
 * WEB SERVER
 * EMAIL
 * AUTENTICAÇÃO
 * SERVIDOR de CONFIGURAÇÃO
 * BANCO de DADOS
 * STORAGE e NFS
 * FIREWALL e GATEWAY
 * DADOS de USUÁRIOS
 * SERVIDOR ADMIN
 * IMPRESSORA

## Justificativas

### DHCP
Precisamos de DHCP para fixar os endereços IPv4 das máquinas, servidores, containers _etc._ na Rede.

### DNS
Precisamos de ─ pelo menos ─ um _domain name server_ para resolver o nosso endereço para o mundo nos 
encontrar. Um DNS interno pode ser desnecessário se tivermos um outro que a gente não se importe em 
confiar.

### SSH
Precisamos de uma entrada SSH para nos conectar remotamente e permitir que os usuários façam o 
mesmo. Os tipos de uso que esperamos do SSH serão descritos posteriormente a fim de determinar que 
tipo de setup será necessário.

### WEB SERVER
Precisamos de um servidor web pois, além de já hospedamos sites de usuários e o BCC costuma pedir 
que os alunos hospedem uma página para o TCC, além do mais, há professores que tem páginas na rede, 
e a nossa própria página precisa ser hospedada em algum lugar.

### EMAIL
A rede tem disponibilizado emails há algum tempo, não há muitos que usem. Mas há usuários que de 
fato usam. Acredito que devemos continuar oferecendo este serviço, afinal admin@linux.ime.usp.br é o
nosso email de contato. Atualmente não usamos muito disco com isso, uns 300Gb, acho que não é um
grande sacrifício continuar com ele. Como estamos especificando o _bare bones_ da rede, acho que 
podemos dar prioridade baixa para este serviço.

### AUTENTICAÇÃO
Os usuários precisam se autenticar de alguma forma para entrar na rede. Algo como o Kerberos, que 
emite tickets para os usuários e estes tickets são enviados como autenticação seria necessário para
ser possível montar os arquivos deles nas homes quando logarem, _etc_.

### SERVIDOR de CONFIGURAÇÕES
Atualmente, algumas configurações são localmente configuradas, outras vivem no puppet, que por vezes
morre ou não manda coisas para as máquinas, outras, ainda, ficam escondidas sabe-se lá onde. Apesar 
do modo atual ser sub-ótimo, algo semelhante é necessário. Precisamos, sim, de uma forma organizada
de configurar os nossos serviços, certificados, etc.

Para sugestões, veja o arquivo de [sugestões](configuration-server.md) que deixei linkado.

### BANCO de DADOS
Precisamos de um banco de dados dos nossos usuários. Acho que um banco de dados _para todo mundo_ 
que nem fazemos hoje em dia é meio overkill. Estamos especificando o _bare bones_ da rede, não acho 
que convenha fazer um servidor para todo mundo, inclusive porque fica mais fácil de migrar os nossos 
dados se só for o nosso, que é mais importante. 

Um banco de dados para os usuários pode aparecer, mas acho que não seria no mesmo lugar e seria 
secundário em prioridade.

### STORAGE e NFS
Obviamente necessário, né. Temos que montar os arquivos dos usuários. Talvez outra coisa que não 
NFS, mas temos que pesquisar.

### FIREWALL e GATEWAY
Já temos os dois via PFSENSE na Kaiser. É necessário para expôr a Rede ao mundo, sem expôr demais.

### DADOS de USUÁRIOS
Algo para o mesmo fim que usamos LDAP, para guardar os dados do usuário: 
 * shell que ele usa
 * nome 
 * nid 
 * uid
 * quota de impressão
 * email de contanto
 * possivelmente _etc._

### SERVIDOR ADMIN
Acho bom ter uma `moira`, que fica lá com os scripts de administração mais comuns, sipá um FAQ e um 
guia atualizado da Rede, acho legal também colocar serviços de auditoria tipo `audictd` acho que se 
chama.

### IMPRESSORA
Muitos usuários dependem da quota da rede para imprimir seus documentos e temos uma impressora para
este fim. Precisamos de algo como o CUPS e Pykota para lidar com impressão de coisas.