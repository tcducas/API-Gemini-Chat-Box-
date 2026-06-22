# Chatbot com Gemini AI (Flask)

Este projeto é um chatbot simples que usa a API do Gemini (Google GenAI) para responder mensagens em português.

## Como funciona

- O frontend em `templates/index.html` mostra um chat flutuante no navegador.
- Quando o usuário envia uma mensagem, o JavaScript faz um POST para `/chat` com um JSON contendo `message`.
- O backend em `app.py` recebe a requisição, normaliza a mensagem e envia ao modelo Gemini.
- O modelo responde com texto, e o backend devolve essa resposta ao navegador.
- O chat exibe essa resposta para o usuário na interface.

## Arquivos principais

- `app.py`: servidor Flask que processa o chat e chama a API do Gemini.
- `templates/index.html`: interface do chat e lógica de envio/recebimento de mensagens.
- `requirements.txt`: dependências do Python.

## Fluxo de requisição

1. O usuário digita uma pergunta no campo de chat.
2. O JavaScript faz POST para `/chat`:
   - corpo: `{"message": "..."}`
3. `app.py` lê essa mensagem e converte para o formato esperado.
4. O backend chama `client.models.generate_content(...)` com o modelo `gemini-2.5-flash-lite`.
5. A resposta do Gemini é retornada como JSON, por exemplo:
   - `{"response": "..."}`
6. O frontend exibe a resposta no chat.

## Configuração do token

O projeto usa a variável de ambiente `GEMINI_API_KEY` para autenticar a chamada ao Gemini.

1. Crie um arquivo `.env` na raiz do projeto.
2. Adicione:

```env
GEMINI_API_KEY=seu_token_aqui
```

3. O `app.py` usa `python-dotenv` para carregar essa variável automaticamente.

## Rodando localmente

1. Instale as dependências:

```bash
pip install -r requirements.txt
```

2. Crie o `.env` com `GEMINI_API_KEY`.
3. Execute o servidor:

```bash
python app.py
```

4. Abra o navegador em:

```
http://localhost:5001
```

## Uso grátis do Gemini (token rotativo)

Uma forma que encontrei para usar a IA sem pagar é trocar o token sempre que ele expira:

- Vá ao Google Studio (da API do Gemini / Google GenAI).
- Gere um novo token / API key.
- Cole esse novo valor em `GEMINI_API_KEY` no arquivo `.env`.
- Reinicie o servidor Flask.

Se o token expirar ou ficar inválido, a integração da API falha e o chat retorna erro. Por isso, trocar o token manualmente é necessário quando o acesso gratuito acaba.

## Observações importantes

- O backend já possui um prompt de sistema (`SYSTEM_PROMPT`) que define o comportamento do assistente.
- A resposta é gerada com temperatura baixa (`0.1`) para manter respostas mais estáveis e menos criativas.
- Se a mensagem do usuário for muito longa, o backend pede uma dúvida mais objetiva.

## Personalização

- Você pode ajustar `SYSTEM_PROMPT` em `app.py` para mudar o perfil do assistente.
- Também pode alterar o modelo para outra versão disponível na API do Gemini.

## Erros comuns

- `GEMINI_API_KEY` inválido ou expirado: gere novo token no Google Studio.
- Erro de conexão: verifique se o Flask está rodando e se o navegador consegue acessar `http://localhost:5001`.

