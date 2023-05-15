---
tag: 
idioma: 
midia: 
Curado: false
Rejeitado: false
alcance: 10
impacto: 10
confianca: 10
esforco: 10
nota_rice: 100
enviado_para_tradução: false
curador: "[[ANeMa]]"
---
```dataviewjs
const { createButton } = app.plugins.plugins['buttons'];
const { update } = this.app.plugins.plugins["metaedit"].api;
const move = this.app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[1].static_functions.get('move');

(async function updateRICE(){
	let rice = dv.current()?.nota_rice;
	let new_rice = ((dv.current()?.alcance * dv.current()?.impacto * dv.current()?.confianca) / dv.current()?.esforco) || 0;
	if(new_rice !== rice){
		await update('nota_rice', new_rice, dv.current()?.file.path)
	}
})()

async function moveNote(moveTo){
	await move(`Curadoria/${moveTo}s/teste`, {...dv.current()?.file, extension: 'md'})
}

async function defer(key, value, file){
	await update(key, value, file?.path);
	if(key=='Rejeitado'){
		await update('nota_rice', 0, file?.path);
		await update('alcance', 0, file?.path);
		await update('impacto', 0, file?.path);
		await update('confiança', 0, file?.path);
		await update('esforço', 0, file?.path);
		await update('Curado', false, file?.path);
	}else if(key=='Curado'){
		await update('Rejeitado', false, file?.path);
	}
	await moveNote(key);
}

createButton({
	app,
	el: this.container,
	args: {
		name: dv.current()?.enviado_para_tradução ? "Não enviar" : "Enviar para a tradução"
	},
	clickOverride: {
		click: update,
		params: [
			"enviado_para_tradução",
			!dv.current()?.enviado_para_tradução,
			dv.current().file.path
		]
	}
})

if(!dv.current()?.Aprovado){
	createButton({
		app,
		el: this.container,
		args: {
			name: "Curado"
		},
		clickOverride: {
			click: defer,
			params: [
				"Curado",
				!dv.current()?.Aprovado,
				dv.current()?.file
			]
		}
	})
}

if(!dv.current()?.Reprovado){
	createButton({
		app,
		el: this.container,
		args: {
			name: "Rejeitado"
		},
		clickOverride: {
			click: defer,
			params: [
				"Rejeitado",
				!dv.current()?.Reprovado,
				dv.current()?.file
			]
		}
	})
}
```

```dataviewjs
dv.header(1, "Alcance: `$= dv.current()?.alcance || 0`")
```

```dataviewjs
dv.header(1, "Impacto: `$= dv.current()?.impacto || 0`")
```

```dataviewjs
dv.header(1, "Confiança: `$= dv.current()?.confianca || 0`")
```

```dataviewjs
dv.header(1, "Esforço: `$= dv.current()?.esforco || 0`")
```

```dataviewjs
dv.header(1, "Nota RICE: `$= dv.current()?.nota_rice || 0`")
```
