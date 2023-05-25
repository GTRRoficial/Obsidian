---
sort: nota_rice
order: desc
---
```button
name Novo Projeto
type note(Untitled) template
action TemplateProjetoCuradoria
templater true
```
```dataviewjs
const {createButton} = app.plugins.plugins["buttons"];
const {update} = this.app.plugins.plugins["metaedit"].api;
const prompt = app.plugins.plugins['templater-obsidian'].templater.functions_generator.internal_functions.modules_array[4].static_functions.get('prompt');

async function updateSort(key, value, file){
	await update(key, value, file);
}

if(dv.current().sort !== 'idioma')
	createButton({
		app,
		el: this.container,
		args: {
			name: `Ordenar por idioma`
		},
		clickOverride: {
			click: updateSort,
			params: [
				"sort",
				'idioma',
				dv.current().file.path
			]
		}
	})

if(dv.current().sort !== 'midia')
	createButton({
		app,
		el: this.container,
		args: {
			name: `Ordenar por midia`
		},
		clickOverride: {
			click: updateSort,
			params: [
				"sort",
				'midia',
				dv.current().file.path
			]
		}
	})

if(dv.current().sort !== 'nota_rice')
	createButton({
		app,
		el: this.container,
		args: {
			name: `Ordenar por RICE`
		},
		clickOverride: {
			click: updateSort,
			params: [
				"sort",
				'nota_rice',
				dv.current().file.path
			]
		}
	})
createButton({
		app,
		el: this.container,
		args: {
			name: `Ordenar ${dv.current().order == 'asc' ? 'Decrescente' : 'Crescente'}`
		},
		clickOverride: {
			click: updateSort,
			params: [
				"asc_desc",
				dv.current().order == 'asc' ? 'desc' : 'asc',
				dv.current().file.path
			]
		}
	})
async function defer(key, file){
	const value = await prompt(`Insira a pontuação para ${key}: `) || 0;
	
	await update(key, value, file)
		.finally(() => {
			setTimeout(async () => {
				const alcance = dv.page(file).alcance;;
				const impacto = dv.page(file).impacto;
				const confianca = dv.page(file).confianca;
				const esforco = dv.page(file).esforco;
				
				const rice = (alcance * impacto * confianca) / esforco || 0;
				
				await update('nota_rice', rice, file)
			}, 500)
		});
	
}

dv.header(1, 'Projetos Pendentes');
dv.table(
	['Projeto', 'Idioma', 'Mídia', 'RICE', 'Notas', 'Encaminhado'],
	dv.pages('"Curadoria/Pendentes"')
		.sort(p => {
			if(dv.current().order == 'asc'){
				return p[dv.current().sort]
			} else {
				return !p[dv.current().sort]
			}
		})
		.map(item => [
			item.file.link,
			item.idioma,
			item.midia,
			item.nota_rice,
			[
				createButton({
					app,
					el: this.container,
					args: {
						name: "Alcance: " + item.alcance
					},
					clickOverride: {
						click: defer,
						params: [
							"alcance",
							item.file.path
						]
					}
				}),
				createButton({
					app,
					el: this.container,
					args: {
						name: "Impacto: " + item.impacto
					},
					clickOverride: {
						click: defer,
						params: [
							"impacto",
							item.file.path
						]
					}
				}),
				createButton({
					app,
					el: this.container,
					args: {
						name: "Confiança: " + item.confianca
					},
					clickOverride: {
						click: defer,
						params: [
							"confianca",
							item.file.path
						]
					}
				}),
				createButton({
					app,
					el: this.container,
					args: {
						name: "Esforço: " + item.esforco
					},
					clickOverride: {
						click: defer,
						params: [
							"esforco",
							item.file.path
						]
					}
				})
			],
			`<input ${item.enviado_para_tradução ? "checked" : ""} type="checkbox" disabled />`
		])
)

dv.header(1, 'Projetos Curados');
dv.table(
	['Projeto', 'Idioma', 'Mídia', 'RICE', 'Encaminhado'],
	dv.pages('"Curadoria/Curados"')
		.sort(p => p[dv.current().sort])
		.map(item => [
			item.file.link,
			item.idioma,
			item.midia,
			dv.el('p', `<input ${item.enviado_para_tradução ? "checked" : ""} type="checkbox" disabled />`)
		])
)

dv.header(1, 'Projetos Rejeitados');
dv.table(
	['Projeto', 'Idioma', 'Mídia'],
	dv.pages('"Curadoria/Rejeitados"')
		.sort(p => p[dv.current().sort])
		.map(item => [
			item.file.link,
			item.idioma,
			item.midia
		])
)
```
