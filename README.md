# Crontab. <br>

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

