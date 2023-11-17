
# Fiz uma documentação do que aprendi até o momento em Django

Links utéis: https://github.com/treinaweb/treinaweb-django-fundamentos
https://docs.djangoproject.com/en/4.2/intro/tutorial01/
https://github.com/marinho/aprendendo-django/blob/master/08-trabalhando-com-arquivos-estaticos.md

- O que é o Django?
    Django e um framework web Python de alto nivel que permite o rápido
    desenvolvimento de sites seguros e de fácil manutenção.

- Arquitetura MVT 
    O Django usa o modelo Cliente sevidor, e o seu fluxo de dados segue a arquitetura MVT.

    Model: Mapeamento do banco de dados do projeto.
    Views: E responsavel pela logica da aplicação. Ou seja controlam o comportamento e as funcionalidades do app.
    Template: Aqui onde serão renderizados o html da aplicação.
    Fluxo de dados Django:
        
        O cliente(usuario) faz uma requisição pro nosso servidor(back-end);
        o urls.py verifica a URL pra ver se eéuma rota valida;
        A views.py processa os dados dependendo da url fornecida;
        O template e renderizado e retorna uma response
        Fonts:
            (https://pythonacademy.com.br/blog/desenvolvimento-web-com-python-e-django-introducao)
            (https://www.youtube.com/watch?v=91aD2aCRZIg&pp=ygUVZmx1eG8gZGUgZGFkb3MgREpBTkdP)

- Instalação Django 
    - Ambiente Virtual: Por boas praticas isolamos nosso projeto com uma venv 
        install: pip install virtualenv 

        Criar e Ativar o ambiente:
            Criar: 
                py -m venv nome_do_ambiente
            Ativar:
                windows: 
                    nome_do_ambiente/scripts/activate
                Linux: 
                    source nome_do_ambiente/scripts/activate or nome_do_ambiente

        Instale o Django no Ambiente:
            pip install django        

- Criação do Projeto 
    Projeto Django pode ser considerado um container que abriga todo o aplicativo.

    Criação: django-admin startproject nome_projeto . (O ponto e para não criar uma pasta a mais)

    O comando ira criar um projeto Django com a seguinte estrutura de pastas:
        manage.py E o arquivo utilitario do Django. Ele e basicamte o gerente do projeto.
        settings.py Contém as configurações gerais do projeto.
        urls.py: Configura o Roteamento de url do app.
        wsgi.py Usado para implantar o aplicativo em servers da web compativeis como a apache.
        
- Criação da app
    O Django é dividido em 'apps'(aplicações) para promover o conceito de 'dividir e conquistar' para facilitar grandes aplicações.

    Criação: py manage.py startapp nome_app 

- Rotas Django
	As rotas em Django são responsáveis por direcionar as requests do navegador para funções apropriadas em seu aplicativo.
	O sistema de Rotas do Django e gerenciado seguindo uma hierarquia.
	
	
	Urls pai(urls do projeto): Elas organizam a estrutura geral de URLs do seu projeto e direcionam o tráfego para os aplicativos
	específicos.Ou seja: Ele cria outras urls. Ex: uma url chamada accounts/, a função dela e criar outras urls filhas login e register, etc.

		* Informe o caminho da da url do app	
			path('accounts/', include('app.urls'))

	Urls filha:(Urls dos apps): São responsáveis por mapear caminhos URLS especificos para as funções de views
		
		path('login/', views.login, name='login'),

- Configuração e criação da pasta templates
	Em Django templates são usados para criar a camada de apresentação da aplicação.
	
	- Configurar
		Em settings.py procure por TEMPLATES
		Na DIRS: [] especifique o caminho que vocé quer que o Django procure os templates
		DIRS: [os.path.join(BASE_DIR, 'templates')] utilizamos a biblioteca os, mas podeser a pathlib

		Na 'APP_DIRS': True e especificado em Boolean Se você quer que o Django também procure por templates de dentro dos apps.

		OPTIONS: contém configurações específicas de back-end.

- Configuração e criação dos Arquivos estaticos
	Arquivos estaticos são arquivos css, js e de img, ou seja arquivos que irão ser renderizados no template.
	
	- Explicando:
		- STATIC_URL: E a url base na qual os arquivos estaticos irão ser servidos para os usuarios.
			
			Configurando em settings.py:
				A variavel STATIC_URL deve receber uma string informando o caminho. Por padrão e utilizado o '/static/'.

				Exemplo de uso: /static/img/img_.png


		- STATIC_ROOT: E o local onde você quer que seja reunido todos os arquivos estaticos para quando for colocar em produção.
			Configurando em settings.py: 
				Com a biblioteca lib importada concatenamos o diretorio absoluto do projeto com a string informando o caminho a ser criado.
					Exemplo: os.path.join(BASE_DIR, 'staticfiles')

		- STATICFILES_DIRS: São caminhos personalizados que voce queira que o Django reconheça como locais adicionais para buscar os arquivos estaticos.
			Configurando em settings.py:
				Utilizamos uma lista ou uma tupla para informar os caminhos personalizados.
				Exemplo:
				STATICFILES_DIRS = [
					os.path.join(BASE_DIR, 'templates/static')
				]
	- Modo de uso:
		Para carregar os arquivos staticos utilizamos a tag do Django {% load static %}(No topo do HTML). Essa tag permite que o Django reconheça e interprete a tag {% static '' %} 
		para gerar URLS para os arquivos estaticos.
		
		E para se importado utilizamos {% static 'path' %} do arquivo a ser utilizado.

	- fonts:
		(https://www.youtube.com/watch?v=w9KFXoNoJT4),
		(https://docs.djangoproject.com/en/4.2/howto/static-files/)

- Configuração e criação dos arquivos de média
	Arquivos média no contexto do Django se referem a arquivos enviados pelos usuarios como imagens, audio, pdf, videos e etc.

	- Explicando:
		MEDIA_URL: Ela define a URL base para ser servido ao usuario o arquivo media.
			- Configurando: 
				Cria uma variavel MEDIA_URL = e informe uma string representando o caminho URL. Por padrão e configurado '/media/'.
			
		MEDIA_ROOT: Ela define o caminho absoluto no sistema onde os arquivos serão armazenados.
			- Configurando: crie uma variavel MEDIA_ROOT= os.path.join(BASE_DIR, 'nome_path')(e concatene usando a biblioteca os com a raiz do projeto)
			  Por padrão, e utilizado o 'media/'. Não e necessario criar a pasta o proprio Django já cria ao receber algum arquivo.


- Como trabalhar com formularios no Django
	- Formulários são componentes HTML/widgets em uma página da web que coletam informações dos usuários para serem enviadas e processadas no servidor.
	  Um formulário HTML é definido com os atributos action (especifica o URL onde os dados serão enviados) e method (determina o método HTTP utilizado).

	- Explicando:

		- Action: E especificado o path/url para onde os dados devem ser enviados para a URL processar os dados no servidor.
		- Method: O método HTTP utilizado para enviar os dados.
			-POST: E utilizado se os dados forem resultar em uma alteração no banco de dados, pois é mais resistente a ataques de falsificação.
			-GET: deve ser utilizado somente para formulários que não alteram dados. Ex: um form de busca.
		
	- Exemplo:
		<form action="/processar-dados" method="POST">
    			<label for="usuario">Usuário:</label>
    			<input type="text" id="usuario" name="usuario">
    			<button type="submit">Enviar</button>
		</form>	
	
	- Django forms:
		O Django fornece recursos que facilitam a criação e renderização de um formulario a serem reutilizados. .
		Fontes: (https://docs.djangoproject.com/en/4.2/topics/forms/)

	- Fontes: (https://developer.mozilla.org/pt-BR/docs/Learn/Server-side/Django/Forms),
		  (https://developer.mozilla.org/pt-BR/docs/Web/HTTP)

- Django ORM
	- O que e o ORM
		O ORM(Mapeador objeto-relacional) é uma estrutura web Python para interagir com banco de dados como criar consultas complexas ao banco de dados.
		Em outras palavras utilizamos a ORM como ponte de comunicação entre o banco e a aplicação sem precisar escrever codigos SQL.

	- QuerySet
		Queryset é um conjunto de ações que serão realizadas no banco de dados, ou seja, podemos criar, buscar, atualizar ou deletar os dados sem escrever a query SQL.

	- Django shell
		O shell do Django é uma ferramento interativa que permite que você faça consultas interativas. Ela é uma maneira conveniente de explorar e testar as funcionalidade
		Django, além de realizar consultas ao Banco de dados de forma interativa. E útil para depurar e testar seu código durante desenvolvimento.

		Para usar:
			py manage.py shell

		Fontes: https://docs.djangoproject.com/en/4.2/topics/db/queries/
	- Models
		Como vimos na arquitetura do Django, a models e a parte onde será feita os moldes do banco de dados.
		
		Um modelo é a fonte unica e definitiva de informações sobre seus dados. Ele contém os campos e comportamentos
		essenciais dos dados a ser armazenados.

		Fonte: https://docs.djangoproject.com/en/4.2/topics/db/models/
		https://docs.djangoproject.com/en/4.2/topics/db/examples/many_to_one/


	- Fontes: (https://www.alura.com.br/artigos/django-query-sets-e-orm
		   https://afropython.gitbook.io/tutorial/queryset_e_orm_do_django/shell_do_django


- Rotas Dínamicas Django
	- O que são
		Urls dinãmicas são caracteristicas que ermite criar padrões de URL flexíveis que podem incluir partes variáveis
		como números inteiros, strings, slugs e outros tipos de dados. A principal função é permitir que voce capture informações
		diretamento da URL e use essas informações para determinar qual view será executada.

		Ex: Em urls.py(app), crie um path('<int:variavel>')
		Em views def home(request, variavel):
				return httpresponse(variavel)
	
	- Fontes: (https://docs.djangoproject.com/en/4.2/topics/http/urls/#path-converters)


- Django Filtros
	- O que são
		Em Django, os filtros são utilizados para renderizar dados dinâmicos em uma página web. Eles permitem que você modifique ou formate
		os valores dentro de um template antes de exibi-los. São usados para tarefas comuns. como formatar datas, números, strings e muito mais.
		
	- Fontes:
		(https://docs.djangoproject.com/en/4.2/ref/templates/builtins/)


- Django Migrações
	As migrações do Django são a maneira de propagar as alterações feitas em seus modelos(adicionando, excluindo, etc) em seu esquema de banco de dados.
	Eles foram projetados para serem quase sempre autómaticos, mas você precisará saber quando fazer migrações, quando executa-las e os probelams comuns que podem ocorrer.

	- Comandos
		Existem vários comandso que você usará para interagir com migrações e manipulação do banco do Django:
		
		* migrate: Responsável por aplicar e cancelar as aplicações de migrações.
		* makemigrations: Responsavel por criar novas migrações com base nas alterações feitas em seus modelos.
		* sqlmigrate: Exibir as intruções SQL para uma migração.
		* shomigrations lista as migrações de um projeto e seu status.

	Você deve pensar nas migrações como um sistema de controle de versão para seu banco de dados. makemigrations e responsavel por empacotar as alterações
	do seu modelo em arquivos de migraç~eos individuais - analogo ao commit e migrate e responsavel por aplicar-los ao seu banco de dados

- Django Admin
	- O que são
		E uma ferramenta poderosa e amigável para gerenciar o contéudo de um site ou aplicativo web. Permite que os administradores realizem tarefas de 
		gerenciamento de contéudo sem a necessidade de escrever código personalizado.
		
	- URL:
		Para ser acessado usa a URL ('/admin/', por padrão).
	
	- Criar super usuario: 
		Para criar um super usúario para fazer login use o comando: py manage.py createsuperuser.
	
	- Adicionar uma models no admin:
		Para adicionar models ao Admin, siga as etapas em admin.py:
 			importe o models: from nome_app.models import nome_models;
			Registre o nome da models: admin.site.register(nome_models);


	- Personalização da Admin:	
		Os django oferece várias maneiras de personalizar o Admin, para torna-la mais adequada ãs necessidades especificas
		do seu aplicativo. Voce pode personalizar campos de exibição, criar ações personalizadas e até mesmo personalizar os templatesde administração.
		
		
		Saber-mais:  https://docs.djangoproject.com/en/4.2/ref/contrib/admin/

		List_display: Controla quais campos são exibidos na página da lista de alterações do administrador.
		
	
	- Fontes:
		(https://docs.djangoproject.com/en/4.2/ref/contrib/admin/)
		https://docs.djangoproject.com/en/4.2/ref/contrib/admin/filters/


- Django Models
	- O que é:
		Um models é a fonte unica e defnitiva de informaç~eos sobre os dados. Ele contém os campos e comportamentos essenciais dos dados armazenados.
		Cada modelo é mapeado para uma única tabela do DataBase.
		
		- O básico: 
			Cada modelo é uma classe Python que subsclassifica django.db.models.Model;
			Cada atributo do modelo representa um campo do DataBase;
			o Django possui uma API de acesso ao banco de dados gerada automaticamente, vimos anteriormente o Shell.
		
		- Migrações
			Ao adicionar novos apps no INSTALLED_APPS, certifique-se de executar o, opcialente fazendo migrações para que eles primeiro com o manage.py migrate e makemigrations.
		
		- Fields
			São a parte mais importante de um modelo, é a lista de campos de um banco de dados. Os campos são especificados por atributos da classe. Tenha cuidado para não escolher
			nomes e campos que entrem em conflito com a API de models, como clean, save ou delete.

		- Fields Types
			Cada campo do sue modelo deve ser uma instáncia da Field classe apropriada. Django usa os tiposdeclasse de campo para determinar algumas coisas. 
			Ex: Tipo de coluna, widget html e os requisitos minimos de vlaidação.

			O Django vem com dezenas de tipos de cmapos integrados; você pode encontrar a lista completa na (https://docs.djangoproject.com/en/4.2/ref/models/fields/#model-field-types)
				
		- Field Options
			Cada campo recebe um determinado conjunto de argumentos especificos do campo. Ex: CharField que requer um max_lenght, que especifique o tamanho do VARCHAR campo do DB.
				
			Há também em conjunto de argumentos comuns disponíveis para todos os tipos de campos. Todos são opcionais.

			null: Se True, o Django armazenará valores vazios como NULL no DB. O padrão e False.
			blank: Se True, o campo poderá ficar em branco. Padrão e False.

			O null trabalha diretamente com o banco de dados enquanto o blank está mais relacionando com as válidações.

			choices: Uma sequência de 2 tuplas apra usar como opções para este campo. Se isso for fornecido, o widget de formulário padrão será uma caixa de seleção em vez do cmapo 
			de texto padrão e limitará as opções ãs opções fornecidas.
				
			default: Valor padrão do campo. Pode ser um valor ou um objeto que pode ser chamado.
				
			help_text: Texto extra de "ajuda" a ser exibido com o widget de formulario. É util para documentação mesmo que seu campo não seja usado em um formulario.
				
			primary_key: Se true, este cmapo será a chave primaria do modelo

		- Por padrão o Django fonece a cada modelo uma chave primária de incremento automatico.
			Exemplo: id = models.BigAutoField(primary_key=True)


		- Relacionamentos
			Claramente, o poder dos bancos de dados relacionais reside em relacionar as tabelas entre si. Django oferece maneiras de definir os trés tipos mais comuns de relacionamentos de Banco de Dados.: 
			Muitos para Um, Muitos para Muitos e Um para Um.
			
			- ManyToOne:
				Para declarar o relacionamento muitos para um, use o ForignKey. E usado como qualquer outro Field type: Incluindo-o como um atributo de classe do seu modelo.
				ForeIgnKey requer um argumento posicional: a classe á qual o modelo está relacionado.
				
				Ex: Um Cliente e um Pedido. Enquanto o Cliente pode ter vários pedido um Pedido só pode ter um Cliente.

			- ManyToMany
				Para definir um relacionamento muitos para muitos, use ManyToManyField.

				E uma boa pratica colocar o nome do campo do ManyToManyField seja um plural que descreva o conjunto de objetos de modelo relacionados.

				ManyToManyField: Os campos também aceitam vários argumentos extras.
					https://docs.djangoproject.com/en/4.2/ref/models/fields/#manytomany-arguments
				
				Você também pode usar add(), create() ou set()(Precisa ser passado um iteravel) para criar relacionamentos, desde que especifique through_defaults quaisquer campos.
		
				Remove(): E possivel remover especificando a instancia a ser desvinculada. Usado para desvincular a relação entre objetos.
			
				Clear(): Método usado para remover todos os relacionamentos ManyToMany de uma instáncia. Desvincula TODAS as relações com o objeto em questão.
				

			- OneToOne
				Para definir um relacionamento Um-Para-Um, use OneToOneField. E um tipo de relacionamento em que cada registro em uma tabela está associado a exatamente um registro. Isso é geralmente usado quando 
				você tem duas entidades que têm relação direta e cada instãncia de uma está relacionado a uma unica instancia de outra.

				Ex: Um Perfil com uma relação OneToOne com uma model Usuario.
	Fontes: 
		https://docs.djangoproject.com/en/4.2/topics/db/models/ (Documentação Models)
		https://docs.djangoproject.com/en/4.2/ref/models/fields/#model-field-types (Tipos de campos)
		https://docs.djangoproject.com/en/4.2/ref/models/fields/#common-model-field-options (Referencia de campo)


- Django Sessions
	- O que são?
		As sessões no Django são uma maneira de armazenar informações especificas de um usúario entre solicitações HTTP. Eles permitem que voce rastreie o estado do usúario e armazene dados temporariamente ennquanto o usúario interage
		com sua aplicação. As sessoes s~ão úteis para implementar recursos como autenticação, carrinhos de compras em lojas onine e personalização de contéudo para cada usúario.

		Exemplo figurativo: Um guarda que protege uma aréa reservada pra pessoas especificas.

	- Habilitar sessões:
		As sessões são implementadas por meio de um middleware.

		Edite a MIDDLLEWARE configuração e verifique se ela contém arquivos 'django.contrib.sessions.middleware.SessionMiddleware'.
		O padrão criado o settings.py criado pelo django-admin startproject tem SessionMiddleware ativado.

	
	- Configurando o mecanismo de sessão
		Por padrão, o Django armazena sessões em seu banco de dados(usando o model django.contrib.session.models.session). Embora isso seja conveniente
		em algumas configurações é mais rapido armazenas dados de sessão em outro lugar, então o Django pode serconfigurando para armazenar dados de sessão em seu sistema de arquivos ou em seu cache.
	
		- Usando sessões baseadas em Banco de dados
			Se quiser usar uma sessão em banco de dados, você precisará adicionar 'django.contrib.session' em sua INSTALLED_APPS configuração.
			
			Depois de configurar, execute o migrate.

		- Usando sessões em cache:
			Para melhor performance, você pode usar um back-end de sessão baseado em cache.
		
			Para armazenas dados de sessão usando o sistema de cache do Django, primeiro você precisa ter certeza de ter configurado seu cache,


			Documentação cache: https://docs.djangoproject.com/en/4.2/topics/cache/
	
		- Usando sessões baseadas em arquivos:
			Para usar sessões baseadas em arquivo, defina a SESSION_ENGINE configuração como "django.contrib.sessions.backends.file".
			
			VocÊ também pode querer definir a SESSION_FILE_PATH configuração(cujo padrão é a saida de tempfile.gettempdir(), provavelmente /tmp)
			 para controlar onde o Django armazena os arquivos de sessão. Certifique-se de verificar se o seu servidor web tem eprmissões para ler e gravar neste local.

		- Usando sessões baseadas em cookies:
			Para usar sessões baseadas em cookeis, defina a SESSION_ENGINE como "django.contrib.sessions.backends.signed_cookies". Os dados da sessao serão armazenados usando as ferramentas do Django para assinatura criptografica e SECRET_KEY configuração.
	
	- Usando sessões em views:
		Quando SessionMiddleware ativado, cada Httprequest objeto - o primeiro argumento para qualquer função de views do Django- terá um session atributo, que é um objeto semelhante a um dicionario.

		Você pode lêlo e escrever request.session em qualquer ponto de sua views. Você pode editá-lo várias vezes.
			
		- class backends.base.SessionBase:
			Está e a classe base para todos os objetos de sessão. Possui os seguintes metodosde dicionario padrão.
			
			__getitem__(key): Exemplo: fav_color = request.session['fav_color']
			
			__setitem__(key, value): exemplo: request.session['fav-color'] = 'Blue'
			
			__delitem__(key): exemplo: del request.session['fav-color']. Levanta um keyerror se o argumento key não existe na sessão.

			__contains__(key): 'fav_color' in request.session

			get(key): Exemplo: fav_color = request.session.get("fav_color")

			pop(key): fav_color = request.session.pop("fav_color", 'blue)
			
			keys() , items(), setdefault()
			
			clear(): É usado para apagar todos os elementos armazenados na sessão do usúario, mas a sessão em si permanece ativa.
			Isso significa que a sessão não é encerrada ou destrúida; somente os dados de dentro da sessão serão removidos.

			flush(): É usado apra apagar todos os dados da sessão, assim como o método clear(), mas com uma diferença importante:
			aós chamar o flush() a sessão é encerrada e uma nova sessão vazia é criada. Isso significa que o usúario receberá um
			novo ID de sessão na próxima solicitação.
		
		- Tempo de vida da sessão:
			O tempo de vida da sessão em Django é controlado pela configuração 'SESSION_COOKIE_AGE'. Essa configuração define o
			 tempo, em segundos, que uma sessão permanecerá ativa antes de expirar. Por padrão o valor configurado para SESSION_COOKIE_KEY e de 2 semanas(1209600 segundos).
		
			Você pode personalizar o tempo de vida ajustando o valor da 'SESSION_COOKIE_AGE' nas configurações do seu projeto Django.

			get_session_cookie_age: Retorna o valor da configuração SESSION_COOKIE_AGE. Isso pode ser substituido em um back-end de sessão eprsonalizada.

			set_expiry(value): Define o tempo de expiração de sessão. Você pode passar vários valores diferentes.

			get_expiry_age(): Retorna o número de segundos até que está sessão expire. Para sessões sem expiração personalizada(ou aquelas configuradas para expirar ao fechar o navegador, isso será igual a SESSION_COOKIE_AGE)
			

	- Fontes: https://docs.djangoproject.com/en/4.2/topics/http/sessions/#django.contrib.sessions.backends.base.SessionBase.items
		  https://www.tutorialspoint.com/django/django_sessions.htm


- Django Auth
	- Oque é:
			Django vem com um sistema de autenticação de usuário. Ele lida com contas de usuários, grupos e permissões e sessões de usuários baseadas em cookies.
			O sistema de autenticação consiste em: Usuários, permissões, grupos, hash de criptocracia, formularios e ferramentas de vizualização e um sistema backend bonectavel.
	Instalação:
		O suporte á autenticação é fornecido como um módulo de contribuição do Django no django.contrib.auth. Por padrão, a configuração necessária já esta incluida no settings.py.

	- Django.contrib.auth 
		Fornece um conjunto de módulos e funcionalidades que fazem parte do Django e são usados para lidar com a autenticação e a autorização em um projeto web.

		- User model:
			É responsável por representar informações sobre os usuários registrados no sistema. O django um modelo de usuario padrão, mas também permite que você crie modelos de usuario
			personalizados, estendendo o modelo padrão ou criando um novo do zero.

			User objetos possuem diversos campos já prontos. Alguns:
				• username: Campo obrigatório. máximo de 150 caracterés. Os nomes podem conter caracterés alfanumericos @_+-.
				• first_name: Opcional. 150 caracterés ou menos.
				• email: Opcional.
				• password: Obrigatório. Um hash e metadados sobre a senha.(O django não armazena a senha bruta).

		- Atributos:
			• is_authenticated: A property is_authenticated em um objeto de usuario retorna True se o usuário estiver autenticado, ou seja, se existir no banco de dados e fez login com sucesso.
			• is_anonymous: Está e uma forma de diferenciar User e o AnonymousUser objetos.


		- Methods: Os objetos de usuário no Django vêm com vários métodos úteis que permitem realizar ações relacionadas á autenticação, verificação de permissões e outras operações relacionadas aos usuarios. Alguns:
			•
	- Fontes:
		https://docs.djangoproject.com/en/4.2/topics/auth/
		https://docs.djangoproject.com/en/4.2/ref/contrib/auth/
		https://docs.djangoproject.com/en/4.2/topics/auth/customizing/
	
	)https://github.com/treinaweb/treinaweb-django-fundamentos
https://docs.djangoproject.com/en/4.2/intro/tutorial01/
https://github.com/marinho/aprendendo-django/blob/master/08-trabalhando-com-arquivos-estaticos.md
- O que é o Django?
    Django e um framework web Python de alto nivel que permite o rápido
    desenvolvimento de sites seguros e de fácil manutenção.

- Arquitetura MVT 
    O Django usa o modelo Cliente sevidor, e o seu fluxo de dados segue a arquitetura MVT.

    Model: Mapeamento do banco de dados do projeto.
    Views: E responsavel pela logica da aplicação. Ou seja controlam o comportamento e as funcionalidades do app.
    Template: Aqui onde serão renderizados o html da aplicação.
    Fluxo de dados Django:
        
        O cliente(usuario) faz uma requisição pro nosso servidor(back-end);
        o urls.py verifica a URL pra ver se eéuma rota valida;
        A views.py processa os dados dependendo da url fornecida;
        O template e renderizado e retorna uma response
        Fonts:
            (https://pythonacademy.com.br/blog/desenvolvimento-web-com-python-e-django-introducao)
            (https://www.youtube.com/watch?v=91aD2aCRZIg&pp=ygUVZmx1eG8gZGUgZGFkb3MgREpBTkdP)

- Instalação Django 
    - Ambiente Virtual: Por boas praticas isolamos nosso projeto com uma venv 
        install: pip install virtualenv 

        Criar e Ativar o ambiente:
            Criar: 
                py -m venv nome_do_ambiente
            Ativar:
                windows: 
                    nome_do_ambiente/scripts/activate
                Linux: 
                    source nome_do_ambiente/scripts/activate or nome_do_ambiente

        Instale o Django no Ambiente:
            pip install django        

- Criação do Projeto 
    Projeto Django pode ser considerado um container que abriga todo o aplicativo.

    Criação: django-admin startproject nome_projeto . (O ponto e para não criar uma pasta a mais)

    O comando ira criar um projeto Django com a seguinte estrutura de pastas:
        manage.py E o arquivo utilitario do Django. Ele e basicamte o gerente do projeto.
        settings.py Contém as configurações gerais do projeto.
        urls.py: Configura o Roteamento de url do app.
        wsgi.py Usado para implantar o aplicativo em servers da web compativeis como a apache.
        
- Criação da app
    O Django é dividido em 'apps'(aplicações) para promover o conceito de 'dividir e conquistar' para facilitar grandes aplicações.

    Criação: py manage.py startapp nome_app 

- Rotas Django
	As rotas em Django são responsáveis por direcionar as requests do navegador para funções apropriadas em seu aplicativo.
	O sistema de Rotas do Django e gerenciado seguindo uma hierarquia.
	
	
	Urls pai(urls do projeto): Elas organizam a estrutura geral de URLs do seu projeto e direcionam o tráfego para os aplicativos
	específicos.Ou seja: Ele cria outras urls. Ex: uma url chamada accounts/, a função dela e criar outras urls filhas login e register, etc.

		* Informe o caminho da da url do app	
			path('accounts/', include('app.urls'))

	Urls filha:(Urls dos apps): São responsáveis por mapear caminhos URLS especificos para as funções de views
		
		path('login/', views.login, name='login'),

- Configuração e criação da pasta templates
	Em Django templates são usados para criar a camada de apresentação da aplicação.
	
	- Configurar
		Em settings.py procure por TEMPLATES
		Na DIRS: [] especifique o caminho que vocé quer que o Django procure os templates
		DIRS: [os.path.join(BASE_DIR, 'templates')] utilizamos a biblioteca os, mas podeser a pathlib

		Na 'APP_DIRS': True e especificado em Boolean Se você quer que o Django também procure por templates de dentro dos apps.

		OPTIONS: contém configurações específicas de back-end.

- Configuração e criação dos Arquivos estaticos
	Arquivos estaticos são arquivos css, js e de img, ou seja arquivos que irão ser renderizados no template.
	
	- Explicando:
		- STATIC_URL: E a url base na qual os arquivos estaticos irão ser servidos para os usuarios.
			
			Configurando em settings.py:
				A variavel STATIC_URL deve receber uma string informando o caminho. Por padrão e utilizado o '/static/'.

				Exemplo de uso: /static/img/img_.png


		- STATIC_ROOT: E o local onde você quer que seja reunido todos os arquivos estaticos para quando for colocar em produção.
			Configurando em settings.py: 
				Com a biblioteca lib importada concatenamos o diretorio absoluto do projeto com a string informando o caminho a ser criado.
					Exemplo: os.path.join(BASE_DIR, 'staticfiles')

		- STATICFILES_DIRS: São caminhos personalizados que voce queira que o Django reconheça como locais adicionais para buscar os arquivos estaticos.
			Configurando em settings.py:
				Utilizamos uma lista ou uma tupla para informar os caminhos personalizados.
				Exemplo:
				STATICFILES_DIRS = [
					os.path.join(BASE_DIR, 'templates/static')
				]
	- Modo de uso:
		Para carregar os arquivos staticos utilizamos a tag do Django {% load static %}(No topo do HTML). Essa tag permite que o Django reconheça e interprete a tag {% static '' %} 
		para gerar URLS para os arquivos estaticos.
		
		E para se importado utilizamos {% static 'path' %} do arquivo a ser utilizado.

	- fonts:
		(https://www.youtube.com/watch?v=w9KFXoNoJT4),
		(https://docs.djangoproject.com/en/4.2/howto/static-files/)

- Configuração e criação dos arquivos de média
	Arquivos média no contexto do Django se referem a arquivos enviados pelos usuarios como imagens, audio, pdf, videos e etc.

	- Explicando:
		MEDIA_URL: Ela define a URL base para ser servido ao usuario o arquivo media.
			- Configurando: 
				Cria uma variavel MEDIA_URL = e informe uma string representando o caminho URL. Por padrão e configurado '/media/'.
			
		MEDIA_ROOT: Ela define o caminho absoluto no sistema onde os arquivos serão armazenados.
			- Configurando: crie uma variavel MEDIA_ROOT= os.path.join(BASE_DIR, 'nome_path')(e concatene usando a biblioteca os com a raiz do projeto)
			  Por padrão, e utilizado o 'media/'. Não e necessario criar a pasta o proprio Django já cria ao receber algum arquivo.


- Como trabalhar com formularios no Django
	- Formulários são componentes HTML/widgets em uma página da web que coletam informações dos usuários para serem enviadas e processadas no servidor.
	  Um formulário HTML é definido com os atributos action (especifica o URL onde os dados serão enviados) e method (determina o método HTTP utilizado).

	- Explicando:

		- Action: E especificado o path/url para onde os dados devem ser enviados para a URL processar os dados no servidor.
		- Method: O método HTTP utilizado para enviar os dados.
			-POST: E utilizado se os dados forem resultar em uma alteração no banco de dados, pois é mais resistente a ataques de falsificação.
			-GET: deve ser utilizado somente para formulários que não alteram dados. Ex: um form de busca.
		
	- Exemplo:
		<form action="/processar-dados" method="POST">
    			<label for="usuario">Usuário:</label>
    			<input type="text" id="usuario" name="usuario">
    			<button type="submit">Enviar</button>
		</form>	
	
	- Django forms:
		O Django fornece recursos que facilitam a criação e renderização de um formulario a serem reutilizados. .
		Fontes: (https://docs.djangoproject.com/en/4.2/topics/forms/)

	- Fontes: (https://developer.mozilla.org/pt-BR/docs/Learn/Server-side/Django/Forms),
		  (https://developer.mozilla.org/pt-BR/docs/Web/HTTP)

- Django ORM
	- O que e o ORM
		O ORM(Mapeador objeto-relacional) é uma estrutura web Python para interagir com banco de dados como criar consultas complexas ao banco de dados.
		Em outras palavras utilizamos a ORM como ponte de comunicação entre o banco e a aplicação sem precisar escrever codigos SQL.

	- QuerySet
		Queryset é um conjunto de ações que serão realizadas no banco de dados, ou seja, podemos criar, buscar, atualizar ou deletar os dados sem escrever a query SQL.

	- Django shell
		O shell do Django é uma ferramento interativa que permite que você faça consultas interativas. Ela é uma maneira conveniente de explorar e testar as funcionalidade
		Django, além de realizar consultas ao Banco de dados de forma interativa. E útil para depurar e testar seu código durante desenvolvimento.

		Para usar:
			py manage.py shell

		Fontes: https://docs.djangoproject.com/en/4.2/topics/db/queries/
	- Models
		Como vimos na arquitetura do Django, a models e a parte onde será feita os moldes do banco de dados.
		
		Um modelo é a fonte unica e definitiva de informações sobre seus dados. Ele contém os campos e comportamentos
		essenciais dos dados a ser armazenados.

		Fonte: https://docs.djangoproject.com/en/4.2/topics/db/models/
		https://docs.djangoproject.com/en/4.2/topics/db/examples/many_to_one/


	- Fontes: (https://www.alura.com.br/artigos/django-query-sets-e-orm
		   https://afropython.gitbook.io/tutorial/queryset_e_orm_do_django/shell_do_django


- Rotas Dínamicas Django
	- O que são
		Urls dinãmicas são caracteristicas que ermite criar padrões de URL flexíveis que podem incluir partes variáveis
		como números inteiros, strings, slugs e outros tipos de dados. A principal função é permitir que voce capture informações
		diretamento da URL e use essas informações para determinar qual view será executada.

		Ex: Em urls.py(app), crie um path('<int:variavel>')
		Em views def home(request, variavel):
				return httpresponse(variavel)
	
	- Fontes: (https://docs.djangoproject.com/en/4.2/topics/http/urls/#path-converters)


- Django Filtros
	- O que são
		Em Django, os filtros são utilizados para renderizar dados dinâmicos em uma página web. Eles permitem que você modifique ou formate
		os valores dentro de um template antes de exibi-los. São usados para tarefas comuns. como formatar datas, números, strings e muito mais.
		
	- Fontes:
		(https://docs.djangoproject.com/en/4.2/ref/templates/builtins/)


- Django Migrações
	As migrações do Django são a maneira de propagar as alterações feitas em seus modelos(adicionando, excluindo, etc) em seu esquema de banco de dados.
	Eles foram projetados para serem quase sempre autómaticos, mas você precisará saber quando fazer migrações, quando executa-las e os probelams comuns que podem ocorrer.

	- Comandos
		Existem vários comandso que você usará para interagir com migrações e manipulação do banco do Django:
		
		* migrate: E responsável por aplicar e cancelar as aplicações de migrações.
		* makemigrationns: E responsavel por criar novas migrações com base nas alterações feitas em seus modelos.
		* sqlmigrate: Exibir as intruções SQL para uma migração.
		* shomigrations lista as migrações de um projeto e seu status.

	Você deve pensar nas migrações como um sistema de controle de versão para seu banco de dados. makemigrations e responsavel por empacotar as alterações
	do seu modelo em arquivos de migraç~eos individuais - analogo ao commit e migrate e responsavel por aplicar-los ao seu banco de dados

- Django Admin
	- O que são
		E uma ferramenta poderosa e amigável para gerenciar o contéudo de um site ou aplicativo web. Permite que os administradores realizem tarefas de 
		gerenciamento de contéudo sem a necessidade de escrever código personalizado.
		
	- URL:
		Para ser acessado usa a URL ('/admin/', por padrão).
	
	- Criar super usuario: 
		Para criar um super usúario para fazer login use o comando: py manage.py createsuperuser.
	
	- Adicionar uma models no admin:
		Para adicionar models ao Admin, siga as etapas em admin.py:
 			importe o models: from nome_app.models import nome_models;
			Registre o nome da models: admin.site.register(nome_models);


	- Personalização da Admin:	
		Os django oferece várias maneiras de personalizar o Admin, para torna-la mais adequada ãs necessidades especificas
		do seu aplicativo. Voce pode personalizar campos de exibição, criar ações personalizadas e até mesmo personalizar os templatesde administração.
		
		
		Saber-mais:  https://docs.djangoproject.com/en/4.2/ref/contrib/admin/

		List_display: Controla quais campos são exibidos na página da lista de alterações do administrador.
		
	
	- Fontes:
		(https://docs.djangoproject.com/en/4.2/ref/contrib/admin/)
		https://docs.djangoproject.com/en/4.2/ref/contrib/admin/filters/


- Django Models
	- O que é:
		Um models é a fonte unica e defnitiva de informaç~eos sobre os dados. Ele contém os campos e comportamentos essenciais dos dados armazenados.
		Cada modelo é mapeado para uma única tabela do DataBase.
		
		- O básico: 
			Cada modelo é uma classe Python que subsclassifica django.db.models.Model;
			Cada atributo do modelo representa um campo do DataBase;
			o Django possui uma API de acesso ao banco de dados gerada automaticamente, vimos anteriormente o Shell.
		
		- Migrações
			Ao adicionar novos apps no INSTALLED_APPS, certifique-se de executar o, opcialente fazendo migrações para que eles primeiro com o manage.py migrate e makemigrations.
		
		- Fields
			São a parte mais importante de um modelo, é a lista de campos de um banco de dados. Os campos são especificados por atributos da classe. Tenha cuidado para não escolher
			nomes e campos que entrem em conflito com a API de models, como clean, save ou delete.

		- Fields Types
			Cada campo do sue modelo deve ser uma instáncia da Field classe apropriada. Django usa os tiposdeclasse de campo para determinar algumas coisas. 
			Ex: Tipo de coluna, widget html e os requisitos minimos de vlaidação.

			O Django vem com dezenas de tipos de cmapos integrados; você pode encontrar a lista completa na (https://docs.djangoproject.com/en/4.2/ref/models/fields/#model-field-types)
				
		- Field Options
			Cada campo recebe um determinado conjunto de argumentos especificos do campo. Ex: CharField que requer um max_lenght, que especifique o tamanho do VARCHAR campo do DB.
				
			Há também em conjunto de argumentos comuns disponíveis para todos os tipos de campos. Todos são opcionais.

			null: Se True, o Django armazenará valores vazios como NULL no DB. O padrão e False.
			blank: Se True, o campo poderá ficar em branco. Padrão e False.

			O null trabalha diretamente com o banco de dados enquanto o blank está mais relacionando com as válidações.

			choices: Uma sequência de 2 tuplas apra usar como opções para este campo. Se isso for fornecido, o widget de formulário padrão será uma caixa de seleção em vez do cmapo 
			de texto padrão e limitará as opções ãs opções fornecidas.
				
			default: Valor padrão do campo. Pode ser um valor ou um objeto que pode ser chamado.
				
			help_text: Texto extra de "ajuda" a ser exibido com o widget de formulario. É util para documentação mesmo que seu campo não seja usado em um formulario.
				
			primary_key: Se true, este cmapo será a chave primaria do modelo

		- Por padrão o Django fonece a cada modelo uma chave primária de incremento automatico.
			Exemplo: id = models.BigAutoField(primary_key=True)


		- Relacionamentos
			Claramente, o poder dos bancos de dados relacionais reside em relacionar as tabelas entre si. Django oferece maneiras de definir os trés tipos mais comuns de relacionamentos de Banco de Dados.: 
			Muitos para Um, Muitos para Muitos e Um para Um.
			
			- ManyToOne:
				Para declarar o relacionamento muitos para um, use o ForignKey. E usado como qualquer outro Field type: Incluindo-o como um atributo de classe do seu modelo.
				ForeIgnKey requer um argumento posicional: a classe á qual o modelo está relacionado.
				
				Ex: Um Cliente e um Pedido. Enquanto o Cliente pode ter vários pedido um Pedido só pode ter um Cliente.

			- ManyToMany
				Para definir um relacionamento muitos para muitos, use ManyToManyField.

				E uma boa pratica colocar o nome do campo do ManyToManyField seja um plural que descreva o conjunto de objetos de modelo relacionados.

				ManyToManyField: Os campos também aceitam vários argumentos extras.
					https://docs.djangoproject.com/en/4.2/ref/models/fields/#manytomany-arguments
				
				Você também pode usar add(), create() ou set()(Precisa ser passado um iteravel) para criar relacionamentos, desde que especifique through_defaults quaisquer campos.
		
				Remove(): E possivel remover especificando a instancia a ser desvinculada. Usado para desvincular a relação entre objetos.
			
				Clear(): Método usado para remover todos os relacionamentos ManyToMany de uma instáncia. Desvincula TODAS as relações com o objeto em questão.
				

			- OneToOne
				Para definir um relacionamento Um-Para-Um, use OneToOneField. E um tipo de relacionamento em que cada registro em uma tabela está associado a exatamente um registro. Isso é geralmente usado quando 
				você tem duas entidades que têm relação direta e cada instãncia de uma está relacionado a uma unica instancia de outra.

				Ex: Um Perfil com uma relação OneToOne com uma model Usuario.
	Fontes: 
		https://docs.djangoproject.com/en/4.2/topics/db/models/ (Documentação Models)
		https://docs.djangoproject.com/en/4.2/ref/models/fields/#model-field-types (Tipos de campos)
		https://docs.djangoproject.com/en/4.2/ref/models/fields/#common-model-field-options (Referencia de campo)


- Django Sessions
	- O que são?
		As sessões no Django são uma maneira de armazenar informações especificas de um usúario entre solicitações HTTP. Eles permitem que voce rastreie o estado do usúario e armazene dados temporariamente ennquanto o usúario interage
		com sua aplicação. As sessoes s~ão úteis para implementar recursos como autenticação, carrinhos de compras em lojas onine e personalização de contéudo para cada usúario.

		Exemplo figurativo: Um guarda que protege uma aréa reservada pra pessoas especificas.

	- Habilitar sessões:
		As sessões são implementadas por meio de um middleware.

		Edite a MIDDLLEWARE configuração e verifique se ela contém arquivos 'django.contrib.sessions.middleware.SessionMiddleware'.
		O padrão criado o settings.py criado pelo django-admin startproject tem SessionMiddleware ativado.

	
	- Configurando o mecanismo de sessão
		Por padrão, o Django armazena sessões em seu banco de dados(usando o model django.contrib.session.models.session). Embora isso seja conveniente
		em algumas configurações é mais rapido armazenas dados de sessão em outro lugar, então o Django pode serconfigurando para armazenar dados de sessão em seu sistema de arquivos ou em seu cache.
	
		- Usando sessões baseadas em Banco de dados
			Se quiser usar uma sessão em banco de dados, você precisará adicionar 'django.contrib.session' em sua INSTALLED_APPS configuração.
			
			Depois de configurar, execute o migrate.

		- Usando sessões em cache:
			Para melhor performance, você pode usar um back-end de sessão baseado em cache.
		
			Para armazenas dados de sessão usando o sistema de cache do Django, primeiro você precisa ter certeza de ter configurado seu cache,


			Documentação cache: https://docs.djangoproject.com/en/4.2/topics/cache/
	
		- Usando sessões baseadas em arquivos:
			Para usar sessões baseadas em arquivo, defina a SESSION_ENGINE configuração como "django.contrib.sessions.backends.file".
			
			VocÊ também pode querer definir a SESSION_FILE_PATH configuração(cujo padrão é a saida de tempfile.gettempdir(), provavelmente /tmp)
			 para controlar onde o Django armazena os arquivos de sessão. Certifique-se de verificar se o seu servidor web tem eprmissões para ler e gravar neste local.

		- Usando sessões baseadas em cookies:
			Para usar sessões baseadas em cookeis, defina a SESSION_ENGINE como "django.contrib.sessions.backends.signed_cookies". Os dados da sessao serão armazenados usando as ferramentas do Django para assinatura criptografica e SECRET_KEY configuração.
	
	- Usando sessões em views:
		Quando SessionMiddleware ativado, cada Httprequest objeto - o primeiro argumento para qualquer função de views do Django- terá um session atributo, que é um objeto semelhante a um dicionario.

		Você pode lêlo e escrever request.session em qualquer ponto de sua views. Você pode editá-lo várias vezes.
			
		- class backends.base.SessionBase:
			Está e a classe base para todos os objetos de sessão. Possui os seguintes metodosde dicionario padrão.
			
			__getitem__(key): Exemplo: fav_color = request.session['fav_color']
			
			__setitem__(key, value): exemplo: request.session['fav-color'] = 'Blue'
			
			__delitem__(key): exemplo: del request.session['fav-color']. Levanta um keyerror se o argumento key não existe na sessão.

			__contains__(key): 'fav_color' in request.session

			get(key): Exemplo: fav_color = request.session.get("fav_color")

			pop(key): fav_color = request.session.pop("fav_color", 'blue)
			
			keys() , items(), setdefault()
			
			clear(): É usado para apagar todos os elementos armazenados na sessão do usúario, mas a sessão em si permanece ativa.
			Isso significa que a sessão não é encerrada ou destrúida; somente os dados de dentro da sessão serão removidos.

			flush(): É usado apra apagar todos os dados da sessão, assim como o método clear(), mas com uma diferença importante:
			aós chamar o flush() a sessão é encerrada e uma nova sessão vazia é criada. Isso significa que o usúario receberá um
			novo ID de sessão na próxima solicitação.
		
		- Tempo de vida da sessão:
			O tempo de vida da sessão em Django é controlado pela configuração 'SESSION_COOKIE_AGE'. Essa configuração define o
			 tempo, em segundos, que uma sessão permanecerá ativa antes de expirar. Por padrão o valor configurado para SESSION_COOKIE_KEY e de 2 semanas(1209600 segundos).
		
			Você pode personalizar o tempo de vida ajustando o valor da 'SESSION_COOKIE_AGE' nas configurações do seu projeto Django.

			get_session_cookie_age: Retorna o valor da configuração SESSION_COOKIE_AGE. Isso pode ser substituido em um back-end de sessão eprsonalizada.

			set_expiry(value): Define o tempo de expiração de sessão. Você pode passar vários valores diferentes.

			get_expiry_age(): Retorna o número de segundos até que está sessão expire. Para sessões sem expiração personalizada(ou aquelas configuradas para expirar ao fechar o navegador, isso será igual a SESSION_COOKIE_AGE)
			

	- Fontes: https://docs.djangoproject.com/en/4.2/topics/http/sessions/#django.contrib.sessions.backends.base.SessionBase.items
		  https://www.tutorialspoint.com/django/django_sessions.htm


- Django Auth
	- Oque é:
			Django vem com um sistema de autenticação de usuário. Ele lida com contas de usuários, grupos e permissões e sessões de usuários baseadas em cookies.
			O sistema de autenticação consiste em: Usuários, permissões, grupos, hash de criptocracia, formularios e ferramentas de vizualização e um sistema backend bonectavel.
	Instalação:
		O suporte á autenticação é fornecido como um módulo de contribuição do Django no django.contrib.auth. Por padrão, a configuração necessária já esta incluida no settings.py.

	- Django.contrib.auth 
		Fornece um conjunto de módulos e funcionalidades que fazem parte do Django e são usados para lidar com a autenticação e a autorização em um projeto web.

		- User model:
			É responsável por representar informações sobre os usuários registrados no sistema. O django um modelo de usuario padrão, mas também permite que você crie modelos de usuario
			personalizados, estendendo o modelo padrão ou criando um novo do zero.

			User objetos possuem diversos campos já prontos. Alguns:
				• username: Campo obrigatório. máximo de 150 caracterés. Os nomes podem conter caracterés alfanumericos @_+-.
				• first_name: Opcional. 150 caracterés ou menos.
				• email: Opcional.
				• password: Obrigatório. Um hash e metadados sobre a senha.(O django não armazena a senha bruta).

		- Atributos:
			• is_authenticated: A property is_authenticated em um objeto de usuario retorna True se o usuário estiver autenticado, ou seja, se existir no banco de dados e fez login com sucesso.
			• is_anonymous: Está e uma forma de diferenciar User e o AnonymousUser objetos.


		- Methods: Os objetos de usuário no Django vêm com vários métodos úteis que permitem realizar ações relacionadas á autenticação, verificação de permissões e outras operações relacionadas aos usuarios. Alguns:
			•
	- Fontes:
		https://docs.djangoproject.com/en/4.2/topics/auth/
		https://docs.djangoproject.com/en/4.2/ref/contrib/auth/
		https://docs.djangoproject.com/en/4.2/topics/auth/customizing/
	
	
