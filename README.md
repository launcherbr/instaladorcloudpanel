# Instalador Whaticket SaaS

Esse instalador foi modificado para ser instalado em um Servidor com CloudPanel.

```bash
sudo apt -y update && apt -y upgrade
```

FAZENDO DOWNLOAD DO INSTALADOR & INICIANDO A PRIMEIRA INSTALAÇÃO (USAR SOMENTE PARA PRIMEIRA INSTALAÇÃO):

```bash
sudo apt install -y git && git clone https://github.com/launcherbr/instalador.git instalador && sudo chmod -R 777 instalador  && cd instalador  && sudo ./install_primaria
```

ACESSANDO DIRETORIO DO INSTALADOR & INICIANDO INSTALAÇÕES ADICIONAIS (USAR ESTE COMANDO PARA SEGUNDA OU MAIS INSTALAÇÃO:

```bash
cd instalador  && sudo ./install_instancia
```

## Requisitos

| --- | Mínimo | Recomendado |
| --- | --- | --- |
| Node JS | 20.x | 22.x |
| Ubuntu | 20.x | 22.x |
| Memória RAM | 4Gb | 8Gb |

-----

## Configurando Domínio e SSL no CloudPanel

Depois de instalar as instâncias, você precisará apontar um domínio (ou subdomínio) para cada uma delas. Siga os passos abaixo para configurar o proxy reverso e o SSL.

**Pré-requisito:** Certifique-se de que os registros DNS (Tipo A) dos seus domínios/subdomínios já estão apontando para o endereço IP do seu servidor.

### 1\. Proxy Reverso para o Frontend (Porta 3250)

Esta configuração irá direcionar um subdomínio para a API da sua instância.

1.  Faça login no seu CloudPanel.
2.  Navegue até **Sites** -\> **Add Site**.
3.  Selecione a opção **Create a Reverse Proxy**.
4.  Preencha os campos da seguinte forma:
      * **Domain Name:** `api.seudominio.com` (use um subdomínio para o backend).
      * **Reverse Proxy URL:** `http://127.0.0.1:3250`
      * **PHP Version:** No PHP needed.
5.  Clique em **Add Site**.

### 2\. Proxy Reverso para o Backend (Porta 4000)

Esta configuração irá direcionar seu domínio principal ou um subdomínio para a interface de usuário da sua instância.

1.  Volte para **Sites** -\> **Add Site**.
2.  Selecione novamente **Create a Reverse Proxy**.
3.  Preencha os campos:
      * **Domain Name:** `app.seudominio.com` (ou o domínio que seus clientes usarão para acessar).
      * **Reverse Proxy URL:** `http://127.0.0.1:4000`
      * **PHP Version:** No PHP needed.
4.  Clique em **Add Site**.

### 3\. Emitindo o Certificado SSL (Let's Encrypt)

Você deve repetir este processo para os dois sites criados (backend e frontend).

1.  No painel do CloudPanel, clique em **Sites** para listar todos os sites.
2.  Clique no nome do domínio para o qual você deseja emitir o SSL (por exemplo, `api.seudominio.com`).
3.  No menu de gerenciamento do site à esquerda, clique em **SSL/TLS**.
4.  Clique no botão **New SSL Certificate**.
5.  Selecione a opção **Let's Encrypt Certificate**.
6.  Clique no botão **Create and Install**. O CloudPanel irá verificar seu DNS e instalar o certificado automaticamente.
7.  Repita os passos 2 a 6 para o outro domínio (ex: `app.seudominio.com`).

Após seguir estes passos, suas instâncias do Whaticket estarão acessíveis através dos domínios configurados e com a conexão segura (HTTPS).
