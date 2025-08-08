**Capítulo 4 – Fase 3: Importação Inteligente (CSV, JSON, OCR)**

---

## 📊 Descrição Geral

Esta etapa tem como objetivo habilitar o RC-FINANCE-IA a **receber dados financeiros por meio de arquivos**, de forma prática e inteligente. Serão suportados formatos como **CSV**, **JSON** e **PDF/Imagem** (via OCR), otimizando o cadastro de transações e metas.

O usuário poderá carregar um arquivo de extrato bancário, planilha pessoal ou nota fiscal escaneada, e o sistema irá processar, identificar e inserir automaticamente os dados no banco de dados, com classificação IA opcional.

---

## ⚖️ Ferramentas e Dependências

- Python (mesmo ambiente `iafinance`)
- Biblioteca `pytesseract`
- Biblioteca `Pillow` (para manipulação de imagem)
- Biblioteca `pdf2image`
- Instalador do **Tesseract OCR**:
  - Windows: [https://github.com/tesseract-ocr/tesseract/wiki](https://github.com/tesseract-ocr/tesseract/wiki)
- CSV / JSON: nativas via `pandas`

### Instalar dependências:

```bash
pip install pytesseract Pillow pdf2image
```

---

## 🔧 Guia Passo a Passo

### 1. **Instale o Tesseract OCR (somente uma vez):**

- Acesse o link acima e baixe a versão Windows Installer.
- Durante a instalação, habilite a opção **"Add to PATH"**.
- Anote o caminho completo (exemplo: `C:\Program Files\Tesseract-OCR\tesseract.exe`).

### 2. **Configure o Tesseract no Python:**

Adicione este código no topo de `scripts/utils/ocr_importer.py`:

```python
import pytesseract
pytesseract.pytesseract.tesseract_cmd = r"C:\\Program Files\\Tesseract-OCR\\tesseract.exe"
```

### 3. **Criar pasta **``** e script de importação:**

```bash
mkdir scripts\utils
code scripts\utils\ocr_importer.py
```

### 4. **Adicione o seguinte código base:**

```python
from PIL import Image
import pytesseract
import os

def extrair_texto(caminho):
    texto = pytesseract.image_to_string(Image.open(caminho), lang='por')
    return texto
```

### 5. **Criar interface para upload em **``**:**

Adicione no bloco `if page == "Transações":`, abaixo do form:

```python
st.subheader("Importar por Arquivo")
arquivo = st.file_uploader("Escolha um arquivo", type=["csv", "json", "png", "jpg", "pdf"])

if arquivo is not None:
    if arquivo.name.endswith("csv"):
        df = pd.read_csv(arquivo)
        for _, row in df.iterrows():
            db["transactions"].insert(row.to_dict(), alter=True)
        st.success("Transações importadas com sucesso!")
    elif arquivo.name.endswith("json"):
        df = pd.read_json(arquivo)
        for _, row in df.iterrows():
            db["transactions"].insert(row.to_dict(), alter=True)
        st.success("Importação concluída!")
    elif arquivo.name.endswith(("png", "jpg")):
        from scripts.utils.ocr_importer import extrair_texto
        with open("temp_img.png", "wb") as f:
            f.write(arquivo.read())
        texto = extrair_texto("temp_img.png")
        st.text_area("Texto Reconhecido:", texto)
```

---

## ✅ Pontos de Validação

- Upload aceita diferentes formatos
- Dados inseridos no banco com sucesso
- OCR extrai texto corretamente de imagens

---

## ⚠️ Gestão de Erros

### 🚫 Erro mais comum:

**"Tesseract is not installed or not in PATH"**

- **Sinais:** erro ao importar imagem ou usar OCR
- **Causa:** Tesseract não instalado ou não reconhecido pelo sistema
- **Impacto:** OCR não funciona, apenas CSV/JSON disponíveis

### ✅ Quick Fix:

1. Verifique se o Tesseract está instalado com o caminho correto
2. Abra terminal e rode:
   ```bash
   tesseract -v
   ```
   Se funcionar, copie o caminho e atualize `tesseract_cmd` no script

### 🔁 Contingência:

- Ignore arquivos de imagem temporariamente e continue com CSV/JSON

### 🔒 Prevenção:

- Sempre testar OCR após instalação
- Documentar caminho no código e README

---

🚀 Capítulo 4 pronto!

Deseja que eu inicie o **Capítulo 5: Metas Inteligentes + Alocação Automática**?

