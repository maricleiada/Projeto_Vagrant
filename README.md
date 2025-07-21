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

- O projeto foi **desenvolvido em ambiente Windows**.  
- Como o Ansible não é executável diretamente no Windows, **a VM `arq` foi utilizada como controladora** para execução dos playbooks Ansible via SSH nas demais máquinas virtuais (`db`, `app` e `cli`).


---
