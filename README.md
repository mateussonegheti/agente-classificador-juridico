

# Agente Classificador Jurídico

## Visão Geral

O "Agente Classificador Jurídico" é um projeto que visa **otimizar a classificação e organização de documentos jurídicos** utilizando inteligência artificial. Atualmente, o projeto está na fase de **simulação (protótipo)**, com o objetivo de futuramente desenvolver um agente de IA autônomo capaz de aprender e refinar suas classificações continuamente.

## O Problema

Em **sistemas legados de processamento jurídico**, a classificação inconsistente de documentos é um desafio. Erros na categorização podem levar à **eliminação indevida de documentos importantes**, como sentenças que são erroneamente registradas como despachos, colocando em risco registros de "Guarda Permanente".

## A Solução Proposta: Reclassificação Assistida por IA

A solução prevê uma **reclassificação assistida**, onde a IA atua como um auxiliar para identificar e corrigir inconsistências. A intenção não é substituir a intervenção humana, mas sim:
*   Classificar documentos de uma base de dados.
*   **Identificar discrepâncias** entre a classificação da IA e a classificação existente (legada) no sistema.
*   **Focar a análise humana** apenas nos casos de divergência, permitindo que a equipe verifique a classificação correta.
*   Implementar um **ciclo de treinamento fechado**, onde as correções manuais são usadas para aprimorar a precisão do agente ao longo do tempo.

## Status Atual: Simulador (Protótipo)

A versão atual é um **simulador de agente**, que opera com a técnica de *few-shot prompting*. Isso significa que a IA classifica documentos com base em **exemplos predefinidos diretamente no prompt** do código, sem registrar o aprendizado de forma autônoma. Sua capacidade pode ser melhorada adicionando-se novos exemplos no formato de perguntas/respostas ao prompt.

O simulador é uma **aplicação web em um único arquivo HTML** com estilização Tailwind CSS. Suas funcionalidades incluem:
*   **Entrada de Dados**: Permite colar texto de um documento ou fazer upload de arquivos **PDF ou CSV**. Para CSVs, pode baixar PDFs a partir de URLs em uma coluna 'LINK'.
*   **Extração de Conteúdo**: Utiliza `pdf.js` para texto nativo e `tesseract.js` para **OCR** como fallback, se a extração nativa falhar. A leitura do **inteiro teor** do documento é crucial.
*   **Classificação**: Conecta-se à API do Gemini para classificar documentos jurídicos, retornando informações detalhadas em formato JSON com critérios como `tipo_documento`, `classificacao` (`Guarda Permanente`, `Eliminação`, `Avaliação Manual`), `palavra_chave_encontrada`, e `motivo`. A `classificacao` é inferida do `tipo_documento`.
*   **Saída de Dados**: Exibe resultados na tela para documentos individuais e gera um **arquivo CSV para download** em processamento em lote, contendo "nome do documento", "Classificação", "tipo de documento", "palavra-chave encontrada", "motivo" e "inteiro teor".
*   **Experiência do Usuário**: Inclui indicadores de carregamento e progresso, além de mensagens para erros e sucesso.

## Próximos Passos e Visão Futura

O projeto visa evoluir para um **agente especializado, independente do ambiente Gemini**. Para isso, a disponibilização de uma **chave API** é um passo fundamental.

Uma funcionalidade futura importante será um **mecanismo de feedback para aprimoramento contínuo**:
1.  O simulador classifica e gera uma planilha CSV.
2.  O usuário corrige inconsistências na planilha e marca as linhas corrigidas.
3.  O agente usará as linhas corrigidas para gerar novos prompts de aprendizado, aprimorando sua acurácia.

Considerações futuras incluem a otimização de **armazenamento para o "inteiro teor"** dos documentos, possivelmente em repositórios em nuvem, e um **módulo de adaptação de Input** para integrar a aplicação com diferentes sistemas, repositórios ou bancos de dados.

## Pilha Tecnológica (Protótipo Atual)

*   **Front-end**: HTML, Tailwind CSS, JavaScript.
*   **Processamento de PDFs**: `pdf.js` (extração nativa) e `tesseract.js` (OCR).
*   **Processamento de CSVs**: `PapaParse` (para leitura e extração de URLs).
*   **IA/Classificação**: Gemini API (modelo `gemini-2.5-flash-preview-05-20`) para classificação via prompt.
*   **Acesso a URLs externas**: `cors-anywhere.herokuapp.com/` (PROXY_URL) para contornar restrições de CORS ao baixar documentos de links em CSVs.

## Como Usar (Protótipo)

Para utilizar o simulador, siga os passos abaixo:

1.  **Clone o repositório:**
    ```bash
    git clone [URL_DO_SEU_REPOSITORIO]
    cd [nome_do_seu_repositorio]
    ```
2.  **Abra o arquivo HTML:** Simplesmente abra o arquivo `index.html` (ou o nome do arquivo HTML principal) em seu navegador web.
3.  **Insira a Chave API do Gemini (necessário para classificação via IA):**
    *   No código-fonte (`script` no HTML), localize a linha `const apiKey = "";`.
    *   Substitua as aspas vazias pela sua chave de API do Gemini para que o simulador possa se conectar ao modelo de IA.
4.  **Classifique Documentos:**
    *   Você pode colar o texto de um documento jurídico diretamente na caixa de texto.
    *   Ou, selecione um ou mais arquivos **PDF** ou **CSV** para processamento em lote. Se você selecionar um CSV, certifique-se de que ele contenha uma coluna chamada "LINK" com URLs para documentos PDF online.
5.  **Visualize e Baixe os Resultados:**
    *   Os resultados da classificação serão exibidos na tela.
    *   Para processamento em lote, um botão "Download Resultados (CSV)" aparecerá, permitindo que você baixe um arquivo CSV completo com todas as classificações e detalhes.

## Contribuição

O projeto está em desenvolvimento contínuo. Novas atualizações e funcionalidades serão implementadas.

