# Estrutura de um projeto Django (MTV)

No Django, o padrão clássico **MVC (Model–View–Controller)** é interpretado de forma diferente, resultando no padrão **MTV (Model–Template–View)**.  
O “controller” do MVC, no Django, é o próprio **framework**, que recebe a requisição e a direciona para a view correta com base nas configurações de URL.

---

## Models e ORM (Banco de Dados com SQLite)

### Banco de Dados com SQLite

- Por padrão, o Django usa **SQLite**, que já vem incluído no Python.
- Isso elimina a necessidade de instalar ou configurar sistemas de banco de dados.
- Para criar as tabelas necessárias (apps do usuário e apps padrão do Django), basta rodar:

```bash
python manage.py migrate
```

Esse comando analisa todos os apps ativados em `INSTALLED_APPS` e cria as tabelas adequadas no banco.

---

### Models no Django

- Um **Model** é a representação de uma estrutura de dados (tabela) no banco.
- Ele é definido como uma **classe Python** que herda de `django.db.models.Model`.
- Cada atributo da classe corresponde a uma **coluna** no banco de dados.

**Exemplo clássico (tutorial polls):**

```python
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField("date published")

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

- Uma **pergunta** tem texto e data de publicação.
- Cada **opção** está ligada a uma pergunta, com texto e contagem de votos.
- O Django gera automaticamente nomes de tabelas (`app_modelname`), campos, chaves primárias e tipos específicos.

---

### ORM — Object-Relational Mapper

- O **ORM do Django** permite interagir com o banco de dados usando **Python ao invés de SQL**.
- Facilita operações CRUD: **criar, ler, atualizar, deletar** registros.

**Exemplo no shell interativo:**

```python
# Criar uma pergunta
q = Question(question_text="What's new?", pub_date=timezone.now())
q.save()

# Acessar dados
q.id
q.question_text

# Listar todas as perguntas
Question.objects.all()
```

- O ORM gera **migrações** com `makemigrations` e aplica ao banco com `migrate`.
- As migrações são **versionáveis** e mantêm o histórico de alterações.

---

## Views e URLs (Rotas e Lógica do Backend)

### Views

- Uma **View** é uma função (ou classe) Python que recebe um `request` e retorna um `response`.
- O retorno pode ser: **HTML, redirecionamento, JSON, imagem, erro 404**, etc.

**Exemplo do tutorial polls:**

- `index`: lista as últimas perguntas.
- `detail`: mostra os detalhes de uma pergunta específica.
- `results`: exibe resultados das votações.
- `vote`: processa um voto.

As views geralmente:

1. Buscam dados dos Models.
2. Processam formulários.
3. Retornam o conteúdo via `HttpResponse`.

---

### URLs e Rotas (URLconf)

- O **URLconf** é um módulo `urls.py` que define uma lista `urlpatterns`.
- Cada entrada mapeia um padrão de URL para uma View.

**Exemplo (`polls/urls.py`):**

```python
urlpatterns = [
    path("", views.index, name="index"),                       # /polls/
    path("<int:question_id>/", views.detail, name="detail"),   # /polls/5/
    path("<int:question_id>/results/", views.results, name="results"), # /polls/5/results/
    path("<int:question_id>/vote/", views.vote, name="vote"),  # /polls/5/vote/
]
```

---

## Templates e Renderização

### Templates no Django

- Arquivos de texto (normalmente **HTML**) que misturam conteúdo estático com **dados dinâmicos**.
- Usam a **linguagem de templates do Django (DTL)**.

**Exemplo de recursos:**

- Variáveis: `{{ variavel }}`
- Tags de controle: `{% if ... %}`, `{% for ... %}`
- Filtros, comentários, etc.

---

### Estrutura de Pastas

```
polls/
    templates/
        polls/
            index.html
```

**Exemplo (`index.html`):**

```html
{% if latest_question_list %}
<ul>
  {% for question in latest_question_list %}
  <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
  {% endfor %}
</ul>
{% else %}
<p>No polls are available.</p>
{% endif %}
```

---

### Renderização na View

A view prepara o contexto (dicionário de dados) e renderiza:

```python
return render(request, "polls/index.html", context)
```

Isso gera um `HttpResponse` com o HTML final.

---

### Renderização como API

- **HTML tradicional:** usa-se o sistema de templates conforme descrito acima, adequado para páginas web.
- **APIs JSON:** normalmente não se usa templates. Em aplicações Django tradicionais, você retornaria algo como `JsonResponse(...)`.
- Com **Django REST Framework (DRF)**, usam-se **Renderers** (ex: `JSONRenderer`) para gerar JSON.

---

## Django Admin

### O que é?

- Interface administrativa **automática** gerada pelo Django para permitir que administradores (como editores ou gerentes de conteúdo) criem, atualizem e apaguem dados dos seus modelos de forma rápida e sem necessidade de programar formulários ou views.
- Essa interface não é voltada para usuários públicos do site, mas sim para gente da equipe que precisa gerenciar o conteúdo.

---

### Como ativar

1. O app `django.contrib.admin` já está ativo por padrão.
2. Criar superusuário:
   ```bash
   python manage.py createsuperuser
   ```
3. Acessar no navegador:
   ```
   http://127.0.0.1:8000/admin/
   ```
4. Registrar os Models em `admin.py`:

```python
from django.contrib import admin
from .models import Question

admin.site.register(Question)
```

---

## Autenticação e Autorização

### Autenticação (Authentication)

- Processo de **verificar a identidade do usuário** (ex: login com usuário e senha).
- O Django oferece:
  - Modelos para usuários, grupos e permissões.
  - Hashing seguro de senhas.
  - Views prontas para login/logout.
  - Backends de autenticação customizáveis.

---

### Autorização (Authorization)

- Define **o que um usuário autenticado pode fazer** na aplicação.
- Baseada em **permissões** associadas a modelos (ex: adicionar, alterar, deletar).
- Permissões podem ser atribuídas a **usuários ou grupos**.

**Exemplo:**

```python
if user.has_perm("polls.add_choice"):
    # Usuário pode adicionar uma escolha
```
