
# aws-infra-atividade

Primeiramente eu criei o **Security Group** para EC2 com o nome de `SegurancaAwsIFF`, liberando o acesso para o **SSH** na porta **22** para o meu IP pessoal, conforme solicitado, além de liberar o **HTTP** na porta **80**.

![Security Group EC2](https://github.com/user-attachments/assets/7c5d03ba-3cc7-4257-b707-53195fe1b6b4)

Também configurei o **Security Group** para RDS com o nome de `RDS-IFF`, configurando a conexão e permitindo apenas **PostgreSQL** para a EC2.

![Security Group RDS](https://github.com/user-attachments/assets/11649bf7-b5bc-4c2d-bee6-0114699c7ad7)

---

## Criação da Instância EC2
- Criei uma instância EC2 utilizando a **AMI Amazon Linux**.  
- Associei o **Security Group** que criei (`SegurancaAwsIFF`).  
- Criei um **par de chaves** com o nome de `ChaveAwsIFFSSH` (arquivo `.pem`).

---

## Criação do Banco de Dados (RDS)
- Criei um **Banco de Dados** via RDS utilizando **PostgreSQL**.  
- Configurei usuário, senha e nome do banco, conforme imagem abaixo.  
- Anotei a informação do **endpoint** para conexão.

![Configuração do Banco de Dados](https://github.com/user-attachments/assets/e5f4695e-2fe8-48b1-bf4e-67d18f2ca9a5)

---

## Acessando a Instância EC2 via SSH

Acessei a pasta onde se encontra minha `ChaveAwsIFFSSH.pem` e executei os comandos abaixo:

```bash
icacls "ChaveAwsIFFSSH.pem" /inheritance :r

whoami
icacls "ChaveAwsIFFSSH.pem" /inheritance:r /grant:r "casa\range:R"
```
Depois, para conectar usando o DNS público, utilizei:
```bash
ssh -i "ChaveAwsIFFSSH.pem" ec2-user@ec2-3-83-98-175.compute-1.amazonaws.com
```

![image](https://github.com/user-attachments/assets/16b249bd-5dc3-44e8-97bb-dc8a690a1a3d)

## Instalando Postgre na EC2

Instalei o PostgreSQL com o comando:
```bash
sudo dnf install -y postgresql16 postgresql16-server
```
![image](https://github.com/user-attachments/assets/ccfbed9e-edc8-4187-8c9f-4f44fe053568)

## Conectando ao Banco de Dados
Conectei ao banco de dados usando o endpoint anotado anteriormente:

```bash
psql -h database-iff-aws.cafy4kwoowbg.us-east-1.rds.amazonaws.com \
     -U adm \
     -d dbAwsIff \
     -p 5432 \
     -W
```
Depois digitei a minha senha para conectar.

![image](https://github.com/user-attachments/assets/e2c406a7-5c14-416a-8138-03370486bc9b)

Após isso, executei um SELECT básico só para validar a conexão com o banco e ver se estava tudo certo.
