# Projeto_Vagrant

Disciplina: Administração de Sistemas Abertos  
**Professor:** Leonidas Lima  
**Período:** 2025.1  
**Instituição:** Instituto Federal da Paraíba - Campus João Pessoa

## 👥 Integrantes da Equipe
- maricleia - Matrícula: 20232380032

## 🎯 Objetivo
O projeto tem como objetivo desenvolver de uma infraestrutura virtual composta por 4 máquinas e a automatização da configuração de diversos serviços essenciais em ambiente Linux.

---

## 🖥️ Infraestrutura Virtual

### Características Gerais
- Provider: VirtualBox  
- Box: `debian/bookworm64`  
- Geração de chaves SSH desativada  
- Uso de clones (linked_clone)  
- Verificação de guest additions desabilitada  

### Máquinas Virtuais

#### 🔐 Servidor de Arquivos - `arq`
- IP: `192.168.56.132`
- Hostname: `arq.maricleia.devops`
- Discos adicionais: 3x10GB
- Serviços: DHCP, LVM, NFS

#### 🧮 Servidor de Banco de Dados - `db`
- IP via DHCP
- Hostname: `db.maricleia.devops`
- Serviços: MariaDB, Autofs

#### 🌐 Servidor de Aplicação - `app`
- IP via DHCP
- Hostname: `app.maricleia.devops`
- Serviços: Apache2, Autofs

#### 💻 Host Cliente - `cli`
- IP via DHCP
- Hostname: `cli.maricleia.devops`
- Recursos: 1024MB RAM, Firefox, X11 Forwarding, Autofs

---

## ⚙️ Automatizações com Ansible

As configurações foram automatizadas utilizando playbooks organizados na pasta `ansible/`.

### 🛠️ Tarefas Comuns
- Atualização do sistema (update & upgrade)
- Instalação do `chrony` com pool `pool.ntp.br`
- Ajuste de timezone para `America/Recife`
- Criação do grupo `ifpb` e usuários `nome1` e `nome2`
- Configuração de SSH:
  - Apenas chave pública
  - Bloqueio do root
  - Permissão apenas para `vagrant` e `ifpb`
  - Banner de saudação
- Instalação do cliente NFS
- Permissão de uso do `sudo` para o grupo `ifpb`

### 📁 Servidor `arq`
- DHCP (domínio `maricleia.devops`, faixa 192.168.56.50-200, DNS: 1.1.1.1 e 8.8.8.8)
- LVM: VG `dados` com LV `ifpb` (15GB, ext4)
- Compartilhamento NFS `/dados/nfs`:
  - Acesso apenas por `nfs-ifpb`
  - Mapeamento automático de usuários remotos
  - Escrita controlada, sync ativado
  - Shell do `nfs-ifpb` desativado

### 🧮 Servidor `db`
- Instalação do `mariadb-server`
- Montagem automática via `autofs` em `/var/nfs`

### 🌐 Servidor `app`
- Instalação do `apache2`
- Substituição da página padrão por `index.html` com:
  - Descrição do projeto
  - Nomes e matrículas dos integrantes
- Autofs com montagem em `/var/nfs`

### 💻 Cliente `cli`
- Instalação de `firefox-esr` e `xauth`
- Ativação de `X11Forwarding` no SSH
- Autofs com montagem em `/var/nfs`

- O projeto foi **desenvolvido em ambiente Windows**.  
- Como o Ansible não é executável diretamente no Windows, **a VM `arq` foi utilizada como controladora** para execução dos playbooks Ansible via SSH nas demais máquinas virtuais (`db`, `app` e `cli`).


---
