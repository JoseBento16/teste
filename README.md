🗺️ MAPA 1: ROTAS E NAVEGAÇÃO (EXPO ROUTER)A Metáfora: Imagine o seu aplicativo como um Prédio Inteligente.1. File-Based Routing (O Prédio sem Recepcionista)Como funciona: No Expo Router, você não precisa de uma recepcionista (um arquivo central) para registrar onde fica cada sala. O prédio é inteligente: se você cria uma sala (arquivo) chamada home.tsx dentro da pasta app/, a porta /home aparece magicamente para o usuário.Exemplo Prático:Criou app/index.tsx? É o Saguão de Entrada (a rota /).Criou app/perfil.tsx? É a Sala do Perfil (a rota /perfil).2. Os Comandos do Elevador (push, navigate e replace)router.push('/sala') [O Elevador Panorâmico]: Ele te leva para uma nova sala, mas joga a sala antiga para baixo. Você consegue olhar para trás e clicar no botão "Voltar" para retornar exatamente onde estava.router.navigate('/sala') [O Elevador Inteligente]: Se você pedir para ir para a "Sala 2" e ela já estiver aberta em algum lugar do histórico, o elevador te leva de volta para aquela que já existe, em vez de construir uma sala nova e idêntica.router.replace('/sala') [O Alçapão de Sentido Único]: Ele te joga na próxima sala e tranca a porta antiga com cadeado. Você não pode voltar.🎯 Cai na Prova: Usamos o replace logo após o Login. Se o usuário logou com sucesso e foi para a Home, usamos replace para que, se ele apertar o botão de voltar do celular, ele não seja jogado de volta para a tela de login. Afinal, ele já está logado!3. Parâmetros na URL (A Mala de Viagem Despachada)O Conceito: Quando você viaja de uma tela para outra levando um dado (ex: /detalhes?id=42), esse dado vai "despachado" no teto do elevador (na URL).O Problema: Tudo o que viaja na URL é envelopado como Texto (string). Mesmo que você envie o número 42, ele chega na outra tela como o texto "42".O que você deve fazer: Antes de usar, você precisa transformar o texto em número de novo.TypeScript// Na tela que recebe:
const idEmNumero = Number(params.id); // Converte "42" para 42

---

## 🎨 MAPA 2: COMPONENTES E RESPONSIVIDADE
> **A Metáfora:** Imagine que você está montando a interface usando blocos de **LEGO**.

### 1. A Batalha dos Botões (`Button` vs. `TouchableOpacity` vs. `Pressable`)
* **`Button` [O Bloco de Gesso]:** Ele já vem pronto de fábrica com a cor do sistema (Android ou iOS). O grande problema? Ele é rígido. **Não aceita a propriedade `style`**. Se você quiser um botão redondo, com borda neon e degradê, o `Button` não serve.
* **`TouchableOpacity` [O Bloco de Borracha]:** Esse aceita qualquer estilo (`style`). Além disso, quando o usuário clica, ele dá uma leve "piscada" (reduz a opacidade) para mostrar que entendeu o clique.
* **`Pressable` [O Bloco Vivo]:** É o mais moderno. Ele consegue mudar de estilo *enquanto* o usuário está com o dedo em cima dele.
  * **Exemplo:** Você pode programar para ele ficar azul quando estiver parado, mas se o dedo encostar (`pressed`), ele fica verde instantaneamente.

### 2. Componentes Controlados (O Cachorrinho Adestrado)
* **Como funciona:** Um campo de texto (`TextInput`) controlado é igual a um cachorrinho adestrado: ele só faz o que o React manda. O texto exibido na tela vem estritamente de uma variável de estado (`value={nome}`). Quando o usuário digita uma letra, o componente não guarda essa letra sozinho; ele avisa o React (`onChangeText`), o React atualiza a variável, e a variável atualiza a tela. O React é o dono absoluto.

### 3. Fita Métrica Estática vs. Elástica (`Dimensions` vs. `useWindowDimensions`)
* **`Dimensions.get('window')` [A Fita Métrica de Papel]:** Você mede a tela uma vez quando o aplicativo abre. Se o usuário deitar o celular de lado (girar a tela), a fita de papel rasga ou fica com a medida antiga. O app não se ajusta sozinho.
* **`useWindowDimensions()` [A Fita Métrica Elástica]:** É um sensor vivo. Se o usuário girar o celular, ela estica na hora, descobre o novo tamanho e avisa o componente para redesenhar tudo bem bonito.

---

## 📋 MAPA 3: LISTAS E MEMÓRIA RAM (`FLATLIST`)
> **A Metáfora:** Imagine uma timeline com **10.000 fotos de gatinhos**.

### 1. `ScrollView` vs. `FlatList` (O Garçom Desastrado vs. O Garçom Eficiente)
* **`ScrollView`:** Tenta carregar as 10.000 fotos de uma vez só na memória do celular, mesmo que o usuário só consiga ver duas por vez na tela. Resultado? O celular esquenta, trava e o aplicativo fecha.
* **`FlatList` [Virtualização]:** Ela é inteligente. Se na tela só cabem 4 fotos, ela renderiza apenas essas 4. Conforme o usuário vai arrastando para baixo, ela destrói as fotos que sumiram lá em cima e cria as novas que estão aparecendo embaixo. A memória RAM agradece!

### 2. `keyExtractor` (A Etiqueta do Guarda-Roupas)
* **O Conceito:** O React precisa saber exatamente quem é quem na lista para não se confundir na hora de atualizar a tela.
* **🎯 Pegadinha de Prova:** **Nunca use o `index` (a posição 0, 1, 2...) como chave** se a sua lista puder ser filtrada ou ordenada.
  * **Por quê?** Imagine que o Gatinho A está na posição `0`. Se você ordenar a lista de trás para frente, o Gatinho Z vai para a posição `0`. O React vai achar que o Gatinho A virou o Gatinho Z e vai causar bugs visuais bizarros. Use sempre um ID único e fixo (ex: `item.id`).

---

## ⏱️ MAPA 4: O CICLO DE VIDA (`USEEFFECT`)
> **A Metáfora:** O `useEffect` é o **Guarda-Costas** do seu componente. Ele fica vigiando o que acontece e reage de acordo com as ordens.

As reações dele dependem do **Array de Dependências** (os colchetes no final):

```typescript
// Caso 1: Os Colchetes Vazios []
useEffect(() => {
  console.log("Fui montado na tela!");
}, []); // Roda SÓ UMA VEZ quando a tela abre. Ideal para buscar dados na internet.

// Caso 2: Os Colchetes Olheiros [categoria]
useEffect(() => {
  console.log("A categoria mudou! Vou buscar novos produtos.");
}, [categoria]); // Roda quando a tela abre E toda vez que a variável 'categoria' mudar.

// Caso 3: Sem colchetes (O Descontrolado)
useEffect(() => {
  console.log("Estou rodando a cada milissegundo!");
}); // Roda em QUALQUER mudança. Se você atualizar um estado aqui dentro, gera um LOOP INFINITO e trava o celular.
🔌 MAPA 5: CONSUMINDO APIS (RESTAURANTE HTTP)A Metáfora: O seu aplicativo é o Cliente, a API é o Garçom e o Banco de Dados é a Cozinha.1. O Cardápio de Pedidos (Métodos HTTP)GET: "Garçom, me traga o prato número 5." (Apenas lê dados, não altera nada na cozinha).POST: "Garçom, adicione esse novo prato ao cardápio." (Cria um dado novo).PUT: "Troque todo o meu prato por um totalmente novo." (Substituição completa).PATCH: "Só jogue um pouquinho mais de sal no meu prato." (Alteração parcial, muda só um pedaço).DELETE: "Leve esse prato embora, não quero mais." (Exclui o dado).2. O Idioma do Garçom: JSONPara o cliente e o garçom conversarem, eles usam uma língua chamada JSON. A regra gramatical mais importante do JSON é: Tudo o que for texto ou chave precisa de aspas duplas "".JSON{
  "nome": "José Bento",
  "idade": 20
}
3. A Disputa: fetch Nativo vs. AxiosSituaçãofetch (O Funcionário Padrão)Axios (O Super Mordomo)Quando a comida vem estragada (Erro 404 ou 500)Ele não reclama. Ele aceita o prato estragado e diz que deu tudo certo (response.ok = false). Você precisa checar manualmente.Ele grita na hora! Interrompe o código e joga o erro direto para o bloco catch().Interpretar o cardápio (Parse JSON)Você precisa pedir para ele traduzir manualmente digitando await response.json().Ele já traz tudo traduzido e mastigado dentro da caixinha response.data.🛡️ MAPA 6: SEGURANÇA COM TYPESCRIPT1. O Operador keyof (O Segurança do Camarote)O que faz: Imagine que você tem uma lista de convidados permitidos (as chaves de um objeto). O keyof cria uma barreira baseada nessa lista.Exemplo Prático: Se o seu formulário só tem os campos nome e email, e você tentar atualizar um campo chamado telefone, o TypeScript acende uma luz vermelha antes mesmo do app rodar, avisando: "Ei! 'telefone' não está na lista de convidados do formulário!".2. Expressão Regular / Regex (O Teste do DNA)O que é: Uma fórmula matemática mágica usada para checar se um texto segue um padrão estrito (como e-mails ou CPFs).O Teste do E-mail: A fórmula /^[^\s@]+@[^\s@]+\.[^\s@]+$/ exige:Letras antes do @.O símbolo @.Letras depois do @.Um ponto ..Mais letras depois do ponto (ex: com, br).🎯 Pegadinha de Prova: O texto "joao@email" é INVÁLIDO porque falta o ponto e a extensão final (como .com).
