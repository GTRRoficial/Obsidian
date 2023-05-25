---
tag: membros/atual
nome: Anna
lingua: "#Inglês #Espanhol"
nivel: Fluente
disponibilidade: 1h-2h por dia de segunda a sexta, de preferencia à noite e finais de semana
nucleo: 
recesso: false
ex-membro: false
dupla: 
---
```dataviewjs
const { createButton } = app.plugins.plugins['buttons'];
const { update } = this.app.plugins.plugins["metaedit"].api;
const rename = this.app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[1].static_functions.get('rename');

async function defer(key, value, file){
	await update(key, value, file)
		.finally(setTimeout(async()=>{
			if(key=='recesso' && value){
				await rename(`Anna!`);
			}else if(key=='ex-membro' && value){
				await rename(`Anna%`);
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
"Inglês fluente, Espanhol avançado. Arranho no Francês"