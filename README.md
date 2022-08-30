# DESAFIO-ALURA-BI
Esse desafio tem como objetivo estimular o estudo e o desenvolvimento dos alunos na plataforma do cursos online Alura, na área de Data Science, e também incentivar o uso de ferramentas de visualização de dados como Power BI. 

Esse repositório contém o Dashboard desenvolvido para o desafio e também os arquivos utilizados.

<ul>
  <li><a href="#week1">Desafio de Logística</a></li>

</ul>

## <a id="week1">Desafio de Logística (AluraLog)</a>

Link do Dashboard: https://app.powerbi.com/view?r=eyJrIjoiNGU4OWQ1ZDYtY2Y0MC00N2JhLThlMmUtNzAwMzdkZTVhYmExIiwidCI6IjU0YWVlZDRmLWUxZTgtNDVhMy05OTFkLTJmOTcyMDdjMjE3ZiJ9&pageName=ReportSection

![image](https://user-images.githubusercontent.com/78363684/134266350-7959a354-cb81-4861-a76c-fc0fd2805833.png)


Nesse desafio o objetivo era importar a base de dados, fazer os tratamentos e modelagens necessários 
e em seguida responder algumas perguntas de negócio para a confecção do Dashboard.


### Criação do Dashboard
Utilizando liguagem DAX, realizei as seguintes operações:

- Visualizar quantas entregas foram feitas no prazo
  1. Criei uma coluna para informar o Status da entrega

  ```
  Status Entrega = IF('Tabela - Pedidos'[Status do pedido]="Em trânsito", "Em trânsito", 
                   IF('Tabela - Pedidos'[Data de entrega]<='Tabela - Pedidos'[Data previsão], "No prazo", "Atrasado"))
      
    ```
   2. Criei uma medida 
   
   ```
  Pedidos no Prazo = CALCULATE(COUNT('Tabela - Pedidos'[Status Entrega]), 'Tabela - Pedidos'[Status Entrega]="No Prazo")
   
   ```
   ![image](https://user-images.githubusercontent.com/78363684/134265637-fdc88829-2b06-4950-8d3e-3cfdb5b2f8fb.png)

   
- Visualizar quantas entregas foram feitas com atraso
 
  ```
  Pedidos Atrasados = CALCULATE(COUNT('Tabela - Pedidos'[Status Entrega]), 'Tabela - Pedidos'[Status Entrega]="Atrasado")
   
   ```
   ![image](https://user-images.githubusercontent.com/78363684/134265652-47291278-c87b-43f8-aaa2-63a697e18956.png)

- Mostrar o número de veículos disponíveis
  ```
  Veículos Disponíveis = CALCULATE(COUNT('Tabela - Veículos'[Status]), 'Tabela - Veículos'[Status] = "Disponível")
  
  ```
  
- Calcular a média de Ship to Door (tempo de pedido até a entrega) em dias
  1. Criei uma coluna (tive algum problema na substituição das datas que estavam como "Não disponível" e então não consegui medir o S2D por uma medida direta.)
  ```
  Periodo de entrega-S2D (dias) = DATEDIFF( 'Tabela - Pedidos'[Data da compra].[Date], 
                                    'Tabela - Pedidos'[Data de entrega].[Date], DAY)
  
  ```
  2. Criei uma medida com a média
  
  ```
  Média de dias para entrega S2D = AVERAGEX('Tabela - Pedidos', 'Tabela - Pedidos'[Periodo de entrega-S2D (dias)])  
  ```
  ![image](https://user-images.githubusercontent.com/78363684/134266276-8d3cc443-9928-4276-8408-fe1ce64f26b8.png)

- Mostrar a quantidade de pedidos por Estado
  
  ![image](https://user-images.githubusercontent.com/78363684/134264936-a0bb0771-02f8-4883-a88f-9c336e878741.png)
  
  * Achei interessante adicionar ao Tooltip a soma do faturamento para cada Estado
  
     ![image](https://user-images.githubusercontent.com/78363684/134265000-e3df7498-85c1-4373-935c-67ba59ebd21a.png)

  
- Visualizar o nível médio de estoque por ano
 
  ![image](https://user-images.githubusercontent.com/78363684/134264860-9fda3983-32b8-4596-9b5e-260758374e5d.png)

 
*Extras (indo além)

- Visualizar o faturamento
 
  ```
  Valor da Compra = (
      LOOKUPVALUE(
    'Tabela - Produtos'[preço],
    'Tabela - Produtos'[ID Produto],
    [ID Produto])*[Quantidade] 
  )

  ```
  ![image](https://user-images.githubusercontent.com/78363684/134265513-129beffa-bc10-4c13-a6e6-0283ebfa1c62.png)


- Tooltips
  1. Top 10 produtos com mais estoque e seu respectivo preço e quantia em estoque
  
  ![image](https://user-images.githubusercontent.com/78363684/134265823-911944c3-9bd8-4ee8-b798-1d3ecceaed9e.png)
  
  2. Top 5 produtos com maior faturamento 

  ![image](https://user-images.githubusercontent.com/78363684/134265892-4aff7db8-873b-4e7f-ba50-e6020ff99874.png)

  3. Tipos e quantidade de cada veículos disponível
  
  ![image](https://user-images.githubusercontent.com/78363684/134265950-09f496eb-b319-4efd-9ecd-6b8aa987867d.png)

  4. Top 5 Estados com entregas atrasadas 
  
  ![image](https://user-images.githubusercontent.com/78363684/134266030-fedf4d1f-968e-4409-9656-9625219debad.png)

### Relacionamento das tabelas

![image](https://user-images.githubusercontent.com/78363684/134266575-c960f35b-34bf-40f8-979b-635b733d3078.png)

### Considerações finais

O desafio foi inspirador e me mostrou os pontos onde preciso melhorar. Aprendi muito sobre Power Query e Tooltips. Como melhoria, poderia ter utilizado menos criações de colunas para não aumentar o tamanho da base de dados.







