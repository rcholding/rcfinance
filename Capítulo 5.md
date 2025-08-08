**Capítulo 5 – Fase 4: Metas Inteligentes + Alocação Automática**

---

## 🌟 Descrição Geral

Nesta etapa, o RC-FINANCE-IA evolui para se tornar um **gestor de metas financeiras realista, adaptativo e orientado por IA**. O objetivo é permitir que o sistema compreenda, armazene e acompanhe metas como:

- Reserva de emergência
- Caixa de giro
- Compra de ativos
- Quitação de dívidas
- Metas empresariais ou familiares

Além de cadastrar metas manualmente, o sistema oferecerá sugestões automáticas de alocação com base no saldo disponível, prioridades definidas e vencimentos próximos.

---

## ⚖️ Ferramentas e Requisitos

- Banco de dados SQLite (com nova tabela `goals`)
- Interface `Streamlit` com abas para gerenciar metas
- Módulo IA (simples) para sugerir alocações por prioridade
- Formulário de criação e listagem com filtros

---

## 🔧 Guia Passo a Passo

### 1. Criar Tabela `goals` no banco

Em `scripts/init_db.py`, adicione:

```python
# cria tabela goals
if "goals" not in db.table_names():
    db["goals"].create({
        "id": int,
        "name": str,
        "target_amount": float,
        "current_amount": float,
        "due_date": str,
        "priority": str,
        "type": str
    }, pk="id")
```

### 2. Atualizar interface no `scripts/ui.py`

Na aba lateral, já existe a opção "Metas". Substitua o bloco de controle `if aba == ...` por:

```python
if aba == "Visualizar":
    st.dataframe(list(db["goals"].rows))

elif aba == "Cadastrar":
    with st.form("form_metas"):
        nome = st.text_input("Nome da Meta")
        valor = st.number_input("Valor Alvo", step=10.0)
        atual = st.number_input("Valor Atual", step=10.0)
        data = st.date_input("Data Limite")
        prioridade = st.selectbox("Prioridade", ["Alta", "Média", "Baixa"])
        tipo = st.selectbox("Tipo", ["Reserva", "Investimento", "Dívida", "Outro"])
        enviar = st.form_submit_button("Salvar Meta")

    if enviar:
        db["goals"].insert({
            "id": int(time.time()),
            "name": nome,
            "target_amount": valor,
            "current_amount": atual,
            "due_date": str(data),
            "priority": prioridade,
            "type": tipo
        })
        st.success("Meta salva!")
        st.rerun()

elif aba == "Importar":
    st.info("Importação de metas via CSV em construção")

elif aba == "Exportar":
    metas = list(db["goals"].rows)
    df = pd.DataFrame(metas)
    st.download_button("Exportar CSV", df.to_csv(index=False), "metas.csv")
```

### 3. Sugerir Alocações Inteligentes

Ainda no final da página "Metas", adicione:

```python
st.subheader("🔮 Sugestão de Alocação Inteligente")
goals = list(db["goals"].rows)
saldo = sum([t["amount"] for t in db["transactions"].rows if t["type"] == "income"])

if saldo > 0 and goals:
    st.write(f"Saldo disponível: R$ {saldo:.2f}")
    sugestoes = []
    for g in goals:
        fator = 0.5 if g["priority"] == "Alta" else (0.3 if g["priority"] == "Média" else 0.2)
        sugestao = min(g["target_amount"] - g["current_amount"], saldo * fator)
        sugestoes.append((g["name"], sugestao))
    for nome, valor in sugestoes:
        st.write(f"Sugestão: Alocar R$ {valor:.2f} para meta '{nome}'")
else:
    st.info("Sem metas ou saldo para sugerir")
```

---

## 🚀 Validação

- Cria metas com sucesso
- Exibe corretamente metas salvas
- Sugere alocações realistas com base no saldo e prioridade
- Exporta metas como CSV

---

## ⚠️ Gestão de Erros

### 🚫 Erro mais comum:

**"Tabela 'goals' inexistente"**

- **Sinais:** erro ao tentar salvar ou listar metas
- **Causa:** não inicializou banco com `init_db.py`
- **Impacto:** funcionalidades de metas não funcionam

### ✅ Quick Fix:

1. Rodar `init_db.py` novamente:

```bash
python scripts/init_db.py
```

2. Verificar se `data/finance.db` contém tabela "goals"

### 🔁 Contingência:

- Deletar `finance.db` e reiniciar banco (atenção: perderá transações)

### 🔐 Prevenção:

- Sempre rodar `init_db.py` antes do primeiro uso
- Testar funcionalidade de salvar metas após inicializar projeto

---

🚀 Capítulo 5 concluído! Deseja avançar para o **Capítulo 6: Input de Voz com STT (Whisper)**?

