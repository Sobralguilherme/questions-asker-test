# main.py

import os
from django.conf import settings
from django.urls import path
from django.http import HttpResponse
from django.shortcuts import render, redirect

# Configurações do Django
settings.configure(
    DEBUG=True,
    SECRET_KEY='mysecretkey',
    ROOT_URLCONF=__name__,
    MIDDLEWARE_CLASSES=('django.middleware.common.CommonMiddleware',),
    INSTALLED_APPS=('django.contrib.staticfiles',),
    TEMPLATES=[{
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(os.path.dirname(__file__), 'templates')],
    }],
    STATIC_URL='/static/',
)

from django.core.wsgi import get_wsgi_application
from django.http import HttpRequest
from django.urls import path

# Código simples para fazer perguntas interativas

class Pergunta:
    def __init__(self, enunciado, opcoes, resposta_correta):
        self.enunciado = enunciado
        self.opcoes = opcoes
        self.resposta_correta = resposta_correta

    def verificar_resposta(self, resposta_usuario):
        if resposta_usuario.upper() in ('A', 'B', 'C', 'D'):
            if resposta_usuario.upper() == self.resposta_correta:
                return "Parabéns! Você acertou."
            else:
                return f"Ops! Resposta incorreta. A resposta correta é: {self.resposta_correta}"
        else:
            return "Ops! Opção inválida. Por favor, escolha entre A, B, C ou D."

# Função para interagir com o usuário e obter respostas
def responder_perguntas(request, pergunta_atual):
    if request.method == 'POST':
        resposta_usuario = request.POST.get('resposta_usuario')
        resultado = pergunta_atual.verificar_resposta(resposta_usuario)

        # Redirecione para a próxima pergunta (se houver)
        return redirect('pergunta')  # Assumindo que você tem um caminho chamado 'pergunta' definido nas URLs
    else:
        return render(request, 'pergunta.html', {'pergunta': pergunta_atual})

# Lista de perguntas inicialmente vazia
todas_perguntas = []

# Adicionar perguntas até o usuário decidir parar (você pode ajustar isso conforme necessário)
while True:
    try:
        nova_pergunta = adicionar_pergunta()
        todas_perguntas.append(nova_pergunta)

        continuar = input("Deseja adicionar mais perguntas? (Digite 's' para sim ou 'n' para não): ")
        if continuar.lower() != 's':
            break
    except UnicodeDecodeError:
        print("Erro de codificação. Certifique-se de estar usando um ambiente que suporte caracteres Unicode.")

# URLs
urlpatterns = [
    path('pergunta/', lambda request: responder_perguntas(request, todas_perguntas.pop(0)), name='pergunta'),
]

settings.ROOT_URLCONF = __name__

# Execução do servidor de desenvolvimento
if __name__ == "__main__":
    from django.core.management import execute_from_command_line
    execute_from_command_line(['main.py', 'runserver', '0.0.0.0:3000'])
