<%*
let title = tp.file.title;
const re = /[*:"\\|<>/?]/g;
const dv = this.app.plugins.plugins['dataview'].api;
const allMembers = await dv.pages(`"Membros & Camaradas"`).where(m=>m.nucleo == "Curadoria").map(m=>m.nome);

if (title.startsWith("Untitled") || title.startsWith("Sem título")){
	title = await tp.system.prompt("Nome do projeto: ");
}

let idioma = await tp.system.prompt("Informe o idoma do projeto:") || '';
if(idioma)
	idioma = `"#${idioma}"`;
const midia = await tp.system.prompt("Informe a mídia do projeto:") || '';
const member = await tp.system.suggester(allMembers, allMembers, false, "Informe quem é o Curador") || '';

await tp.file.move(`Curadoria/Pendentes/${title.replace(re, '_')}`);
%>---
tag: 
idioma: <% idioma %>
midia: <% midia  %>
Curado: false
Rejeitado: false
alcance: 0
impacto: 0
confianca: 0
esforco: 0
nota_rice: 0
enviado_para_tradução: false
curador: "[[<% member %>]]"
---
```dataviewjs
const { createButton } = app.plugins.plugins['buttons'];
const { update } = this.app.plugins.plugins["metaedit"].api;
const move = this.app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[1].static_functions.get('move');

(async function updateRICE(){
	let rice = dv.current()?.nota_rice;
	let new_rice = ((dv.current()?.alcance * dv.current()?.impacto * dv.current()?.confianca) / dv.current()?.esforco) || 0;
	if(new_rice !== rice){
		await update('nota_rice', new_rice, dv.current()?.file.path);
	}
})()

async function moveNote(moveTo){
	await move(`Curadoria/${moveTo}s/<% title.replace(re, '_') %>`, {...dv.current()?.file, extension: 'md'});
}

async function defer(key, value, file){
	await update(key, value, file?.path);
	if(key=='Rejeitado'){
		await update('nota_rice', 0, file?.path);
		await update('alcance', 0, file?.path);
		await update('impacto', 0, file?.path);
		await update('confianca', 0, file?.path);
		await update('esforco', 0, file?.path);
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
			dv.current()?.file.path
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
dv.header(1, "Alcance: `$= dv.current()?.alcance || 0`");

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


```dataviewjs
dv.header(1, "Impacto: `$= dv.current()?.impacto || 0`");

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
dv.header(1, "Confiança: `$= dv.current()?.confianca || 0`");

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
dv.header(1, "Esforço: `$= dv.current()?.esforco || 0`");

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
dv.header(1, "Nota RICE: `$= dv.current()?.nota_rice || 0`")
```
