**Capítulo 6 – Fase 5: Entrada de Voz (Reconhecimento de Fala - Whisper)**

---

## 🎤 Descrição Geral

Este capítulo apresenta a implementação de um sistema de **entrada de dados financeiros via voz**, onde o usuário pode gravar mensagens faladas e a IA converte em texto com transações estruturadas (valores, categorias, datas etc.). Isso proporciona praticidade e acessibilidade para o RC-FINANCE-IA.

Usaremos o **Whisper** da OpenAI para o reconhecimento de voz. As mensagens serão processadas, transcritas e analisadas para sugerir entradas automáticas de transações.

---

## ⚖️ Ferramentas e Requisitos

- Python 3.11 (ambiente `iafinance`)
- Biblioteca `openai-whisper`
- Biblioteca `streamlit_audio_recorder` (ou alternativa local)
- Biblioteca `pydub`, `soundfile`, `ffmpeg`
- Acesso à CPU ou GPU com suporte a Whisper

### Instalar dependências:

```bash
pip install git+https://github.com/openai/whisper.git
pip install streamlit_audio_recorder pydub soundfile
```

> **Nota**: certifique-se que o `ffmpeg` esteja instalado e no PATH ([https://ffmpeg.org/download.html](https://ffmpeg.org/download.html))

---

## 🛠️ Guia Passo a Passo

### 1. Criar novo script `scripts/utils/speech_to_text.py`:

```python
import whisper
import os

def transcrever_audio(caminho_audio):
    modelo = whisper.load_model("base")
    resultado = modelo.transcribe(caminho_audio, language='pt')
    return resultado["text"]
```

### 2. Adicionar gravador de voz na interface `ui.py`

No bloco `if page == "Transações":`, após o formulário de transação, adicione:

```python
st.subheader("Entrada por Áudio")
from streamlit_audio_recorder import audio_recorder
from scripts.utils.speech_to_text import transcrever_audio

audio = audio_recorder()

if audio:
    with open("voz.wav", "wb") as f:
        f.write(audio)
    texto = transcrever_audio("voz.wav")
    st.text_area("Transcrição:", texto)
```

> Opcionalmente, pode-se aplicar classificação de categoria via IA e preenchimento automático dos campos com o texto transcrito.

---

## ✅ Validação

- ✔️ Audio gravado e salvo como `.wav`
- ✔️ Whisper transcreve corretamente falas simples como: "gastei 50 reais com supermercado dia 4 de agosto"
- ✔️ Texto exibido na tela para revisão manual

---

## ⚠️ Gestão de Erros

### 🚫 Erro mais comum:

**"ffmpeg not found"**

- **Sinais**: erro ao processar `.wav`, Whisper não funciona
- **Causa**: `ffmpeg` não instalado ou fora do PATH
- **Impacto**: não é possível transcrever

### ✅ Quick Fix:

1. Instale o ffmpeg via chocolatey (Windows):
   ```bash
   choco install ffmpeg
   ```
2. Reinicie o terminal e confirme com:
   ```bash
   ffmpeg -version
   ```

### 🔁 Contingência:

- Permitir upload manual de arquivos `.wav` caso a gravação falhe

### 🔒 Prevenção:

- Verificar ffmpeg após instalação
- Testar trecho simples (3–5 segundos) antes de usar

---

🚀 Capítulo 6 pronto para execução! Deseja iniciar o **Capítulo 7: OCR para Notas Fiscais e Imagens**?

