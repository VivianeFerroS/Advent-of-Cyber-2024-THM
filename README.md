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

AcessE o diretório como http://IP_DA_MAQUINA/download, assim ele já faz download do arquivo zip denominado download.zip.

Descompacte o arquivo zip, e lá conterá dois arquivos .mp3, sendo eles song.mp3 e somg.mp3


![image](https://github.com/user-attachments/assets/45ce33f5-bab7-4c73-b64f-6bede4101ee0)

