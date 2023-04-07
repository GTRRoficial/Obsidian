Membros em recesso com !, ex-membros com %

## Todos os membros e camaradas
```dataview
table
	lingua,
	nucleo as Núcleo,
	disponibilidade as Disponibilidade
from #membros/atual
sort nome
```

## Membros
```dataview
table
	nucleo as Núcleo,
	disponibilidade as Disponibilidade
from #membros/admin and #membros/atual 
sort nome
```

## Camaradas iniciantes
```dataview
table
	nucleo as Núcleo,
	disponibilidade as Disponibilidade
from #membros/iniciante and #membros/atual 
sort nome
```

## Membros em recesso
```dataview
table
	nucleo as Núcleo,
	disponibilidade as Disponibilidade
from #membros/recesso 
sort nome
```

## Ex-camaradas
```dataview
table
	nucleo as Núcleo,
	disponibilidade as Disponibilidade
from #membros/ex
sort nome
```
