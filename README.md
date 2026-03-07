# DESAFIO-QA-BEEDOO-2026
Desafio para a vaga de Analista de Qualidade da Beedo.

## INFORMAÇÕES DO CANDIDATO

* Nome: Wesley Lima
* Ambiente de teste: Google Chrome

## Estratégia de Teste

Para este desafio utilizei as seguintes abordagens:

- Testes exploratórios
- Análise estrutural da interface
- Validação de fluxos críticos
- Identificação de riscos

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

A partir desse ponto, ao escolher o tipo de curso do dropdown, aparece um novo campo a ser preenchido, chamado: 'Link de Inscrição' no caso de Online, 'Endereço' no caso de Presencial, os dois do tipo Text.

| Campo | Valor Inserido |
|:--:|:--:|
| Link de Inscrição | https://www.google.com/ |

**Resultado:**
* PopIn verde com a mensagem 'Curso cadastrado com sucesso!' na tela.
* Redirecionado para a Home.
* Item de teste adicionado a Lista de Cursos no corpo da página.
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
* A aplicação opera como um gerenciador de catálogo simples (CRUD)
* Formulário com 09 campos disponíveis. Sendo 01 com valores estáticos ('Tipo de Curso').
* Os elementos do formulário possuem IDs que parecem ser gerados dinamicamente, o que representa um desafio para a estabilidade e consistência de scripts de automação. Apesar da possibilidade de usar o "arial-label" como seletor, o ID seria a forma mais otimizada, o que requeriria mais confiabilidade dele.
* Na exclusão, existe uma desconexão entre a camada de apresentação e a persistência de dados, pois o feedback visual de sucesso não reflete a alteração real nos dados.
* Visualização dos cursos disponíveis através da URL base.
* Não há feature de busca de cursos diponíveis.

Devo acrescentar que uma análise dos requisitos aliada aos testes exploratórios, seria o ideal para um melhor entendimento e conclusões mais assertivas das features. Porém, não há material informativo dos requisitos.

Posso agora iniciar uma análise de fluxos e riscos, partindo das features já desenvolvidas.

## 2. Análise de Fluxos e Riscos

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
    3.

* **Exclusão de Cursos**
    1. A partir da URL Base, clicar no botão 'EXCLUIR CURSO'
    2. Receber feedback de confirmação de exclusão.

### 2.2 Matriz de Riscos

