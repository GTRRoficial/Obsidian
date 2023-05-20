<%*
let title = tp.file.title;
const re = /[*:"\\|<>/?]/g;

if (title.startsWith("Untitled") || title.startsWith("Sem título")){
	title = await tp.system.prompt("Nome do projeto: ");
}

const areas = [
		'Curadoria',
		'Tradução',
		'Revisão',
		'Legendagem',
		'TI',
		'Design',
		'Redes Sociais',
		'Organização',
		'Administração',
		'Formação',
		'Projetos',
		'Comunicação'
	]
const soviete = await tp.system.suggester(
	areas,
	areas,
	false, "Informe o núcleo da pessoa:") || '';

let lingua = await tp.system.prompt('Línguas faladas (separadas por ","): ');
if(lingua)
	lingua = '#' + lingua.replaceAll(', ', ' #');

const nivelFlencia = await tp.system.prompt('Fluência nas linguas: ');

const type = await tp.system.suggester(['Membro', 'Camarada'], ['Membro', 'Camarada'], false)
await tp.file.move(`Membros & Camaradas/${type}s/${title.replace(re, '_')}`);
%>---
tag: 
nome: <% title %>
lingua: <% lingua %>
nivel: <% nivelFlencia %>
disponibilidade: 
nucleo: <% soviete %>
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
				await rename(`<% title.replace(re, '_') %>!`);
			}else if(key=='ex-membro' && value){
				await rename(`<% title.replace(re, '_') %>%`);
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
