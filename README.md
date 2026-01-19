# Crontab - Noções básicas de uso e funcionamento. <br>

## _Sistema utilizado - Proxmox 8.1.3_

### __1 - Entendendo o crontab. <br>__

### __O que é o crontab?__ <br>

O crontab é uma ferramenta nativa de sistemas Unix que também está presente em sistemas MacOs e serve como uma agenda de rotinas para o sistemas. Com ele é possivel criar automatizações de tarefas repetidas pelo usuário no que diz respeito à gerenciamento do sistema, como backup, sincronização de arquivos entre sistemas, limpeza de logs, envio de e-mails e ativações/desligamentos de maquinas virtuais ou containes e muito mais. <br>

### __Como funciona o crontab?__ <br>

O crontab dentro de um sistema roda como um serviço, na maioria dos sistemas chamado de **cron** e roda em segundo plano durante todo o funcionamento da maquina, verificando a cada minuto se há alguma tarefa designada para aquele minuto. A verificação é feita dentro de um arquivo de texto simples que pode ser editado a principio por qualquer usuário, cada um tendo seu próprio arquivo. Apenas o root ou alguem com privilégios de adm pode acessar crontabs de outros usuários; todavia é possivel criar restrições e regras para gerenciar quem pode fazer o que no sistema. <br> 

### __Como saber se tenho permissão?__ 
Basta um simples comando. Digitando `crontab -l` você consegue verificar se tem permissão para editar o seu crontab. Se a resposta for `no crontab for [usuario]` ou abrir um editor de texto, então você tem permissão. Já se retornar `you (usuario) are not allowed to use this program (crontab)` então você não tem permissão. <br>

Para gerenciar quem tem permissão, é necessario usar um super-usuario para acessar os arquivos de definição. Existem 2 desses arquivos, o `cron.allow` e o `cron.deny`, ambos localizados em __/etc/__, sendo usados para adicionar usuários à uma lista de permitidos e adicionar usuários à lista de proibidos respectivamente. <br>

Usuários apenas podem adicionar tarefas ao crontab que sejam compativeis com seu nivel de privilégio dentro do sistema sendo assim, se por acaso for inserida uma tarefa que exceda o nivel de privilégio daquele usuário(precise utilizar o sudo), a terefa não será executada por que o `crontab` não pode receber senhas, ele apenas executa tarefas em segundo plano. Para tarefas voltadas à administração do sistema, utilize um super-usuário. <br>

### __2 - Como usar?__ <br>

Para abrir o arquivo de texto do crontab use o comando `crontab -e`. Quando fizer isso, um editor de texto(como o nano) será aberto. Nele haverá um pequeno texto introduzindo algumas noções básicas sobre o seu uso, mas se quiser, pode apagar tudo sem qualquer problema. <br>

Primeiramente, vamos  entender a estruturação que as tarefas devem seguir para que o __cron__ consiga lê-las. Considere a seguinte tarefa: <br>

`00 20 * * 1-5 /home/vinicius/backup.sh` <br>

#### __2.1 - Definições de tempo.__ <br>

Comecemos com o que cada parte dele significa individualmente e suas variações e possibilidades. Vamos começar com a primeira parte.
`00 20 * * 1-5` <br>

O sistema vai antes de tudo, procurar alguma tarefa dentro do crontab que se encaixe naquela data, analizando na ordem **minuto** -> **hora** -> **dia do mês** -> **mês** -> **dia da semana**. Sempre que quisermos dizer _todos/tudo_ usamos o simbolo __*__. <br>
O minuto e a hora são as duas primeiras coisas ao declarar (`00 20`), depois declaramos os dias do mês que queremos rodar aquele comando(* - todos os dias do mês), em seguida os meses(* - todos os meses) e o dia da semana, que serão contabilizados de 0 a 6 onde 0 representa _domingo_(1-5 - nesse caso, de segunda a sexta). <br>

Além do __*__, tambem temos outro operadores com funções distintas: <br>

__(,)__: Lista valores especificos. Por exemplo: `00 20 5,25 * 1-5` Aqui a tarefa será executada as 20:00 do dia 5 e 25 durante dos os meses de segunda a sexta. É possivel usar __(,)__ em todos os campos e quantas vezes quiser para definir intervalos específicos para cada tarefa. <br>

__(-)__: Define um intervalo. Tomemos o  seguinte exemplo: `0 8-18 * * 1-5` Aqui a tarefa será executada a cada hora das 8 às 18 todos os dias do mês de todos os meses de segunda a sexta. Toda vez que o __(-)__ for utilizado é importante não se confundir e não preencher os campos corretamente. Nesse esxeplo que foi apresentado é vital não esquecer de por o 0 na frente do intervalo de horas, senão o crontab não permitirá salvar aquela tarefa. <br>

__(/)__: Define incrementos ou intervalos de repetição. O exemplo `*/15 8-18 * * 1-5` mostra que aquela tarefa será executada a cada 15 minutos a partir do 00 das 8 às 18 todos os dias do mês de todos os meses de segunda a sexta, ou seja, ele será executado 4x por hora(minutos 00, 15, 30 e 45). <br>

#### __2.2 - caminho e comando. <br>

