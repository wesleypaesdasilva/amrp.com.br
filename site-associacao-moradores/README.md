# Site da Associação de Moradores

Site institucional em HTML/CSS/JS puro, pronto para publicar no **GitLab Pages**,
com página de apresentação da associação e página de cadastro de documentos dos
moradores (formulário incorporado do Google Forms).

## Estrutura do projeto

```
.
├── .gitlab-ci.yml          # Publica a pasta public/ no GitLab Pages
├── README.md
└── public/                 # Todo o conteúdo do site (é o que fica no ar)
    ├── index.html           # Página inicial (apresentação / história)
    ├── cadastro.html        # Página de cadastro de documentos
    └── assets/
        ├── css/style.css
        ├── js/main.js
        └── img/             # Logos e favicons
```

## 1. Publicar no GitLab Pages

1. Crie um repositório no GitLab (ou use um já existente).
2. Envie todos os arquivos deste projeto para a raiz do repositório:
   ```bash
   git init
   git remote add origin <URL_DO_SEU_REPOSITORIO_GITLAB>
   git add .
   git commit -m "Site inicial da associação"
   git push -u origin main
   ```
3. No GitLab, vá em **Build > Pipelines** e confirme que o pipeline rodou com
   sucesso (o job `pages` é acionado automaticamente pelo `.gitlab-ci.yml`).
4. Depois que o pipeline terminar, acesse **Deploy > Pages** no menu lateral do
   projeto para ver o link público do site (algo como
   `https://SEU_USUARIO.gitlab.io/SEU_PROJETO/`).

> Se o seu projeto usa a branch `master` em vez de `main`, não precisa mudar
> nada — o `.gitlab-ci.yml` já usa `$CI_DEFAULT_BRANCH`, que se ajusta
> automaticamente à branch padrão do repositório.

## 2. Configurar o formulário de cadastro (Google Forms)

O formulário de documentos é um **Google Forms incorporado** — os dados
enviados pelos moradores caem direto numa planilha do Google, sem precisar de
servidor.

1. Acesse [forms.google.com](https://forms.google.com) e crie um novo
   formulário, por exemplo com os campos:
   - Nome completo
   - CPF
   - Endereço / número do imóvel
   - Telefone de contato
   - Upload do RG (arquivo)
   - Upload do CPF (arquivo)
   - Upload do comprovante de residência (arquivo)

   > Para permitir **upload de arquivos**, ative a opção correspondente ao
   > adicionar uma pergunta do tipo "Upload de arquivo". Isso exige que quem
   > responde esteja logado numa conta Google — é a forma mais simples de
   > receber documentos sem precisar de backend próprio.

2. Clique no botão **Enviar** (canto superior direito do formulário).
3. Na janela que abrir, clique no ícone `< >` (Incorporar).
4. Copie o link que aparece dentro de `src="..."`.
5. Abra o arquivo `public/cadastro.html` e localize o trecho:
   ```html
   <iframe
     src="https://docs.google.com/forms/d/e/SUBSTITUA_PELO_LINK_DO_SEU_FORMULARIO/viewform?embedded=true"
     ...
   ```
6. Substitua a URL pelo link que você copiou.
7. Apague o aviso amarelo `.aviso-configuracao` logo acima do formulário no
   mesmo arquivo (ele é só um lembrete para quem está configurando o site).
8. Para consultar os cadastros recebidos, abra o formulário no Google Forms e
   vá na aba **Respostas > Planilha** — isso cria automaticamente uma
   planilha do Google Sheets com todas as respostas.

### Sobre privacidade e LGPD

Como o formulário coleta documentos pessoais (RG, CPF, comprovante de
residência), recomenda-se:
- Restringir o acesso à planilha de respostas somente à diretoria.
- Definir por quanto tempo os documentos ficam armazenados e quem tem acesso.
- Manter o aviso de privacidade já incluído na página `cadastro.html`.

## 3. Personalizar conteúdo

- **Textos**: edite diretamente `index.html` e `cadastro.html`.
- **Cores**: estão centralizadas em variáveis CSS no topo de
  `assets/css/style.css` (bloco `:root`).
- **Logos**: já estão otimizadas em `assets/img/` nas versões clara, escura e
  transparente, usadas conforme o fundo de cada seção.
- **E-mail e endereço**: procure por `contato@associacaomoradores.org.br` e
  `[endereço da associação]` nos dois arquivos HTML e substitua pelos dados
  reais.

## 4. Testar localmente

Não é obrigatório, mas para conferir antes de publicar:

```bash
cd public
python3 -m http.server 8000
```

Depois acesse `http://localhost:8000` no navegador.
