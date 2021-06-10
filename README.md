\title{Sobre o pré-processamento de dados de espectrometria de massa: instruções e ferramentas para tratamento e uso em modelos de aprendizado de máquina para classificação de biomarcadores}

\author{D. A. S. Mendes\inst{1}, R. J. Rangel\inst{1}, A. B. Pavan\inst{1}, A. F. Tavares da Silva\inst{1}, \\ 

A. F. Oliveira\inst{1}, I. N. Drummond\inst{1}, G. de Assis Mello\inst{1} }

\address{ {\it GREMLING} -- Grupo de Pesquisa e Estudos em Machine Learning\\
Universidade Federal de Itajubá (UNIFEI)\\
  Caixa Postal 50 -- CEP: 37500 903 -- Itajubá -- MG -- Brazil
\email{alan@unifei.edu.br}
}

\begin{document} 

\maketitle

\begin{abstract}
This work presents a discussion on the importance and types of mass spectrometry data pre-processing techniques when used in machine learning models to classify biomarkers. A survey of computational tools that perform such analysis was carried out and a dataset was organized.

\end{abstract}
     
\begin{resumo}
  
  Neste trabalho é apresentada uma discussão sobre a importância e os tipos de técnicas de pré-processamento de dados de espectrometria de massa, quando são usados em modelos de aprendizado de máquina para classificar biomarcadores. Um levantamento de ferramentas computacionais que realizam tais tratamentos foi realizado e um {\it dataset} foi organizado.
  
  
 
  \end{resumo}

\section{Introdução}
 Desde Dezembro de 2019, no município de Wuhan (Hubei, China), quando o primeiro caso de COVID-19 foi comunicado pela comissão de saúde do município à Organização Mundial de Saúde -- OMS \footnote{https://www.who.int/}, o vírus rapidamente se espalhou pelo mundo. Com isso, vários grupos de pesquisa vem buscando formas de identificar as causas que levam as pessoas infectadas a óbito e como diagnosticar a presença do coronavírus. A seleção e classificação de biomarcadores em dados de espectrometria de massa (EM) empregando modelos de aprendizado de máquina (AM) é uma proposta promissora \cite{metabo10060243} nessa direção. Recentemente, \cite{doi:10.1021/acs.analchem.0c04497} propuseram a criação de um teste de diagnóstico de COVID-19 que utiliza AM na análise de dados de EM.
 
Essas técnicas também tem sido aplicadas, tanto no desenvolvimento de sensores metabólicos \cite{10.3389/fbioe.2020.00006}, quanto no diagnóstico de outras doenças, tais como Dengue e o Zika vírus \cite{10.3389/fbioe.2018.00031}.


Em todos esses importantes trabalhos mencionados foram utilizadas diferentes técnicas de pré-processamento (PP) dos dados brutos de EM para sua adequada inserção nos algoritmos de AM. Contudo, segundo investigou-se, tal assunto é muito pouco discutido em trabalhos que são essencialmente da área da saúde.

Neste contexto, este trabalho apresenta uma discussão sobre a importância e os tipos de PP de dados de EM e uma indicação de quais ferramentas podem ser utilizadas para realizar este tratamento.   




\section{Tipos de pré-processamento (PP)}


Em geral, os equipamentos de EM podem gerar diferentes formatos de arquivos de saída de dados. Um dos mais populares é o .RAW, proveniente dos instrumentos científicos da Thermo Fisher Scientific (para análise no \href{https://www.thermofisher.com/order/catalog/product/OPTON-30965#/OPTON-30965}{Xcalibur Software}). Outros, de formato aberto, igualmente populares são o .mzML e o .mzXML. 

A etapa de PP consiste na análise e manipulação de vetores de dados que estão contidos nos arquivos de saída e cujas componentes são, geralmente, a razão massa/carga ($m/z$), a intensidade e o tempo. Os algoritmos de PP identificam, alinham e normalizam os picos de intensidade medidos e filtram ruídos por meio de manipulações matemáticas e estatísticas dos dados. Por isso, é fundamental que essa etapa seja feita de maneira criteriosa, evitando assim, o comprometimento dos dados experimentais e a inserção de informações espúrias.

%A seguir, são descritas de forma detalhada algumas técnicas de  PP que normalmente são usadas.
As seções \ref{F}, \ref{A}, \ref{D} e \ref{N} descrevem 4 técnicas de PP comumente empregadas na análise de dados de EM.

\subsection{Filtragem dos dados (F)} \label{F}

A filtragem dos dados visa a redução da quantidade de ruídos provenientes da operação do instrumento de medição. Para tanto é interessante obter alguns arquivos de dados apenas com o ruído gerado pelo equipamento, ou seja, sem a presença da amostra. Isso será importante para a definição do limite inferior durante o processo de operação matemática de filtragem. Dentre os {\it softwares} mais comuns para esta técnica podemos citar o MZmine, Analyst , OpenMS e o XCMS \cite{Li2021, PrezCova2021}.


\subsection{Alinhamento (A)}
\label{A}

 Durante a operação de aquisição dos dados é interessante a utilização de uma solução calibrante conhecida, juntamente com sua amostra, em cada análise. Tal solução vai permitir um ajuste da exatidão de $m/z$ dos íons, detectados em diferentes intervalos de peso molecular \cite{Chaerkady2021}. Se tal prática não é adotada os picos de intensidade podem aparecer desalinhados ou deslocados podendo gerar classificações incorretas dos metabólitos. As principais causas dessas variações são devidas a mudanças de temperatura, variações no pH, degradação da coluna cromatográfica dentre outros. O alinhamento pode ser realizado utilizado uma referência ou durante a detecção dos picos, estimando a variabilidade dos sinais. Alguns {\it softwares}, já citados, possuem essa função, como é o caso do MZmine e o XCMS.

\subsection{Detecção dos picos (D)} \label{D}

O objetivo desse processo é localizar de forma robusta os picos verdadeiros referentes aos metabólitos da amostra. Tal processo permite excluir picos falsos, principalmente os provenientes de ruídos do equipamento, reduzindo a complexidade dos dados, antes de sua análise. Dentre as bibliotecas e funções que realizam essa técnica podemos citar o ``find\_peaks" do Scipy, que encontra picos dentro de um sinal com base nas propriedades de pico, e o F-score do MZmine, que detecta os picos através da construção do cromatograma, que cria uma lista de massas e da deconvolução dos picos \cite{Leier2020}.

\subsection{Normalização (N)}
\label{N}

A normalização permite uma comparação quantitativa, removendo variações não desejadas entre as amostras. Isso garante que, nas análises multivariadas, a comparação entre sinais de duas amostras preparadas em concentrações distintas possam ser realizados. O MZMine é um dos {\it softwares} mais utilizados para esta função.

Desta forma, pode-se observar o quão importante é a etapa de PP dos dados e como ela interfere diretamente na qualidade do dado que será inserido nos modelos de AM, afetando seu desempenho de maneira significativa. 



\section{Discussões}

A apresentação e o detalhamento das técnicas de PP dos dados de EM mostram-se cruciais para o entendimento e reprodução dos resultados obtidos dos modelos de AM de diversos trabalhos que abordam o tema. 

Após uma busca na literatura  foi realizada uma compilação dos {\it softwares} mais utilizados no PP de dados de EM. Na Tabela~\ref{tab} foram indicadas uma descrição breve, o tipo e as principais funções disponíveis em cada {\it software}. 

\input{table}
 

Dentre os {\it softwares} analisados pode-se apontar, como uma opção bastante acessí-vel, o MZmine já que é um \textit{software open source} e possui diversos tutoriais sobre seu funcionamento e operação. O MZmine  fornece diferentes algoritmos para detecção de picos: o algoritmo de limiar recursivo reduz o número de falsos positivos evitando a detecção de ruído; o algoritmo de transformação {\it Wavelet} é adequado para filtragem de dados com ruído; o algoritmo de ``Massa exata" supõe espectros de alta qualidade e determina o centro de cada $m/z$ utilizando a largura a meia altura.

Um {\it dataset} mais detalhado com outros {\it softwares} utilizados no tratamento de dados de EM pode ser encontrado no repositório {\it Github} do grupo de pesquisa -- \href{https://github.com/Gremling-Machine-Learning-Study-Group/Pre-processamento-em-Espectrometria-de-Massa-Modelos-de-Aprendizado-de-Maquina-na-Classificacao/blob/main/TABLE.md}{Gremling Research Group}

Com esse levantamento realizado espera-se lançar alguma luz e apontar algumas estratégias de abordagem ao problema da apresentação e discussão do PP de dados de EM em trabalhos que aplicam modelos de AM na área de saúde.





\section{Considerações Finais}


A análise de dados de EM revela-se complexa e delicada devido, especialmente, à natureza do aparato experimental e das características dos materiais estudados.  Essa dificuldade vem sendo gradualmente vencida com o advento dos algoritmos de AM que são usados para selecionar, classificar e catalogar substâncias de interesse para a área da saúde. Contudo, sem um bom PP dos dados a análise executada por esses algoritmos fica extremamente prejudicada afetando as conclusões e a reprodução dos resultados obtidos. 

Observa-se ser premente uma melhor apresentação e discussão das técnicas de PP utilizadas em todo trabalho, na área da saúde, que lida com dados de EM como entrada em modelos de AM.
Uma última questão ainda merece consideração: Existiria alguma técnica ou algoritmo alternativo ao PP que permita o uso direto de dados brutos de EM?  
Uma possível resposta, que ainda pretende-se investigar, pode surgir da análise do emprego de redes neurais profundas.
Esse procedimento pode ser promissor especialmente na tarefa de classificação de biomarcadores.
