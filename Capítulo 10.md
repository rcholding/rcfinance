**Capítulo 10 – Fase 6: Entrada Inteligente de Dados (Upload, OCR e Classificação)**

---

## 💡 Visão Geral
Nesta etapa, implementamos a capacidade do RC-FINANCE-IA de receber dados de forma automatizada e inteligente, incluindo:

- **Upload de arquivos** financeiros (CSV, JSON, imagens de notas)
- **OCR (Reconhecimento de Texto)** com Tesseract para extrair informações de imagens e PDFs
- **Classificação automática por IA** para categorizar transações com base na descrição e no contexto

O objetivo é permitir que o usuário alimente o sistema sem precisar digitar manualmente todas as transações, garantindo praticidade e velocidade.

---

## 🔧 Ferramentas & Requisitos

- Biblioteca `pytesseract`
- Executável do Tesseract instalado no sistema (Windows ou Linux)
- Módulo `streamlit` para input
- Módulo `pandas` para parse de CSV
- Módulo `sentence_transformers` para classificação IA
- Imagens de nota, extratos (JPG, PNG, PDF), CSV/JSON de transações

---

## ✅ Procedimento Passo a Passo

### 1. Instalar o Tesseract

- Baixe e instale: [https://github.com/tesseract-ocr/tesseract](https://github.com/tesseract-ocr/tesseract)
- Configure o path para o executável no script (ex: `tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'`)

### 2. Criar pasta `scripts/utils/ocr.py`
```python
import pytesseract
from PIL import Image

def extrair_texto(imagem_path):
    return pytesseract.image_to_string(Image.open(imagem_path))
```

### 3. Atualizar `scripts/ui.py` para importar e usar OCR
```python
from scripts.utils.ocr import extrair_texto
uploaded = st.file_uploader("Envie uma imagem ou PDF escaneado")
if uploaded:
    with open("temp.png", "wb") as f:
        f.write(uploaded.getbuffer())
    texto_extraido = extrair_texto("temp.png")
    st.text_area("Texto Extraído", texto_extraido)
```

### 4. Importar CSV/JSON e cadastrar
```python
arquivo = st.file_uploader("Envie CSV de transações")
if arquivo:
    df = pd.read_csv(arquivo)
    for _, row in df.iterrows():
        db["transactions"].insert({
            "id": int(time.time()),
            "type": row["type"],
            "description": row["description"],
            "amount": row["amount"],
            "category": classify_transaction(row["description"]),
            "date": row["date"]
        })
    st.success("Transações importadas com sucesso!")
```

### 5. Classificador IA (referenciado de `scripts/utils/ai_classifier.py`)
```python
from sentence_transformers import SentenceTransformer, util

model = SentenceTransformer('paraphrase-MiniLM-L6-v2')
classes = ["Alimentação", "Transporte", "Salário", "Lazer", "Contas"]

def classify_transaction(desc):
    embedding = model.encode(desc, convert_to_tensor=True)
    sims = [util.pytorch_cos_sim(embedding, model.encode(c, convert_to_tensor=True)) for c in classes]
    return classes[sims.index(max(sims))]
```

---

## ✔️ Indicadores de Sucesso
- Sistema recebe arquivos CSV, JSON, imagens e extrai dados automaticamente
- IA classifica corretamente categorias sem necessidade de digitação
- OCR funciona com imagens nítidas e transações importadas sem erros

---

## ❌ GESTÃO DE ERROS (Bloco Padrão)

### 🚫 Erro mais comum:
`pytesseract.pytesseract.TesseractNotFoundError`

**Sinais:** Mensagem de erro ao extrair texto.

**Causa provável:** Tesseract não está instalado ou não configurado corretamente no PATH.

**Impacto:** Falha total na extração de texto por OCR.

---

### ✅ Quick Fix:
1. Instale o Tesseract do link oficial.
2. Localize o caminho do executável (ex: `C:\Program Files\Tesseract-OCR\tesseract.exe`)
3. Configure com:
```python
pytesseract.pytesseract.tesseract_cmd = r'C:\...\tesseract.exe'
```

---

### 🔁 Contingência:
- Use OCR online gratuito como alternativa temporária
- Permita input manual caso OCR falhe

---

### 🔒 Prevenção:
- Verificar `pytesseract.get_tesseract_version()` na inicialização
- Validar extensão de arquivos
- Alertar se resolução da imagem for baixa (<200dpi)

---

**Capítulo 10 concluído com sucesso! Deseja seguir para o último: Capítulo 11 - Implantacao Local e Publicação na Web?**

