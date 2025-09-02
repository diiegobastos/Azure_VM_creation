# Criação de Máquina Virtual no Microsoft Azure

## Visão Geral

Este guia detalha o processo completo para criar uma Máquina Virtual (VM) no Microsoft Azure, desde os pré-requisitos até a configuração final. As VMs do Azure fazem parte dos serviços IaaS (Infrastructure as a Service) e oferecem controle total sobre o sistema operacional e aplicações.

## Pré-requisitos

- Conta ativa do Microsoft Azure
- Assinatura do Azure com créditos ou método de pagamento configurado
- Permissões adequadas para criar recursos (Contributor ou Owner)
- Conhecimento básico sobre redes e sistemas operacionais

## Processo de Criação - Portal Azure

### 1. Acesso ao Portal
- Acesse [portal.azure.com](https://portal.azure.com)
- Faça login com suas credenciais
- No painel principal, clique em **"Criar um recurso"** ou use o botão **"+"**

### 2. Seleção do Recurso
- Na página "Criar um recurso", procure por **"Máquina Virtual"**
- Clique em **"Máquina Virtual"** nos resultados
- Selecione **"Criar"** para iniciar o assistente

### 3. Configurações Básicas

#### Detalhes do Projeto
- **Assinatura**: Selecione a assinatura desejada
- **Grupo de Recursos**: 
  - Crie um novo grupo ou selecione existente
  - Recomendação: criar grupo específico para organização
  - Exemplo: `rg-vm-producao` ou `rg-teste-desenvolvimento`

#### Detalhes da Instância
- **Nome da VM**: Escolha um nome descritivo
  - Deve ter entre 1-15 caracteres para Windows
  - Deve ter entre 1-64 caracteres para Linux
  - Exemplo: `vm-web-server-01`

- **Região**: Selecione a região geográfica
  - Considere latência e conformidade
  - Regiões populares no Brasil: Brazil South, Brazil Southeast

- **Opções de Disponibilidade**:
  - Nenhuma redundância de infraestrutura necessária
  - Zona de disponibilidade (para alta disponibilidade)
  - Conjunto de disponibilidade (para redundância)

- **Tipo de Segurança**: 
  - Standard (padrão)
  - Trusted Launch (segurança aprimorada)

#### Configuração da Imagem
- **Imagem**: Escolha o sistema operacional
  - Windows Server 2019/2022
  - Ubuntu Server 20.04/22.04 LTS
  - CentOS, Red Hat, SUSE Linux
  - Outras distribuições disponíveis

- **Executar com Desconto do Azure Spot**: 
  - Marque para usar instâncias com desconto
  - Menor confiabilidade, maior economia

#### Configuração de Tamanho
- **Tamanho**: Selecione especificações de hardware
  - **B-series**: Uso básico, rajadas de CPU
  - **D-series**: Uso geral, balanceado
  - **E-series**: Otimizado para memória
  - **F-series**: Otimizado para computação
  - Exemplo: `Standard_B2s` (2 vCPUs, 4 GB RAM)

### 4. Conta de Administrador

#### Para Windows
- **Nome de usuário**: Criar usuário administrador
- **Senha**: Senha complexa (12+ caracteres)
- **Confirmar senha**: Repetir a senha

#### Para Linux
- **Tipo de Autenticação**:
  - Senha: usuário e senha tradicionais
  - Chave SSH: mais seguro, recomendado
- **Nome de usuário**: usuário administrativo
- **Chave SSH**: cole a chave pública ou gere nova

### 5. Regras de Porta de Entrada
- **Portas de entrada públicas**: 
  - Nenhuma (mais seguro)
  - Permitir portas selecionadas
- **Selecionar portas de entrada**:
  - **Windows**: RDP (3389), HTTP (80), HTTPS (443)
  - **Linux**: SSH (22), HTTP (80), HTTPS (443)

### 6. Configurações de Disco

#### Opções de Disco
- **Tipo de disco do SO**:
  - **Premium SSD**: Melhor performance, maior custo
  - **Standard SSD**: Balanceado
  - **Standard HDD**: Economia, menor performance

- **Criptografia**: 
  - Padrão (Microsoft-managed keys)
  - Customer-managed keys (maior controle)

#### Discos de Dados (Opcional)
- Adicione discos extras conforme necessário
- Configure tamanho e tipo de cada disco
- Defina cache e criptografia

### 7. Configurações de Rede

#### Interface de Rede
- **Rede Virtual**: Selecione ou crie nova VNet
- **Sub-rede**: Escolha sub-rede apropriada
- **IP Público**: 
  - Criar novo (acesso externo)
  - Nenhum (apenas acesso interno)
- **Grupo de Segurança de Rede**: 
  - Básico (regras automáticas)
  - Avançado (NSG personalizado)
  - Nenhum

#### Balanceamento de Carga
- **Opções de balanceamento**: 
  - Nenhum
  - Balanceador de carga do Azure
  - Gateway de aplicativo

### 8. Configurações de Gerenciamento

#### Monitoramento
- **Diagnóstico de inicialização**: Habilitado (recomendado)
- **Diagnósticos do SO convidado**: Para métricas detalhadas
- **Conta de armazenamento de diagnóstico**: Para logs

#### Gerenciamento
- **Identidade**: 
  - Atribuída pelo sistema (managed identity)
  - Atribuída pelo usuário
- **Logon do Azure AD**: Para autenticação integrada
- **Desligamento automático**: Agendar desligamento

#### Atualizações
- **Atualizações automáticas do SO**: 
  - Para Windows: Windows Update
  - Para Linux: patches automáticos

### 9. Configurações Avançadas

#### Extensions
- **Extensões**: Adicionar funcionalidades
  - Antivírus
  - Agentes de monitoramento
  - Custom scripts

#### Configuração de Nuvem
- **Dados personalizados**: Scripts de inicialização
- **Cloud-init** (Linux): Configurações automatizadas

### 10. Tags (Opcional)
- Adicione tags para organização e cobrança
- Exemplos:
  - `Ambiente: Producao`
  - `Projeto: WebApp`
  - `Owner: time-ti@empresa.com`

### 11. Revisão e Criação
- **Validação**: Azure valida configurações
- **Revisar**: Verifique todas as configurações
- **Estimativa de custo**: Visualize custos mensais
- **Termos**: Aceite os termos de uso
- **Criar**: Confirme a criação da VM

## Após a Criação

### Verificações Iniciais
- **Status**: Verifique se VM está "Em execução"
- **IP Público**: Anote o endereço para acesso
- **Teste de Conectividade**: 
  - Windows: RDP na porta 3389
  - Linux: SSH na porta 22

### Configurações Pós-Deploy
- **Atualizações de Sistema**: Instale updates
- **Software**: Instale aplicações necessárias
- **Backup**: Configure Azure Backup
- **Monitoramento**: Configure alertas

## Métodos Alternativos de Criação

### Azure CLI
```bash
# Criar grupo de recursos
az group create --name myResourceGroup --location eastus

# Criar VM
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

### Azure PowerShell
```powershell
# Criar grupo de recursos
New-AzResourceGroup -Name "myResourceGroup" -Location "East US"

# Criar VM
New-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM" -Location "East US" -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress"
```

### ARM Template
- Templates JSON para automação
- Reutilização de configurações
- Versionamento de infraestrutura

## Boas Práticas

### Segurança
- Use autenticação por chave SSH (Linux)
- Desabilite RDP/SSH público quando possível
- Configure NSG com regras restritivas
- Habilite criptografia de disco
- Use Azure Key Vault para segredos

### Performance
- Escolha tamanho adequado à carga de trabalho
- Use Premium SSD para aplicações críticas
- Configure cache de disco apropriadamente
- Monitore métricas de CPU, memória e disco

### Custos
- Use instâncias Spot para workloads tolerantes
- Configure desligamento automático
- Monitore custos regularmente
- Use reservas para workloads previsíveis
- Aplique tags para rastreamento

### Disponibilidade
- Use Availability Sets ou Zones
- Configure backup regular
- Implemente disaster recovery
- Monitore saúde da aplicação

## Solução de Problemas

### Problemas Comuns
- **VM não inicia**: Verifique configurações de disco
- **Não consegue conectar**: Verifique NSG e firewall
- **Performance baixa**: Ajuste tamanho da VM
- **Custo alto**: Otimize tamanho e uso

### Logs e Diagnósticos
- **Boot Diagnostics**: Para problemas de inicialização
- **Activity Log**: Para mudanças de configuração
- **Metrics**: Para monitoramento de performance
- **Azure Monitor**: Para alertas e dashboards

## Conclusão

A criação de VMs no Azure é um processo flexível que permite desde configurações simples até implementações complexas. Seguindo este guia e as boas práticas recomendadas, você terá uma base sólida para deploy e gerenciamento de máquinas virtuais na nuvem Azure.

---

**Lembre-se**: Sempre teste em ambiente de desenvolvimento antes de implementar em produção! 🚀