# Sistema de Gestão Integrado — BNP TECH

Sistema documental da **BNP TECH – Comércio e Prestação de Serviços, Lda.**
("BNP TECH"), implementado segundo as normas **ISO 9001:2015** (Gestão da Qualidade) e
**ISO 14001:2015** (Gestão Ambiental).

**Certificação.** As auditorias de certificação decorreram a **18.11.2026** (Fase 1) e a
**23.11.2026** (Fase 2), para as normas **ISO 9001:2015** e **ISO 14001:2015**. Os certificados estão
a aguardar emissão. Quando chegarem, coloque os PDF em `certificados/` e preencha a tabela em
`html/certificados.html` — é o único sítio do sistema onde se registam números de certificado,
organismo certificador e datas de validade.

- **Razão social:** BNP TECH – Comércio e Prestação de Serviços, Lda.
- **Sede:** Rua dos Fumantes, Bairro Maianga, PR 117Z, n.º 16, Luanda – Angola
- **Escopo:** Prestação de serviços especializados aos setores de petróleo e gás e industrial:
  limpeza e manutenção de tanques, limpeza de cascos, manutenção de parafusos, controlo de sólidos,
  inspeção ambiental, pintura industrial e decapagem abrasiva, soldadura industrial, trabalhos em
  altura e acesso por corda, fornecimento de equipamentos e bens e de mão de obra especializada
- **NIF:** 5001950509
- **Director Geral:** Baião André Gomes
- **Departamento de Qualidade:** titular a designar (responsável pelo SGI)
- **HSE:** Joana Neto
- **Trabalhadores administrativos:** 12 (relação nominal fornecida pela organização)
- **Contactos:** geral@bnptech.ao · +244 947 964 019 · www.bnptech.ao

> **Dados da organização.** Nenhum dado foi inventado. O que ainda não foi comunicado está
> marcado como *a confirmar*.

> **Estrutura organizacional.** O organograma em `PDF-01/DOC-011-ORGANOGRAMA.html` é o que a
> BNP TECH forneceu (original em `logo/organograma-bnptech.pdf`). A estrutura é: **Direção Geral**
> (Baião André Gomes); **Departamento Financeiro** (Pedro Pemba), do qual dependem o Gabinete
> Executivo (Anderson Mangovo), o RH/Administração (Agnela Cardoso) e o Gabinete Jurídico
> (Miguel Suzana); **Departamento Operacional** (Nelson Cardoso), do qual dependem a Logística
> (Kassia Neto) e o HSE (Joana Neto); e **Departamento de Qualidade** (titular a designar).
> Cada unidade tem a sua descrição de cargo, DES-001 a DES-010, alinhada com este organograma.
> A designação nominal de cada titular deve ser registada e comunicada — é a evidência de
> atribuição de responsabilidade e autoridade exigida pela cláusula 5.3 das duas normas.

> **Pontos a verificar com a organização.**
> - O **Departamento de Qualidade** consta do organograma mas não tem titular na relação nominal
>   fornecida. A designação deve ser registada antes da auditoria de supervisão.
> - A relação nominal abrange os **trabalhadores administrativos**; falta a relação dos técnicos de
>   campo que executam os serviços do escopo.
> - O **alvará** e as licenças da organização não foram fornecidos — confirmar que cobrem as
>   atividades do escopo, em especial trabalhos em espaços confinados, em altura e soldadura.
> - A relação nominal indica uma morada diferente da do sistema (Morro Bento 2, Rua dos Generais,
>   Município da Samba). O sistema usa a morada indicada pela organização para o certificado:
>   Rua dos Fumantes, Bairro Maianga, PR 117Z, n.º 16, Luanda. **Confirmar qual deve constar do
>   certificado** — tem de coincidir com a do certificado emitido.
> - **Imparcialidade:** quem redige e mantém o sistema não pode auditar internamente o próprio trabalho.

---

## 1. Como publicar no servidor

1. Envie **toda a pasta `bnp-tech/`** para o servidor (Apache ou Nginx), mantendo a estrutura de subpastas.
2. O ponto de entrada é o `index.html`, que deve ficar na **raiz** do domínio ou da subpasta escolhida.
3. Não é necessária instalação, base de dados nem configuração de servidor: o sistema é estático (HTML, CSS e JavaScript).
4. Todos os caminhos são relativos, pelo que o sistema funciona em qualquer domínio ou subpasta.

**Servir por HTTP, não abrir do disco.** O sistema deve ser aberto através de um servidor web
(`http://…`), e não por duplo clique nos ficheiros (`file://…`). Uma das páginas lê outro ficheiro por
`fetch()`, o que os navegadores bloqueiam em `file://`. Para testar localmente basta, dentro da pasta:

```
python3 -m http.server 8000
```

e abrir `http://localhost:8000`.

**Funciona sem internet.** Todas as bibliotecas — Tailwind CSS, Font Awesome (com os tipos de letra),
e html2pdf — estão na pasta `vendor/` e são carregadas por caminho relativo. Não há qualquer
dependência de CDN, o que elimina também o risco de ficheiros de terceiros alterados sem aviso. As duas
exceções, que precisam de internet, são os vídeos de formação incorporados do YouTube em `html/cursos.html`
e as ligações externas de verificação da certificação, quando forem acrescentadas.

## 2. Acesso ao sistema (utilizador e código)

O sistema abre numa página de entrada (`login.html`) que pede **utilizador** e **código de acesso**.
As credenciais de partida são:

| | |
|---|---|
| **Utilizador** | `wgf` |
| **Código** | `Sgi2026` |

**Para as mudar**, abra o ficheiro `auth-guard.js` e edite as duas primeiras linhas:

```js
var SGI_UTILIZADOR = 'bnptech';
var SGI_CODIGO     = 'Sgi2026';
```

Guarde e recarregue. A sessão dura enquanto o separador do navegador estiver aberto; o botão
**Terminar sessão**, no fundo do menu lateral, fecha-a antes disso.

### O que esta proteção é — e o que não é

Este ecrã de entrada corre **no navegador de quem visita**. Impede que o sistema fique à vista de
quem encontre o endereço por acaso, e as páginas abertas diretamente por URL também são
reencaminhadas para a entrada. Mas **não é uma proteção verdadeira**: quem souber o que procura
consegue ler o utilizador e o código no código-fonte da página.

**Para proteção a sério**, use a autenticação do servidor. O ficheiro `.htaccess` que acompanha o
sistema já está preparado para isso em alojamentos Apache (cPanel e semelhantes): basta descomentar
as cinco linhas de autenticação, indicar o caminho do `.htpasswd` e gerá-lo — as instruções estão
dentro do próprio ficheiro. Com isso, nenhuma página chega ao navegador sem palavra-passe válida e
a palavra-passe nunca é exposta.

No GitHub Pages não há autenticação de servidor: aí a única forma de restringir mesmo o acesso é
manter o repositório privado e usar outro alojamento para publicar.

## 3. Requisitos técnicos

- Alojamento de ficheiros estáticos (Apache, Nginx ou equivalente).
- Não requer PHP, base de dados nem backend.
- Compatível com Chrome, Firefox, Edge e Safari em computador, tablet e telemóvel.

## 4. Estrutura de pastas

```
bnp-tech/
  login.html                     página de entrada: utilizador e código
  index.html                     menu e moldura do sistema
  auth-guard.js                  controlo de acesso (utilizador e código definidos aqui)
  .htaccess                      proteção do servidor em alojamentos Apache
  README.md                      este ficheiro
  html/                          páginas do sistema e cláusulas
  PDF-01/                        documentos, políticas, procedimentos, processos, cargos
  Excel-trinormas/               formulários e modelos de registo
  Manualesyleyes/                manuais externos de referência
  Registros/                     registos internos
  NormasISO/                     textos das normas (uso interno)
  pop/                           procedimentos operacionais padrão (a desenvolver)
  certificados/                  certificados ISO em PDF e pré-visualizações
  vendor/                        bibliotecas locais (Tailwind, Font Awesome, html2pdf)
  logo/ Logotipo/ img/           recursos visuais
```

## 5. Como atualizar o logótipo e os dados da empresa

- **Logótipo:** substitua o ficheiro `logo.png` nas pastas `Logotipo/`, `logo/` e `Registros/Logotipo/` (a mesma imagem nas três).
- **Dados de contacto:** aparecem no rodapé do `index.html` e em `html/apresentacao.html`.
- **Organograma:** a imagem está em `logo/organigrama.jpg` e o original em `logo/organograma-bnptech.pdf`. Ao substituí-la, atualize também o texto do `PDF-01/DOC-011-ORGANOGRAMA.html`.

## 6. Como adicionar um novo documento

1. Guarde o ficheiro HTML na pasta correspondente (`PDF-01/` para documentos controlados, `Excel-trinormas/` para formulários).
2. Siga a codificação documental do sistema:

   | Prefixo | Tipo |
   |---|---|
   | DOC | Documentos institucionais |
   | POL | Políticas |
   | PRO | Procedimentos |
   | PSC / PCS | Processos |
   | MAN | Manuais |
   | OA | Objetivos ambientais |
   | PLA | Planos |
   | DES | Descrições de cargo |
   | FORM | Formulários |
   | REG | Registos |
   | MAT | Matrizes |
   | INS | Instruções de trabalho |
   | POP | Procedimentos operacionais padrão |
   | EXT | Documentos externos |

3. Acrescente a entrada correspondente no Índice Documental (ver ponto 7).

## 7. Como alterar os menus

O menu principal está no `index.html`, dentro da secção `<nav id="sideMenu">`. Cada item é uma ligação `<a>`
com `target="contentFrame"`, que carrega a página dentro da moldura principal.

## 8. Como atualizar o Índice Documental

Edite `html/indice_documental_moderno.html`. A lista de documentos está definida num array JavaScript, com
uma entrada por documento no formato:

```javascript
{cat:'Procedimentos', id:'PRO-001', name:'Controlo de Informação Documentada',
 url:'../PDF-01/PRO-001-control-informacion-documentada.html', type:'html', status:'active'},
```

**Importante:** sempre que criar ou eliminar um documento, atualize este ficheiro. Um índice que aponta para
documentos inexistentes, ou que omite documentos existentes, é uma não conformidade em auditoria.

## 9. Como criar cópia de segurança

Copie periodicamente a pasta completa `bnp-tech/`. Sendo um sistema estático, o backup é a simples cópia
dos ficheiros. Recomenda-se guardar uma cópia datada antes de cada revisão significativa.

## 10. Limitações do sistema

- **Sem base de dados central.** O módulo "Sistema de Registos" funciona no navegador de cada utilizador
  (armazenamento local). Os dados preenchidos num computador **não** aparecem noutro e **não** constituem
  uma base de dados partilhada.
- **Sem autenticação.** Não existe controlo de acesso. Quem tiver o endereço pode consultar o sistema.
  As páginas com modo de edição alteram apenas a cópia local do navegador de quem edita.
- **Sem fluxo de aprovação.** As aprovações de documentos têm de ser geridas fora do sistema.

## 11. Recomendações para evolução futura

1. **Desenvolver os 18 POP** identificados na página de Procedimentos Operacionais. São o documento que
   descreve como o trabalho é realmente executado e é o que o auditor compara com a prática no terreno.
2. **Concluir o levantamento dos requisitos legais angolanos** aplicáveis em matéria laboral, de segurança e
   saúde no trabalho, **ambiental** (resíduos, efluentes, emissões, produtos químicos e licenciamento) e do
   setor de petróleo e gás, e registá-lo numa matriz de requisitos legais (PRO-022).
3. **Atualizar os diagramas quando a estrutura mudar.** O organograma é o fornecido pela BNP TECH
   (`logo/organograma-bnptech.pdf`, publicado em `logo/organigrama.jpg`); ao alterá-lo, atualize também a
   descrição em `PDF-01/DOC-011-ORGANOGRAMA.html` e as descrições de cargo afetadas. O mapa de
   processos tem fonte editável em `logo/mapa-processos.svg`; após editar, exporte e substitua `logo/mapa.png`.
4. Migrar o Sistema de Registos para uma solução com base de dados, caso se pretenda registo centralizado.
5. **Preparar a auditoria de supervisão** (prevista para 12.09.2027): manter atualizados os registos de auditoria interna
   (PRO-004), a Revisão pela Direção e as ações corretivas, que são a evidência exigida para manter a certificação.

## 12. Documentação da ISO 14001 (Gestão Ambiental)

A componente ambiental foi criada de raiz para a BNP TECH, a partir das atividades declaradas no
escopo — construção, manutenção, limpeza, decapagem abrasiva e apoio operacional — que geram resíduos,
efluentes, poeiras, ruído e implicam o manuseamento de produtos químicos, tintas e solventes.

| Documento | Conteúdo |
|---|---|
| `PDF-01/DOC-006-Politica-Ambiental.html` | Política Ambiental, com sete pilares de compromisso |
| `PDF-01/PSC-003-Gestion-Ambiental.html` | Processo de Gestão Ambiental: entradas, atividades, saídas, recursos, indicadores, riscos e oportunidades |
| `PDF-01/PRO-014-Aspetos-e-Impactes-Ambientais.html` | Metodologia de identificação e avaliação de aspetos e impactes (cláusula 6.1.2) |
| `PDF-01/PRO-032-Gestao-de-Residuos.html` | Gestão de resíduos, efluentes, emissões, ruído e resposta a derrames |
| `PDF-01/OA-001-Objetivos-Ambientais.html` | Sete objetivos ambientais, com indicador, fórmula, metas e fonte |
| `Excel-trinormas/FORM-016-Matriz-Aspetos-Impactes-Ambientais.html` | Matriz com 27 aspetos ambientais avaliados, com cálculo automático do índice de significância |
| `html/registros-ambientais.html` | Portal dos registos ambientais e lista do que a organização tem de produzir |

**Nota sobre as pontuações da matriz FORM-016:** os valores de gravidade, frequência, requisito legal e
partes interessadas são uma **proposta de partida** deduzida das atividades, não uma medição. Devem ser
confirmados ou corrigidos pela organização para cada obra e cliente.

---

*Sistema adaptado a partir de uma estrutura-modelo. Todo o conteúdo institucional, normativo e de identidade
foi revisto para a BNP TECH. Os documentos têm data de emissão de 14.01.2026 e o Sistema de Gestão Integrado
está em funcionamento. Nenhuma evidência foi inventada: onde um registo depende da operação — guias de
resíduos, fichas de aplicação de produtos químicos industriais, registos de formação, atas de auditoria —, é a organização que
o produz e arquiva.*
