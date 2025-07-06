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
-all: Habilita todas as checagens (pode gerar falsos positivos).

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
Feito isso, execute o seguinte comando:
```
west build -b native_sim -t cppcheck zephyr/samples/basic/blinky -- -DCONFIG_CPPCHECK=y -DCONFIG_CPPCHECK_EXTRA_ARGS="--enable=all --library=std"
```
ainda em produção...*
