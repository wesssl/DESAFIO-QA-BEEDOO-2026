## 3.2 Prioridade P1: Fluxos Críticos, Happy Paths e Vulnerabilidades
Foco em funcionalidades que impedem o uso ou comprometem a integridade imediata.

| Código | Descrição | Passos | Resposta Esperada | Massa de Dados | Automação |
|:---:|:---|:---|:---|:---|:---:|
| **CT-01** | Cadastro Completo - Curso Online | 1. Preencher campos válidos.<br>2. Selecionar 'Online' e inserir Link.<br>3. Salvar. | PopIn de sucesso, redirecionamento e item visível na Home. | Dados válidos padrão | Sim |
| **CT-02** | Cadastro Completo - Curso Presencial | 1. Preencher campos válidos.<br>2. Selecionar 'Presencial' e inserir endereço.<br>3. Salvar. | PopIn de sucesso, redirecionamento e item visível na Home. | Rua das Flores, 123 | Sim |
| **CT-03** | Exclusão de Curso | 1. Identificar curso na Home.<br>2. Clicar em 'Excluir' e confirmar.<br>3. Recarregar página (F5). | O curso deve ser removido permanentemente (não deve reaparecer). | Curso existente | Sim |
| **CT-04** | Persistência de Dados (Visualização) | 1. Cadastrar curso.<br>2. Voltar à Home.<br>3. Validar se todos os dados aparecem no card. | Os dados exibidos no card devem ser idênticos aos inseridos no cadastro. | Descrição > 100 chars | Sim |
| **CT-05** | Obrigatoriedade de Campos | 1. Tentar salvar formulário totalmente vazio. | Exibir alertas de "Campo Obrigatório" nos inputs correspondentes. | N/A | Sim |
| **CT-10** | Injeção de Script (XSS) no Nome | 1. No campo 'Nome', inserir script de alerta.<br>2. Salvar e visualizar na Home. | O sistema deve tratar o texto como string e não executar o alerta. | `<script>alert(1)</script>` | Sim |
| **CT-11** | Injeção de SQL (SQLi) básico | 1. No campo 'Instrutor', inserir comandos SQL.<br>2. Tentar salvar. | O sistema deve tratar a entrada como texto comum, sem erros de banco. | `' OR 1=1 --` | Sim |
| **CT-21** | Duplicidade de Cadastro | 1. Cadastrar o Curso "Teste A".<br>2. Tentar cadastrar o mesmo curso novamente. | O sistema deve avisar duplicidade ou gerar IDs distintos. | Curso Duplicado | Sim |

### 3.3 Prioridade P2: Regras de Negócio e UX
Foco no comportamento esperado do sistema e experiência do usuário.

| Código | Descrição | Passos | Resposta Esperada | Massa de Dados | Automação |
|:---:|:---|:---|:---|:---|:---:|
| **CT-06** | Lógica de Datas Inversas | 1. Data Início: 20/05/2026.<br>2. Data Fim: 10/05/2026.<br>3. Salvar. | Erro de validação: "Data de fim não pode ser anterior à data de início". | Datas inválidas | Sim |
| **CT-07** | Limite de Vagas Negativo | 1. Inserir "-10" ou "0" no campo de vagas.<br>2. Tentar salvar. | O sistema deve aceitar apenas números inteiros positivos (> 0). | Vagas: -10 | Sim |
| **CT-09** | Alternância de Tipo de Curso | 1. Selecionar 'Online' e preencher Link.<br>2. Mudar para 'Presencial'. | O formulário deve alternar os campos e limpar o estado do anterior. | N/A | Sim |
| **CT-13** | HTML Injection na Descrição | 1. Inserir tags HTML na descrição.<br>2. Salvar e ver na Home. | O layout não deve ser alterado (exibido como texto puro). | `<h1>TESTE</h1>` | Sim |
| **CT-15** | Responsividade Mobile | 1. Aceder à Home em resolução 375x812.<br>2. Abrir o formulário. | Elementos ajustados sem scroll horizontal ou sobreposição. | N/A | Sim |
| **CT-18** | Tabulação de Formulário | 1. Usar a tecla 'TAB' para navegar entre os campos. | O foco deve seguir a ordem visual lógica do formulário. | Tecla TAB | Sim |
| **CT-19** | Persistência após Refresh (F5) | 1. Preencher metade do formulário.<br>2. Atualizar a página (F5). | O sistema deve manter os dados ou limpar com aviso de perda. | Dados parciais | Sim |
| **CT-23** | Caracteres Especiais em Datas | 1. Tentar inserir letras nos campos de Data via console. | O sistema deve rejeitar qualquer formato fora do padrão de data. | "ab/cd/efgh" | Sim |
| **CT-24** | Formatação de Endereço | 1. Inserir apenas números ou símbolos no campo 'Endereço'. | O sistema deve validar se o endereço possui um formato mínimo aceitável. | "12345678" | Sim |
| **CT-26** | Espaços em Branco (Trim) | 1. No campo 'Nome', inserir apenas espaços ("    "). | O sistema deve considerar o campo como vazio. | "    " | Sim |

## 3.4 Prioridade P3: Casos de Borda e Resiliência de Interface
Foco em polimento, limites extremos e estabilidade técnica.

| Código | Descrição | Passos | Resposta Esperada | Massa de Dados | Automação |
|:---:|:---|:---|:---|:---|:---:|
| **CT-08** | Caracteres em Campo Numérico | 1. Digitar letras ou símbolos (e, +, -) no campo de Vagas. | O campo deve bloquear a entrada de caracteres não numéricos. | "ABC", "@#$" | Sim |
| **CT-12** | Payload de Imagem Maliciosa | 1. Inserir link de executável no campo 'URL da Imagem'. | Validar extensão/tipo do arquivo antes de permitir o cadastro. | `executal.exe` | Sim |
| **CT-14** | Spam de Cadastro (Bot-like) | 1. Clicar repetidamente no botão 'Cadastrar'. | O sistema deve possuir debouncing para evitar cadastros duplicados. | Cliques rápidos | Parcial |
| **CT-16** | Quebra de Layout (Nomes) | 1. Inserir Nome com 100 caracteres sem espaços.<br>2. Ver na Home. | O CSS deve aplicar word-break para não quebrar a largura do card. | "CURSO"*100 | Não |
| **CT-17** | Placeholder e Labels | 1. Entrar no formulário.<br>2. Validar visibilidade de labels e placeholders. | Devem seguir o padrão visual e auxiliar o preenchimento. | N/A | Não |
| **CT-20** | Navegação por URL Direta | 1. Aceder a `/new-course` offline.<br>2. Usar botão 'Back'. | A aplicação deve lidar com o estado de navegação sem travar. | N/A | Sim |
| **CT-22** | URL de Imagem Gigante | 1. Inserir URL com > 2000 caracteres no campo 'URL da capa'. | O sistema deve limitar a string ou validar o header da URL. | String > 2000 chars | Sim |