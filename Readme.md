# Módulo para Assistente de Correção de Questões Abertas - Provas Virtuais Web 3.0


A correção de provas em ambientes de educação a distância pode ser realizada de maneira automática para questões objetivas. No entanto, a correção de questões discursivas costuma ser manual e trabalhosa.
 
Pesquisas na área de inteligência artificial e processamento de linguagem natural reportam que é possível reduzir o esforço humano na correção de questões discursivas e até mesmo de redações. Vale destacar algoritmos que analisam a ortografia e gramática com alto nível de precisão e algoritmos que extraem indicadores de questões discursivas, como fuga do tema. De forma geral, esses últimos algoritmos funcionam analisando questões já corrigidas pelo professor. Se uma resposta obteve uma pontuação baixa, então outras respostas similares (tanto no tema quanto na estrutura) também terão uma baixa pontuação. Quando mais respostas forem corrigidas pelos professores, maior é a taxa de acerto do assistente de correção de questões discursivas.  Outra funcionalidade relacionada às questões discursivas é a detecção de plágio, que busca textos similares em bases de respostas anteriores, bases de dados públicas (e.g. Wikipedia e motores de busca) e bases de dados fornecidas pelo gestor da plataforma.

Esse módulo foi desenvolvido como parte do projeto PLATAFORMA DE GERENCIAMENTO DE PROVAS VIRTUAIS PARA A WEB 3.0, apoiado pelo Edital de Inovação 40/2017 - Provas Virtuais, da CAPES (Coordenação de Aperfeiçoamento de Pessoal de Nível Superior).

O módulo foi desenvolvido de forma a ser integrada a qualquer ambiente de apoio à ensino a distância, incluindo o Moodle.

* Uma descrição de como instalar o módulo em um novo ambiente está disponível aqui.
* Uma descrição de como utilizar o módulo via API RESTful é apresentada a seguir.

## Assistente de Correção Ortográfica e Gramatical

O produto desenvolvido já está integrado com o corretor open-source [LanguageTool](https://languagetool.org/dev), que possui boa taxa de acerto nos erros mais frequentes e eficiência computacional (tempo de resposta). Assim, todas as questões abertas da plataforma podem contam com esse assistente de correção. Uma vantagem do assistente utilizado nessa plataforma é que há uma (1) indicação do erro, (2) uma sugestão de correção e (3) fontes para o estudante aprender mais sobre o tipo de erro.

| Parâmetro (GET/POST)  | Valores           |Descrição                         |
|----------------|-------------------------------|-----------------------------|
|service |`corretor (String)`            |'Define o serviço de correção ortográfica'            |
|text          |`Texto a ser analisado (String)"`            |"A recomendação é que o texto seja enviado por sentenças. Se o texto for grande, então realizar múltiplas requisições (uma requisição por sentença)."            |

- Exemplo:
	> http://IP_SERVIDOR:8080/?service=corretor&text=O+gata+toma+leite.


## Assistente de Detecção de Plágio

A instituição pode cadastrar banco de dados de referências contendo textos para consulta de plágio. Quando um novo texto é submetido para consulta, o módulo desenvolvido identifica a similaridade com os textos de referência e computa uma probabilidade de plágio. De forma nativa, o Detector de Plágio utiliza como base de referência o Wikipedia em Português, porém, o módulo contém scripts para adicionar outras bases de referências conforme a necessidade. 

Uma inovação do Detector de Plágio utilizado nesta plataforma é oferecer respostas em tempo real. Para isso, foi utilizado um mecanismo de indexação textual baseado  [ElasticSearch](https://www.elastic.co/pt/), uma arquitetura open-source conhecida na área de Recuperação de Informação. Assim, professores e tutores podem validar, em tempo real, as fontes de probabilidade de plágio dos textos e questões abertas submetidos pelos estudantes.


| Parâmetro (GET/POST)  | Valores           |Descrição                         |
|----------------|-------------------------------|-----------------------------|
|service |`plagio (String)`            |'Define o serviço de detecção de plágio'            |
|text          |`Texto a ser analisado (String)"`            |"Enviar o texto completo ou parágrafos."            |

- Exemplo:
	> http://IP_SERVIDOR:8080/?service=plagio&text=A+ideia+de+calcular+o+número+de+manchas+solares+se+origina+em+Rudolf+Wolf,+em+1848,+em+Zurique,+Suíça


## Assistente de Detecção de Fuga de Tema

Esse recurso é útil para estudantes e docentes verificarem a diferença semântica entre um texto de referência (gabarito) e um texto submetido como resposta de uma questão aberta. Os próprios estudantes podem utilizar esse recurso para explorar a qualidade do seu texto em relação ao critério de tangenciamento do tema. Para desenvolvimento, foi utilizado um recurso de inteligência artificial baseado em processamento de linguagem natural denominado Modelo Neural de Linguagem. Esse recurso foi treinado com milhões de textos sobre diferentes temas (como wikipedia e notícias) de forma a construir um modelo de correlação semântica entre dois textos. Como resultado, é possível quantificar, entre 0 e 1, o nível de fuga de tema semântica entre um texto novo e um texto do gabarito. Quanto mais próximo de 1, maior o índice de fuga de tema.

| Parâmetro (GET/POST)  | Valores           |Descrição                         |
|----------------|-------------------------------|-----------------------------|
|service |`fuga_tema (String)`            |'Define o serviço de detecção de fuga de tema'            |
|text          |`Texto a ser analisado (String)"`            |"Enviar o texto completo ou parágrafos."            |
|reference          |`Texto de referência (String)"`            |"Enviar um gabarito (resposta modelo da questão)."            |

- Exemplo:
	> http://IP_SERVIDOR:8080/?service=fuga_tema&text=A+ideia+de+calcular+o+número+de+manchas+solares+se+origina+em+Rudolf+Wolf,+em+1848,+em+Zurique,+Suíça&reference=O+número+de+Wolf+é+conhecido+como+número+internacional+de mancha+solar

