---
---
- [[#Introdução|Introdução]]
- [[#Criando um quadro|Criando um quadro]]
- [[#Criação de listas e cards|Criação de listas e cards]]
	- [[#Criação de listas e cards#Criação de de listas|Criação de de listas]]
	- [[#Criação de listas e cards#Criação de cards|Criação de cards]]
- [[#Personalização dos cards|Personalização dos cards]]
- [[#Configurações avançadas|Configurações avançadas]]
	- [[#Configurações avançadas#Começando a explicação|Começando a explicação]]
		- [[#Começando a explicação#Notas criadas pelo kanban|Notas criadas pelo kanban]]
		- [[#Começando a explicação#Metadados das notas|Metadados das notas]]

## Introdução
O plugin Kanban é utilizado para criação de quadros de tarefas iguais as do Trello.
Exemplo de como é um quadro Kanban:
![[kanban-board.png]]

## Criando um quadro
Existem duas alternativas para criar um quadro de tarefas, que serão descritas abaixo:

1. Ao clicar com o botão esquerdo o mouse, aparece a opção `New kaban board`
![[kanban-menu-new-board.png]]
3. A segunda forma de criar um quadro kanban é utilizando a notação de yaml, que é definida, obrigatoriamente, como a primeira coisa na nota e entre `---`, seguindo a notação:
![[kanban-notação-criação-board.png]]

## Criação de listas e cards

### Criação de de listas
1. Ao criar pela primeira forma, será aberto automaticamente uma aba e uma pequena janela onde é perguntado ao usuário o nome da lista. E esta mesma janelinha pode ser aberta clicando no botão:
![[kanban-nova-lista.png]]
2. O segundo método é para quadros criados pela segunda forma, que é feito com a notação:
```markdown
## Título da lista
```

### Criação de cards
1. A opção mais simples é criar pelo próprio quadro, no seguinte botão: ![[kanban-add-card.png]]

2. A segunda opção é pela segunda forma de criar listas. Então, abaixo da sintaxe que cria a lista, adicione a notação `- [ ] Título do card` que criará um simples card sem muitos detalhes. O título do card pode ser um direcionamento para um arquivo, como por exemplo: `- [ ] [[nome do arquivo]]`.

## Personalização dos cards
1. A forma mais simples é adição de tarefas dentro do card, a sintaxe tem que ser `- [ ] Título do card<br>- [ ] Tarefa 1<br>- [ ] Tarefa 2`. Assim o carde terá tarefas menores dentro dele, ficando como no exemplo a seguir: ![[kanban-card-com-tarefas.png]]
*Observação*: Ao seguir a primeira explicação no [Criação de cards](Ajuda/Obsidian/Plugins/Kanban#Criação%20de%20cards), ao invés de digitar o informado no ponto, a melhor forma é digitar o *título* e para digitar as tarefas aperte `shift + enter` para quebrar a linha. Então siga a notação substuindo os `<br>` por `shift + enter`.

2. Outra forma de adicionar mais detalhes dentro de um card é usando a notação `![[arquivo]]` que é responsável por carregar o arquivo no lugar onde a notação foi feita. E dentro desse arquivo entra os detalhes que precisam aparecer no card no quadro kanban.

## Configurações avançadas
Este plugin possui dois pontos de configuração, as configurações globais, acessadas pelas Configurações>Kanban, e a configuração local, que é exclusiva somente para o quadro onde ela foi feita. Ambas as configurações são identicas, mudando apenas se é expecífica apenas para um quadro, ou para todos os quadros.

### Começando a explicação
Existem 2 pontos de extremo interesse na imagem a seguir, que é o informe das pastas de templates e padrão de notas do kanban (Será explicada mais a frente).
![[kanban-config-1.png]]
1. Apresenta a opção de selecionar o modelo que deve ser seguido para a criação de novas notas pelo kanban.
2. Qual a pasta onde essas notas serão salvas.

#### Notas criadas pelo kanban
As notas citadas acima fazem referência a opção ![[kanban-criar-nota-do-cartão.png]] 
Onde o título do card começa apontar para a nota que foi criada

Outros pontos de interesse nas configurações, são sobre datas e horas
![[kanban-config-2-hora.png]]
Os pontos 1 e 2 são para a forma de apresentação da data, sendo `D` = dias, `M` = Meses e `Y` = Anos, ex: 31/12/2023

#### Metadados das notas
O mais interessante (ao meu ver), são as configurações dos chamados metadados (dados dos dados)
![[kanban-config-3-medados.png]]
1. Representa a chave na nota "linkada" que deve ser exibida no cartão
2. Representa o nome que deve ser exibido no cartão ao lado do valor lido na nota
3. Precisa estar ligado caso tenha sintaxe de Markdown (linguagem padrão usada no Obsidian para criar as notas)
Exemplo de uma nota com essa configuração de metadados: ![[kanban-config-4-metadados.png]]
Enquanto que na nota linkada precisa ter a seguinte notação: ![[kanban-config-5-metadados.png]]
Onde o nome que aparece na imagem acima é o mesmo informado no campo `responsavel` na nota.