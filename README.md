# parallel-programming


<!--


python -m venv .venv
source .venv/bin/activate # Linux/Mac
.venv\Scripts\activate # Windows


/////////

pip install -r requirements.txt

-->

<p><b>UNICAP - 2026.1</b></p>

<p>Projeto desenvolvido para a disciplina de <b>Programação Paralela e Distribuída.</b></p>

<p> <b>Alunos:</b> Matheus Veríssimo, Maria Luiza Ribeiro, Rafael Sampaio, Vinícius Martins. </p>
<br>

<p> <b>Resumo do projeto:</b>
Esse projeto compara desempenho entre <b>CPU e GPU</b> na encriptação de grandes
volumes de dados, identificando o ponto em que o paralelismo em GPU passa a
compensar em relação à CPU. O algoritmo escolhido é o <b>ChaCha20</b>, uma cifra
de fluxo moderna usada em TLS 1.3, WireGuard e no protocolo do Signal. Cada bloco
de 64 bytes depende apenas de um contador, sem dependência entre blocos, o que
permite paralelismo total na GPU mapeando <b>uma thread CUDA para cada bloco da
mensagem</b>.
</p>

<p> <b>Por que mudamos de AES-CTR para ChaCha20:</b>
Inicialmente o algoritmo escolhido era o AES no modo CTR. Migramos para o
ChaCha20 porque ele preserva exatamente a mesma propriedade de paralelismo
(blocos independentes baseados em contador), mas elimina a complexidade interna
do AES, como S-box, MixColumns em GF(2⁸) e key expansion. Como o foco da
disciplina é <b>programação paralela</b> e não criptografia, o ChaCha20 nos
permite gastar o tempo estudando o paralelismo em vez de implementar detalhes
do algoritmo criptográfico.
</p>

---

## Pontos de atenção

### Movimentação de memória CPU x GPU

Em PyCUDA existem duas formas equivalentes de mover dados entre CPU e GPU.
Ambas resultam nas mesmas chamadas internas (`cudaMemcpy`); a diferença é apenas
sintática.

**Forma apresentada no livro em sala** (`gpuarray`):

```python
import pycuda.gpuarray as gpuarray

d_a = gpuarray.to_gpu(h_a)        # CPU -> GPU
kernel(d_a)                       # executa
resultado = d_a.get()             # GPU -> CPU
```

**Forma utilizada neste projeto** (`drv.In / drv.Out / drv.InOut`):

```python
import pycuda.driver as drv

kernel(drv.In(h_a), drv.Out(resultado), grid=..., block=...)
```

**As duas formas são sinônimos funcionais.** Produzem o mesmo resultado e a
mesma performance. A diferença é que `drv.In/Out/InOut` permite passar entrada
e saída diretamente na chamada do kernel, sem variáveis intermediárias.

Adotamos a forma `drv.In/Out/InOut` por ser **mais concisa e intuitiva**. Foi uma alternativa sugerida pela LLM de apoio que
utilizamos durante o desenvolvimento. **A funcionalidade é equivalente** ao
padrão apresentado em aula, e qualquer trecho pode ser reescrito usando
`gpuarray.to_gpu(...)` / `.get()` sem alteração de comportamento.
