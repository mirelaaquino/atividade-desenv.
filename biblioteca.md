# atividade-desenv.
django-admin startproject meu_projeto
cd meu_projeto
python manage.py startapp biblioteca

# biblioteca/models.py
from django.db import models

class Autora(models.Model):
    nome = models.CharField(max_length=100)
    nacionalidade = models.CharField(max_length=50)

    def __str__(self):
        return self.nome

class Livro(models.Model):
    titulo = models.CharField(max_length=200)
    data_publicacao = models.DateField()
    autora = models.ForeignKey(Autora, on_delete=models.CASCADE)

    def __str__(self):
        return self.titulo

        # meu_projeto/settings.py
INSTALLED_APPS = [
    ...
    'biblioteca',
]

# biblioteca/views.py
from django.views.generic import ListView, CreateView, UpdateView, DeleteView
from django.urls import reverse_lazy
from .models import Livro, Autora

# Views para Autora
class AutoraList(ListView):
    model = Autora
    template_name = 'autora_list.html'

class AutoraCreate(CreateView):
    model = Autora
    template_name = 'autora_form.html'
    fields = ['nome', 'nacionalidade']
    success_url = reverse_lazy('autora-list')

class AutoraUpdate(UpdateView):
    model = Autora
    template_name = 'autora_form.html'
    fields = ['nome', 'nacionalidade']
    success_url = reverse_lazy('autora-list')

class AutoraDelete(DeleteView):
    model = Autora
    template_name = 'autora_confirm_delete.html'
    success_url = reverse_lazy('autora-list')

# Views para Livro
class LivroList(ListView):
    model = Livro
    template_name = 'livro_list.html'

class LivroCreate(CreateView):
    model = Livro
    template_name = 'livro_form.html'
    fields = ['titulo', 'data_publicacao', 'autora']
    success_url = reverse_lazy('livro-list')

class LivroUpdate(UpdateView):
    model = Livro
    template_name = 'livro_form.html'
    fields = ['titulo', 'data_publicacao', 'autora']
    success_url = reverse_lazy('livro-list')

class LivroDelete(DeleteView):
    model = Livro
    template_name = 'livro_confirm_delete.html'
    success_url = reverse_lazy('livro-list')

    # biblioteca/urls.py
from django.urls import path
from .views import (
    AutoraList, AutoraCreate, AutoraUpdate, AutoraDelete,
    LivroList, LivroCreate, LivroUpdate, LivroDelete
)

urlpatterns = [
    # URLs para Autora
    path('autoras/', AutoraList.as_view(), name='autora-list'),
    path('autoras/novo/', AutoraCreate.as_view(), name='autora-create'),
    path('autoras/<int:pk>/editar/', AutoraUpdate.as_view(), name='autora-update'),
    path('autoras/<int:pk>/deletar/', AutoraDelete.as_view(), name='autora-delete'),

    # URLs para Livro
    path('livros/', LivroList.as_view(), name='livro-list'),
    path('livros/novo/', LivroCreate.as_view(), name='livro-create'),
    path('livros/<int:pk>/editar/', LivroUpdate.as_view(), name='livro-update'),
    path('livros/<int:pk>/deletar/', ⬤
