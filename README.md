# Módulo 4 — Álgebra Linear: Vetores e Geometria

O [notebook](lab4.ipynb) traz só os quatro exercícios aplicados. As aulas do curso incluem também demonstrações de geometria de vetores (plots com `quiver`/`arrow`, soma, subtração, multiplicação escalar visualizadas no plano) — ficam resumidas em prosa aqui embaixo, porque o valor delas é conceitual, não o código de plotagem em si.

Vetores no NumPy carregam mais informação do que o valor bruto sugere. `np.array([1, 2, 3])` tem shape `(3,)`, mas `np.array([[1, 2, 3]])` tem shape `(1, 3)` — mesmo conteúdo numérico, estruturas diferentes. Essa distinção volta o tempo todo em pipelines de ML: multiplicação de matrizes, concatenação de features e broadcasting quebram silenciosamente quando você mistura vetor-linha com vetor-coluna sem prestar atenção no shape.

## Operações fundamentais

Soma, subtração e multiplicação escalar de vetores são operações elemento a elemento. O notebook mostra as duas formas de fazer isso: um loop manual preenchendo `np.empty(3)` posição por posição, e depois a versão vetorizada (`c = a + b`). O loop existe só para deixar explícito o que o NumPy está fazendo por baixo — na prática, a versão vetorizada é a única que se usa, porque é mais rápida e menos sujeita a erro de índice.

Produto vetorial (`np.cross`) só existe em R³ e retorna um vetor ortogonal aos dois vetores de entrada. A aplicação prática mais direta é encontrar o vetor normal a um plano definido por três pontos — é assim que motores gráficos calculam orientação de superfícies, e a mesma lógica aparece em geometria computacional.

Produto escalar (`np.dot`) foi testado quanto à propriedade distributiva: `u·(v+w) = u·v + u·w`. Confirmar isso na prática, não só de cabeça, ajuda a entender por que operações de dot product em batches de vetores (como em camadas densas de redes neurais) podem ser reordenadas sem mudar o resultado — é a mesma propriedade algébrica.

Transposição (`.T`) inverte o shape sem alterar os valores. Um vetor `(1, 3)` vira `(3, 1)`. Parece trivial, mas é a fonte mais comum de bug silencioso em multiplicação de matrizes: `A @ B` funciona ou quebra dependendo só da orientação dos vetores envolvidos.

## Exercícios

**1. Similaridade de usuários via produto escalar.** Dois vetores de preferência (`user1 = [4, 3, 2]`, `user2 = [1, 5, 4]`) e o produto escalar bruto como medida de similaridade. Funciona, mas tem uma limitação que já foi discutida em outro módulo: o produto escalar bruto é sensível à magnitude do vetor, não só à direção. Um usuário que avalia tudo com notas altas vai ter dot product alto com qualquer outro perfil, mesmo que as preferências relativas sejam diferentes. Em sistemas de recomendação de produção, isso é resolvido com similaridade de cosseno, que normaliza pela norma dos vetores.

**2. Ajuste percentual de preços.** Reajuste de 15% sobre um array de preços, feito como `prices + (prices * 0.15)` em vez de um loop com `for preco in prices`. Essa é a mesma lógica de normalização e scaling de features que aparece em pré-processamento de dados para modelos — na reconciliação ASSIST/Klini, por exemplo, ajustes proporcionais em massa sobre uma coluna de valores seguem o mesmo padrão vetorizado.

**3. Vetor unitário que maximiza o produto escalar com `v = [3, 4, 5]`.** Resolvido de duas formas. A analítica usa a identidade `cos(θ) = (u·v)/(‖u‖‖v‖)`: o produto escalar é máximo quando os vetores são paralelos e apontam na mesma direção (`θ = 0°`), o que dá `u = v/‖v‖` diretamente. A numérica usa `scipy.optimize.minimize` para achar o mesmo resultado por otimização com restrição (`‖x‖ = 1`), minimizando o negativo do produto escalar. As duas convergem para o mesmo vetor, o que serve como checagem cruzada: a solução analítica confirma que o otimizador numérico está configurado corretamente. Esse mesmo padrão — resolver por fórmula fechada quando existe, validar com um otimizador iterativo — é o que acontece quando você compara uma regressão linear resolvida por mínimos quadrados com a mesma regressão treinada por gradient descent.

**4. Vetor normal a um plano definido por três pontos.** `A = [1,2,3]`, `B = [4,5,6]`, `C = [7,8,9]`. Resultado: `[0, 0, 0]`, porque os três pontos são colineares (estão todos sobre a mesma reta, não definem um plano). O exercício não avisa isso — o resultado zero é o próprio dado informando que a premissa geométrica não se sustenta. Vale mais como lição sobre validar entrada do que como exercício de cross product em si.


