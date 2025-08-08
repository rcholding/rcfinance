**Capítulo 7 – Fase 6: OCR para Notas Fiscais e Imagens**

---

## 📊 Descrição Geral

Nesta fase, o RC-FINANCE-IA ganhará a capacidade de **importar informações financeiras a partir de imagens e PDFs escaneados**, como notas fiscais, recibos, boletos e comprovantes bancários. A funcionalidade utiliza **OCR (Reconhecimento Óptico de Caracteres)** para extrair dados relevantes como valores, datas e categorias.

Essa etapa torna o sistema ainda mais prático e eficaz, reduzindo a digitação manual de despesas.

---

## ⚖️ Ferramentas e Requisitos

- Python 3.11 (ambiente `iafinance`)
- Biblioteca `pytesseract` (OCR)
- Biblioteca `Pillow`, `opencv-python`
- OCR Engine: [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) instalado

### Instalação de dependências:

```bash
pip install pytesseract opencv-python Pillow
```

> ⚠️ **Requer Tesseract instalado** e incluído no PATH do sistema. [Download](https://github.com/UB-Mannheim/tesseract/wiki)

---

## 🛠️ Guia Passo a Passo

### 1. Instalar o Tesseract

**Windows**:

1. Baixe o instalador em: [https://github.com/UB-Mannheim/tesseract/wiki](https://github.com/UB-Mannheim/tesseract/wiki)
2. Execute e instale em `C:\Program Files\Tesseract-OCR`
3. Inclua este caminho na variável de ambiente `PATH`
4. Reinicie o terminal e teste com:

```bash
tesseract -v
```

### 2. Criar script OCR `scripts/utils/ocr.py`

```python
import pytesseract
from PIL import Image
import cv2
import os

# Define o caminho do executável do tesseract
pytesseract.pytesseract.tesseract_cmd = r"C:\\Program Files\\Tesseract-OCR\\tesseract.exe"

def extrair_texto(caminho):
    img = cv2.imread(caminho)
    img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    texto = pytesseract.image_to_string(img, lang='por')
    return texto
```

### 3. Atualizar UI `ui.py` com aba de importação

Em `if page == "Transações"`, adicione o bloco abaixo:

```python
st.subheader("Importar via OCR")
from scripts.utils.ocr import extrair_texto

arquivo = st.file_uploader("Envie nota ou recibo (JPG, PNG, PDF)", type=["jpg", "png"])
if arquivo:
    caminho_temp = os.path.join("temp_ocr.jpg")
    with open(caminho_temp, "wb") as f:
        f.write(arquivo.read())

    texto_extraido = extrair_texto(caminho_temp)
    st.text_area("Texto OCR:", texto_extraido)
```

> 🔹 Opcionalmente, podemos aplicar IA para sugerir preenchimento de campos com base nesse texto (futura etapa).

---

## ✅ Validação

- Envia arquivo .JPG e extrai com OCR
- Mostra o texto em campo editável
- Permite copiar e colar dados no formulário de transação

---

## ⚠️ Gestão de Erros

### 🚫 Erro mais comum:

**"Tesseract não encontrado"**

- **Sinais**: erro ao tentar rodar o OCR, falha na importação
- **Causa**: tesseract não instalado ou não adicionado ao PATH
- **Impacto**: OCR não funciona

### ✅ Quick Fix:

1. Verifique se Tesseract está instalado
2. Abra o terminal e rode `tesseract -v`
3. Se falhar:
   - Reinstale com opção de PATH ativada
   - Ou defina manualmente o caminho em `ocr.py`

### 🔁 Contingência:

- Disponibilizar modo de upload manual e transcrição por texto

### 🔒 Prevenção:

- Validar `tesseract -v` após instalar
- Testar OCR com imagem de teste antes do uso real

---

🚀 Capítulo 7 finalizado com sucesso! Deseja seguir para o **Capítulo 8: Upload e Importação de Extratos Bancários (.OFX/.CSV)**?

