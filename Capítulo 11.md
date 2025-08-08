**Capítulo 11 – Fase Final: Implantação Local e Publicação Web (RC-FINANCE-IA)**

---

## 🌎 Visão Geral

Esta é a última etapa da versão 1.0 do projeto **RC-FINANCE-IA**, onde vamos disponibilizar o sistema para uso real em ambiente local e/ou web. Serão abordadas:

- Execução local com Streamlit
- Configuração para abertura automática no navegador
- Empacotamento com `.bat` ou `.sh`
- Publicação simples com ferramentas gratuitas (Streamlit Community Cloud / Render / Railway)

---

## 🔧 Ferramentas & Requisitos

- Ambiente Conda `iafinance`
- Pasta do projeto: `RC-Finance-IA`
- Streamlit instalado (`pip install streamlit`)
- Conta gratuita no [https://share.streamlit.io](https://share.streamlit.io)
- Git instalado (para deploy online)
- VS Code (opcional para ajustes rápidos)

---

## ✅ Procedimento Passo a Passo

### 1. Rodar localmente

Terminal Conda:
```bash
cd C:\RC-Finance-IA
streamlit run scripts/ui.py
```

O sistema estará disponível em: `http://localhost:8501`

### 2. Tornar execução automática (Windows)

Criar `iniciar.bat` dentro da pasta `RC-Finance-IA`:
```bat
@echo off
cd /d %~dp0
conda activate iafinance
streamlit run scripts/ui.py
pause
```

Executar com duplo clique.

### 3. Publicar no Streamlit Cloud

1. Suba o projeto para um repositório GitHub (público ou privado)
2. Acesse [https://share.streamlit.io](https://share.streamlit.io)
3. Clique em "New App"
4. Escolha seu repositório e o caminho `scripts/ui.py`
5. Clique em "Deploy"

### 4. Alternativas gratuitas para deploy

- **Render.com** (fácil, suporte a Python):
  - Crie conta, conecte GitHub, selecione repo
  - Build Command: `pip install -r requirements.txt && streamlit run scripts/ui.py`
  - Port: 8501

- **Railway.app** (build mais lento, mas funcional)
  - Adicione como projeto Python e especifique os comandos

---

## ✔️ Indicadores de Sucesso

- O sistema abre no navegador local sem erros
- Interface responsiva e funcional
- Publicação online acessível por URL externa (no mínimo em modo de leitura/teste)

---

## ❌ GESTÃO DE ERROS (Bloco Padrão)

### 🚫 Erro mais comum:
`OSError: Streamlit não encontrado ou erro ao abrir navegador`

**Sinais:** não abre a interface após o comando, erro no terminal

**Causa provável:** ambiente errado, Streamlit não instalado, erro de PATH

**Impacto:** impossibilidade de iniciar a aplicação

---

### ✅ Quick Fix:

1. Verifique se está no ambiente `iafinance` com `conda activate iafinance`
2. Reinstale Streamlit com `pip install streamlit`
3. Teste manualmente: `python -m streamlit run scripts/ui.py`

---

### 🔁 Contingência:

- Rodar o script manualmente com VS Code (`F5`) ou terminal Python
- Usar `.bat` como alternativa para abertura

---

### 🔐 Prevenção:

- Criar atalho `.bat` para sempre rodar no ambiente correto
- Documentar dependências no `requirements.txt`
- Sempre testar deploy local antes de subir para nuvem

---

**Parabéns! Com isso finalizamos a implantação do RC-FINANCE-IA – Versão 1.0**

Deseja que eu gere um “Resumo Final do Livro + Roadmap de Expansão”?

