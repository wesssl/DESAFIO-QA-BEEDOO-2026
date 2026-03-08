## 3.1 Listagem de Casos de Teste


| Código | Descrição | Passos | Resposta Esperada | Massa de Dados | Automação |
|:---:|:---|:---|:---|:---|:---:|
| **CT-01** | Cadastro Completo - Curso Online | 1. Preencher campos válidos.<br>2. Selecionar 'Online'.<br>3. Inserir URL no 'Link de Inscrição'.<br>4. Salvar. | PopIn de sucesso, redirecionamento e item visível na Home com link funcional. | Dados válidos padrão | Sim |
| **CT-02** | Cadastro Completo - Curso Presencial | 1. Preencher campos válidos.<br>2. Selecionar 'Presencial'.<br>3. Inserir endereço físico.<br>4. Salvar. | PopIn de sucesso, redirecionamento e item visível na Home com endereço correto. | Rua das Flores, 123 | Sim |
| **CT-03** | Exclusão de Curso (Funcional) | 1. Identificar curso na Home.<br>2. Clicar em 'Excluir'.<br>3. Confirmar alerta.<br>4. Recarregar página (F5). | O curso deve ser removido do banco e da interface (não deve reaparecer). | Curso existente | Sim |
| **CT-04** | Persistência de Dados (Visualização) | 1. Cadastrar curso com descrição longa.<br>2. Voltar à Home.<br>3. Validar se todos os dados aparecem no card. | Os dados exibidos no card devem ser idênticos aos inseridos no cadastro. | Descrição > 100 chars | Sim |
| **CT-05** | Obrigatoriedade de Campos | 1. Tentar salvar formulário totalmente vazio. | Sistema deve exibir alertas de "Campo Obrigatório" em todos os inputs. | N/A | Sim |
| **CT-06** | Lógica de Datas Inversas | 1. Data Início: 20/05/2026.<br>2. Data Fim: 10/05/2026.<br>3. Tentar salvar. | Erro de validação: "Data de fim não pode ser anterior à data de início". | Datas inválidas | Sim |
| **CT-07** | Limite de Vagas Negativo | 1. Inserir "-10" ou "0" no campo de vagas.<br>2. Tentar salvar. | O sistema deve aceitar apenas números inteiros positivos (> 0). | Vagas: -10 | Sim |
| **CT-08** | Caracteres em Campo Numérico | 1. Tentar digitar letras ou símbolos (e, +, -) no campo de Vagas. | O campo deve bloquear a entrada de caracteres não numéricos. | "ABC", "@#$" | Sim |
| **CT-09** | Alternância de Tipo de Curso | 1. Selecionar 'Online' e preencher Link.<br>2. Mudar para 'Presencial'.<br>3. Verificar se o Link sumiu e Endereço apareceu. | O formulário deve limpar o estado do campo anterior para evitar dados "sujos". | N/A | Sim |
| **CT-10** | Injeção de Script (XSS) no Nome | 1. No campo 'Nome do Curso', inserir script de alerta.<br>2. Salvar e visualizar na Home. | O sistema deve tratar o texto como string (sanitização) e não executar o alerta. | `<script>alert(1)</script>` | Sim |
| **CT-11** | Injeção de SQL (SQLi) básico | 1. No campo 'Instrutor', inserir comandos SQL.<br>2. Tentar salvar. | O sistema deve tratar a entrada como texto comum e não sofrer erro de banco de dados. | `' OR 1=1 --` | Sim |
| **CT-12** | Payload de Imagem Maliciosa | 1. No campo 'URL da Imagem', inserir link de script ou executável. | O sistema deve validar a extensão/tipo do arquivo antes de permitir o cadastro. | `executavel.exe` | Sim |
| **CT-13** | HTML Injection na Descrição | 1. Inserir tags HTML (Ex: `<h1>`) na descrição.<br>2. Salvar e ver na Home. | O layout não deve ser alterado pelo HTML inserido (exibido como texto puro). | `<h1>TESTE</h1>` | Sim |
| **CT-14** | Spam de Cadastro (Bot-like) | 1. Clicar repetidamente no botão 'Cadastrar' em curto intervalo. | O sistema deve possuir debouncing ou rate limit para evitar cadastros duplicados. | Cliques rápidos | Parcial |
| **CT-15** | Responsividade Mobile | 1. Aceder à Home em resolução 375x812.<br>2. Abrir o formulário. | Os elementos devem se ajustar sem gerar scroll horizontal ou sobreposição. | N/A | Sim |
| **CT-16** | Quebra de Layout (Nomes sem espaços) | 1. Inserir um Nome de Curso com 100 caracteres sem espaços.<br>2. Salvar e ver na Home. | O CSS deve conter `word-break` para não "estourar" a largura do card. | "CURSO"*100 | Não |
| **CT-17** | Placeholder e Labels | 1. Entrar no formulário.<br>2. Verificar se todos os campos têm labels e placeholders claros. | Todos os campos devem seguir o padrão visual e auxiliar o preenchimento. | N/A | Não |
| **CT-18** | Tabulação de Formulário (Acessibilidade) | 1. Usar a tecla 'TAB' para navegar entre os campos.<br>2. Verificar a ordem lógica. | O foco deve seguir a ordem visual do formulário. | Tecla TAB | Sim |
| **CT-19** | Persistência após Refresh (F5) | 1. Preencher metade do formulário.<br>2. Atualizar a página (F5). | O sistema deve manter os dados ou limpar com aviso, evitando perda acidental. | Dados parciais | Sim |
| **CT-20** | Navegação por URL Direta | 1. Tentar aceder a `/new-course` estando offline.<br>2. Tentar voltar via botão 'Back'. | A aplicação deve lidar com o estado de navegação sem travar. | N/A | Sim |
| **CT-21** | Duplicidade de Cadastro | 1. Cadastrar o Curso "Teste A".<br>2. Tentar cadastrar o mesmo curso novamente. | O sistema deve avisar duplicidade ou gerar identificadores únicos distintos. | Curso Duplicado | Sim |
| **CT-22** | Upload de URL de Imagem Gigante | 1. Inserir URL de imagem com > 2000 caracteres no campo 'URL da capa'. | O sistema deve limitar o tamanho da string ou validar o header da URL. | String > 2000 chars | Sim |
| **CT-23** | Caracteres Especiais em Datas | 1. Tentar forçar a inserção de letras nos campos de Data via console. | O sistema deve rejeitar qualquer formato que não seja padrão de data. | "ab/cd/efgh" | Sim |
| **CT-24** | Formatação de Endereço (Presencial) | 1. Inserir apenas números ou símbolos no campo 'Endereço'. | O sistema deve validar se o endereço possui um formato mínimo aceitável. | "12345678" | Sim |
| **CT-25** | Limite de Vagas Extremo | 1. Inserir um número astronômico no campo vagas (ex: 9^12). | O sistema deve ter um limite máximo para evitar erro de estouro de memória (Integer Max). | 999.999.999.999 | Sim |
| **CT-26** | Espaços em Branco (Trim) | 1. No campo 'Nome', inserir apenas espaços ("    ").<br>2. Tentar salvar. | O sistema deve considerar o campo como vazio (invalidar espaços em branco). | "    " | Sim |