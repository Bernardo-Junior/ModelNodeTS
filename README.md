<h1> Modelo usando Node Typescript Eslint typeORM </h1>

yarn add express


yarn add typescript -D -> importar o typescript em modo de desenvolvimento


yarn tsc --init -> para gerar o tsconfig.json 


	abrir o tsconfig.json e descomentar o outDir e adicionar o diretório dist
	descomentar o rootDir e adicionar o diretório src 


yarn add @types/nome da biblioteca que o typescript não encontra a referência


<h3>Usar 'yarn tsc' para gerar o codigo js do ts para o dist</h3>


yarn add ts-node-dev -D -> serve para rodar o codigo ts sem converter para o js

	abrir o package.json e criar o:

		"scripts": {
			"build": "tsc",
			"dev:server": "ts-node-dev --inspect --transpile-only --ignore-watch node_modules src/server.ts",
		}

##usar o 'yarn dev:server' para rodar o codigo

<h2>Editor Config</h2>
<h3>Serve para padronizar codigo em diversas ferramentas para codificar</h3>
	ir nos plugins do vs code e instalar o 'EditorConfig for VS Code'
	clicar na raiz com o lado direito e selecionar 'Generate .editorConfig'

	abrir o .editorConfig mudar o:

		ident_size para '2'
		trim_trailing_whitespace para 'true'
		insert_final_newline para 'true'

	acrescentar no .editorconfig:

		end_of_line = lf

<h2>Eslint</h2> 

yarn add eslint -D 
yarn eslint --init 

	Qual a forma que quer usar? -> To check syntax, find problems, and enforce code style


	Como esta fazendo as importações? -> JavaScript modules (import/export)


	Esta usando algum framework? -> None of these


	O projeto usa typecript? -> y


	Onde você esta rodando o codigo?
		.aperta o backspace pra desmarcar Browser e seleciona o node apertando backspace


 	Qual guia de estilo? -> Use a popular style guide
		.Selecionar o Airbnb


	formato de config do eslint -> JSON


	seleciona Não na ultima etapa 

	Copiar o codigo e instalar com yarn add -D @typescript-eslint/eslint-plugin@latest eslint-config-airbnb-base@latest eslint-plugin-import@^2.21.2 @typescript-eslint/parser@latest


ir nos plugins do vs code e instalar o eslint

abrir o preferences: Open Settings(JSON)
	  colar lá dentro:

		"[javascript]": {
		   "editor.codeActionsOnSave": {
		     "source.fixAll.eslit": true,
                   }
		},
		"[javascriptreact]": {
		   "editor.codeActionsOnSave": {
		     "source.fixAll.eslit": true,
                   }
		},
		"[typescript]": {
		   "editor.codeActionsOnSave": {
		     "source.fixAll.eslit": true,
                   }
		},
		"[typescriptreact]": {
		   "editor.codeActionsOnSave": {
		     "source.fixAll.eslit": true,
                   }
		}

depois quando você der CTRL + S o codigo se organiza 


<h2>Importando Arquivos TS</h2>

yarn add -D eslint-import-resolver-typescript -> para funcionar as importações ts, pois o vs só é acostumado com importações js

	ir em .eslintrc.json
		adicionar:
		 "settings": {
		   "import/resolver":{
		     "typescript":{}
		   }
		 }
	ir no rules e adicionar:
		 "import/extensions": [
		   "error",
		   "ignorePackages",
		   {
		     "ts": "never"
		   }
		 ]
			

<h2>Prettier</h2>

Não deixa o codigo ficar feio

yarn add prettier eslint-config-prettier eslint-plugin-prettier -D

	ir no .eslintrc.json no extends adicionar:
		"plugin:@typescript-eslint/recommended",
		"prettier/@typescript-eslint",
		"plugin:prettier/recommended"
	
	ir em plugins e adicionar:
		"prettier"
	
	ir em rules e adicionar:
		"prettier/prettier": "error"

vai da error de aspas duplas 
	criar na raiz do projeto 'prettier.config.js'
		adicionar:


			module.exports = {
				singleQuote: true,
				trailingComma: 'all',
				arrowParens: 'avoid'
			}


o eslint vai tentar fazer tratativa do prettier
	criar um arquivo .eslintignore e dentro dele adicionar:


		/*.js
		node_modules
		dist


<h2>Debugg</h2>
clicar no botao de play com um virus
	iniciar o debug
			apagar a linha "program"
	colocar attach no lugar do launch


			name: "debug"

			colocar: 

			"protocol": "inspector"
			"restart": true

<h2> typeORM -> Banco de dados postgres</h2>
criar na raiz da pasta o ormconfig.json

usar yarn add typeorm pg -> para instalar o postgress com o typeORM


		{
		   "type": "postgres",
		   "host": "localhost",
		   "port": 5432,
		   "username": "postgres",
		   "password": "docker",
		   "database": "gostack_gobarber"
 		}

criar dentro do src uma pasta chamada 'database e dentro o index.ts'

	colocar:


		import { createConnection } from 'typeorm';
		
		createConnection();

ir no server.ts e adicionar 


	+ import './database'


ir no dbeaver clicar com o direito na conexao e criar a database

e rodar no terminal

		
	yarn dev:server


criar uma pasta vazia na pasta database chamada migrations
ir no ormconfig.json e adicionar informações para gerar as migrations


	+ "migrations": [
		"./src/database/migrations/*.ts"
	],
	+ "cli": {
		"migrationsDir": "./src/database/migrations"
	}

para fazer que as migrations entendam ts vamos no doc package.json em scripts e adicionar
	+ "typeorm": "ts-node-dev ./node_modules/typeorm/cli.js"

executar 


	yarn typeorm
	yarn typeorm migration:create -n CreateAppointments -> nome da tabela que vai criar no banco de dados

ir no documento que foi criado na pasta migrations 

ir no .eslintrc.json em rules e adicionar 


	"class-methods-use-this": "off",

depois de criar a tabela rodar


	yarn typeorm migration:run -> para criar a tabela
