**Capítulo 8 – Fase 7: Upload e Importação de Extratos Bancários (.OFX/.CSV)**

---

## 📃 Descrição Geral

Nesta etapa, o RC-FINANCE-IA passará a importar **extratos financeiros bancários nos formatos .OFX e .CSV**, permitindo o registro em lote de várias transações. Isso é essencial para integrar dados reais de movimentação de contas e oferecer uma visão abrangente das finanças.

A importação automática facilita a vida do usuário, evita erros manuais e alimenta a IA com histórico real para sugestões de metas e categorizacão inteligente.

---

## ⚖️ Ferramentas e Requisitos

- Python 3.11+
- `pandas` (leitura .CSV)
- `ofxparse` (leitura .OFX)
- `streamlit` para interface

### Instalar dependências:

```bash
pip install ofxparse pandas
```

---

## 🛠️ Guia Passo a Passo

### 1. Criar script: `scripts/utils/ofx_csv_import.py`

```python
import pandas as pd
import ofxparse
from io import StringIO
from datetime import datetime

def importar_csv(arquivo):
    df = pd.read_csv(arquivo)
    df = df.rename(columns={
        "Data": "date", "Descricao": "description", "Valor": "amount", "Categoria": "category"
    })
    df["type"] = df["amount"].apply(lambda x: "income" if x > 0 else "expense")
    df["id"] = [int(datetime.now().timestamp()) + i for i in range(len(df))]
    return df.to_dict(orient="records")

def importar_ofx(arquivo):
    ofx = ofxparse.OfxParser.parse(arquivo)
    transacoes = []
    for t in ofx.account.statement.transactions:
        transacoes.append({
            "id": int(datetime.now().timestamp()),
            "type": "income" if t.amount > 0 else "expense",
            "description": t.memo,
            "amount": t.amount,
            "category": "",
            "date": t.date.strftime("%Y-%m-%d")
        })
    return transacoes
```

### 2. Atualizar UI (`ui.py`) com aba de importação

Adicione no bloco `if page == "Transações":`:

```python
st.subheader("Importar Extrato Bancário")
from scripts.utils.ofx_csv_import import importar_csv, importar_ofx

arquivo_ext = st.file_uploader("Escolha o extrato", type=["csv", "ofx"])

if arquivo_ext:
    if arquivo_ext.name.endswith(".csv"):
        dados = importar_csv(arquivo_ext)
    elif arquivo_ext.name.endswith(".ofx"):
        dados = importar_ofx(arquivo_ext)

    if dados:
        st.success(f"{len(dados)} transações detectadas. Salvando...")
        for item in dados:
            db["transactions"].insert(item)
        st.rerun()
```

---

## ✅ Validação

- Upload de CSV funcional (com colunas padrão)
- Upload de OFX de bancos nacionais como Itaú, Nubank ou Inter
- Transações salvas corretamente com tipo, valor e data

---

## ⚠️ Gestão de Erros

### 🚫 Erro mais comum:

**"UnicodeDecodeError: codec can't decode byte" (CSV)**

- **Sinais**: erro no upload CSV
- **Causa**: encoding incorreto (geralmente precisa ser `ISO-8859-1` ou `utf-8-sig`)
- **Impacto**: quebra no parser e interrupção

### ✅ Quick Fix:

1. Abrir o CSV no Excel
2. Salvar como `CSV UTF-8`
3. Reimportar normalmente

### 🔁 Contingência:

- Oferecer opção de colar o conteúdo do extrato em um campo de texto (modo avançado)

### 🔒 Prevenção:

- Aceitar apenas arquivos `.csv` com separador "," ou ";"
- Testar o arquivo antes de processar (pandas `read_csv(..., nrows=2)`)

