# Desafio DIO: Laboratório com Azure Speech Studio e Language Studio

Este repositório documenta minha experiência prática utilizando as ferramentas **Azure Speech Studio** e **Language Studio** para análise de fala e linguagem natural, como parte do desafio da DIO.

---

## **Objetivos do Desafio**
- Praticar conceitos de **Speech-to-Text** e **Text-to-Speech** usando o Azure Speech Studio.
- Explorar **análise de linguagem natural** no Azure Language Studio (sentimento, tradução, extração de entidades etc.).
- **Documentar a experiência** de forma clara e estruturada no GitHub.
- Desenvolver habilidades para **compartilhar conhecimento técnico**.

---

## **Ferramentas Utilizadas**
- **Microsoft Azure Speech Studio**  
- **Microsoft Azure Language Studio**  
- **GitHub** (para versionamento e documentação)  
- [Markdown](https://guides.github.com/features/mastering-markdown/) para formatação

---

## **Passo a Passo da Prática**

### 1. Preparação do Ambiente
- Criei conta no Azure e confirmei acesso às ferramentas.
#### Utilização dos sites:
- https://aka.ms/ai900-speech
- https://aka.ms/ai900-text-analysis
- https://speech.microsoft.com/portal
- https://language.cognitive.azure.com

#### Demais configurações:
- Em relação às configurações Git e local, não foi necessário, pois o ambiente já estava preparado.
  Caso necessário, você dever instalar a versão do Git correspondente e configurar os parâmetros
  de conexão com GitHub, além de instalar o Vs Code.

---

### 2. Experimentos no Speech Studio
- **Speech-to-Text:**  
  - Texto de teste usado: AI enables us to build amazing software that can improve healthcare, enable people to overcome physical disadvantages, empower smart infrastructure, create incredible entertainment experiences, and even save the planet.   
  
  - Idioma selecionado: English
  - Resultados observados:  
    
    - Rápida análise do arquivo de áudio;
    - Precisão na tradução do áudio para o texto;
    - Escrita e pontuação corretas;
    - Possíveis aplicações práticas para construção de produtos/serviços.

- **Text-to-Speech:**  
  - Texto de teste usado: 
        `Olá! Estou pronto para transformar cada palavra em uma experiência incrível.Vamos dar vida às suas ideias com uma voz cheia de energia!`
  - Voz escolhida: `Thalita Multilingual`
  - Observações:  
    - Excelente qualidade da voz;
    - Clareza e leitura fluida, quase indistinguível de uma pessoal real;
    - Devido ao acesso restrito, não foi possível utilizar SSML no teste.

- [text_to_speech_step_1](files_images/text_to_speech_step_1.png)

- [text_to_speech_step_2](files_images/text_to_speech_step_2.png)

---

### 3. Experimentos no Language Studio
- **Análise de Sentimento:**  
  - Texto analisado:    Tired hotel with poor service
                        The Royal Hotel, London, United Kingdom
                        5/6/2018
                        This is an old hotel (has been around since 1950's) and the room furnishings are average - becoming a bit old now and require changing. The internet didn't work and had to come to one of their office rooms to check in for my flight home. The website says it's close to the British Museum, but it's too far to walk.  
    - Resultado: `Negative`
    - Contexto e detalhes: No Language Studio, selecionei a funcionalidade Análise de Sentimento, carreguei o texto e defini o idioma como English (US). O modelo retornou classificação negativa com alta confiança, destacando trechos relacionados a falhas no serviço, qualidade do quarto e problemas de conectividade. O resultado foi coerente com o conteúdo do texto, evidenciando a capacidade da ferramenta em identificar pontos críticos na experiência do cliente. Além disso, a ferramenta permite ajustar parâmetros como detecção automática de idioma ou modo em lote (batch mode) para processar grandes volumes de texto.

- [analysis_sentiment_file.json](files_images/analysis_sentiment_file.json)
- [analysis_sentiment_step_1](files_images/analysis_sentiment_step_1.png)
- [analysis_sentiment_step_2](files_images/analysis_sentiment_step_2.png)
- [analysis_sentiment_step_3](files_images/analysis_sentiment_step_3.png)

---

## **Resultados e Insights**
- Principais aprendizados sobre **uso do Speech Studio**:  
  - `Interface intuitiva com inúmeras possibilidades de criação para desenvolvimento de aplicações que podem ser aplicadas em diversos segmentos, desde micro até macro organizações. Exemplo: Supermercados, Hospitais, Serviço Público e outros.`
- Principais aprendizados sobre **uso do Language Studio**:  
  - `Ferramenta versátil para compreensão de sentimento de cliente, possibilitando diversas aplicações em empresas do varejo e demais setores que necessitam compreender a percepção do cliente sobre um produto ou marca.`
- Desafios enfrentados:  
  - `Compreendo que a ocorrência de custos pode ser um limitador quanto a ampla adoção e aplicação do serviço, contudo se bem estruturada, torna-se uma ferramenta de grande contribuição para a evolução dos serviços e os custos se justificam.`
- Possíveis aplicações futuras:  
  - `Compreendo que existe uma grande oportunidade de mercado para segmentos que atuam com atendimento ao público e no setor de vendas (varejo e outros).`

---

## **Conclusão e Próximos Passos** 

Este laboratório permitiu explorar, de forma prática, os recursos do Azure Speech Studio e do Language Studio, reforçando como inteligência artificial pode agregar valor a aplicações que envolvem fala e linguagem natural. A experiência mostrou que essas ferramentas oferecem alta precisão, flexibilidade e potencial de integração em diferentes setores, como varejo, saúde, educação e atendimento ao cliente.

Como próximos passos, pretendo:

- Realizar testes adicionais com SSML (Speech Synthesis Markup Language) para personalizar entonação e pausas.

- Explorar o modo em lote (batch mode) no Language Studio para processar grandes volumes de dados.

- Integrar os serviços a uma aplicação real para validar desempenho em cenários práticos.

---
## **Estrutura do Repositório**
```plaintext
azure-speech-language-lab/
├── README.md          # Documentação principal
├── notas.md           # Anotações detalhadas (opcional)
└── images/            # Capturas de tela e diagramas