# 🎥 Youtube-Transcription (Gemini & Faster-Whisper)

<a href="https://www.python.org/"><img src="https://img.shields.io/badge/PYTHON-000000?style=for-the-badge&logo=python&logoColor=facc56" alt="Python"></a>
<a href="https://colab.research.google.com/"><img src="https://img.shields.io/badge/GOOGLE_COLAB-000000?style=for-the-badge&logo=googlecolab&logoColor=F9AB00" alt="Google Colab"></a>
<a href="https://aistudio.google.com/api-keys"><img src="https://img.shields.io/badge/GEMINI_API-000000?style=for-the-badge&logo=google&logoColor=4285F4" alt="Google Gemini"></a>
<a href="https://github.com/SYSTRAN/faster-whisper"><img src="https://img.shields.io/badge/FASTER_WHISPER-000000?style=for-the-badge&logo=openai&logoColor=white" alt="Faster Whisper"></a>
<a href="https://docs.streamlit.io/"><img src="https://img.shields.io/badge/STREAMLIT-000000?style=for-the-badge&logo=streamlit&logoColor=FF4B4B" alt="Streamlit"></a>

Este projeto foi criado para explorar a extração e transcrição de áudios de vídeos do YouTube. Abaixo tem o link para dois notebooks (`.ipynb`) voltados para execução no Google Colab, demonstrando duas abordagens diferentes: uma via API na nuvem (**Google Gemini 2.5 Flash**) e outra via inferência local em GPU (**Faster-Whisper**).

[**Gemini**](https://colab.research.google.com/drive/19YNeiKKtJJe0YPIM_bDxGvvEE_NC06B8?usp=sharing)  
[**Faster-Whisper**](https://colab.research.google.com/drive/17hXtGpwk5aPvnSYpwU7Xj5MD33ztl0GW?usp=sharing)

## 💻 Pré-requisitos
Para executar os notebooks base, você precisará apenas de:

- Uma conta Google para acessar o **Google Colab**.
- Uma chave de API do **Google AI Studio** (necessária apenas para o notebook do Gemini).
- Para o modelo Faster-Whisper, é estritamente necessário habilitar o uso de GPU no Colab (Acelerador de Hardware -> **T4 GPU**).

## 🚀 Entendendo os Notebooks

O projeto é dividido em dois arquivos principais. Ambos compartilham a mesma etapa inicial de extração de áudio, utilizando o `yt-dlp` aliado ao `ffmpeg` para baixar a melhor qualidade de áudio do YouTube e convertê-la para o formato `.wav`.

### 1. Modelo com Gemini API (`transcrição_youtube_com_gemini.ipynb`)
Esta abordagem utiliza a inteligência artificial do Google para processar o áudio. É ideal para quem precisa de rapidez e não quer depender de poder computacional local robusto (GPU).

**Passos de Execução:**
1. **Instalação:** A primeira célula instala as dependências (`yt-dlp`, `ffmpeg`, `google-generativeai`).
2. **Autenticação:** O código busca a sua chave de API de forma segura utilizando os `Secrets` do Colab. Recomendamos criar uma chave chamada `GEMINI_API_KEY` na barra lateral de segredos do seu Colab.
3. **Upload e Transcrição:** O áudio baixado é enviado para os servidores do Google via *File API* (obrigatório para arquivos de mídia). O modelo `gemini-2.5-flash` escuta o áudio, gera o texto e, em seguida, o script deleta o arquivo da nuvem para economizar sua cota.

### 2. Modelo com Faster-Whisper (`transcrição_youtube_com_faster-whisper.ipynb`)
Esta abordagem roda o modelo de transcrição localmente na máquina virtual do Colab. O Faster-Whisper é uma reimplementação otimizada do Whisper da OpenAI, consumindo menos VRAM e rodando muito mais rápido.

**Passos de Execução:**
1. **Configuração da GPU:** Antes de rodar, certifique-se de que o Colab está configurado com a GPU T4  
(`Ambiente de execução` > `Alterar o tipo de ambiente de execução` > `GPUs: T4` > `Salvar`).
2. **Instalação:** Instala o `faster-whisper` e o `yt-dlp`.
3. **Inferência Otimizada:** O modelo (`large-v3-turbo`) é carregado na GPU. Durante a transcrição, o código processa o áudio e itera sobre cada **fragmento** de texto detectado, imprimindo na tela os timestamps exatos (início e fim) de cada fala em tempo real, antes de compilar tudo em um arquivo `.txt`.

## 🎨 Criando uma Interface com Streamlit

Se você deseja evoluir esses scripts soltos para uma aplicação web real e interativa, o **Streamlit** é a ferramenta ideal. Você pode rodar isso localmente na sua máquina para ter uma interface onde você terá acesso por meio de um URL e poderá criar uma visualização mais bonita e otimizada para sua aplicação, facilitando a utilização e proporcionando a experiência de unir um Front-End com um Back-End.

### 1. Instalação do Gerenciador de Pacotes
Para rodar a interface na sua máquina, utilize o gerenciador de pacotes da sua preferência (como o `uv`):
```shell
# Instale o uv
# MacOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### 2. Instalação do FFMpeg
> [!WARNING]
> Lembre-se de que é obrigatório ter o **FFmpeg** instalado nativamente no sistema operacional da sua máquina para que o **yt-dlp** consiga converter os áudios.

Para desenvolvimento, o ambiente Linux ou por meio da utilização do **WSL** (Windows Subsystem for Linux) é sempre mais fácil para instalação. Dessa forma, bastaria executar o seguinte comando:

```
sudo apt install ffmpeg
```

No entanto, existe a possibilidade de instalá-lo no Windows, mas é necessário seguir uma série de passos. Para isso, recomendo seguirem o tutorial do Professor João Tolentino, é simples, sem problemas e bastante visual: [Instalar FFMpeg no Windows](https://www.tolentino.pro.br/post/ffmpeg/).


### 3. Criação e Clonagem do Repositório
> [!IMPORTANT]
> Certifique-se de possuir o Git instalado em sua máquina.

Crie ou acesse a sua conta do Github, crie um repositório com um README.md ativado e execute o comando a seguir para cloná-lo em sua máquina:

```shell
# Crie um repositório na sua conta do Github e Clone-o em sua máquina
git clone https://github.com/SEU_USUARIO/NOME_REPOSITORIO.git

# Acesse a pasta
cd NOME_REPOSITORIO
```

### 4. Inicialização do Ambiente Virtual e Instalação de Dependências.
```shell
# Inicialize e crie um ambiente virtual. Por fim, ative-o.
uv init --python 3.14
uv venv

# MacOS/Linux
source .venv/bin/activate  

# Windows
.venv\Scripts\activate

# Instale as dependências
uv add streamlit yt-dlp google-generativeai faster-whisper
```

### 5. Estruture a sua aplicação (`main.py`)
Ao inicializar o gerenciador de pacotes `uv`, ele cria uma série de arquivos, sendo um deles o `main.py`. Não precisa ser necessariamente nele, mas se quer fazer algo rápido e simples, aplique toda a lógica que foi ensinada e que está disponibilizada nos notebooks e eu lhe desafio a criar uma interface de Streamlit para essa aplicação.

Abaixo darei algumas dicas de componentes que você pode utilizar para estruturar melhor a sua aplicação e gerar possíveis `insights`:

- **Barra Lateral (Sidebar):**
    - Pode adicionar um campo `st.text_input` para inserir a Chave da API do Gemini, se for usá-lo.
    - Utilizar o componente `st.selectbox` para o caso de possuir a lógica de transcrição do Gemini e do Faster-Whisper, possibilitando alternar entre os modelos.

- **Área Principal:**
    - Adicionar um simples `st.text_input` para colar a URL do vídeo do Youtube.
    - Adicionar um `st.button("Baixar vídeo")` para iniciar o fluxo de download.

- **Gerenciamento de Estado:**
    - Utilize um `st.spinner("Baixando áudio...")` para dar feedback enquanto o `yt-dlp` trabalha.
    - Após o download, utilizar o componente `st.audio()` para renderizar um player na tela, permitindo ouvir o áudio baixado.
    - Utilizar o `st.write()` ou `st.text_area()` para exibir fragentos de texto gerados pela transcrição.

### 6. Executando o Streamlit
Com o `main.py` ou o arquivo que você decidiu utilizar para criar sua interface, rode o comando abaixo no terminal, certificando-se de estar com o ambiente virtual ativado:

```shell
streamlit run main.py
```

Dessa maneira, irá ser aberto ou você pode abrir o servidor `localhost` que é gerado para visualizar a sua interface criada. Além disso, é possível utilizá-lo enquanto roda em segundo plano, permitindo visualizar as alterações, desde que a página seja atualizada (**F5**).

> [!IMPORTANT]
> Ao utilizar o Faster-Whisper localmente, é necessário certificar-se de possuir uma máquina "potente" com GPU (placa de vídeo), visto que para ter uma melhor experiẽncia, é necessário ter a configuração **cuda** ativada. Por fim, assegure-se de possuir bastante memória, visto que durante a sua utilização, ele irá consumi-la em grande escala (recomenda-se entre 16GB para cima).
