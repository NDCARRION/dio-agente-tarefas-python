# Agente Inteligente de Gerenciamento de Tarefas — DIO

## Sobre o projeto

Este projeto foi desenvolvido como parte de um desafio prático da
Digital Innovation One (DIO).

O objetivo é criar um agente de inteligência artificial capaz de
gerenciar tarefas automaticamente no Trello por meio de comandos
em linguagem natural.

O agente utiliza Python, Google Agent Development Kit (ADK),
Google Gemini e a API do Trello.

## Funcionalidades

O agente permite:

- Criar tarefas no quadro DIO do Trello.
- Adicionar descrições e prazos aos cartões.
- Listar tarefas cadastradas.
- Consultar tarefas por status.
- Mover tarefas entre as listas A fazer, Em andamento e Concluído.
- Interpretar solicitações em linguagem natural.

## Melhorias implementadas

### 1. Prevenção de tarefas duplicadas

Antes de criar um cartão, o sistema verifica se já existe uma tarefa
com o mesmo nome no quadro.

A verificação ignora diferenças entre letras maiúsculas e minúsculas.

Se a tarefa já existir, o agente informa sua existência e evita
a criação de outro cartão.

### 2. Reconhecimento de diferentes expressões de status

O agente reconhece diferentes maneiras de informar o andamento
de uma tarefa.

Exemplos:

- "Comecei" → Em andamento.
- "Estou trabalhando" → Em andamento.
- "Finalizei" → Concluído.
- "Terminei" → Concluído.
- "Preciso refazer" → A fazer.

### 3. Correção da data de vencimento

Foi implementado um tratamento para aceitar datas nos formatos:

- DD/MM/AAAA
- AAAA/MM/DD
- AAAA-MM-DD

Também foi ajustado o horário enviado ao Trello para evitar
que o prazo apareça no dia anterior.

## Tecnologias utilizadas

- Python
- Google Agent Development Kit (ADK)
- Google Gemini
- API do Trello
- Biblioteca py-trello
- python-dotenv
- Visual Studio Code

## Como executar

1. Instale o Python.
2. Crie um ambiente virtual:

   `py -m venv .venv`

3. Instale as dependências:

   `.\.venv\Scripts\python.exe -m pip install -r requirements.txt`

4. Configure as credenciais no arquivo `agenttaskmanager/.env`,
   utilizando `agenttaskmanager/.env.exemplo` como referência.

5. Crie um quadro no Trello chamado `DIO`, com as listas:

   - A fazer
   - Em andamento
   - Concluído

6. Inicie o agente:

   `.\.venv\Scripts\adk.exe web`

7. Acesse:

   `http://127.0.0.1:8000`

## Testes realizados

| Teste | Resultado |
|---|---|
| Criação de tarefa no Trello | Aprovado |
| Data de vencimento em 15/11/2026 | Aprovado |
| Prevenção de tarefa duplicada | Aprovado |
| Movimentação para Em andamento | Aprovado |
| Comando "Finalizei" | Aprovado |

## Segurança

As chaves de API e os tokens são armazenados em variáveis de ambiente.

O arquivo `.gitignore` foi configurado para impedir que credenciais,
ambientes virtuais e arquivos temporários sejam incluídos no Git.

**As credenciais não devem ser publicadas no GitHub.**

---

# Guia de configuração do Trello

A seguir, está o guia original de registro e autorização
da API do Trello.
# Guia Completo: Registro e Autorização de App no Trello com Python

Este guia orienta você no processo de registrar, autorizar e usar as APIs do Trello com Python.

## Pré-requisitos

- Conta ativa no Trello
- Python 3.7 ou superior instalado
- pip (gerenciador de pacotes Python)
- Navegador web

---

## Instalação das Dependências

Primeiro, instale as bibliotecas necessárias:

```bash
pip install -r requirements.txt
```

Ou crie um arquivo `requirements.txt`:

```txt
google-adk
py-trello
datetime
dotenv
```

E instale com:

```bash
pip install -r requirements.txt
```

---

## Passo 1: Criar um Novo Power-Up (Aplicativo)

### 1.1 Acessar o Portal de Power-Ups

1. Acesse o portal de administração de Power-Ups do Trello:
   ```
   https://trello.com/power-ups/admin/
   ```

2. Faça login com sua conta Trello

3. Clique no botão **"New"** ou **"Criar novo Power-Up"**

### 1.2 Preencher Informações do Aplicativo

Na tela "Novo aplicativo", preencha os seguintes campos:

| Campo | Valor Exemplo | Descrição |
|-------|---------------|-----------|
| **Nome do aplicativo** | `AppDio` ou `Meu App Python` | Nome que identificará seu aplicativo |
| **Área de trabalho** | Selecione seu workspace | Workspace onde o app será gerenciado |
| **Email** | `me@company.com` | Email para contato sobre o aplicativo |
| **Contato de suporte** | `support@company.com` | Email ou link para suporte aos usuários |
| **Autor** | `Seu Nome` ou `Sua Empresa` | Nome do desenvolvedor/empresa |
| **URL de conector iframe** | `https://seu-dominio.com/` | URL do iframe (opcional para API básica) |

> 💡 **Dica:** Para uso apenas da API REST (sem interface visual), você pode deixar a "URL de conector iframe" em branco ou colocar uma URL placeholder.

### 1.3 Criar o Power-Up

1. Revise as informações preenchidas

2. Clique em **"Criar"** no canto inferior direito

3. Você será redirecionado para a página de gerenciamento do seu Power-Up

---

## Passo 2: Obter a API Key

Após criar o Power-Up:

1. Na página de gerenciamento do seu Power-Up, procure pela seção **"API Key"** ou **"Chave de API"**

2. Você verá sua **API Key** ou **chave de API**(uma string alfanumérica longa) e o ** Secret ** ou ** Segredo ** 

3. **Copie e guarde essa chave** em um local seguro

> ⚠️ **Importante:** A API Key é única para seu Power-Up e deve ser tratada como informação sensível.

**Formato da API Key:**
```
abc123def456ghi789jkl012mno345pqr678
```

---

## Passo 3: Gerar o Token de Autorização

### 3.1 Construir a URL de Autorização

Agora você precisa gerar um token de acesso para fazer requisições em nome do usuário.

Use a seguinte URL, substituindo `SUA_API_KEY_AQUI` pela sua API Key:

```
https://trello.com/1/authorize?expiration=never&name=AppDio&scope=read,write&response_type=token&key=SUA_API_KEY_AQUI
```

### 3.2 Parâmetros Explicados

| Parâmetro | Valor | Descrição |
|-----------|-------|-----------|
| `expiration` | `never` | Token não expira<br>Opções: `1hour`, `1day`, `30days`, `never` |
| `name` | `AppDio` | Nome do aplicativo (use o mesmo do Passo 1) |
| `scope` | `read,write` | Permissões solicitadas<br>Opções: `read`, `write`, `account` |
| `response_type` | `token` | Tipo de resposta (sempre `token`) |
| `key` | `SUA_API_KEY` | Sua API Key obtida no Passo 2 |

### 3.3 Exemplo de URL Completa

Se sua API Key for `abc123def456ghi789`, a URL ficaria:

```
https://trello.com/1/authorize?expiration=never&name=AppDio&scope=read,write&response_type=token&key=abc123def456ghi789
```

### 3.4 Escopos de Permissão Disponíveis

| Escopo | Descrição |
|--------|-----------|
| `read` | Permite ler informações de boards, cards, listas, etc. |
| `write` | Permite criar, editar e deletar recursos |
| `account` | Permite acesso a informações da conta do usuário |

Para múltiplos escopos, separe com vírgula: `scope=read,write,account`

---

## Passo 4: Autorizar o Aplicativo

### 4.1 Acessar a URL de Autorização

1. Cole a URL completa (com sua API Key) no navegador

2. Pressione **Enter**

### 4.2 Revisar Permissões

Você será redirecionado para uma página de autorização do Trello que mostrará:

- ✅ Nome do aplicativo (ex: "AppDio")
- ✅ Permissões solicitadas (read, write)
- ✅ Lista de boards e organizações acessíveis
- ✅ Duração do token (never = sem expiração)

### 4.3 Conceder Acesso

1. Revise cuidadosamente as permissões

2. Se estiver de acordo, clique no botão **"Permitir"** ou **"Allow"**

3. Você será redirecionado para uma página de sucesso

---

## Passo 5: Obter o Token

### 5.1 Copiar o Token

Após autorizar, o Trello exibirá seu **Token de Acesso** em texto simples na página.

O token será uma string alfanumérica longa, similar a:

```
a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e1f2
```

### 5.2 Guardar com Segurança

**⚠️ CRÍTICO: Copie e guarde esse token imediatamente!**

Você precisará dele para todas as requisições à API. Se perder o token, será necessário gerar um novo seguindo os Passos 3 e 4 novamente.

---

### Bibliotecas Python
- **py-trello:** https://github.com/sarumont/py-trello
---

**Última atualização:** Fevereiro 2026  
**Versão da API Trello:** v1  
**Python:** 3.7+