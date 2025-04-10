# Bootstrap - Codespaces Test Environment

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)
![VS Code](https://img.shields.io/badge/VS_Code-0078D4?style=for-the-badge&logo=visual%20studio%20code&logoColor=white)
![GitHub Codespaces](https://img.shields.io/badge/GitHub_Codespaces-000000?style=for-the-badge&logo=github&logoColor=white)

## Índice
- [Pré-requisitos](#pré-requisitos)
- [Passo 1: Criar seu próprio repositório](#passo-1-criar-seu-próprio-repositório)
- [Passo 2: Abrir o Codespaces](#passo-2-abrir-o-codespaces)
- [O que testar](#o-que-testar)
  - [Comandos no Terminal](#comandos-no-terminal)
  - [PostgreSQL Setup with Docker](#postgresql-setup-with-docker)
    - [Instalação e Configuração](#instalação-e-configuração)
    - [Queries de Teste](#queries-de-teste)
  - [Acessando o JupyterLab](#acessando-o-jupyterlab)
    - [Testando Conexão com PostgreSQL via Python](#testando-conexão-com-postgresql-via-python)
    - [Salvando e Voltando ao Terminal](#salvando-e-voltando-ao-terminal)
  - [Salvando as Alterações no Git](#salvando-as-alterações-no-git)
- [Troubleshooting](#troubleshooting)
- [Recursos Adicionais](#recursos-adicionais)
- [Licença](#licença)

Esse repositório foi criado para facilitar o teste e validação do uso do GitHub Codespaces como ambiente de desenvolvimento totalmente na nuvem, sem a necessidade de instalar nada localmente.

Aqui você vai encontrar instruções passo a passo para:

- Criar seu próprio repositório a partir deste template
- Acessar o Codespaces via navegador
- Rodar comandos no terminal (CLI)
- Usar o JupyterLab
- Validar o funcionamento do docker (usando um database Postgres) via terminal e notebook
- Testar funcionalidades básicas como Ctrl+C/Ctrl+V e upload de arquivos

---

## Pré-requisitos

- Conta no GitHub (pode ser pessoal)
- Acesso liberado ao site [github.com](https://github.com)

---

## Passo 1: Criar seu próprio repositório

1. Acesse este repositório:  
   [https://github.com/brunoalviani/bootstrap-codespaces](https://github.com/brunoalviani/bootstrap-codespaces)

2. No canto superior direito da tela, clique no botão verde **`Use this template`**

3. Escolha a opção **`Create a new repository`**

4. Dê ao novo repositório **o mesmo nome deste aqui (bootstrap-codespaces)**:

5. Escolha onde criar:
- **Na sua conta pessoal** ou
- **Na organização da empresa (ex: `github.com/empresa/bootstrap-codespaces`)**

*Se possível, teste nos dois cenários para ver se há alguma limitação de permissão ou acesso.*

6. Marque a opção **Private** para manter o repositório privado.  

*Se der erro ou bloqueio por permissão, vale também testar como público só pra validar o acesso inicial.*

---

## Passo 2: Abrir o Codespaces

1. Acesse o **repositório que você acabou de criar** (não o original!)

   Dica: você pode confirmar isso olhando a URL no navegador.  
   Depois de `github.com/`, o nome que aparece deve ser **o seu usuário ou organização**, e **não** `brunoalviani`.  
   Exemplo:  
   ✅ `github.com/{your_username}/bootstrap-codespaces`  
   ❌ `github.com/brunoalviani/bootstrap-codespaces`

2. Clique no botão verde **`Code`**

3. Vá até a aba **`Codespaces`**

4. Clique no botão **`Create codespace on master`** (ou `main`, dependendo da configuração)

5. O ambiente será carregado direto no navegador, com terminal, arquivos e tudo pronto pra testar

---

## O que testar

### Comandos no Terminal

Assim que o ambiente carregar, o terminal estará aberto e pronto pra uso.

**Teste 1: Verificar Git**
```bash
git status
```
O resultado esperado deve ser algo assim:
```
On branch main
nothing to commit, working tree clean
```

**Teste 2: Validar Permissões de Clone**
Este é apenas um teste rápido para verificar se temos as permissões necessárias para clonar repositórios externos:

```bash
cd ..
git clone https://github.com/brunoalviani/sample-repository.git
```

Se uma nova pasta `sample-repository` aparecer, significa que o ambiente tem as permissões corretas para operações git. Você pode confirmar navegando até ela:
```bash
cd sample-repository
ls
```

> **Nota**: Este é apenas um repositório de teste usado para validar as permissões do ambiente. Após confirmar que o clone funcionou, você pode remover este repositório de teste com:
> ```bash
> cd ..
> rm -rf sample-repository
> ```

**Teste 3: Retornar ao Diretório do Projeto**
```bash
cd /workspaces/bootstrap-codespaces
```

### PostgreSQL Setup with Docker

#### Instalação e Configuração

1. **Verificar a Instalação do Docker**
   ```bash
   docker --version
   ```
   Saída esperada:
   ```
   Docker version 27.5.1-1, build 9f9e4058019a37304dc6572ffcbb409d529b59d8
   ```

2. **Executar o Container PostgreSQL**
   ```bash
   docker run --name postgres-container -e POSTGRES_PASSWORD=senha123 -p 5432:5432 -d postgres
   ```

3. **Verificar Status do Container**
   ```bash
   docker ps
   ```

4. **Instalar o Cliente PostgreSQL**
   ```bash
   sudo apt update && sudo apt install -y postgresql-client
   ```

5. **Conectar ao PostgreSQL**
   ```bash
   psql -h localhost -p 5432 -U postgres -W
   ```
   Senha: `senha123`

   Saída esperada:
   ```
   psql (12.22 (Ubuntu 12.22-0ubuntu0.20.04.2), server 17.4 (Debian 17.4-1.pgdg120+2))
   WARNING: psql major version 12, server major version 17.
            Some psql features might not work.
   Type "help" for help.

   postgres=#
   ```

6. **Instalar e Configurar a Extensão PostgreSQL Client**
   ```bash
   code --install-extension cweijan.vscode-postgresql-client2
   ```

   Após a instalação, configure a conexão:
   1. Abra a aba Database Client na barra lateral do VS Code (ícone de banco de dados)
   2. Clique no botão '+' para adicionar uma nova conexão
   3. Selecione 'PostgreSQL'
   4. Preencha os dados de conexão:
      - Host: localhost
      - Port: 5432
      - Username: postgres
      - Password: senha123
      - Database: postgres
   5. Clique em 'Connect'

   Para aprender mais sobre como usar a extensão, incluindo:
   - Como executar queries
   - Como visualizar e editar dados em tabelas
   - Como exportar resultados
   - Como usar o histórico de queries
   
   Consulte a [documentação oficial da extensão](https://marketplace.visualstudio.com/items/?itemName=cweijan.vscode-postgresql-client2).

#### Queries de Teste

Após configurar tudo, teste com estas queries básicas:

```sql
-- Criar uma tabela de teste
CREATE TABLE test_table (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Inserir alguns dados
INSERT INTO test_table (name) VALUES 
    ('Test 1'),
    ('Test 2');

-- Consultar dados
SELECT * FROM test_table;
```

### Acessando o JupyterLab

Para acessar o ambiente JupyterLab:

1. Volte para a página do seu repositório no GitHub (ex: https://github.com/brunoalviani/bootstrap-codespaces )

2. Clique no botão verde **`Code`**

3. Vá até a aba **`Codespaces`**

4. Localize seu ambiente ativo
   - Ele será indicado com um círculo verde e a palavra "Active"
   - Este é o mesmo ambiente que você criou anteriormente

5. Clique nos três pontos (...) ao lado do ambiente ativo

6. Selecione **`Open in JupyterLab`**

O JupyterLab abrirá em uma nova aba do navegador, com todas as funcionalidades disponíveis para criar e executar notebooks.

#### Testando Conexão com PostgreSQL via Python

No JupyterLab, crie um novo notebook Python (`.ipynb`) com o seguinte 
conteúdo:
Este notebook demonstra:
1. Como instalar a biblioteca necessária
2. Como estabelecer uma conexão com o PostgreSQL
3. Como executar uma query simples
4. Como tratar erros de conexão
5. Como fechar a conexão adequadamente

```python
# Primeira célula - Instalação da biblioteca
!pip install psycopg2-binary
```

Saída esperada:

```
Collecting psycopg2-binary
Downloading psycopg2_binary-2.9.9-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (3.0 MB)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.0/3.0 MB 8.2 MB/s eta 0:00:00
Installing collected packages: psycopg2-binary
Successfully installed psycopg2-binary-2.9.9
```

```python
# Segunda célula - Testando a conexão
import psycopg2

# Configurações de conexão
conn_params = {
    'host': 'localhost',
    'database': 'postgres',
    'user': 'postgres',
    'password': 'senha123',
    'port': '5432'
}

# Tentando conectar
try:
    conn = psycopg2.connect(**conn_params)
    print("✅ Conexão bem sucedida!")
    
    # Criando um cursor e executando um comando simples
    cur = conn.cursor()
    cur.execute("SELECT version();")
    version = cur.fetchone()
    print("\nVersão do PostgreSQL:")
    print(version[0])
    
    # Fechando cursor e conexão
    cur.close()
    conn.close()
    print("\nConexão fechada.")
    
except Exception as e:
    print("❌ Erro ao conectar:")
    print(e)
```

Saída esperada:
```
Versão do PostgreSQL:
PostgreSQL 17.4 (Debian 17.4-1.pgdg120+2) on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
Conexão fechada.
```

#### Salvando e Voltando ao Terminal

1. **Salvar o Notebook**
   - No JupyterLab, clique no ícone de disquete 💾 ou use `Ctrl+S` (`Cmd+S` no Mac)
   - Certifique-se que o arquivo `postgres_test.ipynb` foi salvo

2. **Retornar ao Codespace**
   - Se a aba do Codespace ainda estiver aberta, simplesmente alterne para ela
   - Caso tenha fechado a aba, você pode reabrir assim:
     1. Acesse seu repositório no GitHub
     2. Clique no botão verde **`Code`**
     3. Vá até a aba **`Codespaces`**
     4. Clique no Codespace ativo (indicado com círculo verde)
     5. Selecione **`Open in browser`**

3. **Navegar para o Diretório Correto**
   ```bash
   cd /workspaces/bootstrap-codespaces
   ```

   Para confirmar que você está no diretório correto, você pode executar:
   ```bash
   pwd
   ```
   
   A saída deve ser exatamente:
   ```
   /workspaces/bootstrap-codespaces
   ```

### Salvando as Alterações no Git

Agora vamos registrar as alterações no repositório usando Git. Execute os seguintes comandos:

1. **Verificar o Status das Alterações**
   ```bash
   git status
   ```
   
   Você deverá ver os arquivos modificados ou novos em vermelho

2. **Adicionar Todas as Alterações**
   ```bash
   git add .
   ```

3. **Criar o Commit**
   ```bash
   git commit -m "Add PostgreSQL connection test notebook"
   ```
   
   Saída esperada:
   ```
   [main 1234abc] Add PostgreSQL connection test notebook
    1 file changed, XX insertions(+)
    create mode 100644 postgres_test.ipynb
   ```

4. **Enviar para o GitHub**
   ```bash
   git push origin main
   ```
   
   Saída esperada:
   ```
   Enumerating objects: 4, done.
   Counting objects: 100% (4/4), done.
   Delta compression using up to 8 threads
   Compressing objects: 100% (3/3), done.
   Writing objects: 100% (3/3), XXX bytes | XXX MiB/s, done.
   Total XXX (delta X), reused 0 (delta 0)
   To github.com:seu-usuario/git-basics-demo.git
      1234abc..5678def  main -> main
   ```

5. **Confirmar que Está Tudo Certo**
   ```bash
   git status
   ```
   
   Deve mostrar:
   ```
   On branch main
   nothing to commit, working tree clean
   ```

Pronto! Suas alterações já estão salvas no GitHub. Você pode verificar acessando seu repositório no navegador.

## Troubleshooting

### Problemas Comuns

1. **Erro de Conexão com PostgreSQL**
   - Verifique se o container está rodando: `docker ps`
   - Confirme as credenciais
   - Teste a conexão via psql primeiro

2. **Extensão não Aparece no VS Code**
   - Tente recarregar a janela: `Ctrl/Cmd + Shift + P` -> "Reload Window"
   - Verifique se a instalação foi bem sucedida

3. **Container não Inicia**
   - Verifique logs: `docker logs postgres-container`
   - Tente remover e recriar: 
     ```bash
     docker rm -f postgres-container
     docker run --name postgres-container -e POSTGRES_PASSWORD=senha123 -p 5432:5432 -d postgres
     ```
## Recursos Adicionais

- [Documentação PostgreSQL](https://www.postgresql.org/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [GitHub Codespaces Documentation](https://docs.github.com/en/codespaces)
- [VS Code PostgreSQL Extension](https://marketplace.visualstudio.com/items?itemName=cweijan.vscode-postgresql-client2)
- [JupyterLab Documentation](https://jupyterlab.readthedocs.io/)

## Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.
