Membros em recesso com !, ex-membros com %
## Todos os membros ativos
```dataview
table
	nome as Nome,
	nucleo as Núcleo,
	disponibilidade as Disponibilidade
from #membros/atual
sort nome
```
## Membros administrativos
```dataview
table
	nome as Nome,
	nucleo as Núcleo,
	disponibilidade as Disponibilidade
from #membros/admin and #membros/atual 
sort nome
```
## Membros iniciantes
```dataview
table
	nome as Nome,
	nucleo as Núcleo,
	disponibilidade as Disponibilidade
from #membros/iniciante and #membros/atual 
sort nome
```
## Membros em recesso
```dataview
table
	nome as Nome,
	nucleo as Núcleo,
	disponibilidade as Disponibilidade
from #membros/recesso 
sort nome
```
## Ex membros
```dataview
table
	nome as Nome,
	nucleo as Núcleo,
	disponibilidade as Disponibilidade
from #membros/ex
sort nome
```
