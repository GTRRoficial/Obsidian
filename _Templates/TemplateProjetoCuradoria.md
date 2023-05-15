<%*
let title = tp.file.title;
const re = /[*:"\\|<>/?]/g;
const dv = this.app.plugins.plugins['dataview'].api;
const allMembers = await dv.pages(`"Membros & Camaradas"`).where(m=>m.nucleo == "#svt/curadoria").map(m=>m.nome);

if (title.startsWith("Untitled") || title.startsWith("Sem título")){
	title = await tp.system.prompt("Nome do projeto: ");
}

const idioma = await tp.system.prompt("Informe o idoma do projeto:") || '';
const midia = await tp.system.prompt("Informe a mídia do projeto:") || '';
const member = await tp.system.suggester(allMembers, allMembers, false, "Informe quem é o Curador") || '';

await tp.file.move(`Curadoria/Pendentes/${title.replace(re, '_')}`);
%>---
tag: 
idioma: <% idioma %>
midia: <% midia  %>
enviado_para_tradução: false
nota_rice: 0
alcance: 0
impacto: 0
confianca: 0
esforco: 0
Aprovado: false
Reprovado: false
curador: <% member %>
---
```dataviewjs
const { createButton } = app.plugins.plugins['buttons'];
const { update } = this.app.plugins.plugins["metaedit"].api;
const move = this.app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[1].static_functions.get('move');

async function moveNote(moveTo){
	await move(`Curadoria/${moveTo}/<% title.replace(re, '_') %>`, {...dv.current().file, extension: 'md'})
}

async function defer(key, value, file){
	await update(key, value, file.path);
	if(key=='Reprovado'){
		await update(nota_rice, 0, file.path);
		await update(alcance, 0, file.path);
		await update(impacto, 0, file.path);
		await update(confianca, 0, file.path);
		await update(esforco, 0, file.path);
		await update('Aprovado', false, file.path);
	}else if(key=='Aprovado'){
		await update('Reprovado', false, file.path);
	}
	await moveNote(key);
}

createButton({
	app,
	el: this.container,
	args: {
		name: dv.current().enviado_para_tradução ? "Não enviar" : "Enviar para a tradução"
	},
	clickOverride: {
		click: update,
		params: [
			"enviado_para_tradução",
			!dv.current().enviado_para_tradução,
			dv.current().file.path
		]
	}
})

if(!dv.current().Aprovado){
	createButton({
		app,
		el: this.container,
		args: {
			name: "Aprovado"
		},
		clickOverride: {
			click: defer,
			params: [
				"Aprovado",
				!dv.current().Aprovado,
				dv.current().file
			]
		}
	})
}

if(!dv.current().Reprovado){
	createButton({
		app,
		el: this.container,
		args: {
			name: "Reprovado"
		},
		clickOverride: {
			click: defer,
			params: [
				"Reprovado",
				!dv.current().Reprovado,
				dv.current().file
			]
		}
	})
}
```

```dataviewjs
dv.header(2, "Alcance: `$= dv.current().alcance || 0`")
```


```dataviewjs
dv.header(2, "Impacto: `$= dv.current().impacto || 0`");
```


```dataviewjs
dv.header(2, "Confiança: `$= dv.current().confianca || 0`");
```


```dataviewjs
dv.header(2, "Esforço: `$= dv.current().esforco || 0`");
```


```dataviewjs
dv.header(2, "Nota RICE: `$= dv.current().nota_rice || 0`")
```
