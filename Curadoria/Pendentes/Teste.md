---
tag: 
idioma: 
midia: 
enviado_para_tradução: false
nota_rice: 0
impacto: 0
confianca: 0
esforco: 0
Aprovado: false
Reprovado: false
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
		await update('nota_rice', new_rice, dv.current().file.path)
	}
})()

async function moveNote(moveTo){
	await move(`Curadoria/${moveTo}s/Teste`, {...dv.current().file, extension: 'md'})
}

async function defer(key, value, file){
	await update(key, value, file.path);
	if(key=='Reprovado'){
		await update('nota_rice', 0, file.path);
		await update('alcance', 0, file.path);
		await update('impacto', 0, file.path);
		await update('confianca', 0, file.path);
		await update('esforco', 0, file.path);
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
dv.header(2, "Alcance: `$= dv.current()?.alcance || 0`");

const {createButton} = app.plugins.plugins["buttons"];
const {update} = this.app.plugins.plugins["metaedit"].api;
const prompt = app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[4].static_functions.get('prompt');

async function defer(){
	const novaNota = await prompt('Nova nota: ') || 0;
	await update('alcance', novaNota, dv.current().file.path)
}

createButton({
	app,
	el: this.container,
	args: {name: "Mudar nota"},
	clickOverride: {
		click: defer,
		params: []
	}
})
```

# Alcance:: 5
```dataviewjs
dv.header(2, "Impacto: `$= dv.current()?.impacto || 0`");

const {createButton} = app.plugins.plugins["buttons"];
const {update} = this.app.plugins.plugins["metaedit"].api;
const prompt = app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[4].static_functions.get('prompt');

async function defer(){
	const novaNota = await prompt('Nova nota: ') || 0;
	await update('impacto', novaNota, dv.current().file.path)
}

createButton({
	app,
	el: this.container,
	args: {name: "Mudar nota"},
	clickOverride: {
		click: defer,
		params: []
	}
})
```


```dataviewjs
dv.header(2, "Confiança: `$= dv.current()?.confianca || 0`");

const {createButton} = app.plugins.plugins["buttons"];
const {update} = this.app.plugins.plugins["metaedit"].api;
const prompt = app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[4].static_functions.get('prompt');

async function defer(){
	const novaNota = await prompt('Nova nota: ') || 0;
	await update('confianca', novaNota, dv.current().file.path)
}

createButton({
	app,
	el: this.container,
	args: {name: "Mudar nota"},
	clickOverride: {
		click: defer,
		params: []
	}
})
```


```dataviewjs
dv.header(2, "Esforço: `$= dv.current()?.esforco || 0`");

const {createButton} = app.plugins.plugins["buttons"];
const {update} = this.app.plugins.plugins["metaedit"].api;
const prompt = app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[4].static_functions.get('prompt');

async function defer(){
	const novaNota = await prompt('Nova nota: ') || 0;
	await update('esforco', novaNota, dv.current().file.path)
}

createButton({
	app,
	el: this.container,
	args: {name: "Mudar nota"},
	clickOverride: {
		click: defer,
		params: []
	}
})
```


```dataviewjs
dv.header(2, "Nota RICE: `$= dv.current()?.nota_rice || 0`")
```
