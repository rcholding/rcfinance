**Capítulo 2 – Fase 1: Ambiente Local Preparado para RC-FINANCE-IA**

---

## 📃 Descrição Geral

Este capítulo descreve como preparar um ambiente local completo, estável e funcional para o desenvolvimento do projeto **RC-FINANCE-IA**. Inclui a configuração do VS Code, instaladores, ambientes Conda e dependências iniciais. Esta base garante que o projeto funcione com segurança e previsibilidade.

---

## ⚖️ Ferramentas e Requisitos

- **Sistema Operacional**: Windows 10 Pro (Compilação 19045)
- **Python**: 3.11+
- **Conda**: Miniconda 25.5.1 ou superior
- **Editor**: Visual Studio Code (VS Code)
- **Prompt**: Anaconda Prompt como Administrador
- **Extensões VS Code**:
  - Python
  - Pylance
  - Jupyter
  - Jupyter Keymap
  - Jupyter Notebook Renderers
  - Streamlit for VS Code

---

## 💪 Guia Passo a Passo

### 1. Instalar Miniconda (se ainda não tiver)
- Acesse: [https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)
- Baixe e instale a versão do Windows 64-bit
- Reinicie o PC após a instalação

### 2. Criar ambiente Conda
Abra o **Anaconda Prompt como Administrador**:
```bash
conda create -n iafinance python=3.11 -y
conda activate iafinance
```

### 3. Instalar dependências
```bash
pip install streamlit pandas matplotlib sqlite-utils
```

### 4. Criar estrutura de pastas do projeto
```bash
cd C:\
mkdir RC-Finance-IA
cd RC-Finance-IA
mkdir data scripts env
copy con README.md
copy con main.py
```

Ou via VS Code:
1. Abra o VS Code
2. Selecione: `Arquivo > Abrir Pasta > C:\RC-Finance-IA`
3. Crie arquivos `main.py`, `README.md`, e pastas `scripts`, `data`, `env`

### 5. Iniciar banco e scripts iniciais
```bash
cd scripts
copy con init_db.py
copy con ui.py
```

> Escreva o conteúdo inicial dos arquivos seguindo os próximos capítulos.

### 6. Inicializar banco
```bash
python scripts/init_db.py
```

### 7. Rodar aplicação
```bash
streamlit run scripts/ui.py
```

---

## ✅ Validação

- Rodar `http://localhost:8501` com interface funcional
- Verificar se `data/finance.db` foi criado
- Pastas corretas criadas com scripts salvos

---

## ⚠️ Gestão de Erros

### 🚫 Erro mais comum:
**Streamlit não roda ou falha com erro de ambiente.**

- **Sinais**: mensagem como `ModuleNotFoundError`, ou erro ao abrir navegador
- **Causa provável**: não ativou o ambiente Conda, dependência não instalada
- **Impacto**: impossibilidade de iniciar aplicação

### ✅ Quick Fix (passo a passo):
1. Feche tudo
2. Reabra o Anaconda Prompt como administrador
3. Execute:
```bash
conda activate iafinance
pip install streamlit pandas matplotlib sqlite-utils
```
4. Reinicie: `streamlit run scripts/ui.py`

### ♻️ Contingência:
- Se persistir, crie novo ambiente:
```bash
conda create -n novoenv python=3.11 -y
conda activate novoenv
pip install streamlit pandas matplotlib sqlite-utils
```

### 🔐 Prevenção (checklist):
- Sempre usar Anaconda Prompt como Administrador
- Sempre ativar o ambiente correto antes de executar qualquer script
- Verificar extensões do VS Code instaladas

---

Pronto! Ambiente 100% preparado para iniciar o desenvolvimento do RC-FINANCE-IA. Deseja que eu envie agora o Capítulo 3?

