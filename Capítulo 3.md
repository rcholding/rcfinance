**Capítulo 3 – Fase 2: Banco de Dados e Modelo Inicial (RC-FINANCE-IA)**

---

## 📊 Visão Geral

Nesta etapa, estruturamos o banco de dados principal do projeto **RC-FINANCE-IA**. O objetivo é criar um banco funcional, com as tabelas e campos essenciais para armazenar transações e metas financeiras, garantindo consistência e expansão futura. Toda a estrutura é criada utilizando o módulo `sqlite-utils`.

---

## 🔧 Ferramentas Necessárias

- Python 3.11+ (ativo no ambiente `iafinance`)
- Módulo `sqlite-utils`
- Script `init_db.py` localizado em `scripts/`
- Pasta de dados: `data/`

---

## 📅 Procedimento Passo a Passo

### 1. Navegar para o diretório do projeto

```bash
cd C:\RC-Finance-IA
```

### 2. Criar script `init_db.py`

Local: `C:\RC-Finance-IA\scripts\init_db.py`

Conteúdo:

```python
from sqlite_utils import Database

# Cria/abre o banco de dados
db = Database("data/finance.db")

# Tabela de transações
if not db.table("transactions").exists():
    db["transactions"].create({
        "id": int,
        "type": str,  # income ou expense
        "description": str,
        "amount": float,
        "category": str,
        "date": str
    }, pk="id")

# Tabela de metas
if not db.table("goals").exists():
    db["goals"].create({
        "id": int,
        "title": str,
        "target_amount": float,
        "current_amount": float,
        "priority": str,  # baixa, media, alta
        "due_date": str,
        "type": str  # economizar, quitar, adquirir
    }, pk="id")

print("Banco de dados inicializado com sucesso!")
```

### 3. Rodar o script

```bash
python scripts/init_db.py
```

---

## ✅ Indicadores de Sucesso

- Arquivo `finance.db` criado dentro da pasta `data/`
- Execução sem erros no terminal
- Tabelas `transactions` e `goals` visíveis usando um SQLite Viewer (opcional)

---

## 🚫 Erro Mais Comum

**🛑 Erro mais comum:** `sqlite3.OperationalError` ao rodar o script.

- **Sinais:** o terminal exibe mensagens de erro relacionadas ao banco de dados.
- **Causa provável:** conflito de dados antigos ou sintaxe incorreta no `create()`.
- **Impacto:** banco é criado de forma incompleta ou com tabelas ausentes.

### ✅ Quick Fix:

1. Verifique se o script está salvo corretamente.
2. Apague o arquivo `data/finance.db` (caso não tenha dados importantes):

```bash
del data\finance.db
```

3. Execute novamente:

```bash
python scripts/init_db.py
```

### 🔁 Contingência:

- Se ainda falhar, crie um novo script com nome `init_db_alt.py` e reescreva apenas a criação de `transactions`.
- Rode esse script separado para forçar a geração.

### 🔐 Prevenção:

- Não edite diretamente o arquivo `finance.db`
- Sempre verifique a existência das tabelas com `if not db.table("nome").exists()`
- Mantenha backups da pasta `data/`

---

**Capítulo 3 finalizado!** Deseja que eu envie agora o **Capítulo 4 – Interface Inicial com Streamlit**?

