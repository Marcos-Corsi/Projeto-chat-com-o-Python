import flet as ft

def main(pagina):
    texto = ft.Text("Chat")

    nome_usuario = ft.TextField(label="Escreva seu nome")

    chat = ft.Column()

    def enviar_mensagem_tunel(informacoes):
        chat.controls.append(ft.Text(informacoes))
        pagina.update()

    pagina.pubsub.subscribe(enviar_mensagem_tunel)


    def enviar_mensagem(evento):
        texto_campo_mensagem = campo_mensagem.value
        # Colocar o nome do usuário na mensagem

        pagina.pubsub.send_all(texto_campo_mensagem)
        # Limpar o campo da mensagem
        campo_mensagem.value = ""
        pagina.update()

    botão_enviar = ft.ElevatedButton("Enviar", on_click=enviar_mensagem)
    campo_mensagem = ft.TextField(label="Escreva sua mensagem aqui", on_submit=enviar_mensagem)

    def entrar_chat(evento):
        # Fechar o popup
        popup.open = False
        # Remover o botão "Iniciar Chat" da tela
        pagina.remove(botao_iniciar)
        # Adicionar o chat à página
        pagina.add(chat)
        # Criar a linha de mensagem
        linha_mensagem = ft.Row([campo_mensagem, botão_enviar])
        # Adicionar a linha de mensagem à página
        pagina.add(linha_mensagem)
        # Imprimir o nome do usuário no console
        text = f"{nome_usuario.value} entrou no chat"
        pagina.pubsub.send_all(text)
        pagina.update()

    popup = ft.AlertDialog(
        open=False,
        modal=True,
        title=ft.Text("Bem-vindo ao Chat"),
        content=nome_usuario,
        actions=[ft.ElevatedButton("Entrar", on_click=entrar_chat)]
    )

    def iniciar_chat(evento):
        # Definir a janela de diálogo como o popup
        pagina.dialog = popup
        # Abrir o popup
        popup.open = True
        # Atualizar a página
        pagina.update()

    botao_iniciar = ft.ElevatedButton("Iniciar Chat", on_click=iniciar_chat)

    pagina.add(texto)
    pagina.add(botao_iniciar)

# Iniciar a aplicação
ft.app(main)
