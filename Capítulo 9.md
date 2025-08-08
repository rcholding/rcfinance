**Capítulo 9 – Fase 4: Metas Financeiras Inteligentes (Alvo + Alocação Dinâmica)**

---

## 📃 Descrição Geral

Esta etapa foca em permitir que o usuário defina **metas financeiras inteligentes** com base em prioridades, categorias, prazos e disponibilidade financeira. O sistema deve exibir metas existentes, permitir cadastrar novas e sugerir alocações automáticas com base no saldo atual e nos objetivos.

---

## ⚖️ Ferramentas e Requisitos

- Python 3.11+
- `streamlit`
- `sqlite-utils`
- Base de dados `finance.db` com tabela `goals`

---

## 💪 Guia Passo a Passo

### 1. Atualizar banco de dados (`init_db.py`):

Adicione após a criação de `transactions`:

```python
# tabela de metas financeiras
if "goals" not in db.table_names():
    db["goals"].create({
        "id": int,
        "name": str,
        "target": float,
        "progress": float,
        "priority": str,
        "due_date": str
    }, pk="id")
```

---

### 2. Atualizar `ui.py` com aba de Metas

Dentro do bloco `if page == "Metas":`, atualize para:

```python
st.header("🌟 Metas Financeiras")
aba = st.radio("Ações", ["Visualizar", "Cadastrar", "Alocar Inteligentemente"])

if aba == "Visualizar":
    st.dataframe(list(db["goals"].rows))

elif aba == "Cadastrar":
    with st.form("nova_meta"):
        name = st.text_input("Nome da Meta")
        target = st.number_input("Valor Alvo")
        priority = st.selectbox("Prioridade", ["Alta", "Média", "Baixa"])
        due = st.date_input("Data Limite")
        submitted = st.form_submit_button("Salvar Meta")

    if submitted:
        db["goals"].insert({
            "id": int(time.time()),
            "name": name,
            "target": target,
            "progress": 0.0,
            "priority": priority,
            "due_date": str(due)
        })
        st.success("Meta salva com sucesso!")
        st.rerun()

elif aba == "Alocar Inteligentemente":
    saldo_total = sum([t["amount"] for t in db["transactions"].rows if t["type"] == "income"]) - \
                  sum([t["amount"] for t in db["transactions"].rows if t["type"] == "expense"])

    metas = list(db["goals"].rows)
    if not metas:
        st.warning("Nenhuma meta encontrada.")
    else:
        st.info(f"Saldo disponível: R$ {saldo_total:.2f}")
        for m in metas:
            percentual = 0.5 if m["priority"] == "Alta" else 0.3 if m["priority"] == "Média" else 0.2
            alocacao = round(saldo_total * percentual, 2)
            st.write(f"Meta: {m['name']} | Alocação sugerida: R$ {alocacao:.2f}")
```

---

## ✅ Validação

- Cadastro e exibição de metas funcional
- Alocação inteligente de acordo com prioridade e saldo
- Interface intuitiva em Streamlit

---

## ⚠️ Gestão de Erros

### 🛑 Erro mais comum:

\*\*st.rerun() falha com \*\*\`\`

- **Sinais**: erro no log ao cadastrar
- **Causa**: versão desatualizada do Streamlit
- **Impacto**: não recarrega interface após salva

### ✅ Quick Fix:

1. Atualize o pacote:

```bash
pip install --upgrade streamlit
```

2. Reabra o terminal e reexecute:

```bash
streamlit run scripts/ui.py
```

### 🔁 Contingência:

- Substituir `st.rerun()` por `st.experimental_rerun()` em versões antigas

### 🔐 Prevenção:

- Garantir que streamlit esteja na última versão (>=1.27)
- Verificar changelog da biblioteca em cada build

---

Pronto! Agora o RC-FINANCE-IA possui gerenciamento de metas funcionais, com sugestões de alocação baseadas em prioridade.

Deseja seguir para a próxima etapa?

