# ACESSO SEGURO A REGISTROS

Este arquivo permite o acesso seguro e a modificação de um conjunto de registros, geralmente usados para controlar um dispositivo. O uso deste código não tem sobrecarga em comparação com o acesso manual bit a bit, mas é mais seguro e fácil de usar.

Esta customização específica contém configurações para o controle do chip GP22 TDC.

## Uso

### Inicialização

1. Inclua este arquivo e defina `uint32_t GP22::registers_data[7]` em seu código.
   Exemplo:
   ```cpp
   #include "GP22_reg.h"
   uint32_t GP22::registers_data[7] = {0};
   ```

### `regWrite`

- Substitui um registro inteiro.
  Exemplo:
  ```cpp
  regWrite(GP22::REG0, 0x12345678);
  ```

### `regRead`

- Lê um registro inteiro.
  Exemplo:
  ```cpp
  uint32_t reg2Contents = regRead(GP22::REG2);
  ```

### `bitmaskWrite`

- Substitui os bits especificados no registro fornecido.
  Exemplo:
  ```cpp
  bitmaskWrite(GP22::REG0, GP22::REG0_ID0, 0xab);
  ```

**Aviso:** É possível fornecer a máscara errada para esta função sem gerar um erro de compilação. Por exemplo:
```cpp
bitmaskWrite(GP22::REG2, GP22::REG0_ID0, 0xab);
```
Compilará sem erros, mesmo que a máscara `REG0_ID0` não se refira ao registro `REG0`.

### `bitmaskRead`

- Lê os bits especificados no registro fornecido.
  Exemplo:
  ```cpp
  uint8_t IDBits = bitmaskRead(GP22::REG0, GP22::REG0_ID0);
  // Resultado: IDBits = 0xab após a chamada anterior
  ```

## Customização / Cópia

1. Copie este arquivo e renomeie-o.
2. Altere o namespace para um valor apropriado.
3. Defina o tamanho do registro usando a typedef `T_register`.
4. Atualize o enum `registers` para conter o número correto de rótulos de registro.
   Atualize a definição `extern T_register registers_data[<NUM_REGISTERS>];` para corresponder ao número de registros definidos no passo 4a.
5. Atualize os enums de bitset: um para cada registro, definindo a localização e os nomes das configurações disponíveis.
6. Inclua este arquivo em seu código e inicialize os dados antes de chamar qualquer uma das funções.
   Exemplo:
   ```cpp
   T_register MY_NAMESPACE::registers_data[<NUM_REGISTERS>];
   ```
7. Chame as funções conforme descrito acima para ler/escrever nos registros.

## Tamanho do Registro

Isso define o tamanho de um registro. Por exemplo, para um registro de 32 bits, use:
```cpp
typedef uint32_t T_register;
```

## Os Registros

O número de rótulos aqui deve corresponder ao tamanho de `registers_data`.

## Macros de Bitset

Use a macro `REG_BIT_DEFN` para definir as posições de início e término de todos os ajustes disponíveis em seu dispositivo.

## As Configurações

Use os valores de enum definidos aqui com as funções de bitset. Por exemplo:
```cpp
bitmaskWrite(GP22:REG0, GP22:REG0_DIV_FIRE, 0b1011);
```

## Exemplo de Esboço

Este é um exemplo de esboço que mostra como acessar e modificar um conjunto de registros do dispositivo.

Os registros são definidos em "GP22_reg.h" e são para um conversor de tempo-digital GP22, como exemplo.

Para adaptar este código para outro dispositivo, copie o arquivo GP22_reg.h e siga as instruções nele para modificá-lo de acordo com suas necessidades.