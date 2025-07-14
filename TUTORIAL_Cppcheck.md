# Tutorial para uso da ferramenta Cppcheck (Linux)

## Etapa 1 - Uso do Cppcheck
### Passo 1 - Instalação do Cppcheck via APT
Abra um terminal (Ctrl + Alt + T) e digite os seguintes comandos sequencialmente:
```
sudo apt update
sudo apt install cppcheck
```

### Passo 2 - Ativação de mensagens e outras opções úteis
É importante ativar as mensagens que retornarão feedback a respeito dos code smells detectados. É possível fazer isso através do comando:
```
cppcheck --enable=all "Nome do diretório onde o arquivo analisado está"
```
Existem outros comandos como:
```
--enable=all: Habilita todas as checagens (pode gerar falsos positivos).

-warning: Checagens de advertência.

-style: Checagens relacionadas a estilo de código.

-performance: Checagens relacionadas a desempenho.

-portability: Checagens de portabilidade.

-suppress=<id_do_erro>: Suprime um tipo específico de erro.

-I <diretório>: Adiciona um diretório de inclusão (include path).

-std=<padrão>: Especifica o padrão do C/C++. Exemplos: c++11, c99.

-output-file=<arquivo>: Redireciona a saída para um arquivo.

-xml: Gera a saída em formato XML, útil para integração com outras ferramentas.
```

### Passo 3 - Ir até o diretório desejado
Use os seguintes comandos para ir até o diretório onde se encontra o seu arquivo que será analisado:
```
cd "nome do diretório geral"
cd "nome do diretório onde está o arquivo para análise"
```

### Passo 4 - Análise do arquivo
Através do comando abaixo você poderá executar a análise do arquivo se já estiver dentro do diretório onde ele está armazenado, exemplo:
- Meu diretório geral se chama "Documentos"
- "Documentos" armazena a pasta "Codigo_em_C"
- uso o primeiro comando do 'passo 2' : `cd Documentos`
- em seguida utilizo o comando:
```
cppcheck Codigo_em_C
```
É possível realizar uma abordagem mais direta, utilizando dos dois comandos do 'passo 2', sequencialmente, como:
- `cd Documentos`
- `cd Codigo_em_C`
  
Após isso utilizar o comando, abaixo:
```
cppcheck .
```

## Etapa 2 - Uso do Cppcheck em projetos maiores (ex. Zephyr)
### Passo 1 - Criar um workspace e clonar o repositório do projeto a ser analisado (no nosso caso, o Zephyr)
Execute os seguintes comandos em ordem sequencial (o _west_ é uma meta-ferramenta do Zephyr, por isso seu uso aqui, normalmente o CMake seria o utilizado):
```
# Cria um diretório para o projeto com um nome novo e o inicializa
west init "digite_nome_do_novo_diretório"

# Entra no diretório do novo workspace
cd "digite_nome_do_novo_diretório"

# O west agora baixa o repositório do Zephyr e seus módulos
west update
```

### Passo 2 - Configuração e execução da análise do projeto
Antes de realizar esse passo, certifique-se de que está na raiz do workspace criado (comando _cd_).

*É importante destacar que nesse tutorial a execução do cppcheck dentro de um projeto de software de um tamanho considerável, como o Zephyr, está focada na análise de smells dentro de um diretório específico e não do todo, afim de evitar problemas na ferramenta e falta de coesão nas detecções.*

Partindo disso, execute o seguinte comando:
```
cppcheck --enable=all --inconclusive --force zephyr/nome_do_diretorio/nome_do_arquivo.c 2> cppcheck_single.log
```
#### Descrição do comando:
--enable=all
Ativa um conjunto completo de verificações, incluindo avisos de estilo, performance, portabilidade e possíveis bugs, para uma análise bem abrangente.

--inconclusive
Instrui a ferramenta a reportar também os avisos "inconclusivos", que são problemas em potencial mas que o Cppcheck não tem certeza se são erros reais.

--force
Força a análise a continuar mesmo que hajam erros de configuração, como a falta de arquivos de cabeçalho, o que pode diminuir a precisão dos resultados.

2> cppcheck_single.log
É uma operação do terminal que redireciona todas as mensagens de erro ou diagnóstico do Cppcheck para um arquivo de log, em vez de exibi-las na tela.

*Após esse comando já seria possível terminar a execução do tutorial, entretando o relatório .log tem uma finalidade mais imediatista, é utilizado para uma análise mais sucinta e breve, apenas para verificar se há erros. Devido a isso, recomenda-se utilizar dos próximos passos para uma análise de melhor visualização e compreensão*

### Passo 3 - Geração de arquivo HTML para visualização da análise realizada
Tendo sido realizada a análise, é necessário a geração de um arquivo HTML para uma melhor visualização e compreensão dos resultados da ferramenta. 
Execute o comando:
```
cppcheck --enable=all --inconclusive --force --xml zephyr/nome_do_diretorio/nome_do_arquivo.c 2> cppcheck_single.xml
```
#### Descrição do comando: 
--xml
Formata a saída dos resultados da análise (os "code smells") em formato XML, que é ideal para integração com outras ferramentas, como o cppcheck-gui.

2> cppcheck_single.xml
Redireciona a saída de erro padrão (stderr) para um arquivo, onde o cppcheck imprime todos os avisos, erros e resultados de análise.

Em seguida execute o comando:
```
cppcheck-htmlreport --file=cppcheck_single.xml --report-dir=cppcheck-html-single --source-dir=. --title="Relatório - nome arquivo analisado"
```
Esse comando usa o arquivo XML gerado anteriormente e converte para uma página HTML navegável.

### Passo 4 - Visualização do arquivo HTML
O comando abaixo permitirá a visualização do arquivo gerado no passo anterior através de uma aba que será aberta no seu navegador.
```
xdg-open cppcheck-html-single/index.html
```

----------------------------

*Para executar o cppcheck em um arquivo diferente dentro do projeto em análise, basta utilizar o comando ```Ctrl + C``` após o passo 4 e executar os passos a partir da etapa 2 novamente.*
