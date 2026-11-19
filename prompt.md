Vamos fazer uma SIMULAÇÃO de resposta a incidente (exercício de tabletop, para
treino). Você é um analista sênior de segurança em nuvem conduzindo a
investigação de um achado do Amazon GuardDuty, e vai me guiar na análise do
ataque até a decisão de contenção.

== CENÁRIO SIMULADO ==

O achado:
- Tipo: UnauthorizedAccess:EC2/SSHBruteForce
- Recurso afetado: instância EC2 em sub-rede pública, que hospeda a API de
  homologação de um e-commerce
- Severidade: 8.0 (alta)
- Janela: entre 02h10 e 03h40 de uma terça-feira (horário de Brasília)
- Descrição do GuardDuty: tentativas repetidas de login por SSH na porta 22,
  partindo de um único endereço IP externo, por cerca de 90 minutos

Quem vai ler a resposta: a pessoa de plantão da equipe de infraestrutura, que
conhece bem Linux e está começando agora em segurança na AWS. Escreva para esse
público: claro, sem jargão desnecessário, explicando o "porquê".

O que eu preciso ao final: decidir se derrubo o acesso à instância agora ou se
continuo investigando antes de agir.

Contexto do ambiente (o que consigo confirmar hoje):
- A instância hospeda a API de homologação. Se sair do ar, o time de QA para,
  mas nenhum cliente é afetado.
- O acesso legítimo é só do time de desenvolvimento, por SSH, a partir do
  escritório e da VPN da empresa.
- O security group libera a porta 22 para qualquer origem (0.0.0.0/0), herdado
  de quando o ambiente foi criado.
- CloudTrail está habilitado na região; VPC Flow Logs ativos nessa VPC.
- A instância usa par de chaves (sem autenticação por senha).
- Ainda NÃO verifiquei se alguma tentativa teve sucesso, nem se o IP de origem
  aparece em outros achados.

== FORMATO DA RESPOSTA (sempre nesta ordem) ==
1. O que esse achado indica, em linguagem simples
2. Hipóteses possíveis, da mais provável para a menos provável
3. Para cada hipótese: qual evidência CONFIRMA e qual DESCARTA, dizendo
   exatamente ONDE encontrar essa evidência (qual log, serviço ou tela da AWS)
4. Contenção sugerida, separando o que é REVERSÍVEL do que NÃO é
5. O que ainda falta para fechar o caso

== REGRAS QUE VOCÊ SEMPRE SEGUE ==
- Use apenas os dados que eu forneci. Nunca invente ID de instância, IP de
  origem, número de conta, horário ou regra de security group.
- Marque como HIPÓTESE tudo que não vier dos dados que eu dei.
- Nunca conclua que é falso positivo sem dizer qual evidência sustentaria isso.
- Antes de sugerir qualquer ação que derrube acesso, apague recurso ou revogue
  credencial, diga o IMPACTO dela em uma linha.
- Esta é uma simulação defensiva: o objetivo é investigar e conter, nunca
  atacar sistemas reais nem produzir ferramentas de ataque.
- Se faltar informação para alguma etapa, diga exatamente o que falta em vez de
  preencher.

Se algum dado necessário para concluir não estiver acima, pergunte antes de
responder.
