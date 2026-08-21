4. README.md
Markdown
# Agente de IA para Power BI

Assistente conversacional em Python/Streamlit integrado ao Power BI via iframe com filtros dinâmicos e conexão ao OpenRouter.

## Como Executar Localmente

1. Clone este repositório:
```bash
git clone [https://github.com/seu-usuario/meu-agente-powerbi.git](https://github.com/seu-usuario/meu-agente-powerbi.git)
cd meu-agente-powerbi
Instale as dependências:

Bash
pip install -r requirements.txt
Defina sua chave de API e execute:

Bash
export OPENROUTER_API_KEY="sua-chave-aqui"
streamlit run app.py
Deploy no Streamlit Cloud
Conecte seu repositório no Streamlit Community Cloud.

Adicione OPENROUTER_API_KEY na seção Secrets.

Obtenha a URL pública gerada e configure a medida DAX no Power BI.


---

### Como criar o arquivo `.zip` no seu computador:

1. Crie uma pasta chamada `meu-agente-powerbi`.
2. Crie cada um dos 4 arquivos acima com seus respectivos conteúdos dentro dessa pasta.
3. Compacte a pasta em `.zip` (ou use `git init`, `git add .`, `git commit` e faça o

Código DAX: URL_Chatbot_Dinamico
Snippet de código
URL_Chatbot_Dinamico = 
// =========================================================================
// 1. CAPTURA DOS FILTROS ATIVOS (SEGMENTADORES / SLICERS)
// =========================================================================
VAR vAno = 
    SELECTEDVALUE(dCalendario[Ano], "Todos os Anos")

VAR vMes = 
    SELECTEDVALUE(dCalendario[NomeMes], "Todos os Meses")

// Concatena múltiplas seleções se o utilizador selecionar mais do que um item
VAR vRegiao = 
    IF(
        ISFILTERED(dClientes[Regiao]),
        CONCATENATEX(
            VALUES(dClientes[Regiao]), 
            dClientes[Regiao], 
            ", "
        ),
        "Todas as Regioes"
    )

VAR vCategoria = 
    IF(
        ISFILTERED(dProdutos[Categoria]),
        CONCATENATEX(
            VALUES(dProdutos[Categoria]), 
            dProdutos[Categoria], 
            ", "
        ),
        "Todas as Categorias"
    )

// =========================================================================
// 2. CAPTURA DAS PRINCIPAIS MÉTRICAS / KPIS DA PÁGINA
// =========================================================================
VAR vFaturamento = 
    FORMAT([Total Vendas], "R$ #,##0.00")

VAR vMargem = 
    FORMAT([Margem Lucro %], "0.0%")

VAR vQtdPedidos = 
    FORMAT([Qtd Pedidos], "#,##0")

VAR vTicketMedio = 
    FORMAT([Ticket Medio], "R$ #,##0.00")

// =========================================================================
// 3. MONTAGEM DA QUERY STRING E DA URL BASE
// =========================================================================
// Substitua pelo endereço gerado no seu deploy (Streamlit Cloud, Hugging Face, etc.)
VAR vUrlBase = "https://seu-chat.streamlit.app/?embed=true"

VAR vQueryString = 
    "&ano=" & vAno & 
    "&mes=" & vMes & 
    "&regiao=" & vRegiao & 
    "&categoria=" & vCategoria & 
    "&faturamento=" & vFaturamento & 
    "&margem=" & vMargem & 
    "&pedidos=" & vQtdPedidos & 
    "&ticket=" & vTicketMedio

// Codificação simples para evitar quebras com espaços e caracteres especiais na URL
VAR vQueryStringTratada = 
    SUBSTITUTE(
        SUBSTITUTE(
            SUBSTITUTE(vQueryString, " ", "%20"),
            "&", "%26"
        ),
        ",", "%2C"
    )

// Junta a URL base aos parâmetros tratados
VAR vUrlFinal = vUrlBase & vQueryStringTratada

// =========================================================================
// 4. RETORNO DO IFRAME HTML
// =========================================================================
RETURN
"<iframe src='" & vUrlFinal & "' " &
"width='100%' " &
"height='100%' " &
"style='border: none; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);' " &
"allow='clipboard-read; clipboard-write'>" &
"</iframe>"
Como aplicar no Power BI:
Criar a Medida:

Vá ao separador Modelação (Modeling) > Nova Medida (New Measure).

Cole o código acima e substitua as tabelas e medidas (dCalendario[Ano], [Total Vendas], etc.) pelos nomes reais do seu modelo.

Altere a variável vUrlBase com o link público da sua aplicação.

Configurar o Visual:

Insira o visual HTML Content na página do relatório.

Arraste a medida [URL_Chatbot_Dinamico] para o campo Values do visual.

Redimensione o visual para a largura e altura pretendidas na página# chat_pbi
