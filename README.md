# Palavra — bateria de testes de engenharia de software

## Visão geral

Este protótipo é uma interface interativa de jogo de palavras em português, com seleção de conceito visual, modos de jogo, dicionário por tamanho de palavra e persistência local no navegador. A bateria abaixo foi organizada para validar qualidade funcional, experiência de uso e estabilidade do produto em diferentes cenários.

## Objetivo dos testes

Validar que:

- a lógica de seleção e validação de palavras está correta;
- a palavra sorteada respeita o tamanho esperado;
- o sistema aceita apenas palavras válidas do dicionário ativo;
- o fluxo de jogo funciona em diário e infinito;
- os conceitos visuais não quebram a experiência;
- a interface permanece acessível e responsiva;
- o armazenamento local não corrompe progresso ou estado do jogo.

## Escopo

### Funcionalidades cobertas

- seleção de conceito visual (Ritual, Caderno, Acesso);
- modo de jogo: Palavra do dia e Infinito;
- dicionários: Leve, Médio e Completa;
- validação por número de letras;
- tentativas e feedback de acerto/erro;
- revela da resposta ao final da rodada;
- persistência local em localStorage;
- teclado virtual e entrada por teclado físico;
- estado visual de tiles, status e respostas.

### Fora de escopo

- autenticação de usuário;
- backend ou API de palavras;
- sincronização entre dispositivos;
- banco de dados persistente;
- tradução para outros idiomas.

---

## Estratégia de teste

### 1. Teste funcional

Valida o comportamento esperado da regra de negócio e da interface.

### 2. Teste de regressão

Garante que alterações em dicionário, layout ou lógica não quebrem cenários já aprovados.

### 3. Teste de acessibilidade

Verifica foco, contraste, leitura por assistente de tela e interação por teclado.

### 4. Teste de qualidade de experiência

Avalia clareza visual, ritmo de interação e legibilidade.

### 5. Teste de robustez

Simula entradas inválidas, palavras duplicadas, falhas de armazenamento e estados inesperados.

---

## Matriz de testes

| ID | Categoria | Cenário | Resultado esperado |
| --- | --- | --- | --- |
| TC-01 | Funcional | A página carrega sem erro inicial | O jogo aparece e a palavra do dia é sorteada |
| TC-02 | Funcional | Usuário acessa o modo Infinito | Uma palavra é sorteada com tamanho válido em rodada independente |
| TC-03 | Funcional | Usuário escolhe dicionário Leve | Somente palavras do perfil Leve são aceitas |
| TC-04 | Funcional | Usuário escolhe dicionário Médio | Palavras do perfil Médio são aceitas |
| TC-05 | Funcional | Usuário escolhe dicionário Completa | O conjunto maior é usado para validação |
| TC-06 | Funcional | Entrada com número de letras errado | Sistema informa o tamanho correto da palavra |
| TC-07 | Funcional | Palavra aceita não está no dicionário ativo | Sistema bloqueia e exibe mensagem clara |
| TC-08 | Funcional | Palavra repetida | Sistema alerta que a palavra já foi usada |
| TC-09 | Funcional | Acerto na primeira tentativa | Jogo marca como concluído e revela a resposta |
| TC-10 | Funcional | Erro em todas as tentativas | Jogo encerra e mostra a palavra correta |
| TC-11 | Funcional | Troca de conceito visual | Interface altera estilo e textos sem quebrar estado do jogo |
| TC-12 | Funcional | Uso do teclado físico | Teclas digitadas entram no campo corretamente |
| TC-13 | Funcional | Uso do teclado virtual | Clicar e inserir letras funciona sem quebrar o layout |
| TC-14 | Persistência | Recarga da página no modo diário | Progresso recente permanece em localStorage |
| TC-15 | Persistência | Limpeza de localStorage | O jogo reinicia sem erro |
| TC-16 | Acessibilidade | Navegação por teclado no tablist | Foco funciona e muda painel de conceito corretamente |
| TC-17 | Acessibilidade | Foco visível em campos e botões | Input e botões exibem indicador de foco |
| TC-18 | Acessibilidade | Leitor de tela lê status | Mensagens de status são anunciadas em aria-live |
| TC-19 | Responsividade | Tela mobile | Layout ajusta sem esconder elementos principais |
| TC-20 | Responsividade | Resize da janela | Tabuleiro recalcula tamanho dos tiles |
| TC-21 | Robustez | Entrada com acentos e letras minúsculas | Sistema normaliza corretamente para comparação |
| TC-22 | Robustez | Palavra fora da lista com letras válidas | Bloqueio sem causar exceção |
| TC-23 | Robustez | estado.finished verdadeiro | Botões e input ficam desabilitados conforme regra |
| TC-24 | Regressão | Atualização do dicionário | Lógica de validação continua consistente após troca de perfil |

## Bateria 2 — vocabulário e sorteio por tamanho

Esta bateria complementa os testes gerais com foco na ampliação dos três perfis e no contrato entre o tamanho sorteado, a palavra selecionada e as tentativas aceitas. O intervalo válido do protótipo é de 3 a 9 letras, inclusive.

| ID | Categoria | Cenário | Resultado esperado |
| --- | --- | --- | --- |
| B2-01 | Dicionário | Abrir cada perfil | Leve, Médio e Completa exibem conjuntos ampliados e em ordem crescente de quantidade |
| B2-02 | Dicionário | Normalizar palavras com e sem acento | Grafias equivalentes não geram entradas duplicadas após normalização |
| B2-03 | Dicionário | Inspecionar todas as palavras de cada perfil | Cada palavra tem entre 3 e 9 letras |
| B2-04 | Sorteio | Consultar os comprimentos disponíveis do perfil ativo | A lista contém apenas comprimentos entre 3 e 9 com palavras cadastradas |
| B2-05 | Sorteio | Sortear uma palavra para cada comprimento disponível | A palavra sorteada tem exatamente o comprimento solicitado |
| B2-06 | Sorteio | Iniciar uma rodada no modo Infinito | O tamanho sorteado existe no perfil ativo e a palavra pertence ao mesmo grupo |
| B2-07 | Sorteio | Iniciar o desafio diário | O tamanho definido para o dia existe no perfil ativo e a palavra respeita esse tamanho |
| B2-08 | Robustez | Solicitar palavra com tamanho fora do intervalo ou sem opções | A seleção é rejeitada explicitamente; não usa uma palavra de fallback com outro tamanho |
| B2-09 | Perfil | Trocar de dicionário durante uma partida | A nova rodada usa exclusivamente palavras e comprimentos do perfil selecionado |
| B2-10 | Próxima rodada | Sortear novamente havendo outras palavras disponíveis | A nova palavra pertence ao dicionário ativo e não repete a anterior imediatamente |
| B2-11 | Próxima rodada | Testar perfil com somente uma palavra disponível | A rodada continua sem loop infinito e mantém um tamanho válido |
| B2-12 | Metadados | Selecionar palavra com grafia acentuada normalizada | Definição e exemplo pertencem à palavra exibida |
| B2-13 | Interface | Carregar palavras de 3 e de 9 letras | Campo, tabuleiro e quantidade de tentativas acompanham o tamanho da palavra |
| B2-14 | Entrada | Digitar além do tamanho sorteado pelo teclado físico ou virtual | O jogo impede letras excedentes e só aceita palpite com o tamanho exato |

### Resultado da verificação desta alteração

- JavaScript do protótipo analisado sem erro de sintaxe.
- Perfis carregados: Leve com 149 palavras, Médio com 293 e Completa com 539.
- Os três perfis passaram pela checagem de unicidade e pelo limite de 3 a 9 letras.
- A seleção foi exercitada em todos os comprimentos disponíveis, confirmando correspondência exata entre tamanho e palavra.
- A solicitação de 10 letras foi rejeitada e os metadados de palavra acentuada foram conferidos após normalização.
- Os cenários B2-09 a B2-14 permanecem como verificações manuais de interface; não há automação de navegador configurada neste protótipo.

---

## Casos de teste detalhados

### 1. Carregamento inicial

- Verificar que o protótipo abre sem console de erro.
- Confirmar que a primeira palavra do dia é escolhida e o tabuleiro inicia vazio.
- Validar que a área de status mostra instrução clara.

### 2. Validação por número de letras

- Inserir palavra com tamanho menor que a sorteada.
- Inserir palavra com tamanho maior que a sorteada.
- Inserir palavra correta no tamanho, mas fora do dicionário ativo.
- Confirmar mensagens de erro precisas e sem quebra da tela.

### 3. Fluxo de acerto

- Digitar a palavra correta.
- Confirmar mudança de estado para finalizado.
- Validar que a resposta correta é exibida no card final.
- Verificar que o botão de próxima rodada aparece quando aplicável.

### 4. Fluxo de derrota

- Usar todas as tentativas sem acerto.
- Verificar que a palavra revelada aparece.
- Confirmar que o estado fica travado para novas entradas.

### 5. Persistência local

- Iniciar uma partida.
- Fazer um palpite.
- Recarregar a página.
- Validar que o jogo retoma o progresso correto.
- Testar comportamento quando localStorage está vazio ou indisponível.

### 6. Acessibilidade

- Navegar por teclado com Tab e Shift+Tab.
- Mudar de conceito visual usando teclado.
- Verificar foco visível em todos os elementos interativos.
- Conferir contraste entre texto, fundo e indicadores de estado.

### 7. Responsividade

- Verificar layout em telas pequenas e médias.
- Confirmar que o tabuleiro e o teclado virtual se ajustam sem overflow crítico.
- Validar que o botão de envio e o input continuam acessíveis.

---

## Critérios de aceite

A entrega será considerada adequada quando:

- todas as regras de validação por letras e dicionário forem satisfeitas;
- palavras repetidas e inválidas forem rejeitadas com feedback claro;
- o jogo se comporta corretamente em diário e infinito;
- o progresso local funciona de forma consistente;
- o design mantém legibilidade e foco em telas mobile e desktop;
- a interface atende, no mínimo, aos padrões básicos de acessibilidade web.

---

## Riscos e pontos de atenção

- uso de palavras com acento e sem acento pode produzir duplicidade na lógica;
- listas de dicionário muito grandes podem afetar performance em baixa potência;
- progressos salvados em localStorage podem divergir se houver mudança de dicionário ou palavra sorteada;
- alguns nomes de palavras e variações ortográficas podem exigir revisão editorial antes de produção.

---

## Recomendação de automação

Para evoluir o protótipo, recomenda-se automatizar os testes com:

- Playwright para cenários de UI e acessibilidade;
- testes unitários para funções de normalização, seleção e validação;
- testes de regressão do fluxo de jogo e persistência local.

Exemplos de automação sugerida:

- abrir página;
- validar mensagem inicial;
- inserir palavra inválida;
- inserir palavra correta;
- recarregar e verificar estado;
- alternar modos e dicionários;
- verificar foco e navegação por teclado.

---

## Checklist final de QA

- [ ] Página abre sem erros
- [ ] Dicionário selecionado influencia o conjunto válido
- [ ] Palavra sorteada respeita tamanho da rodada
- [ ] Palavra repetida é rejeitada
- [ ] Palavra inválida é rejeitada com feedback
- [ ] Acerto encerra a rodada corretamente
- [ ] Derrota revela a resposta correta
- [ ] localStorage salva e carrega progresso
- [ ] Acessibilidade básica mantém foco e leitura
- [ ] Layout funciona em mobile e desktop
- [ ] Sem regressões em conceitos visuais

---

## Observação final

Este README serve como base de teste de qualidade para o protótipo e deve ser atualizado junto com o produto sempre que houver nova regra de negócio, novo dicionário, mudanças de layout ou novas exigências de acessibilidade.
