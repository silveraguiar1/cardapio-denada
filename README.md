# Cardápio Digital — Restaurante Denada

Cardápio digital em página única (HTML), pensado para celular e para ser aberto por **QR Code**.
Restaurante da Pousada Denada · Av. Vasco Neto, 10 · Barra Grande, Maraú – BA · Site: https://denadapousada.com.br/

**Versão:** protótipo funcional, 17/10/2026
**Escopo:** Nível A (cardápio informativo) com atalhos de contato. Não recebe pedidos, não tem carrinho nem painel administrativo.

---

## 1. O que há nesta pasta

```
cardapio-denada/
├── index.html                      ← o cardápio inteiro (um único arquivo)
├── README.md                       ← este guia
└── assets/
    ├── logo-denada-branco.png      ← logo branco, fundo transparente
    ├── logo-denada-marrom.png      ← logo marrom (#7a5e40), fundo transparente
    └── DENADA_POUSADA_LOGO.pdf     ← arquivo original do logo
```

O `index.html` é **autossuficiente**: o logo, o estilo e o código já estão dentro dele. Não usa bibliotecas, fontes nem imagens externas. A pasta `assets/` serve só para guardar e reutilizar o logo (impressos, redes sociais). O cardápio não depende dela.

## 2. O que está implementado

- Layout para celular, com modo claro e escuro (automático pelo sistema, ou manual pelo botão no topo, com escolha salva no aparelho).
- 13 categorias e 92 itens: Antipasti, Insalate, La Pasta Fresca, La Pasta Seca, Hamburguer, La Piadina Romagnola, Acompanhamentos, Prato Kids, Primi Piatti, Sobremesas, Bebidas, Drinks e Vinhos.
- Seletor de idioma **PT | EN**. Na primeira visita, o idioma vem do celular do cliente; a escolha fica salva no aparelho.
- Busca nos dois idiomas, com contador de resultados.
- Barra de categorias fixa, com destaque automático da categoria visível.
- Rodapé com telefone, endereço, "Como chegar" (rota no mapa) e link para o site da pousada.
- Botão "voltar ao topo".
- Acessibilidade básica: rótulos, foco visível, respeito a "reduzir movimento".
- Otimizado para mobile: alvos de toque de 44px, `touch-action: manipulation` (sem atraso de duplo toque), áreas seguras de notch, `content-visibility` (seções fora da tela não são renderizadas, economizando bateria em celulares modestos) e favicon embutido.
- Modo "adicionar à tela inicial": ao favoritar a página no celular, ela abre sem a barra do navegador (metas `mobile-web-app-capable`).
- Compartilhamento: tags Open Graph e `description` para o preview do link no WhatsApp/redes sociais.

## 3. O que NÃO está implementado

- Pedidos, carrinho, pagamento ou painel administrativo.
- Fotos dos pratos.
- Informações de alergênicos, ingredientes além do impresso, horário de funcionamento e e-mail. **Nada disso foi inventado**; só entra se o restaurante confirmar.
- QR Code (depende do endereço público definitivo; veja a seção 6).
- Métricas de acesso (analytics).

## 4. Como editar o conteúdo

Abra o `index.html` em um editor de texto (Bloco de Notas, VS Code etc.). Todos os dados ficam no início do bloco `<script>`.

### 4.1 Preços e itens

Procure por `const C=[`. Cada categoria tem este formato:

```js
["Nome da categoria", "Inglês", "Nota opcional", [
   ["Nome do prato", "Inglês", "Descrição em português", 48],
   ...
]]
```

Cada **item** tem 4 posições, nesta ordem: `nome`, `inglês impresso`, `descrição em PT`, `preço`.

- Para **mudar um preço**, altere só o número (sem "R$"). Ex.: `["Batata Frita","French Fries","",48]` passa a `...,52]`.
- Para **remover** um item, apague a linha inteira, cuidando das vírgulas.
- Para **adicionar**, copie uma linha existente da mesma categoria e altere os campos.
- Preço `null` (sem aspas) mostra o item sem valor (usado em "Opções de queijo").

### 4.2 Traduções (inglês)

- `const ED={...}`: tradução dos pratos. A chave é o **nome em português, exatamente como está em `C`**; o valor é `["texto em inglês", "complemento em inglês"]`. Se você mudar o nome de um prato em `C`, mude a chave aqui também, ou a tradução some.
- `const CE={...}`: títulos e notas das categorias em inglês.
- `const UI={...}`: textos da interface (botões, rodapé, placeholder da busca) em PT e EN.

### 4.3 Contato

Procure por `5573998430336` (link de ligação no rodapé) e por `(73) 99843-0336` (texto do rodapé). O formato do número no link é código do país + DDD + número, sem espaços: **55 73 99843-0336**. O número foi conferido com o site da pousada.

### 4.4 Endereço e links

No `<footer>`: endereço, link "Como chegar" (coordenadas do mapa) e link do site.

### 4.5 Cores

No início do `<style>`, as variáveis em `:root` controlam as cores (`--g` verde, `--br` marrom da marca, `--bg` fundo, `--card`, `--ink` texto). O bloco `prefers-color-scheme:dark` define o modo escuro automático; o botão `.tm` no topo força claro ou escuro (atributo `data-theme` no `<html>`) e a escolha fica salva no aparelho.

### 4.6 Logo

O logo está embutido no `index.html` (campo `--m:url(data:image/png;base64,...)` no elemento `<h1 class="logo">`). Para trocar, gere o novo PNG branco com fundo transparente, converta para base64 e substitua esse valor. A cor exibida vem de `--br`, não do arquivo.

## 5. Como testar no computador

1. Dê dois cliques no `index.html`, e ele abre no navegador.
2. Confira: troca PT/EN, busca (ex.: "camarão", "shrimp", "tinto"), toque nas categorias, rodapé.
3. No celular, envie o arquivo (ou, melhor, teste depois de publicar) e confira rolagem, teclado da busca e modo escuro.

## 6. Como publicar e criar o QR Code

O cardápio é um arquivo estático, então qualquer hospedagem de sites estáticos serve. Exemplos: Netlify, Cloudflare Pages, GitHub Pages, ou o próprio servidor do site da pousada. Confira nos serviços as condições atuais, pois mudam.

**Recomendação:** use um endereço próprio e estável, como `cardapio.denadapousada.com.br` (subdomínio do site da pousada, configurado com quem cuida do domínio). Assim, o QR Code impresso nunca precisa ser trocado, mesmo que a hospedagem mude.

Passo a passo:

1. Escolha a hospedagem e envie a pasta (ou apenas o `index.html`).
2. Configure o endereço definitivo (subdomínio recomendado).
3. Abra o endereço no celular, em rede móvel e em Wi‑Fi, e teste tudo.
4. Só então gere o QR Code apontando para esse endereço.

**Gerar o QR Code** (exemplo em Python, opcional):

```bash
pip install "qrcode[pil]"
python -c "import qrcode; qrcode.make('https://SEU-ENDERECO').save('qr-denada.png')"
```

Também funciona qualquer gerador de QR Code confiável que ofereça arquivo em alta resolução ou vetorial (SVG/PDF), sem prazo de validade nem redirecionamento pago.

**Boas práticas de impressão:**
- Tamanho mínimo sugerido: cerca de 3 × 3 cm para leitura de perto; maior se ficar longe do cliente.
- Mantenha margem livre ao redor e alto contraste (escuro sobre claro).
- **Teste a leitura com a câmera de pelo menos 2 celulares (iPhone e Android) no material impresso**, antes de produzir em quantidade.
- Para QR Codes por mesa ou campanha, gere um por URL (por exemplo `?mesa=5`). Atenção: a página atual **não usa** esse parâmetro. Ele só serviria para rastreio futuro.

## 7. Lista de verificação antes de publicar

- [ ] Preços conferidos com o cardápio impresso **atual** (os preços foram lidos de fotos do cardápio).
- [ ] Traduções em inglês revisadas pelo restaurante. As que não estavam no impresso foram feitas por nós: bebidas, frozens, hambúrgueres, Prato Kids, vinhos e nomes de categorias como "Burgers" e "Sides". Também "2 scoops" no Sorvete Caseiro (o impresso diz "2 balls").
- [x] Aviso de protótipo no topo: removido.
- [ ] Nomes dos vinhos conferidos (copiados como impressos, inclusive "Rosseto" e "Ca´del Borgo").
- [ ] Teste em iPhone e Android reais.
- [ ] Decisão sobre alergênicos, horário e fotos.

**Aviso de protótipo:** já foi removido (o elemento `<div class="demo" data-k="demo">`, as chaves `demo`/`price` do `UI` e a nota de preços do rodapé). A classe `.demo` continua no CSS apenas para a mensagem de `<noscript>`.

## 8. Limites conhecidos

- Os testes feitos foram em navegador simulado (carregamento, troca de idioma, busca, destaque de categoria). **Não houve teste em celular real nem medição de velocidade.**
- O logo usa uma máscara de imagem do CSS, compatível com navegadores atuais. Em navegadores muito antigos, pode não aparecer corretamente.
- O idioma escolhido é guardado no aparelho (localStorage). Se o navegador bloquear isso, a página apenas volta ao idioma do celular a cada visita.
- Os dados estão dentro do próprio arquivo. A cada mudança de preço é preciso editar e publicar o arquivo de novo. Se o restaurante quiser atualizar sozinho, sem editar código, o próximo passo é um painel administrativo (Nível C), que ainda não existe.

## 9. Próximos passos sugeridos

1. Validar preços e traduções com o restaurante.
2. Publicar no endereço definitivo e gerar o QR Code.
3. Incluir horário de funcionamento, se confirmado.
4. Fotografar os pratos principais (luz natural, fundo neutro, mesma proporção) e adicioná-las.
5. Acompanhar acessos e, depois, avaliar pedidos por WhatsApp (Nível B).
