# Chatbot com IA usando Streamlit e OpenAI

## Visão Geral

Este projeto consiste no desenvolvimento de um **chatbot com Inteligência Artificial**, construído inteiramente em **Python**, utilizando o **Streamlit** para criação da interface (frontend e backend) e a **API da OpenAI** para geração das respostas inteligentes.

O objetivo é demonstrar, de forma prática, como integrar modelos de linguagem a uma aplicação interativa, mantendo o histórico de conversas e oferecendo uma experiência semelhante a um chat em tempo real.

---

## Objetivo do Projeto

* Criar um chatbot funcional com interface simples e intuitiva;
* Integrar Python com a API da OpenAI;
* Manter o histórico de mensagens durante a sessão do usuário;

---

## Tecnologias Utilizadas

* **Python**
* **Streamlit** – criação da interface do chat
* **OpenAI API** – geração das respostas com IA

---

## Estrutura e Explicação do Código

### 1. Importação das Bibliotecas

Nesta etapa, são importadas as bibliotecas responsáveis pela interface e pela comunicação com a OpenAI.

```python
import streamlit as st
from openai import OpenAI
import os
```

---

### 2. Configuração Segura da API da OpenAI

A chave da API **não deve ser inserida diretamente no código**. Neste projeto, ela é lida a partir de uma variável de ambiente, garantindo maior segurança e aderência às boas práticas.

```python
client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))
```

> ⚠️ Antes de executar o projeto, é necessário definir a variável de ambiente `OPENAI_API_KEY` no sistema operacional.

---

### 3. Criação da Interface do Chat

O Streamlit permite criar rapidamente uma interface interativa. Aqui é definido o título do aplicativo.

```python
st.write("# Chatbot com IA")
```

---

### 4. Gerenciamento do Histórico de Mensagens

O histórico da conversa é armazenado no `session_state`, garantindo que as mensagens não sejam perdidas a cada interação.

```python
if "lista_mensagens" not in st.session_state:
    st.session_state["lista_mensagens"] = []
```

---

### 5. Entrada de Mensagens do Usuário

O campo de input permite que o usuário digite mensagens, que são imediatamente exibidas no chat.

```python
texto_usuario = st.chat_input("Digite sua mensagem")
```

As mensagens anteriores são renderizadas novamente para manter o contexto visual da conversa.

```python
for mensagem in st.session_state["lista_mensagens"]:
    st.chat_message(mensagem["role"]).write(mensagem["content"])
```

---

### 6. Envio da Pergunta para a IA e Exibição da Resposta

Quando o usuário envia uma mensagem:

* Ela é adicionada ao histórico;
* Enviada para o modelo da OpenAI;
* A resposta gerada é exibida na tela e armazenada.

```python
if texto_usuario:
    st.chat_message("user").write(texto_usuario)
    st.session_state["lista_mensagens"].append({"role": "user", "content": texto_usuario})

    resposta_ia = client.chat.completions.create(
        model="gpt-4o",
        messages=st.session_state["lista_mensagens"]
    )

    texto_resposta = resposta_ia.choices[0].message.content
    st.chat_message("assistant").write(texto_resposta)
    st.session_state["lista_mensagens"].append({"role": "assistant", "content": texto_resposta})
```

---

## Como Executar o Projeto

1. Instale as dependências:

```bash
pip install streamlit openai
```

2. Defina a variável de ambiente com sua chave da OpenAI:

**Windows (PowerShell):**

```bash
setx OPENAI_API_KEY "sua_api_key_aqui"
```

**Linux / Mac:**

```bash
export OPENAI_API_KEY="sua_api_key_aqui"
```

3. Execute a aplicação:

```bash
streamlit run chat_bot.py
```

---

---

## Possíveis Evoluções

* Controle de contexto e limite de mensagens;
* Adição de autenticação de usuários;
* Deploy em nuvem (Streamlit Cloud, AWS ou GCP);
* Integração com WhatsApp ou Webhooks.

---

## Autor

**Jorge Ferreira da Silva Neto**
Analista de Dados | MBA em Data Science e Analytics
