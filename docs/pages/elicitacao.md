# Elicitação de Requisitos

## Visão Geral

Esta seção apresenta o processo de elicitação de requisitos do sistema AUVET, baseado na análise de personas e necessidades dos usuários.

## Personas

### Persona 1: Dr. Carlos - Veterinário
- **Idade**: 35 anos
- **Experiência**: 8 anos de clínica
- **Necessidades**: Sistema rápido para consultar histórico, agendar consultas e registrar procedimentos

### Persona 2: Maria - Recepcionista
- **Idade**: 28 anos
- **Experiência**: 3 anos na clínica
- **Necessidades**: Interface simples para cadastrar tutores e animais, gerenciar agendamentos

### Persona 3: Ana - Tutora
- **Idade**: 45 anos
- **Experiência**: Primeira vez com pet
- **Necessidades**: Acesso fácil aos dados do animal, lembretes de vacinas e consultas

## Requisitos Funcionais

### RF01 - Gestão de Usuários
- O sistema deve permitir cadastro de administradores, funcionários e tutores
- Cada usuário deve ter login único e senha segura
- Diferentes níveis de acesso baseados no perfil

### RF02 - Gestão de Clínicas
- Cadastro e configuração de clínicas veterinárias
- Configuração de serviços oferecidos
- Gestão de funcionários por clínica

### RF03 - Gestão de Pacientes
- Cadastro completo de animais (tutores e pets)
- Histórico médico digitalizado
- Controle de vacinação

### RF04 - Sistema de Agendamento
- Agendamento de consultas e procedimentos
- Lembretes automáticos
- Controle de disponibilidade

## Requisitos Não-Funcionais

### RNF01 - Performance
- Tempo de resposta < 2 segundos
- Suporte a 100 usuários simultâneos

### RNF02 - Segurança
- Criptografia de dados sensíveis
- Autenticação JWT
- Backup automático

### RNF03 - Usabilidade
- Interface intuitiva e responsiva
- Acessibilidade WCAG 2.1

## Histórico de Versões

| Versão | Data | Autor | Descrição |
|--------|------|-------|-----------|
| 1.0.0 | 2025-09-13 | Izabella Alves | Versão inicial da elicitação de requisitos com personas e requisitos funcionais/não-funcionais |
