# Por que docker-compose?

O servidor mosquitto está configurado usando docker-compose.
Foi escolhido essa tecnologia por alguns motivos muito simples:

1. **REINICIO AUTOMÁTICO**: docker-compose permite muito facilmente configurar o serviço para se reiniciar sozinho caso a máquina virtual seja desligada. Isso é alcançado usando a linha *restart: always* na configuração do *docker-compose.yaml*

2. **PORTABILIDADE**: como a proposta do docker é funcionar indiferente do host, podemos testar e configurar o servidor mosquitto localmente e assim que estarmos satisfeitos, apenas enviamos o único arquivo de configuração para a máquina virtual e ela terá exatamente o mesmo ambiente.

3. **SEGURANÇA**: por ser isolado do resto do sistema, o docker é muito seguro, pois um atacante que conseguir acesso ao container do mosquitto, não conseguirá sair dele, e assim, ter controle da máquina virtual inteira.

## Configuração atual:

Na nossa intalação, usamos a imagem oficial do mosquitto (*eclipse-mosquitto:1.6.12*) e roteamos a porta da máquina virtual *1841* para a porta *1883* do container, que é a porta padrão do mosquitto. Fizemos isso ao invés de configurar o mosquitto para usar a porta 1841 pois a imagem apresenta alguns bug de permissão. Dessa forma, lendo algumas issues no github que resolveram problemas semelhantes ao nosso, decidimos mudar a menor quantidade de configurações do mosquitto em si, e delegar ao docker o que for possível dessa parte de networking.

Configuramos a persistência na pasta *~/mosquitto/data* e os logs  de acesso na pasta *~/mosquitto/logs*.
Testamos o servidor e ele pode ser acesso através da internet na porta *8041* no host *andromeda.lasdpc.icmc.usp.br*, ou através da rede local pela porta *1841*. A configuração atual usa apenas o protocolo MQTT, sem websocket e sem TLS.

