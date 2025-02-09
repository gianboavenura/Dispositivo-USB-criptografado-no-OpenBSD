# 🔒 Criptografar um USB Flash Drive no OpenBSD

Guia passo a passo para criar um dispositivo USB criptografado no OpenBSD.

---

## ⚠️ Aviso Importante
**Substitua `sd2` e `sd3` pelos identificadores corretos do seu dispositivo USB.**  
Verifique com `dmesg | grep sd` antes de executar os comandos.

---

## 📝 Passo a Passo

### 1. Sobrescrever com Dados Aleatórios

dd if=/dev/random of=/dev/rsd2c bs=4m

### 2. Particionamento Inicial

fdisk -iy sd2

### 2.1 Criar Partição RAID

disklabel -E sd2

Passos interativos:

a → Adicionar partição

a → Letra da partição (a)

[Enter] → Tamanho inicial (padrão)

[Enter] → Tamanho final (padrão)

RAID → Tipo da partição

w → Salvar alterações

q → Sair

### 5. Criar Sistema de Arquivos

newfs sd3a

### 6. Montar o Dispositivo

mkdir -p /mnt/USBCriptografado

mount /dev/sd3a /mnt/USBCriptografado


### 🔄 Gerenciamento Diário

### ▶️ Montar o Dispositivo

bioctl -c C -l sd2a softraid0

mount /dev/sd3i /mnt/USBCriptografado  # sd3i para montagem via bioctl

### ⏏️ Desmontar Corretamente

umount /mnt/USBCriptografado

bioctl -d sd3

eject sd2


### 💡 Dicas Importantes

Senha Segura: Use uma senha complexa com mistura de caracteres

Backup: Mantenha backup dos dados fora do dispositivo

Compatibilidade: Este método é específico para OpenBSD

Atualizações: Verifique bioctl(8) e disklabel(8) na documentação oficial


### ❓ FAQ Rápido

Dispositivo correto?

dmesg | grep sd


Partições criadas?

disklabel sd2 e disklabel sd3


Volume montado?

mount | grep USBCriptografado


### 🛑 Problemas Comuns

# Erro: "Device busy"
umount -f /mnt/USBCriptografado  # Forçar desmontagem

bioctl -d sd3                    # Remover dispositivo


# Esqueceu a senha? 😱
dd if=/dev/zero of=/dev/rsd2c bs=1m  ### Destrua os dados e recomece ###


### ⭐ Dica Final: Sempre teste o procedimento com um dispositivo não crítico antes de usar com dados importantes!

