**RC-FINANCE-IA – Guia de Implantação e Operação**

> **Versão:** 1.0\
> **Data de Criação:** 07 de agosto de 2025\
> **Autor:** Equipe RC-Finance-IA (Coordenação por ChatGPT + RC-Holding)\
> **Finalidade:** Este documento tem como objetivo detalhar, passo a passo, a estruturação, execução e continuidade do sistema financeiro automatizado RC-FINANCE-IA, com foco em performance, segurança, IA e produtividade.

---

# ✨ Sumário

| Capítulo | Título                                                                              | Status         |
| -------- | ----------------------------------------------------------------------------------- | -------------- |
| 1        | Introdução Geral: Visão 360º do Projeto RC-FINANCE-IA                               | ✅ Concluído    |
| 2        | Fase 1 – Preparo do Ambiente: Instalação e Configurações Iniciais                   | ✅ Concluído    |
| 3        | Fase 2 – Módulo de Transações com Classificação Inteligente                         | ✅ Concluído    |
| 4        | Fase 3 – Importação Inteligente (CSV, JSON, OCR)                                    | ⏳ Em andamento |
| 5        | Fase 4 – Metas Financeiras Inteligentes (Alvo + Alocação Dinâmica)                  | ⏳ Planejado    |
| 6        | Fase 5 – Input de Voz via STT (Speech to Text) com Whisper                          | ⏳ Planejado    |
| 7        | Fase 6 – Integração Open Finance e APIs Bancárias                                   | ⏳ Planejado    |
| 8        | Fase 7 – Empacotamento em Docker + Deploy Opcional (Local e Nuvem)                  | ⏳ Planejado    |
| 9        | Apêndice A – Gestão de Usuários, Segurança e Criptografia Básica                    | ⏳ Planejado    |
| 10       | Apêndice B – Templates de Código, Snippets, Automatizações com IA no VS Code        | ⏳ Planejado    |
| 11       | Encerramento & Roadmap Contínuo: Versões Futuras, Padrões e Estratégias de Melhoria | ⏳ Planejado    |

---

# 📘 Capítulo 1 – Introdução Geral: Visão 360º do Projeto RC-FINANCE-IA

## 💡 Visão Geral

O RC-FINANCE-IA é um sistema de gerenciamento financeiro inteligente com o objetivo de transformar a gestão familiar e empresarial em uma experiência automatizada, acessível, segura e assistida por inteligência artificial.

Com suporte a entrada manual, arquivos, reconhecimento de voz, OCR, categorização por IA e painel de diagnóstico com recomendações dinâmicas, o projeto visa centralizar todas as decisões financeiras em uma interface moderna e eficiente.

## 🎯 Objetivos do Projeto

- Organizar as finanças de forma simples e automatizada
- Reduzir erros de categorização e alocação de recursos
- Oferecer recomendações de alocação com base em metas e prioridades
- Gerar insights em tempo real sobre fluxo de caixa, gastos e tendências
- Tornar-se uma base escalável para integração bancária (Open Finance)

## 📊 Métricas-Chave de Sucesso

- ✅ Transações salvas com 100% de integridade
- ✅ Classificação automática com >80% de precisão
- ✅ Dashboard funcional e responsivo
- 📈 Recomendação de metas inteligentes (em desenvolvimento)

## 🧩 Componentes Envolvidos

- **Interface principal (Streamlit)**: Dashboard com sidebar, abas e formulários
- **Banco de dados local**: SQLite via `sqlite-utils`
- **Classificação IA**: `sentence-transformers` com regras de fallback
- **Importação inteligente**: OCR, CSV, JSON (a partir da Fase 3)
- **Recomendações futuras**: IA para insights, metas, realocação

## 🚧 Status Atual do Projeto

| Componente                        | Status    |
| --------------------------------- | --------- |
| Conexão com DB                    | ✅ OK      |
| Cadastro e listagem de transações | ✅ OK      |
| Classificação IA                  | ✅ OK      |
| Metas (visualização básica)       | ✅ OK      |
| Importação por arquivo            | ⏳ Parcial |
| Recomendações de metas            | ⏳ Parcial |
| Dashboard e IA contextual         | ⏳ Parcial |

## 🛠️ Ferramentas Utilizadas

- **Python 3.11+** (com ambiente virtual gerenciado por Conda)
- **Streamlit** para front-end
- **sqlite-utils** para manipulação do banco de dados
- **Pandas / Matplotlib** para visualizações
- **Tesseract OCR** (fase 3 em diante)
- **Whisper STT** (fase 5)
- **Docker (futuro deploy)**

## 🔐 Estratégia de Segurança

- Arquivos locais criptografados por padrão (em roadmap)
- Credenciais sensíveis isoladas por `.env`
- Acesso local inicialmente, com plano para autenticação (fase 4–5)

## 📋 Execução Passo a Passo

1. **Criar pasta principal do projeto:**

   ```bash
   mkdir C:\RC-Finance-IA && cd C:\RC-Finance-IA
   ```

2. **Criar estrutura inicial de diretórios:**

   ```bash
   mkdir data scripts
   echo # RC-Finance-IA > README.md
   ```

3. **Instalar Miniconda e criar ambiente Python 3.11:**

   ```bash
   conda create -n iafinance python=3.11 -y
   conda activate iafinance
   ```

4. **Instalar dependências do projeto:**

   ```bash
   pip install streamlit sqlite-utils pandas matplotlib sentence-transformers
   ```

5. **Criar arquivo de banco inicial:**

   ```bash
   type nul > scripts/init_db.py
   ```

   Conteúdo sugerido para `init_db.py`:

   ```python
   from sqlite_utils import Database
   db = Database("data/finance.db")
   db["transactions"].create({
       "id": int,
       "type": str,
       "description": str,
       "amount": float,
       "category": str,
       "date": str
   }, pk="id", if_not_exists=True)
   print("Banco inicializado")
   ```

6. **Criar interface **``** com formulário de transações:**

   ```bash
   type nul > scripts/ui.py
   ```

   Conteúdo sugerido:

   ```python
   import streamlit as st
   from sqlite_utils import Database
   import time
   from datetime import date

   db = Database("data/finance.db")
   st.set_page_config(page_title="RC-Finance-IA")

   st.title("📄 RC-Finance-IA")
   st.header("Transações")
   st.dataframe(list(db["transactions"].rows))

   with st.form("form"):
       type_ = st.selectbox("Tipo", ["income", "expense"])
       amount = st.number_input("Valor")
       desc = st.text_input("Descrição")
       cat = st.text_input("Categoria")
       date_ = st.date_input("Data", value=date.today())
       submit = st.form_submit_button("Salvar")

   if submit:
       db["transactions"].insert({
           "id": int(time.time()),
           "type": type_,
           "description": desc,
           "amount": amount,
           "category": cat,
           "date": str(date_)
       })
       st.success("Transação salva com sucesso!")
       st.rerun()
   ```

7. **Executar aplicação:**

   ```bash
   streamlit run scripts/ui.py
   ```

---

## ⚠️ Gestão de Erros – Capítulo 1

### 🛑 Erro mais comum:

**Ambiente virtual não ativado corretamente**\
**Sinais:** comandos `streamlit` ou `sqlite-utils` não reconhecidos no terminal\
**Causa provável:** Conda ou dependências não ativadas\
**Impacto:** impossibilidade de rodar o sistema ou de interagir com o banco

### ✅ Quick Fix (passo a passo):

1. Fechar o terminal atual
2. Abrir o **Anaconda Prompt** como administrador
3. Rodar:
   ```bash
   conda activate iafinance
   cd C:\RC-Finance-IA
   streamlit run scripts/ui.py
   ```

### 🔁 Contingência/rollback:

- Se o ambiente `iafinance` estiver corrompido, deletar com `conda remove --name iafinance --all` e recriar.

### 🔒 Prevenção (checklist):

- Criar ambiente Conda com `python=3.11`
- Sempre ativar o ambiente antes de executar comandos
- Manter terminal com permissão de administrador

---

✅ Capítulo 1 concluído. Deseja que eu continue para o Capítulo 4 – Fase 3 (Importação Inteligente)?

