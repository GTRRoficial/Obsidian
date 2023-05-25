---
tag: membros/atual
nome: CaioAS
lingua: "#Inglês #Espanhol #Russo"
nivel: Intermediário
disponibilidade: Entre 3 e 7 horas por semana, sem consistência de quais dias estarão disponíveis.
nucleo: 
recesso: false
ex-membro: false
dupla: 
---
```dataviewjs
const { createButton } = app.plugins.plugins['buttons'];
const { update } = this.app.plugins.plugins["metaedit"].api;
const rename = this.app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[1].static_functions.get('rename');
const move = this.app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[1].static_functions.get('move');

async function defer(key, value, file){
	await update(key, value, file)
		.finally(setTimeout(async()=>{
			if(key=='recesso' && value){
				await rename(`CaioAS!`);
			}else if(key=='ex-membro' && value){
				await rename(`CaioAS%`);
				await move(`Membros & Camaradas/Ex-membros/CaioAS`, {...dv.current().file, extension: 'md'})
			}
		}, 500))
}

if(!dv.current().recesso)
	createButton({
		app,
		el: this.container,
		args: {
			name: "Recesso"
		},
		clickOverride: {
			click: defer,
			params: [
				"recesso",
				true,
				dv.current().file.path
			]
		}
	})


if(!dv.current()['ex-membro'])
	createButton({
		app,
		el: this.container,
		args: {
			name: "Ex-membro"
		},
		clickOverride: {
			click: defer,
			params: [
				"ex-membro",
				true,
				dv.current().file.path
			]
		}
	})
```

## Comentários
Inglês fluente, espanhol intermediário e russo avançado.
