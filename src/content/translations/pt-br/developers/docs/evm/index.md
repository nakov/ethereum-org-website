---
title: Máquina virtual do Ethereum (EVM)
description: Uma introdução à máquina virtual do Ethereum e como ela se relaciona com o estado, as transações e os contratos inteligentes.
lang: pt-br
---

O contexto físico da Máquina Virtual do Ethereum (EVM, na sigla em inglês) não pode ser descrito da mesma maneira que é descrita uma nuvem no céu ou uma onda no meio do oceano, se não que deve ser _entendido_ como uma entidade singular mantida por milhares de computadores conectados operando um cliente de Ethereum.

O protocolo do Ethereum existe única e exclusivamente para manter a operação contínua, ininterrupta e imutável desta máquina de estado especial; é o ambiente onde residem todas as contas de Ethereum e os contratos inteligentes. Para qualquer bloco da cadeia, o Ethereum tem um estado "canônico", e a EVM é a responsável por definir as regras para registrar um novo estado válido de um bloco para o seguinte.

## Pré-requisitos {#prerequisites}

Alguma familiaridade básica com a terminologia comum em ciência da computação, como [bytes](https://wikipedia.org/wiki/Byte), [memória](https://wikipedia.org/wiki/Computer_memory) e [pilha](<https://wikipedia.org/wiki/Stack_(abstract_data_type)>) é necessária para entender a EVM. Também seria ótimo estar familiarizado com conceitos de criptografia/blockchain como [funções hash](https://wikipedia.org/wiki/Cryptographic_hash_function), [prova de trabalho](https://wikipedia.org/wiki/Proof_of_work) e [árvore de Merkle](https://wikipedia.org/wiki/Merkle_tree).

## Do livro-razão para a máquina de estado {#from-ledger-to-state-machine}

A analogia de um 'livro-razão distribuído' é muitas vezes usada para descrever blockchains como o Bitcoin, que permite uma moeda descentralizada usando ferramentas fundamentais de criptografia. Uma criptomoeda comporta-se como uma moeda "normal" devido às regras que regem o que pode e não pode se fazer para modificar o livro-razão. Por exemplo, um endereço de Bitcoin não pode gastar mais Bitcoin do que o recebido previamente. Essas regras sustentam todas as transações em Bitcoin e em muitas outras blockchains.

Embora Ethereum tenha sua própria criptomoeda nativa (Ether), que segue quase exatamente as mesmas regras intuitivas, ele também permite dispor de uma função muito mais poderosa: [os contratos inteligentes](/developers/docs/smart-contracts/). Para este recurso mais complexo, uma analogia mais sofisticada é necessária. Em vez de um livro-razão distribuído, Ethereum é uma [máquina de estado distribuída](https://wikipedia.org/wiki/Finite-state_machine). O estado do Ethereum é uma grande estrutura de dados que contém não apenas todas as contas e saldos, mas também um _estado da máquina_, que pode mudar de bloco para bloco de acordo com um conjunto predefinido de regras, as quais podem executar código de máquina arbitrário. As regras específicas para mudar o estado de bloco em bloco são definidas pela EVM.

![Um diagrama mostrando a criação da EVM](./evm.png) _Diagrama adaptado do [Ethereum EVM ilustrado](https://takenobu-hs.github.io/downloads/ethereum_evm_illustrated.pdf)_

## A função de transição do estado Ethereum {#the-ethereum-state-transition-function}

A EVM se comporta como uma função matemática seria: de acordo com a entrada, ele produz uma saída determinística. Portanto, é bastante útil descrever mais formalmente o Ethereum como tendo uma **função de transição de estado**:

```
Y(S, T)= S'
```

Dado um antigo estado `(S)` e um novo conjunto de transações válidas `(T)`, a função de transição de estado de Ethereum `Y(S, T)` produz um novo estado de saída válido `S'`

### Estado {#state}

No contexto do Ethereum, o estado é uma enorme estrutura de dados chamada [Merkle Patricia Trie](https://eth.wiki/en/fundamentals/patricia-tree)modificada, que mantém todas as [contas](/developers/docs/accounts/) vinculadas por hashes e redutíveis a um único hash raiz armazenado na blockchain.

### Transações {#transactions}

Transações são instruções assinadas criptograficamente de contas. Existem dois tipos de transações: as que resultam em chamadas de mensagem e as que resultam na criação de contratos.

A criação do contrato resulta na criação de uma nova conta de contrato que contém o bytecode compilado do [contrato inteligente](/developers/docs/smart-contracts/anatomy/). Sempre que outra conta faz uma mensagem de chamada a esse contrato, ele executa seu bytecode.

## Instruções da EVM {#evm-instructions}

A EVM é executada como uma [máquina de pilha](https://wikipedia.org/wiki/Stack_machine) com uma profundidade de 1.024 itens. Cada item é uma palavra de 256 bits, que foi escolhida para facilitar o uso com criptografia de 256 bits (como hashes Keccak-256 ou assinaturas secp256k1).

Durante a execução, a EVM mantém uma _memória transiente_ (como um array de bytes direcionado por palavra) que não persiste entre as transações.

Os contratos, no entanto, contêm uma árvore Merkle Patricia de _armazenamento_ (como um array direcionado por palavras) associada com a conta em questão e parte do estado global.

O bytecode compilado do contrato inteligente executa como um número de [opcodes de EVM](/developers/docs/evm/opcodes), que realizam operações padrão de stake como `XOR`, `AND`, `ADD`, `SUB` etc. A EVM também implementa um número de operações de stake específicas da blockchain, como `ADDRESS`, `BALANCE`, `BLOCKHASH` etc.

![Diagrama mostrando onde o consumo de gás é utilizado para as operações da EVM](../gas/gas.png) _Diagrama adaptado do [Ethereum EVM ilustrado](https://takenobu-hs.github.io/downloads/ethereum_evm_illustrated.pdf)_

## Implementações da EVM {#evm-implementations}

Todas as implementações da EVM devem aderir à especificação descrita no Ethereum Yellowpaper.

Durante o histórico de 5 anos do Ethereum, a EVM passou por várias revisões e existem várias implementações da EVM em várias linguagens de programação.

Todos os [clientes Ethereum](/developers/docs/nodes-and-clients/#execution-clients) incluem uma implementação de EVM. Além disso, existem várias implementações independentes, incluindo:

- [Py-EVM](https://github.com/ethereum/py-evm) - _Python_
- [evmone](https://github.com/ethereum/evmone) - _C++_
- [ethereumjs-vm](https://github.com/ethereumjs/ethereumjs-vm) - _JavaScript_
- [eEVM](https://github.com/microsoft/eevm) - _C++_

## Leitura adicional {#further-reading}

- [Ethereum Yellowpaper](https://ethereum.github.io/yellowpaper/paper.pdf)
- [Jellopaper também conhecido como KEVM: semânticos de EVM em K](https://jellopaper.org/)
- [O Beigepaper](https://github.com/chronaeon/beigepaper)
- [Códigos de operação da EVM](https://www.ethervm.io/)
- [Uma breve introdução à documentação do Solidy](https://docs.soliditylang.org/en/latest/introduction-to-smart-contracts.html#index-6)

## Tópicos relacionados {#related-topics}

- [Gás](/developers/docs/gas/)
