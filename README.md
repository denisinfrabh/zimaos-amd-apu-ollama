# Ollama AMD APU para ZimaOS

Este projeto disponibiliza um **App Personalizado para o ZimaOS** que permite utilizar a GPU integrada de APUs AMD para acelerar a inferência do Ollama através do backend Vulkan.

No meu ambiente, a imagem oficial `ollama/ollama:rocm` utilizava apenas a CPU. Após criar este App Personalizado utilizando uma imagem preparada para APUs AMD, o Ollama passou a reconhecer e utilizar a GPU integrada.

## Hardware utilizado

### Processador

- AMD Ryzen 5 3400G

### GPU

- Radeon Vega 11 (Integrada)

### Memória

- 16 GB DDR4

### Armazenamento

- SSD NVMe 512 GB

### Sistema Operacional

- ZimaOS v1.6.1

---

# Objetivo

Executar modelos LLM localmente utilizando a GPU integrada da APU AMD.

---

# Modelo testado

Foi realizado o teste com o modelo:

```
deepseek-coder:6.7b
```

Resultado do comando:

```bash
ollama ps
```

```
NAME                 PROCESSOR
deepseek-coder:6.7b  100% GPU
```

Isso confirma que o modelo foi carregado utilizando aceleração por GPU.

---

# Problema encontrado

Utilizando a imagem oficial do Ollama:

```
ollama/ollama:0.30.11-rocm
```

o comando

```bash
ollama ps
```

retornava:

```
PROCESSOR
100% CPU
```

Mesmo expondo os dispositivos:

```
/dev/kfd
/dev/dri
```

o Ollama fazia fallback para CPU.

---

# Solução

Foi criado um **App Personalizado** no ZimaOS utilizando uma imagem preparada especificamente para APUs AMD.

Imagem utilizada:

```
grinco/ollama-amd-apu:latest
```

Além disso foram adicionados os dispositivos:

```
/dev/kfd
/dev/dri
```

e as variáveis:

```
OLLAMA_VULKAN=1
OLLAMA_FLASH_ATTENTION=1
```

---

# Como criar o App Personalizado

No painel do ZimaOS:

```
Apps
    +
Criar App Personalizado
```

Importe o arquivo:

```
Ollama-AMD-APU.yaml
```

Após importar:

- Salve
- Aguarde o download da imagem
- Inicie o container

O Ollama ficará disponível normalmente para integração com:

- Open WebUI
- Continue
- VSCode
- OpenAI Compatible API

---

# Verificando se a GPU está sendo utilizada

Após carregar um modelo execute:

```bash
ollama ps
```

Resultado esperado:

```
NAME                 PROCESSOR
deepseek-coder:6.7b  100% GPU
```

---

# Observações

Este projeto foi desenvolvido especificamente para testar APUs AMD utilizando o ZimaOS.

No hardware utilizado foi possível utilizar a GPU integrada Radeon Vega 11 para inferência local.

Ainda não foram realizados testes com:

- Ryzen 5 5600G
- Ryzen 7 5700G
- Ryzen 7 8700G
- Radeon RX dedicadas

Contribuições e testes em outros hardwares são bem-vindos.

---

# Créditos

Agradecimento ao projeto que disponibiliza a imagem Docker preparada para APUs AMD.

```
grinco/ollama-amd-apu
```

Também agradeço à comunidade do ZimaOS e aos desenvolvedores do Ollama por disponibilizarem um excelente ecossistema para IA local.

---

# Licença

Este repositório disponibiliza apenas o arquivo YAML utilizado para criação do App Personalizado no ZimaOS.

Todos os créditos da imagem Docker pertencem aos respectivos desenvolvedores.
