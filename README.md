# Projeto Reserva Canábica - Antropológico e Fitotécnico

Este projeto integra uma base de dados Google Planilhas (via Google Apps Script) e o Google Colab/Local (Python) para análises avançadas utilizando as APIs do Gemini, Pandas, Seaborn e Matplotlib, permitindo a extração de Laudos Antropológicos em PDF de forma automatizada.

## Estrutura do Projeto

O código inteiro, tanto o pipeline de Python quanto as interfaces HTML e GAS, foi consolidado de forma a tornar o projeto 100% pronto para produção num formato Single-Source-of-Truth no arquivo `notebook.py`.

* **`notebook.py`**: A aplicação principal em Python. Pode ser rodada tanto no Google Colab como num ambiente local e serve como backend do sistema. Ele embute os códigos Google Apps Script (GAS) e a interface UI em constantes (`CODIGO_GAS` e `INTERFACE_HTML`).
* **`requirements.txt`**: Mapeamento das bibliotecas (dependencies) necessárias para a execução.

## Pronta para Produção

As seguintes melhorias de Pronta para Produção foram implementadas:
1. **Tratamento Seguro de Secrets**: Chaves API (Gemini) podem ser configuradas via variável de ambiente `GEMINI_API_KEY` ou via argumento via linha de comando (`--api-key`).
2. **Ambiente Isolado**: Remoção de atalhos e hardcoding para ser livre de Colab lock.
3. **Pilha de Erros e Logging Customizado**: A saída de processamento é realizada via `logging`, gerando traços confiáveis para stdout ao invés de simples `print`.
4. **Gerenciador de Diretórios de Saída**: As saídas são jogadas dinamicamente para o `/tmp/` no Colab ou para `./output_relatorios` localmente.
5. **Gereção FPDF Refinada**: Evita-se erros de decode com tratativas de `latin-1` suportadas pelos renderizadores PDF clássicos.
6. **Interface de Linha de Comando (CLI)**: Utilização do módulo `argparse`.

### Como Instalar (Local)

1. Crie o seu ambiente virtual:
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # ou .venv\Scripts\Activate.ps1 no Windows
   ```

2. Instale as dependências:
   ```bash
   pip install -r requirements.txt
   ```

### Como Rodar

Para acionar a pipeline experimentalmente ou em produção (recebendo um arquivo payload customizado com dados da planilha, ou utilizando mock-data em caso de omissão do payload):

```bash
# Definindo chave ambiente
export GEMINI_API_KEY="SUA-CHAVE-API-AQUI"

# Executando análise com mock data default
python notebook.py

# Se quiser fornecer um JSON exportado do Google Sheets:
python notebook.py --payload mock_test_data.json
```

Isso criará uma pasta local `output_relatorios` que será populada com:
- `.png` Gráficos (NetworkX, Seaborn, Matplotlib).
- `.html` Mapas do Folium.
- `.pdf` Laudo Antropológico.
- Arquivos `.gs` (Google Apps Script) e `.html` exportados pela ferramenta, caso queira copiá-los de volta para a nuvem.
