<%*
let title = tp.file.title;
const re = /[*:"\\|<>/?]/g;

if (title.startsWith("Untitled") || title.startsWith("Sem título")){
	title = await tp.system.prompt("Nome de camarada: ");
}

const areas = [
	'Administração',
	'Curadoria',
	'Design',
	'Formação',
	'Tradução',
	'Revisão',
	'Legendagem',
	'TI',
	'Redes Sociais',
	'Organização',
	'Projetos',
	'Comunicação'
].sort()

let membroEm = '';
let caminhoMove = `Membros & Camaradas/${title.replace(re, '_')}`;
if (await tp.system.suggester(['Sim', 'Não'], [true, false], false, `${title} é membre de algum núcleo?`) || false){
	membroEm = await tp.system.suggester(
	areas,
	areas,
	false, "Informe o núcleo:") || '';
	caminhoMove = `Membros & Camaradas/Membros/${title.replace(re, '_')}`;
}

let camaradaEm = '';
let copyAreas = [...areas];
if(membroEm) copyAreas.splice(copyAreas.indexOf(membroEm), 1);

let nNucleosCamarada = await tp.system.prompt(`${title} é camarada em quantos núcleos?`);
if (nNucleosCamarada > copyAreas.length) nNucleosCamarada = copyAreas.length;
for(let i = 0; i < nNucleosCamarada; i++){
	let aux = await tp.system.suggester(copyAreas, copyAreas, false, "Informe o núcleo da pessoa:") || '';
	camaradaEm += `
 - ${aux}`;
	copyAreas.splice(copyAreas.indexOf(aux), 1);
}

let lingua = await tp.system.prompt('Línguas faladas (separadas por ","): ') || '';
if(lingua){
	lingua = [lingua.split(',').map(i => `#${i.trim()}`)]
	lingua = `"${String(lingua).replaceAll(',', ' ')}"`
}

let nivelFluencia = await tp.system.prompt('Fluência nas línguas: ') || '';

let disponibilidade = await tp.system.prompt('Disponibilidade informada: ') || '';

let dupla = await tp.system.prompt("Informe a dupla da pessoa: ") || '';
if (dupla) dupla = `[[${dupla}]]`;

await tp.file.move(caminhoMove);
%>---
tag: membros/atual
nome: <% title %>
lingua: <% lingua %>
nivel: <% nivelFluencia %>
disponibilidade: <% disponibilidade %>
membro: <% membroEm %>
camarada: <% camaradaEm %>
recesso: false
ex-membro: false
dupla: <% dupla %>
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
				await rename(`<% title.replace(re, '_') %>!`);
			}else if(key=='ex-membro' && value){
				await rename(`<% title.replace(re, '_') %>%`);
				await move(`Membros & Camaradas/Ex-membros/<% title.replace(re, '_') %>`, {...dv.current().file, extension: 'md'})
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
