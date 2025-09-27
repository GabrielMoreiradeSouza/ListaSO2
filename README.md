# ListaSO2

# QUESTÃO 1

Relatório – Corrida de Cavalos com Threads e Mutex (Simulação com Apostas)

Objetivo

Simular uma corrida de cavalos com múltiplas threads concorrentes, onde cada cavalo é controlado por uma thread independente, percorrendo uma distância até a linha de chegada.

Além disso, o programa:

* Permite o usuário apostar em um cavalo antes da corrida;
* Apresenta o progresso dos cavalos em tempo real;
* Garante que a ordem de chegada (colocação) seja registrada de forma correta e sem condições de corrida, usando mutex para sincronização.

Estrutura Geral do Código

Inicialização:

* Um vetor com 6 cavalos é criado. Cada cavalo possui:

  * id, nome, velocidade_base, distancia_percorrida, colocacao e uma seed para aleatoriedade.
* A velocidade base é gerada aleatoriamente entre 4 e 9 para cada cavalo.

Threads:

* Cada cavalo é representado por uma thread, que executa a função "correr".
* Dentro dessa função:

  * O cavalo avança somando sua velocidade base com um número aleatório entre 0 e 7.
  * O tempo de espera entre os avanços é proporcional à sua velocidade.
  * Ao atingir a linha de chegada (100 metros), o cavalo entra numa região crítica protegida por mutex para atualizar a colocação final.

Sincronização com Mutex

Problema abordado:

* Sem sincronização, múltiplas threads poderiam alterar a variável colocacao_atual simultaneamente, resultando em dados inconsistentes ou repetições de colocação.

Solução aplicada:

* Foi utilizado um mutex global para proteger a região onde:

  * A colocação é incrementada e atribuída ao cavalo que chegou.
  * O primeiro cavalo a completar a corrida define o vencedor da aposta.

Resultado:

* O uso do mutex garante integridade nas operações entre as threads.
* A colocação dos cavalos e o vencedor da corrida são sempre definidos corretamente.

Apostas

Antes da corrida:

* O jogador realiza uma aposta selecionando um número de cavalo entre 1 e 6.

Após a corrida:

* O programa compara o cavalo apostado com o vencedor real.
* Mostra a classificação final da corrida e informa se o jogador venceu ou perdeu a aposta.

Exemplo de Execução

🎰 BEM-VINDO AO HIPISMO DIGITAL! 🎰

🐎 CAVALOS DISPONÍVEIS PARA APOSTA:
1 - Relâmpago
2 - Pé de Pano
3 - Trovão
4 - Estrela Cadente
5 - Furacão
6 - Cometa

Em qual cavalo você aposta? (1 a 6): 3
✅ Você apostou no Trovão! Boa sorte!

🏇 PREPARANDO A CORRIDA DE CAVALOS!
...

🐎 Relâmpago: 97 metros
🎉 Relâmpago terminou em 1º lugar!
🐎 Pé de Pano: 100 metros
🎉 Pé de Pano terminou em 2º lugar!
...

🎯 RESULTADO DA APOSTA:
❌ Que pena! Você errou.
😢 Trovão não venceu a corrida.
⭐ O vencedor foi Relâmpago

Conclusão

A utilização de threads permite simular de forma realista uma corrida paralela entre cavalos. No entanto, sem mecanismos de sincronização, como mutex, dados compartilhados como a colocação dos cavalos podem ser corrompidos.

Neste código, a exclusão mútua foi essencial para garantir que cada thread atualizasse os dados de forma segura, sem colisões, garantindo a precisão do resultado final e da aposta do jogador.

Como compilar e executar

O código foi testado em uma máquina virtual Linux (WSL Debian):

gcc -o corrida corrida.c -lpthread
./corrida


# QUESTÃO 2


# QUESTÃO 3

Questão 3
Simular M contas bancárias acessadas por T threads, onde cada thread realiza transferências aleatórias entre contas.
⦁	Sem controle, as threads podem gerar condições de corrida, alterando a soma total de dinheiro.
⦁	Usando mutexes, a soma total deve ser preservada.
⦁	O programa compara as duas situações (sem trava e com trava).
Explicação do codigo
inicio
⦁	Criamos um vetor de contas, cada uma iniciada com um saldo fixo.
⦁	Cada conta possui um mutex próprio, para controlar acessos concorrentes.
Execução das threads
⦁	Cada thread executa várias transferências.
⦁	Em cada transferência, escolhe uma conta de origem, uma de destino e um valor aleatório.
⦁	A origem perde o valor e a conta de destino recebe o mesmo valor.
Versão sem trava
⦁	As threads modificam as contas diretamente, sem usar mutex.
⦁	Como resultado, podem ocorrer condições de corrida, onde dois acessos simultâneos sobrescrevem saldos incorretamente.
⦁	Isso faz a soma total do dinheiro variar, “criando” ou “perdendo” dinheiro.
Versão com trava
⦁	Antes de alterar as contas, as threads travam os mutexes correspondentes.
⦁	A ordem de travamento é sempre a mesma (primeiro a conta de índice menor, depois a maior), para evitar deadlock.
⦁	Após a operação, os mutexes são liberados.
⦁	Dessa forma, nenhuma operação se perde e a soma total se mantém constante.
Verificação do invariante
⦁	O programa calcula a soma inicial e a soma final das contas.
⦁	No final da execução com mutex, há um assert que garante que a soma final seja igual à inicial
Como Compilar e Executar
usei maquina virtual linux wsl debian
⦁	gcc -o transf exer3.c
⦁	./transf

exemplos de saida
peixoto@LAPTOP-SIFK91NF:~/so2/Pthreads$ ./transf  
Sem trava: soma inicial = 10000, soma final = 63039  
Com trava: soma inicial = 10000, soma final = 10000
⦁	Sem trava: a soma final varia a cada execução, podendo ser maior ou menor que a soma inicial.
⦁	Com trava: a soma final é sempre idêntica à inicial, mostrando que a sincronização foi bem-sucedida.
conclusão
concluimos que que a sicronização e essencial em sistemas concorrentes.com mutex garantimos exclusao mutua, ja sem mutex ocorre condicao de corrida onde corrompem os dados.




//QUESTÃO 4

Questão 4
Construção de uma linha de processamento com três threads (captura, processamento e gravação) conectadas por filas limitadas e protegidas por mutex e variáveis de condição.
Como Compilar e Executar
usei maquina virtual linux wsl debian
⦁	gcc -o proc exer4.c
⦁	./proc
Como a Sincronização Funciona
Para evitar que as threads "se atropelem" ao usar as filas, usamos duas ferramentas:
⦁	Mutex (O Cadeado):
Cada fila tem um "cadeado". Antes de mexer na fila, uma thread precisa pegar esse cadeado. Isso garante que só uma thread por vez acesse a fila, evitando bagunça e perda de dados.
⦁	Variáveis de Condição (Os Sinais):
Elas evitam que as threads gastem energia à toa.Se uma thread quer adicionar um item e a fila está cheia, ela dorme até receber o sinal de que um espaço foi liberado.Se uma thread quer remover um item e a fila está vazia, ela dorme até receber o sinal de que um item novo chegou.
Execução
peixoto@LAPTOP-SIFK91NF:~/so2/Pthreads$ ./proc  
Iniciando pipeline...  
[CAPTURA] Gerou: 0  
[CAPTURA] Gerou: 1  
[CAPTURA] Gerou: 2  
[CAPTURA] Gerou: 3  
[CAPTURA] Gerou: 4  
[CAPTURA] Gerou: 5  
[CAPTURA] Gerou: 6  
[PROC] Processou 0 -> 0  
[PROC] Processou 1 -> 2  
[CAPTURA] Gerou: 7  
[GRAVACAO] Gravou item final: 0  
[CAPTURA] Gerou: 8  
[GRAVACAO] Gravou item final: 2  
[PROC] Processou 2 -> 4  
[GRAVACAO] Gravou item final: 4  
[PROC] Processou 3 -> 6  
[CAPTURA] Gerou: 9  
[CAPTURA] Gerou: 10  
[GRAVACAO] Gravou item final: 6  
[PROC] Processou 4 -> 8  
[GRAVACAO] Gravou item final: 8  
[PROC] Processou 5 -> 10  
[CAPTURA] Gerou: 11  
[CAPTURA] Gerou: 12  
[GRAVACAO] Gravou item final: 10  
[PROC] Processou 6 -> 12  
[GRAVACAO] Gravou item final: 12  
[PROC] Processou 7 -> 14  
[CAPTURA] Gerou: 13  
[CAPTURA] Gerou: 14  
[GRAVACAO] Gravou item final: 14  
[PROC] Processou 8 -> 16  
[GRAVACAO] Gravou item final: 16  
[PROC] Processou 9 -> 18  
[PROC] Processou 10 -> 20  
[PROC] Processou 11 -> 22  
[PROC] Processou 12 -> 24  
[PROC] Processou 13 -> 26  
[PROC] Processou 14 -> 28  
[GRAVACAO] Gravou item final: 18  
[GRAVACAO] Gravou item final: 20  
[GRAVACAO] Gravou item final: 22  
[GRAVACAO] Gravou item final: 24  
[GRAVACAO] Gravou item final: 26  
[GRAVACAO] Gravou item final: 28  
[CAPTURA] Enviou SINAL_FIM.  
[PROCESSAMENTO] Recebeu SINAL_FIM. Repassando.  
[GRAVACAO] Recebeu SINAL_FIM. Encerrando.  
Pipeline finalizado.
Análise do Resultado
Tudo foi processado: Nenhum item foi perdido. Todos os 15 itens gerados passaram pelos 3 estágios.  
A ordem está correta: Para cada item, a ordem foi sempre Captura -> Processamento -> Gravação.  
O programa terminou certo: O SINAL_FIM foi passado de thread em thread, garantindo que o programa encerrasse de forma limpa e sem travar.
