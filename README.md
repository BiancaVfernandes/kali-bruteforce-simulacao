🛡️ Simulação de Ataque de Força Bruta (Brute Force) com Kali Linux

Este projeto simula, em ambiente controlado, a descoberta de credenciais fracas em serviços de rede (FTP e SMB) para fins de auditoria de segurança. Demonstra a importância da persistência e da adaptação de ferramentas (Medusa/Hydra) durante um teste de intrusão.

🎯 Objetivos de Aprendizado

Utilizar ferramentas profissionais como Hydra e smbclient.

Compreender a vulnerabilidade de senhas fracas e a diferença entre os protocolos de rede.

Documentar as etapas técnicas e propor medidas de mitigação eficazes.

💻 Configuração do Ambiente

Máquina Atacante: Kali Linux (Usuário: bianca).

Máquina Vítima: Metasploitable 2 (IP: 192.168.56.101).

Topologia de Rede: Host-Only (rede isolada para testes).

1. ⚔️ Cenário FTP (Força Bruta Simples)

Ataque de Brute Force usando o Hydra contra o serviço FTP (Porta 21), simulando a quebra da senha de um usuário conhecido (msfadmin). O parâmetro -t 1 foi usado para estabilizar a conexão.

➡️ Comando Utilizado

hydra -l msfadmin -P /home/bianca/passwords.txt -t 1 ftp://192.168.56.101


🔑 Credencial Encontrada

O ataque foi bem-sucedido, quebrando a senha em segundos:
msfadmin : msfadmin

📸 Evidência de Sucesso 
<img width="1089" height="427" alt="image" src="https://github.com/user-attachments/assets/c3f297f9-3e18-4f4c-97f1-fbb9b29d5976" />


2. 🗄️ Cenário SMB (Password Spraying/Acesso)

O ataque de Password Spraying usando o módulo Hydra SMB falhou devido a incompatibilidade de protocolo com o alvo. A vulnerabilidade de credenciais fracas foi confirmada manualmente usando o smbclient.

➡️ Comando de Prova de Vulnerabilidade

Usamos o smbclient para provar que a mesma credencial (que seria o alvo do password spraying) funciona no serviço SMB (porta 445):

smbclient -L 192.168.56.101 -U msfadmin%msfadmin


🔓 Resultado Confirmado

A credencial msfadmin:msfadmin deu acesso completo ao serviço, listando os compartilhamentos de disco (`IPC$`, `ADMIN$`, etc.).

📸 Evidência de Sucesso <img width="742" height="462" alt="image" src="https://github.com/user-attachments/assets/f9101b8f-f6f4-4979-a87b-fa3e24f97dc4" />


🛑 3. Medidas de Mitigação e Defesa

Para proteger a rede e os serviços (FTP/SMB) contra ataques de força bruta, as seguintes ações de defesa são essenciais:

Bloqueio de IP (Rate Limiting): Implementar o Fail2Ban no servidor Linux para bloquear temporariamente o endereço IP do atacante após um pequeno número de tentativas falhas (ex: 5).

Complexidade de Senha: Aplicar políticas rigorosas que forcem o uso de senhas longas, complexas e que sejam trocadas periodicamente.

Autenticação: Implementar Múltiplos Fatores de Autenticação (MFA) para todos os serviços críticos.

Usuários Padrão: Renomear ou desativar quaisquer contas padrão ou de teste (como msfadmin ou user) que possam ser alvos fáceis.
