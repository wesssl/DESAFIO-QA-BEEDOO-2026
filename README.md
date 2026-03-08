# DESAFIO-QA-BEEDOO-2026
Desafio para a vaga de Analista de Qualidade da Beedo.

## INFORMAÇÕES DO CANDIDATO

* Nome: Wesley Lima

## Resumo

Para este desafio utilizei as seguintes abordagens:

* Testes exploratórios.
* Análise estrutural da interface.
* Validação de fluxos críticos.
* Identificação e Matriz de riscos.
* Testes Manuais.

Escopo:
* Testes funcionais baseados em convenções de mercado e suposição de requisitos.
* Testes de fumaça superficiais.
* Testes de UI/UX.
* Validação de vulnerabilidades básicas.
* Verificação de persistência via Local Storage.

Não-Escopo:
* Testes de carga e performance.
* Testes de Segurança avançados.
* Automação de testes.
* Testes de API.

Resultado:
* **26 Casos de Teste** mapeados e documentados.
* **xx Bugs Críticos** reportados.


## 1. Análise da aplicação

### 1.1 Análise inicial da aplicação e exploração da interface
---
Para me habituar e entender a aplicação, decidi fazer uma análise inicial e seguir com alguns testes exploratórios. Vou dividir a aplicação por telas, a partir da URL inicial (Home - https://creative-sherbet-a51eac.netlify.app/), analisar o conteúdo delas e explorar suas features.

Timebox: 1 hora

**Home**

Na URL base pode-se ver um cabeçalho, com o título da aplicação 'Beedoo QA Challenge' e dois links de navegação.

No corpo, há o título "Lista de Cursos".

**Listar Cursos**

Ao clicar em 'Listar Cursos' no cabeçalho da URL base, o usuário é redirecionado para a própria Home (URL inicial).

**Cadastrar Cursos**

Ao clicar em "Cadastrar Cursos" no cabeçalho da URL base, o usuário é redirecionado para a próxima tela (endpoint: /new-course), onde encontra-se um formulário de cadastro com os seguintes campos e tipos (identificados através da inspeção do DOM, nas ferramentas de desenvolvedor do navegador):

| Campo | Tipo | Identificador |
|:--:|:--:|:--:|
| Nome do Curso | Text | id="f_e689befe-fe67-4a02-b0b1-b5207625841d" |
| Descrição do curso | TextArea | id="f_4474ae18-b5e1-45c9-8c9b-3c4225923677" |
| Instrutor| Text | id="f_87e69d78-5a77-4424-84b5-d4835afbd0c9" |
| Url da imagem de capa | Text | id="f_abfb3a99-0f26-4d4c-bd30-621a9ae1216a" |
| Data de início | Date | f_f0cd4fe7-d59c-4745-8659-6fecfcc26796 |
| Data de fim| Date | id="f_08834a77-1329-4812-ad64-750cd3b94aa5" |
| Número de vagas | Number | id="f_9360c14b-b05f-4c7b-94ef-4a711f488851" |
| Tipo de curso | DropDown ('Presencial', 'Online') |


### 1.2 Teste Exploratório
---

Após a análise dessas páginas, decidi fazer um teste exploratório simples do formulário, onde defini e inseri valores ideais e válidos para cada um desses campos para entender como a aplicação se comporta. 

Neste primeiro momento, considero que achar um resultado válido (mesmo que falso positivo) é mais importante do que encontrar e detalhar um bug, pois minha intenção neste momento é apenas entender a aplicação.

Para esse teste, defini uma timebox de 10 minutos.

**Teste exploratório do formulário:**

| Campo | Valor Inserido |
|:--:|:--:|
| Nome do Curso | Teste01 |
| Descrição do curso | Teste de descrição. |
| Instrutor| Tester01 |
| Url da imagem de capa | https://upload.wikimedia.org/wikipedia/commons/thumb/8/82/Dahlia_03580.JPG/1280px-Dahlia_03580.JPG |
| Data de início | 12/04/2026 |
| Data de fim| 12/08/2026 |
| Número de vagas | 40 |
| Tipo de curso | Online |

A partir desse ponto, ao escolher o tipo de curso do dropdown, aparece um novo campo a ser preenchido chamado: 'Link de Inscrição' no caso de `Online`, 'Endereço' no caso de `Presencial`, os dois do tipo Text.

| Campo | Valor Inserido |
|:--:|:--:|
| Link de Inscrição | https://www.google.com/ |

**Resultado:**
* PopIn verde com a mensagem 'Curso cadastrado com sucesso!' na tela.
* Redirecionado para a Home.
* Item de teste adicionado a Lista de Cursos no corpo da página como um card.
* Item tem botão 'Excluir Curso'.

Para arrematar esse teste exploratório, há a necessidade de testar o item criado, se ele pode ser acessado com suas informações integrais em uma nova página e se o 'Excluir' funciona como esperado.

**Resultado:**
* PopIn verde com a mensagem 'Curso excluído com sucesso!'
* Sem Redirecionamento.
* Item não é excluído. Mesmo com atualização da página.
* Item não é interagível
* Item é incapaz de ser modificado

Com todas as features descobertas nessa visão inicial, pude tirar conclusões sobre as funcionalidades atuais da aplicação e entender o que pode ser problemático para a sua funcionalidade e a sua integridade. 

`Conclusões:`
* A aplicação opera como um gerenciador de catálogo simples (CRUD) para cadastro de cursos.
* Formulário com 09 campos disponíveis. Sendo 01 com valores estáticos ('Tipo de Curso').
* Os elementos do formulário possuem IDs que parecem ser gerados dinamicamente, o que representa um desafio para a estabilidade e consistência de scripts de automação. Apesar da possibilidade de usar o "aria-label" como seletor, o ID seria a forma mais otimizada e confiável se bem construído.
* O campo "Url da imagem de capa" e "Link de inscrição" aceitam qualquer texto. Isso talvez permita que um usuário insira links para sites de phishing ou execute o carregamento de recursos externos não autorizados ou mesmo mídias que causem problemáticas.
* Outros campos talvez permitam injeção de Script, também por aceitarem qualquer texto. 
* Na exclusão, existe uma desconexão entre a camada de apresentação e a persistência de dados, pois o feedback visual de sucesso não reflete a alteração real nos dados.
* Visualização dos cursos disponíveis através da URL base.
* Não há feature de busca de cursos diponíveis.

Há também algumas perguntas sobre o produto que precisariam ser respondidas como:
* Um curso pode ter data de início no passado?
* Existe limite de vagas?
* Cursos podem ser duplicados?
* Cursos podem ser editados após cadastro?
* A exclusão de cursos deveria exigir confirmação do usuário?

Devo acrescentar que uma análise dos requisitos, aliada aos testes exploratórios, seria o ideal para um melhor entendimento e conclusões mais assertivas das features. Porém, não há material informativo dos requisitos, então grande parte desse projeto de teste usará de suposições e convenções para o desenvolvimento dessa análise e dos testes.

Posso agora iniciar uma análise de fluxos e riscos, partindo das features já desenvolvidas.

## 2. Análise de Fluxos, Riscos e Priorização

### 2.1 Fluxos principais

O único fluxo disponível da aplicação é o de criação, visualização e exclusão de cursos. Para esse fluxo, podemos elencar os seguintes passos possíveis de um potencial usuário:

* **Criar Novo Curso**
    1. Ao acessar a URL Base, clicar no botão 'Cadastrar Curso'
    2. Preencher o formulário.
    3. Escolher entre 'Online' ou 'Presencial' no campo 'Tipo de Curso' para liberar o campo específico.
    4. Clicar em 'CADASTRAR CURSO'

* **Visualizar Cursos**
    1. Ao criar um novo curso, redirecionar para a URL base.
    2. Visualizar a lista de cursos cadastrados.
    3. Expandir card da lista de cursos para ver mais informações.

* **Exclusão de Cursos**
    1. A partir da URL Base, clicar no botão 'EXCLUIR CURSO'
    2. Receber feedback de confirmação de exclusão.

Então como provável fluxo temos: `Criar Novo Curso -> Visualizar Cursos -> Excluir Curso`.

### 2.2 Matriz de Riscos

Com a o fluxo definido, posso trabalhar em uma matriz de risco e chegar a uma priorização a partir da severidade das prováveis falhas.

#### Legenda da Escala:
* **Impacto:** O nível de dano que o risco causa à funcionalidade ou ao negócio (1 a 5).
* **Probabilidade:** A chance de o risco se concretizar (1 a 5). No caso da exclusão e automação, a probabilidade é 5 pois já foram confirmados na análise exploratória.
* **Severidade:** A prioridade de atenção/correção sugerida pelo QA (Impacto * Probabilidade).

#### Matriz

| Funcionalidade | Cenário de Risco | Impacto | Probabilidade | Severidade |
|:---:|:---:|:---:|:---:|:---:|
| **Cadastro** | Inconsistência temporal (Data de Fim < Data de Início). | 4 | 4 | 16 |
| **Cadastro** | Permitir o envio do formulário com campos essenciais vazios. | 5 | 3 | 15 |
| **Cadastro** | Vulnerabilidade a XSS e Phishing via campos de texto e URL. | 5 | 3 | 15 |
| **Cadastro** | Campo URL da Imagem aceitar links que não sejam de mídia ou URLs quebradas. | 3 | 4 | 12 |
| **Cadastro** | Campo Número de Vagas aceitar valores negativos, decimais ou zero. | 3 | 3 | 9 |
| **Cadastro** | Permitir o cadastro de cursos com Data de Início em data retroativa. | 3 | 3 | 9 |
| **Cadastro** | Falha de persistência ou erro de redirecionamento no salvamento. | 4 | 2 | 8 |
| **Cadastro** | Falta de máscara/validação para valores numéricos negativos. | 2 | 3 | 6 |
| **Visualizar** | Incapacidade de visualizar a descrição completa ou editar informações. | 3 | 4 | 12 |
| **Visualizar** | Descrições ou nomes excessivamente longos que quebram o layout (UI). | 2 | 4 | 8 |
| **Visualizar** | Dificuldade de localizar cursos devido à ausência de filtros e busca. | 2 | 4 | 8 |
| **Excluir** | **Confirmado:** Falha de persistência: confirma exclusão, mantendo o dado na lista. | 5 | 4 | 20 |
| **Geral / Técnico** | IDs dinâmicos que impedem a estabilidade de scripts de teste. | 2 | 5 | 10 |

### 2.3 Priorização de Teste

Com base no Fluxo e Matriz de Riscos, a estratégia de execução dos testes será dividida em três níveis de prioridade (P1, P2 e P3).

#### P1: Crítico
Foco em funcionalidades que impedem o uso básico, ou comprometem a integridade dos dados, ou provocam vulnerabilidades.
* **Integridade do Cadastro:** Testar a obrigatoriedade de campos e a persistência real dos dados.
* **Vulnerabilidades Críticas:** Validar se o sistema permite injeção de scripts (XSS) que possam comprometer a segurança da aplicação.
* **Exclusão** Validar a correção do fluxo de delegação, garantindo que o dado seja removido da base e da interface.

#### P2: Alta
Foco em garantir que a aplicação se comporte conforme o esperado em cenários comuns.
* **Consistência de Datas:** Validar as travas de "Data de Fim < Data de Início".
* **Sanitização de URLs:** Testar o comportamento do sistema com links de imagem e inscrição inválidos.
* **UX de Visualização:** Validar como o sistema lida com textos longos que podem quebrar o layout da Home.

#### P3: Média/Baixa
Foco em refinamentos e facilitação da manutenção futura.
* **Validação Numérica:** Testar limites de campos como "Número de Vagas" (negativos/decimais).
* **Testabilidade (Automação):** Propor a migração de IDs dinâmicos para 'data-testid' para viabilizar automação estável.
* **Filtros e Busca:** Documentar a necessidade como item de backlog para futura escalabilidade.

## 3. Casos de Teste

Para os casos de teste, defini uma estrutura em tabela com:

| Código | Descrição | Passos | Resposta Esperada | Massa de Dados | Automação |
|:---:|:---|:---|:---|:---|:---:|
|Código do caso | Conteúdo explicativo do teste | Passos a serem testados | Qual a resposta esperada da aplicação | Qual a massa de dados utilizada para o teste. | Se o teste é passível de automação |
---

[Listagem dos Casos de Teste](CT/casos-de-teste.md)

Como já foram discutidos tanto fluxos, quanto os riscos e a prioridade, irei dividir os casos de teste (criados para essa iteração) por prioridade. Essa priorização será usada para os Testes Manuais e para a automação.

[Lista de Casos por Prioridade](CT/prioridades-CT.md)


## 4. Testes Manuais

Os testes manuais foram feitos no seguinte ambiente:
* **Browser:** Google Chrome (Version 142.0.7444.175 (Official Build) (64-bit))
* **Dispositivo:** Desktop
* **Resolução de Tela:** 1920x1080
* **Persistência:** Testes realizados com limpeza de Local Storage.
* **URL da Aplicação:** https://creative-sherbet-a51eac.netlify.app/

[LINK DA PLANILHA](https://docs.google.com/spreadsheets/d/1oyfH7ZFJxfKlOmRqnoUtuw2pmWaIfQirMqN9PRwu20A/edit?usp=sharing)


## 5. Report de Bugs

Para o report de bugs, proponho o seguinte modelo a ser usado:

| Código do Bug | Código do Caso de Teste | Feature | Data e Hora do Teste | Descrição | Passos | Resposta Esperada | Resposta Recebida | Comprovação |
|:---:|:---|:---|:---|:---|:---|:---|:---|:---:|
| BUG-001 | CT-01| Cadastro | 08/03/2026 17:00 | Descrição do Bug | 1. Abrir; 2. Confirmar | Feedback Esperado | Feedback Recebido | (link da screenshot) |

[LINK DO REPORT](https://docs.google.com/spreadsheets/d/1AGsVZkELMJ_DJ7rfDD7Gxwtd_ejlPgp2G1gjNzOqYXI/edit?usp=sharing)

## 6. Conclusão

Depois de rodar os testes, o diagnóstico é: a aplicação é visualmente organizada, mas tecnicamente frágil. O principal problema é a falta de confiança na persistência dos dados e na segurança da aplicação.

O desafio foi muito divertido e me fez percorrer todo o processo de análise e estruturação de um plano de testes. Fico muito grato pela oportunidade!