# db_pandas_evaluate
evaluate code in a dataframe, and then either filter the rows or store the result in a column
## Objetivo
Este script possui alguma funcionalidades gerais que podem ser utilizadas em automatizações tais como scripts csh.
 - Criar novas colunas baseadas em uma expressão
 - Criar colunas em branco caso não existam ainda
 - Renomear colunas
 - Filtrar linhas baseado em um expressão
 - Converter arquivos entre os formatos suportados:
   * Xlsx, xls, csv
   * Shp
   * Dgd
   * 00t
   * Bmf
   * Obj
   * json
  
## Screenshots
![screenshot1](./assets/screenshot1.png?raw=true)  
## Utilização
Como entrada pode-se utilizar um arquivo com dados tabulares em um dos formatos suportados pela biblioteca _gui.  
Os formatos mais comuns são csv e xlsx.  
Além dos campos de seleção para entrada e saída, existem os dois campos de parametros (expression e name) e a forma como são preenchidos irá influenciar em qual modo o script irá operar.
## Modo de criação/atualização de coluna!
![screenshot2](./assets/screenshot2.png?raw=true)  
Se ambos os campos forem preenchidos, uma coluna será criada ou atualizada no resultado armazenando o resultado da expressão.  
O nome da coluna é o definido pela argumento name. O valor sera calculado baseado no campo expression. Pode-se utilizar operadores python. As colunas do dado podem ser referenciadas pelo seu respectivo nome. Ex.:
Name: mass
Expression: volume * density
## Modelo Expressão
![screenshot3](./assets/screenshot3.png?raw=true)  
O modo expressão é semelhante ao modo criar/atualizar.  
A diferença é que em vez do nome da coluna a ser criada/atualizar ser definida no campo nome, ela é incluída na forma de uma atribuição no campo expression. O campo nome por sua vez estará em branco. Ex.:
Name: (em branco)
Expression: mass = volume *  density
## Modelo criar colunas em branco caso nao existam
![screenshot4](./assets/screenshot4.png?raw=true)  
Esse modo é bem semelhante ao principal do script, que é criar novas colunas baseadas em uma expressão.  
A unica diferença é que se a expressão for vazia e a coluna já existir, ela é preservada. Se a coluna não existir ainda, ela é criada com um valor nulo.  
Esse modo é útil em processos onde uma etapa posterior espera que determinadas colunas existam mas o dado de entrada pode não contê-las.
## Modo substituir valores nulos em uma coluna por outra
![screenshot5](./assets/screenshot5.png?raw=true)  
Esse modo permite substiuir apenas as linhas que contenham valores nulo em uma coluna por outra coluna.  
Formato:
`colunaA || colunaB`
Quando o valor da coluna A for nulo, usa o valor da coluna B. Caso a coluna A tenha valor, será usada o seu valor.  
O resultado dessa operação será gravado na coluna definida em name e não há problema ser for uma das colunas de entrada.
Não é possível usar um valor fixo, ele requer duas colunas para funcionar. Ex.: `colunaA || -99` *não funciona.*
## Modo de filtro
![screenshot6](./assets/screenshot6.png?raw=true)  
Se apenas o campo “expression” estiver prenchido, o script irá filtrar as linhas, deixando apenas onde o resultado da expressão for verdadeiro.  
Esse modo é diferenciado do modo expressão pela utilização do operador de atribuição = (um sinal de igual cercado por espaços).  
Note-se que ainda é possível, caso necessário, utilizar o operador de comparação == (dois sinais de igual consecutivos) sem problema.  
## Modo de conversão entre formatos de aquivo
![screenshot7](./assets/screenshot7.png?raw=true)  
Se apenas os campos input e output estiverem preenchidos, o script irá apenas converter o arquivo de entrada para o formato definido pela extensão do arquivo de saída.
## Modo de renomear coluna
![screenshot8](./assets/screenshot8.png?raw=true)  
Se a expressão for apenas um únco nome de coluna existente no dado, sem espaços ou qualquer outro caracter, essa coluna será renomeada para um novo nome indicado no campo name no resultado.
## Modo de substituição
![screenshot9](./assets/screenshot9.png?raw=true)  
Se a expressão estiver no formato:
`s/<valor_antigo>/<valor_novo>/`
O script irá substituir valores em todos os registros(linhas). Se o campo name estiver preenchido, a substituição será apenas na coluna denominada por ele. Se o campo name estiver em branco a substituição será feita em todas as colunas. É possivel usar operadores de expressão regular (sintaxe regex python).
Ex.:
`s/meia duzia/6/`
## Exemplo 1
![screenshot10](./assets/screenshot10.png?raw=true)
Este exemplo é semelhante ao comando head o unix.
É util para gerar uma versão mais leve de arquivos grandes que não abrem no excel ou demoram muito ao rodar processos.
## Exemplo 2
![screenshot10](./assets/screenshot11.png?raw=true)
 1. Cria uma coluna com o resultado de uma soma
 2. Renomeia a propria coluna anterior para outro nome
 3. Filtra linhas onde a condição for verdadeiras
## Example 3 - Dados - Tabela1
geocod	| product	| value	| mask
---|---|---|---
aa|ore|1|1
bb|waste|1|1
aa|ore|10|0
bb|waste|10|0
aa|ore|100|-99
bb|waste|100|-99


